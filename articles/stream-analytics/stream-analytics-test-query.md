---
title: "frågetestning för aaaAzure Stream Analytics | Microsoft Docs"
description: "Hur tootest dina frågor i Stream Analytics-jobb."
keywords: "Testa fråga, felsöka frågan"
documentation center: 
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
ms.openlocfilehash: 3b141d98332fdc170e696e181c8446796a86f78e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a><span data-ttu-id="ba0ef-104">Testa Azure Stream Analytics-frågor i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="ba0ef-104">Test Azure Stream Analytics queries in hello Azure portal</span></span>

<span data-ttu-id="ba0ef-105">Du kan testa frågor i hello Azure-portalen utan att behöva toostart eller stoppa ett jobb med Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-105">With Azure Stream Analytics, you can test queries in hello Azure portal without needing toostart or stop a job.</span></span>

## <a name="test-hello-input"></a><span data-ttu-id="ba0ef-106">Testa hello indata</span><span class="sxs-lookup"><span data-stu-id="ba0ef-106">Test hello input</span></span>

1. <span data-ttu-id="ba0ef-107">tootest med indata exempeldata, högerklicka på någon av dina inmatningar och välj sedan **ladda upp exempeldata från filen**.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-107">tootest with sample input data, right-click any of your inputs, and then select **Upload sample data from file**.</span></span>

    ![Stream analytics query editor Testa fråga](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. <span data-ttu-id="ba0ef-109">När hello överföringen är klar, klickar du på **Test** tootest den här frågan mot hello exempeldata du har angett.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-109">After hello upload is complete, click **Test** tootest this query against hello sample data you have provided.</span></span>

    ![Stream analytics fråga editor test exempeldata](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

<span data-ttu-id="ba0ef-111">hello utdata från frågan visas i hello webbläsare med länken resultat bör du vill toosave hello testet av utdata för senare användning.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-111">hello output of your query is displayed in hello browser, with Download results link should you want toosave hello test output for later use.</span></span> <span data-ttu-id="ba0ef-112">Du kan nu enkelt och upprepade gånger ändra din fråga och testa den upprepade gånger toosee hur hello utdata ändras.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-112">You can now easily and iteratively modify your query and test it repeatedly toosee how hello output changes.</span></span>

![Stream Analytics query editor exempel på utdata](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

<span data-ttu-id="ba0ef-114">Du kan se hello resultat för både utdata separat och växla mellan dem med flera utdata som används i en fråga.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-114">With multiple outputs used in a query, you can see hello results for both outputs separately and easily toggle between them.</span></span>

<span data-ttu-id="ba0ef-115">När du är nöjd med hello resultat visas i hello webbläsare, spara frågan, starta jobbet och kan bearbeta händelser utan fel.</span><span class="sxs-lookup"><span data-stu-id="ba0ef-115">After you are satisfied with hello results shown in hello browser, you can save your query, start your job, and let it process events without error.</span></span>

## <a name="get-help"></a><span data-ttu-id="ba0ef-116">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="ba0ef-116">Get help</span></span>

<span data-ttu-id="ba0ef-117">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ba0ef-117">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba0ef-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba0ef-118">Next steps</span></span>

* [<span data-ttu-id="ba0ef-119">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ba0ef-119">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ba0ef-120">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ba0ef-120">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ba0ef-121">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="ba0ef-121">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ba0ef-122">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="ba0ef-122">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ba0ef-123">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="ba0ef-123">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
