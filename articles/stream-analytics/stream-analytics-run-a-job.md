---
title: "Starta strömning jobb i Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 9a3ff37a893b0f29a2ac2eda6cd50687ee779ead
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a><span data-ttu-id="bd57e-104">Hur du kör ett direktuppspelningsjobb i Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd57e-104">How to run a streaming job in Azure Stream Analytics</span></span>
<span data-ttu-id="bd57e-105">När ett jobb som indata, fråga och utdata har alla angetts du kan starta Stream Analytics-jobbet.</span><span class="sxs-lookup"><span data-stu-id="bd57e-105">When a job input, query and output have all been specified you can start the Stream Analytics job.</span></span>

<span data-ttu-id="bd57e-106">Att starta jobbet:</span><span class="sxs-lookup"><span data-stu-id="bd57e-106">To start your job:</span></span>

1. <span data-ttu-id="bd57e-107">I den klassiska Azure-portalen från jobbet-instrumentpanelen klickar du på **starta** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="bd57e-107">In the Azure Classic portal, from the job dashboard, click **Start** at the bottom of the page.</span></span>
   
   ![Starta jobbet knappen](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   <span data-ttu-id="bd57e-109">I Azure-portalen klickar du på **starta** överst på sidan jobb.</span><span class="sxs-lookup"><span data-stu-id="bd57e-109">In the Azure portal, click **Start** at the top of your job page.</span></span>
   
   ![Azure portal startjobb knappen](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. <span data-ttu-id="bd57e-111">Ange en **starta utdata** värde för att bestämma när jobbet startar utdata.</span><span class="sxs-lookup"><span data-stu-id="bd57e-111">Specify a **Start Output** value to determine when this job will start producing output.</span></span> <span data-ttu-id="bd57e-112">Standardinställningen för jobb som tidigare inte har startats är **jobbet starttid**, vilket innebär att starta jobbet direkt databearbetning.</span><span class="sxs-lookup"><span data-stu-id="bd57e-112">The default setting for jobs that have not previously been started is **Job Start Time**, which means that the job will immediately start processing data.</span></span> <span data-ttu-id="bd57e-113">Du kan också ange en **anpassad** tid i förflutna (för förbrukning av historiska data) eller framtida (för att fördröja behandlingen tills en framtida tid).</span><span class="sxs-lookup"><span data-stu-id="bd57e-113">You can also specify a **Custom** time in the past (for consuming historical data) or the future (to delay processing until a future time).</span></span> <span data-ttu-id="bd57e-114">I fall när ett jobb har tidigare startas och stoppas, alternativet **stoppats senast** är tillgänglig för att återuppta jobbet från den senaste tid för utdata och undvika dataförlust.</span><span class="sxs-lookup"><span data-stu-id="bd57e-114">For cases when a job has been previously started and stopped, the option **Last Stopped Time** is available in order to resume the job from the last output time and avoid data loss.</span></span>  
   
   ![Starta strömning jobbet tid](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal starta direktuppspelningsjobbet tid](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. <span data-ttu-id="bd57e-117">Bekräfta dina val.</span><span class="sxs-lookup"><span data-stu-id="bd57e-117">Confirm your selection.</span></span> <span data-ttu-id="bd57e-118">Jobbet har statusen ändras till *Start* och kommer snart att flytta till *kör* när jobbet har startats.</span><span class="sxs-lookup"><span data-stu-id="bd57e-118">The job status will change to *Starting* and will shortly move to *Running* once the job has started.</span></span> <span data-ttu-id="bd57e-119">Du kan övervaka förloppet för den **starta** åtgärden i den **Meddelandehubben**:</span><span class="sxs-lookup"><span data-stu-id="bd57e-119">You can monitor the progress of the **Start** operation in the **Notification Hub**:</span></span>
   
   ![jobbet pågår för strömning](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-portalen strömning jobb pågår](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a><span data-ttu-id="bd57e-122">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="bd57e-122">Get help</span></span>
<span data-ttu-id="bd57e-123">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="bd57e-123">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="bd57e-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bd57e-124">Next steps</span></span>
* [<span data-ttu-id="bd57e-125">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd57e-125">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="bd57e-126">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="bd57e-126">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="bd57e-127">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="bd57e-127">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="bd57e-128">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="bd57e-128">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="bd57e-129">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="bd57e-129">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

