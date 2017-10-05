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
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Hur du kör ett direktuppspelningsjobb i Azure Stream Analytics
När ett jobb som indata, fråga och utdata har alla angetts du kan starta Stream Analytics-jobbet.

Att starta jobbet:

1. I den klassiska Azure-portalen från jobbet-instrumentpanelen klickar du på **starta** längst ned på sidan.
   
   ![Starta jobbet knappen](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   I Azure-portalen klickar du på **starta** överst på sidan jobb.
   
   ![Azure portal startjobb knappen](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Ange en **starta utdata** värde för att bestämma när jobbet startar utdata. Standardinställningen för jobb som tidigare inte har startats är **jobbet starttid**, vilket innebär att starta jobbet direkt databearbetning. Du kan också ange en **anpassad** tid i förflutna (för förbrukning av historiska data) eller framtida (för att fördröja behandlingen tills en framtida tid). I fall när ett jobb har tidigare startas och stoppas, alternativet **stoppats senast** är tillgänglig för att återuppta jobbet från den senaste tid för utdata och undvika dataförlust.  
   
   ![Starta strömning jobbet tid](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portal starta direktuppspelningsjobbet tid](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Bekräfta dina val. Jobbet har statusen ändras till *Start* och kommer snart att flytta till *kör* när jobbet har startats. Du kan övervaka förloppet för den **starta** åtgärden i den **Meddelandehubben**:
   
   ![jobbet pågår för strömning](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Azure-portalen strömning jobb pågår](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

