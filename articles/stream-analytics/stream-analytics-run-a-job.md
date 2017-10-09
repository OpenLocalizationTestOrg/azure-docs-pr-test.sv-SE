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
# <a name="how-toorun-a-streaming-job-in-azure-stream-analytics"></a>Hur toorun en streaming job i Azure Stream Analytics
När ett jobb indata, har fråga och utdata alla angetts du kan starta hello Stream Analytics-jobbet.

toostart ditt jobb:

1. I hello Azure klassiska portal från hello jobbet instrumentpanelen, klickar du på **starta** på hello hello sidans nederkant.
   
   ![Starta jobbet knappen](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   I hello Azure-portalen klickar du på **starta** hello överst på sidan jobb.
   
   ![Azure portal startjobb knappen](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Ange en **starta utdata** värdet toodetermine när jobbet startas utdata. hello standardinställningen för jobb som tidigare inte har startats är **jobbet starttid**, vilket innebär att jobbet hello omedelbart startar av bearbetningsdata. Du kan också ange en **anpassad** tid i hello tidigare (för förbrukning av historiska data) eller hello framtida (toodelay bearbetning förrän en framtida tid). För fall när ett jobb har tidigare startas och stoppas, hello alternativet **stoppats senast** finns i ordning tooresume hello jobbet från hello senast utdata och undvika dataförlust.  
   
   ![Starta strömning jobbet tid](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal starta direktuppspelningsjobbet tid](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Bekräfta dina val. hello jobbets status ändras för*Start* och kommer inom kort för att flytta*kör* när hello jobbet har startats. Du kan övervaka hello hello **starta** åtgärd i hello **Meddelandehubben**:
   
   ![jobbet pågår för strömning](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-portalen strömning jobb pågår](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

