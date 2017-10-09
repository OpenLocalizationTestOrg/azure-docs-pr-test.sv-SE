---
title: "aaaIntroduction tooStream Analytics fönstrets funktioner | Microsoft Docs"
description: "Läs mer om hello tre fönstrets funktioner i Stream Analytics (rullande hopping, glidande)."
keywords: "rullande fönster glidande fönstret hopping fönster"
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 0d8d8717-5d23-43f0-b475-af078ab4627d
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 7fc36eb9afb2b28e2791d925d26923145eb38074
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toostream-analytics-window-functions"></a>Introduktion tooStream Analytics fönstrets funktioner
I många realtid strömning scenarier, är det nödvändigt tooperform endast åtgärder på hello data i den temporala windows. Inbyggt stöd för fönsterhantering funktioner är en nyckelfunktion i Azure Stream Analytics som flyttar hello nålen på utvecklarproduktivitet i authoring komplexa dataströmmen bearbetar jobb. Stream Analytics gör det möjligt för utvecklare toouse [ **rullande**](https://msdn.microsoft.com/library/dn835055.aspx), [ **Hopping** ](https://msdn.microsoft.com/library/dn835041.aspx) och [ **glidande** ](https://msdn.microsoft.com/library/dn835051.aspx) windows tooperform temporala åtgärder på strömmande data. Det är värt att nämna som alla [fönstret](https://msdn.microsoft.com/library/dn835019.aspx) operations utdata resultat på hello **end** av hello-fönstret. hello utdata från hello fönstret blir enskild händelse baserat på hello mängdfunktion används. hello händelse har hello tidsstämpeln för hello slutet av hello fönstret och alla Windows-funktioner har definierats med en fast längd. Slutligen är det viktigt toonote som alla Windows-funktioner som ska användas i en [ **GROUP BY** ](https://msdn.microsoft.com/library/dn835023.aspx) satsen.

![Stream Analytics fönstret fungerar begrepp](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Rullande fönster
Rullande fönster funktioner är används toosegment en dataström i distinkta tid segment och utför en funktion mot dem, till exempel hello exemplet nedan. hello viktiga skillnaderna i en rullande fönster är att de upprepar, inte överlappar och en händelse kan inte tillhöra toomore än en rullande fönster.

![Stream Analytics-fönstrets funktioner rullande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Hoppande fönster
Hoppande fönster funktioner hopp framåt genom en bestämd tid. Det kan vara enkelt toothink av dem som rullande fönster som kan överlappa så händelser kan höra toomore än Hopping fönstret resultatuppsättningar. toomake ett Hopping fönster hello samma som en rullande fönster något att ange hello hopp storlek toobe hello samma som hello fönsterstorlek. 

![Stream Analytics fönstret fungerar Hoppande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Hoppande fönster
Glidande fönstrets funktioner, till skillnad från rullande eller Hopping windows, skapa ett utgående **endast** när en händelse inträffar. Alla fönster har minst en händelse och hello fönstret flyttar kontinuerligt fram en € (epsilon). Händelser kan höra toomore än ett glidande fönster som Hopping Windows.

![Stream Analytics-fönstrets funktioner glidande introduktion](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="getting-help-with-window-functions"></a>Få hjälp med att fönstrets funktioner
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

