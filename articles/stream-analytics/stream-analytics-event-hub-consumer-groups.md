---
title: med event hub mottagare aaaDebug Azure Stream Analytics | Microsoft Docs
description: "Fråga bästa praxis för att bedöma Händelsehubbar konsumentgrupper i Stream Analytics-jobb."
keywords: "Event hub gränsen konsumentgrupp"
services: stream-analytics
documentationcenter: 
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
ms.openlocfilehash: 89821e6273151de43b5e42d907e547c939e24824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debug-azure-stream-analytics-with-event-hub-receivers"></a>Felsöka Azure Stream Analytics med event hub mottagare

Du kan använda Azure Händelsehubbar i Azure Stream Analytics tooingest eller utdata från ett jobb. Det är bra för att använda Händelsehubbar toouse flera konsumentgrupper, tooensure jobbet skalbarhet. En orsak är att hello antalet läsare i hello Stream Analytics-jobbet för specifika indata påverkar hello antalet läsare i en enskild konsument-grupp. hello exakta antal mottagare baseras på interna implementeringsdetaljerna för hello skalbar topologi logik. hello antal mottagare exponeras inte externt. hello antalet läsare kan ändra vid hello jobbets starttid eller under uppgraderingar av jobbet.

> [!NOTE]
> När hello antalet läsare ändras under en uppgradering av jobb, skrivs tillfälligt varningar tooaudit loggar. Stream Analytics-jobb återskapa automatiskt från dessa tillfälliga problem.

## <a name="number-of-readers-per-partition-exceeds-event-hubs-limit-of-five"></a>Antalet läsare per partition överskrider Händelsehubbar gräns på fem

Scenarier i vilka hello överskrider antalet läsare per partition hello Händelsehubbar gräns på fem inkludera hello följande:

* Flera SELECT-satser: Om du använder flera SELECT-satser som finns för**samma** händelsehubb indata, varje SELECT-sats orsakar en ny mottagare toobe skapas.
* UNION: När du använder en UNION, det är möjligt toohave flera indata som refererar toohello **samma** händelsegruppen NAV- och konsumenter.
* SJÄLVKOPPLING: När du använder en SELF JOIN-åtgärd, det är möjligt toorefer toohello **samma** händelsehubb flera gånger.

## <a name="solution"></a>Lösning

hello kan följande säkerhetsmetoder minimera scenarier i vilka hello överskrider antalet läsare per partition hello Händelsehubbar gräns på fem.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>Dela din fråga i flera steg med hjälp av en WITH-satsen

hello WITH-satsen anger en tillfällig namngivna resultatmängd som kan refereras av en FROM-sats i hello-frågan. Du definierar hello WITH-satsen i hello körning omfånget för en enstaka SELECT-instruktion.

Till exempel i stället för den här frågan:

```
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Använd den här frågan:

```
WITH input (
   SELECT * FROM inputEventHub
) as data

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-toodifferent-consumer-groups"></a>Kontrollera att indata binda toodifferent konsumentgrupper

För frågor som är tre eller flera inmatningar anslutna toohello samma Händelsehubbar konsumenten, skapa separata konsumentgrupper. Detta kräver hello skapandet av ytterligare Stream Analytics-indata.


## <a name="get-help"></a>Få hjälp
Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooStream Analytics](stream-analytics-introduction.md)
* [Kom igång med Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Språkreferens för Stream Analytics-fråga](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strömma Analytics management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
