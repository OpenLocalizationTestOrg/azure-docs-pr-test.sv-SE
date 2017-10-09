---
title: aaa provtagning indata i Azure Stream Analytics | Microsoft Docs
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
ms.openlocfilehash: 9637a8664de099eebb8f5654036d2957f4c6b7ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-input-stream-sampling"></a><span data-ttu-id="7b25d-104">Azure Stream Analytics indata-stream provtagning</span><span class="sxs-lookup"><span data-stu-id="7b25d-104">Azure Stream Analytics input-stream sampling</span></span>

<span data-ttu-id="7b25d-105">Du kan prova inkommande händelser som kommer från en fil och testa frågor i hello portal utan att behöva toostart eller stoppa ett jobb med hjälp av Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="7b25d-105">By using Azure Stream Analytics, you can sample input events that come from a file and test queries in hello portal without needing toostart or stop a job.</span></span>

## <a name="testing-your-query"></a><span data-ttu-id="7b25d-106">Testa din fråga</span><span class="sxs-lookup"><span data-stu-id="7b25d-106">Testing your query</span></span>

<span data-ttu-id="7b25d-107">Öppna hello i hello Stream Analytics-jobbet informationsfönstret **frågeredigeraren** bladet genom att klicka på hello Frågenamnet under **frågan**.</span><span class="sxs-lookup"><span data-stu-id="7b25d-107">In hello Stream Analytics job details pane, open hello **Query editor** blade by clicking hello query name under **Query**.</span></span> <span data-ttu-id="7b25d-108">(I vårt Exempelscenario, eftersom ingen fråga har skapats ännu, klickar du på hello **< >** platshållaren.)</span><span class="sxs-lookup"><span data-stu-id="7b25d-108">(In our example scenario, because no query has been created yet, click hello **< >** placeholder.)</span></span>

![hello Stream Analytics query editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

<span data-ttu-id="7b25d-110">hello omfattande editor bladet för att skapa frågan visas som den var i hello föregående versionen.</span><span class="sxs-lookup"><span data-stu-id="7b25d-110">hello rich editor blade for creating your query is displayed as it was in hello previous release.</span></span> <span data-ttu-id="7b25d-111">Nu hello bladet har uppdaterats med en ny vänster fönsterruta som visar hello indata och utdata som används av hello frågan och beskrivning för det här jobbet.</span><span class="sxs-lookup"><span data-stu-id="7b25d-111">Now hello blade has been updated with a new left pane that shows hello inputs and outputs that are used by hello query and defined for this job.</span></span>

![hello Stream Analytics query editor inmatningar och matar ut listor](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

<span data-ttu-id="7b25d-113">Dessutom visas är ytterligare indata och utdata, som inte har definierats.</span><span class="sxs-lookup"><span data-stu-id="7b25d-113">Also shown are an additional input and output, which are not defined.</span></span> <span data-ttu-id="7b25d-114">De kommer från hello fråga mall som du börjar med.</span><span class="sxs-lookup"><span data-stu-id="7b25d-114">They come from hello new query template that you start with.</span></span> <span data-ttu-id="7b25d-115">De ändrar eller försvinna även helt, när du redigerar hello frågan.</span><span class="sxs-lookup"><span data-stu-id="7b25d-115">They change, or even disappear altogether, as you edit hello query.</span></span> <span data-ttu-id="7b25d-116">Du kan ignorera dem just nu.</span><span class="sxs-lookup"><span data-stu-id="7b25d-116">You can safely ignore them for now.</span></span>

<span data-ttu-id="7b25d-117">tootest med indata exempeldata, högerklicka på någon av dina inmatningar och välj sedan **ladda upp exempeldata från filen**.</span><span class="sxs-lookup"><span data-stu-id="7b25d-117">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

![hello exempeldata för Stream Analytics query editor Överför från fil (kommando)](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

<span data-ttu-id="7b25d-119">När hello överföringen är klar, klickar du på **Test** tootest den här frågan mot hello exempeldata du angav.</span><span class="sxs-lookup"><span data-stu-id="7b25d-119">After hello upload is complete, click **Test** tootest this query against hello sample data you have just provided.</span></span>

![hello Stream Analytics query editor testknappen](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

<span data-ttu-id="7b25d-121">Om du vill toosave hello testet av utdata för senare användning, visas hello utdata från frågan i hello webbläsare med en länk toohello nedladdningsresultaten.</span><span class="sxs-lookup"><span data-stu-id="7b25d-121">If you want toosave hello test output for later use, hello output of your query is displayed in hello browser with a link toohello download results.</span></span> <span data-ttu-id="7b25d-122">Du kan nu enkelt och upprepade gånger ändra din fråga och testa den upprepade gånger toosee hur hello utdata ändras.</span><span class="sxs-lookup"><span data-stu-id="7b25d-122">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor exempel på utdata](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

<span data-ttu-id="7b25d-124">En andra utdata har lagts till i hello föregående bild, kan anropas **HighAvgTempOutput**.</span><span class="sxs-lookup"><span data-stu-id="7b25d-124">In hello preceding image, a second output has been added, called **HighAvgTempOutput**.</span></span>

<span data-ttu-id="7b25d-125">Du kan se hello resultaten för varje utdata separat och växla mellan dem när du använder flera utdata i en fråga.</span><span class="sxs-lookup"><span data-stu-id="7b25d-125">When you use multiple outputs in a query, you can see hello results for each output separately and easily toggle between them.</span></span>

<span data-ttu-id="7b25d-126">När du är nöjd med resultaten hello du spara frågan, starta jobbet, tillbaka sitta och titta på hello Magiskt tal för Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="7b25d-126">After you are satisfied with hello results, you can save your query, start your job, sit back and watch hello magic of Stream Analytics.</span></span>

## <a name="get-help"></a><span data-ttu-id="7b25d-127">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="7b25d-127">Get help</span></span>

<span data-ttu-id="7b25d-128">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="7b25d-128">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7b25d-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7b25d-129">Next steps</span></span>
* [<span data-ttu-id="7b25d-130">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7b25d-130">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="7b25d-131">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="7b25d-131">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="7b25d-132">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="7b25d-132">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="7b25d-133">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="7b25d-133">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="7b25d-134">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="7b25d-134">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
