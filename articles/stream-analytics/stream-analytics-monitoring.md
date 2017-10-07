---
title: "aaaUnderstanding Stream Analytics-jobbet övervakning | Microsoft Docs"
description: "Förstå övervakning Stream Analytics-jobbet"
keywords: "Övervakare för frågan"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 36819025c7b2ddbf4b9694522f1b4820407ca5f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-toomonitor-queries"></a>Förstå övervakning av Stream Analytics-jobb och hur toomonitor frågor

## <a name="introduction-hello-monitor-page"></a>: Hello övervakaren introduktionssida
hello Azure-portalen både yta nyckeltal som kan använda toomonitor och felsöka din fråga och jobbet prestanda. toosee de här måtten Bläddra toohello Stream Analytics-jobbet som du är intresserad av mätvärden för och visa hello **övervakning** avsnitt på översiktssidan för hello.  

![Övervaka länk](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

hello fönster visas som visas:

![Övervaka jobb instrumentpanelen](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Mått som är tillgängliga för Stream Analytics
| Mått                 | Definition                               |
| ---------------------- | ---------------------------------------- |
| SU % användning       | hello användning av hello strömning enhet(er) tilldelas tooa jobb från hello skala för hello jobb. Denna indikator når 80% eller ovan är hög sannolikhet att händelsebearbetning kan fördröjas eller stoppats framsteg. |
| Inkommande händelser           | Mängden data som tagits emot av hello Stream Analytics-jobbet, i antal händelser. Detta kan vara används toovalidate att händelser skickas toohello Indatakällan. |
| Utdatahändelserna          | Mängden data som skickas av hello Stream Analytics-jobbet toohello utdata mål, i antal händelser. |
| Out-ordning händelser    | Antalet händelser som mottagits i felaktig ordningsföljd som antingen tagits bort eller ges en justerade timestamp, baserat på hello händelse ordning princip. Detta kan påverkas av hello konfigurationen av hello Out of Tolerance fönstret inställningen. |
| Data Konverteringsfel | Antal data Konverteringsfel drabbar Stream Analytics-jobbet. |
| Körningsfel         | hello Totalt antal fel som inträffar under körningen av ett Stream Analytics-jobb. |
| Inkommande händelser      | Antal händelser anländer sent från hello-källa som har antingen tagits bort eller tidsstämpel har baserat på hello händelse ordning principkonfigurationen hello sen ankomst tolerans fönstret inställning justerats. |
| Funktionen begäranden      | Antal anrop toohello Azure Machine Learning-funktionen (om sådan finns). |
| Funktionsfel begäranden | Antal misslyckade Azure Machine Learning-funktionsanrop (om sådan finns). |
| Funktionshändelser        | Antal händelser skickas toohello Azure Machine Learning-funktionen (om sådan finns). |
| Händelse-byte      | Mängden data som tagits emot av hello Stream Analytics-jobbet, i byte. Detta kan vara används toovalidate att händelser skickas toohello Indatakällan. |


## <a name="customizing-monitoring-in-hello-azure-portal"></a>Anpassa övervakning i hello Azure-portalen
Du kan justera hello typ av diagram, mått som visas, och tidsintervallet i inställningarna för hello redigera diagram. Mer information finns i [hur tooCustomize övervakning](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Frågan övervaka tid diagram](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

