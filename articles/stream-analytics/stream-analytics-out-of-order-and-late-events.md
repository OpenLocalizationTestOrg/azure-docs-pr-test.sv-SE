---
title: "aaaHandling händelse ordning och lateness med Azure Stream Analytics | Microsoft Docs"
description: "Läs mer om hur Stream Analytics fungerar med out-ordning eller försenade händelser i dataströmmar."
keywords: "i ordning, Sen, händelser"
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
ms.openlocfilehash: 87c028662fbafbf4f72f57f215d017f65bb649f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Azure Stream Analytics ordning händelsehantering

Varje händelse registreras i en temporal dataström för händelser med hello tid att hello-händelsen har tagits emot. Vissa villkor kan orsaka händelseströmmar toooccasionally tar emot vissa händelser i en annan ordning än som de skickades. En enkel TCP skickar eller även en förskjutning av klockan mellan hello skickar enheten och ta emot händelsehubb hello kan leda till att den här toooccur. Händelser som ”skiljetecken” också läggs tooreceived händelseströmmar tooadvance hello tid händelse mottagna hello saknas. Dessa krävs scenarier som ”Avisera mig när inga inloggningar inträffar för 3 minuter”.

Antingen är inkommande dataströmmar som inte är i ordning:
* Sorterad (och därför **fördröjd**).
* Justeras hello filsystem, enligt tooa användardefinierade princip.


## <a name="lateness-tolerance"></a>Lateness tolerans
Stream Analytics kan tolerera dessa typer av scenarier. Stream Analytics har hantering för ”out ordning” och ”sent” händelser. Den hanterar dessa händelser i hello följande sätt:

* Händelser som anländer utanför ordning men hello Ange tolerans är **ordnas om av tidsstämpel**.
* Händelser som kommer senare än tolerans är **släpptes eller justeras**.
    * **Justeras**: justerade tooappear toohave som anlänt på hello senaste acceptabel tid.
    * **Bort**: ignoreras.

![Stream Analytics händelsehantering](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

## <a name="reduce-hello-number-of-out-of-order-events"></a>Minska hello antalet händelser som out-ordning

Eftersom Stream Analytics gäller en temporal omvandling när den bearbetar inkommande händelser (till exempel för fönsteraggregeringar eller temporala kopplingar), sorterar Stream Analytics inkommande händelser efter tidsstämpel ordning.

När hello ”tidsstämpel av” nyckelord är **inte** används, hello Azure Event Hubs sätta händelsetid används som standard. Händelsehubbar garanterar monotonicity av hello tidsstämpeln på varje partition för hello händelsehubb. Det garanterar även sammanfogas händelser från alla partitioner i tidsstämpel ordning. Dessa två Händelsehubbar garanterar att inga händelser för out-ordning.

Ibland är det viktigt att du toouse hello avsändarens tidsstämpel. I så fall väljs en tidsstämpel från hello händelsenyttolast med hjälp av ”tidsstämpel av”. En eller flera källor för händelsen misorder kan införas i följande scenarier:

* Händelsen producenter har jämföra. Detta är vanligt när producenter kommer från olika datorer, så de har olika klockor.
* Det uppstår en fördröjning för nätverk från hello källan för hello händelser toohello mål händelsehubb.
* Jämföra finns mellan event hub partitioner. Stream Analytics sorterar först händelser från alla event hub partitioner genom att sätta händelsetid. Sedan undersöks hello dataströmmen för misordered händelser.

På fliken Konfiguration hello visas hello följande standardinställningar:

![Stream Analytics out ordning hantering](media/stream-analytics-event-handling/stream-analytics-out-of-order-handling.png)

Om du använder 0 sekunder som hello out ordning tolerans fönster, du garanterar att alla händelser som är i ordning alla hello tid. Angivna hello tre källor misordered händelser, är det osannolikt att detta är sant. 

Du kan ange ett nollskiljd out ordning tolerans fönster tooallow Stream Analytics toocorrect misorder en händelse. Stream Analytics buffertar händelser in toothat fönster och sorterar om dem med hjälp av hello tidsstämpel som du har valt. Den gäller sedan hello temporal omvandling. Du kan börja med ett fönster för 3 sekunder och finjustera hello värdet tooreduce hello antalet händelser som är tiden justeras. 

En sidoeffekt av hello buffring är att hello utdata är **skjutas upp med hello samma tid**. Du kan finjustera hello värdet tooreduce hello antalet out ordning händelser och håll nere hello jobbet svarstid.

## <a name="get-help"></a>Få hjälp
Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooStream Analytics](stream-analytics-introduction.md)
* [Kom igång med Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Språkreferens för Stream Analytics-fråga](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strömma Analytics management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)