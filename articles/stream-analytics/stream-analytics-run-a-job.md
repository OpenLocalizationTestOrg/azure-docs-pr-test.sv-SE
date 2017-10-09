---
title: aaaHow toostart direktuppspelningsjobb i Stream Analytics | Microsoft Docs
description: "Hur kör ett direktuppspelningsjobb i Azure Stream Analytics | Learning sökvägssegment."
keywords: direktuppspelningsjobb
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 67aa14860c38cbd0535d0ec4f23729445d0185c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="72978-104">Hur toorun en streaming job i Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72978-104">How toorun a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="72978-105">När ett jobb indata, har fråga och utdata alla angetts du kan starta hello Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="72978-105">When a job input, query and output have all been specified you can start hello Stream Analytics job.</span></span>

<span data-ttu-id="72978-106">toostart ditt jobb:</span><span class="sxs-lookup"><span data-stu-id="72978-106">toostart your job:</span></span>

1. <span data-ttu-id="72978-107">I hello Azure klassiska portal från hello jobbet instrumentpanelen, klickar du på **starta** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="72978-107">In hello Azure Classic portal, from hello job dashboard, click **Start** at hello bottom of hello page.</span></span>
   
   ![Starta jobbet knappen](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="72978-109">I hello Azure-portalen klickar du på **starta** hello överst på sidan jobb.</span><span class="sxs-lookup"><span data-stu-id="72978-109">In hello Azure portal, click **Start** at hello top of your job page.</span></span>
   
   ![Azure portal startjobb knappen](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="72978-111">Ange en **starta utdata** värdet toodetermine när jobbet startas utdata.</span><span class="sxs-lookup"><span data-stu-id="72978-111">Specify a **Start Output** value toodetermine when this job will start producing output.</span></span> <span data-ttu-id="72978-112">hello standardinställningen för jobb som tidigare inte har startats är **jobbet starttid**, vilket innebär att jobbet hello omedelbart startar av bearbetningsdata.</span><span class="sxs-lookup"><span data-stu-id="72978-112">hello default setting for jobs that have not previously been started is **Job Start Time**, which means that hello job will immediately start processing data.</span></span> <span data-ttu-id="72978-113">Du kan också ange en **anpassad** tid i hello tidigare (för förbrukning av historiska data) eller hello framtida (toodelay bearbetning förrän en framtida tid).</span><span class="sxs-lookup"><span data-stu-id="72978-113">You can also specify a **Custom** time in hello past (for consuming historical data) or hello future (toodelay processing until a future time).</span></span> <span data-ttu-id="72978-114">För fall när ett jobb har tidigare startas och stoppas, hello alternativet **stoppats senast** finns i ordning tooresume hello jobbet från hello senast utdata och undvika dataförlust.</span><span class="sxs-lookup"><span data-stu-id="72978-114">For cases when a job has been previously started and stopped, hello option **Last Stopped Time** is available in order tooresume hello job from hello last output time and avoid data loss.</span></span>  
   
   ![Starta strömning jobbet tid](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal starta direktuppspelningsjobbet tid](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="72978-117">Bekräfta dina val.</span><span class="sxs-lookup"><span data-stu-id="72978-117">Confirm your selection.</span></span> <span data-ttu-id="72978-118">hello jobbets status ändras för*Start* och kommer inom kort för att flytta*kör* när hello jobbet har startats.</span><span class="sxs-lookup"><span data-stu-id="72978-118">hello job status will change too*Starting* and will shortly move too*Running* once hello job has started.</span></span> <span data-ttu-id="72978-119">Du kan övervaka hello hello **starta** åtgärd i hello **Meddelandehubben**:</span><span class="sxs-lookup"><span data-stu-id="72978-119">You can monitor hello progress of hello **Start** operation in hello **Notification Hub**:</span></span>
   
   ![jobbet pågår för strömning](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-portalen strömning jobb pågår](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="72978-122">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="72978-122">Get help</span></span>
<span data-ttu-id="72978-123">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="72978-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="72978-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="72978-124">Next steps</span></span>
* [<span data-ttu-id="72978-125">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72978-125">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="72978-126">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="72978-126">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="72978-127">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="72978-127">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="72978-128">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="72978-128">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="72978-129">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="72978-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

