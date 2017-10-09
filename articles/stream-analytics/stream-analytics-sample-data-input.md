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
# <a name="azure-stream-analytics-input-stream-sampling"></a>Azure Stream Analytics indata-stream provtagning

Du kan prova inkommande händelser som kommer från en fil och testa frågor i hello portal utan att behöva toostart eller stoppa ett jobb med hjälp av Azure Stream Analytics.

## <a name="testing-your-query"></a>Testa din fråga

Öppna hello i hello Stream Analytics-jobbet informationsfönstret **frågeredigeraren** bladet genom att klicka på hello Frågenamnet under **frågan**. (I vårt Exempelscenario, eftersom ingen fråga har skapats ännu, klickar du på hello **< >** platshållaren.)

![hello Stream Analytics query editor](media/stream-analytics-sample-data-input/stream-analytics-query-editor.png)

hello omfattande editor bladet för att skapa frågan visas som den var i hello föregående versionen. Nu hello bladet har uppdaterats med en ny vänster fönsterruta som visar hello indata och utdata som används av hello frågan och beskrivning för det här jobbet.

![hello Stream Analytics query editor inmatningar och matar ut listor](media/stream-analytics-sample-data-input/stream-analytics-query-editor-highlight.png)

Dessutom visas är ytterligare indata och utdata, som inte har definierats. De kommer från hello fråga mall som du börjar med. De ändrar eller försvinna även helt, när du redigerar hello frågan. Du kan ignorera dem just nu.

tootest med indata exempeldata, högerklicka på någon av dina inmatningar och välj sedan **ladda upp exempeldata från filen**.

![hello exempeldata för Stream Analytics query editor Överför från fil (kommando)](media/stream-analytics-sample-data-input/stream-analytics-query-editor-upload.png)

När hello överföringen är klar, klickar du på **Test** tootest den här frågan mot hello exempeldata du angav.

![hello Stream Analytics query editor testknappen](media/stream-analytics-sample-data-input/stream-analytics-query-editor-test.png)

Om du vill toosave hello testet av utdata för senare användning, visas hello utdata från frågan i hello webbläsare med en länk toohello nedladdningsresultaten. Du kan nu enkelt och upprepade gånger ändra din fråga och testa den upprepade gånger toosee hur hello utdata ändras.

![Stream Analytics query editor exempel på utdata](media/stream-analytics-sample-data-input/stream-analytics-query-editor-samples-output.png)

En andra utdata har lagts till i hello föregående bild, kan anropas **HighAvgTempOutput**.

Du kan se hello resultaten för varje utdata separat och växla mellan dem när du använder flera utdata i en fråga.

När du är nöjd med resultaten hello du spara frågan, starta jobbet, tillbaka sitta och titta på hello Magiskt tal för Stream Analytics.

## <a name="get-help"></a>Få hjälp

För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)
