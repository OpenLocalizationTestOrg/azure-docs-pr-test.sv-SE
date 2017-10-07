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
# <a name="test-azure-stream-analytics-queries-in-hello-azure-portal"></a>Testa Azure Stream Analytics-frågor i hello Azure-portalen

Du kan testa frågor i hello Azure-portalen utan att behöva toostart eller stoppa ett jobb med Azure Stream Analytics.

## <a name="test-hello-input"></a>Testa hello indata

1. tootest med indata exempeldata, högerklicka på någon av dina inmatningar och välj sedan **ladda upp exempeldata från filen**.

    ![Stream analytics query editor Testa fråga](media/stream-analytics-test-query/stream-analytics-test-query-editor-upload.png)

2. När hello överföringen är klar, klickar du på **Test** tootest den här frågan mot hello exempeldata du har angett.

    ![Stream analytics fråga editor test exempeldata](media/stream-analytics-test-query/stream-analytics-test-query-editor-test.png)

hello utdata från frågan visas i hello webbläsare med länken resultat bör du vill toosave hello testet av utdata för senare användning. Du kan nu enkelt och upprepade gånger ändra din fråga och testa den upprepade gånger toosee hur hello utdata ändras.

![Stream Analytics query editor exempel på utdata](media/stream-analytics-test-query/stream-analytics-test-query-editor-samples-output.png)

Du kan se hello resultat för både utdata separat och växla mellan dem med flera utdata som används i en fråga.

När du är nöjd med hello resultat visas i hello webbläsare, spara frågan, starta jobbet och kan bearbeta händelser utan fel.

## <a name="get-help"></a>Få hjälp

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg

* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
