---
title: "aaa Azure Stream Analytics datadrivna felsökning med hjälp av hello jobbet diagram | Microsoft Docs"
description: "Felsöka Stream Analytics-jobbet med hello jobbet diagram och mått."
keywords: 
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
ms.date: 05/01/2017
ms.author: jeffstok
ms.openlocfilehash: 1af884d485bebb06b034da01a13f7f8240516571
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-driven-debugging-by-using-hello-job-diagram"></a>Datadrivna felsökning med hjälp av hello jobbet diagram

hello jobbet diagram som visar hello **övervakning** bladet i hello Azure-portalen kan hjälpa dig att visualisera dina jobb pipeline. Den visar indata, utdata och frågesteg. Du kan använda hello jobbet diagram tooexamine hello mätvärden för varje steg, toomore Isolera snabbt hello orsaken till ett problem när du felsöker problem.

## <a name="using-hello-job-diagram"></a>Med hjälp av hello jobbet diagram

I hello Azure-portalen när den är i ett Stream Analytics-jobb **stöd + felsökning**väljer **jobbet diagram**:

![Jobbet diagram med - plats](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-1.png)

Markera varje fråga steg toosee hello motsvarande avsnitt i en fråga Redigera fönstret. Ett mått diagram för hello steg visas i nedre fönstret på hello sidan.

![Jobbet diagram med - grundläggande jobb](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-2.png)

toosee hello partitioner i hello Azure Event Hubs indata, Välj **...** En snabbmeny visas. Du kan också se hello inkommande fusion.

![Jobbet diagram med - Utöka partition](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-3.png)

toosee hello mått diagram för endast en partition, Välj hello partition nod. hello mått som visas på hello hello sidans nederkant.

![Jobbet diagram med - flera mått](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-4.png)

toosee hello mått diagram för en fusion väljer hello fusion nod. hello följande diagram visar att inga händelser släpptes eller justeras.

![Jobbet diagram med - rutnätet](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-5.png)

toosee hello information om hello värde och tid, punkt toohello diagram.

![Jobbet diagram med - hovra](./media/stream-analytics-job-diagram-with-metrics/stream-analytics-job-diagram-with-metrics-portal-6.png)

## <a name="troubleshoot-by-using-metrics"></a>Felsöka med mått

Hej **QueryLastProcessedTime** mått som anger när ett specifikt steg tog emot data. Genom att titta på hello topologi arbeta du bakåt från hello utdata processor toosee vilka steg inte tar emot data. Om ett steg inte blir data, går toohello frågesteg innan. Kontrollera om hello föregående frågesteg har ett tidsfönster och om tillräckligt med tid har förflutit för den toooutput data. (Observera då windows är fästa element toohello timme.)
 
Om hello föregående frågesteg har ett indata-processor avsedda användning hello inkommande mått toohelp svar hello följande frågor. De kan hjälpa dig att avgöra om ett jobb hämtar data från dess indatakällor. Granska varje partition om hello frågan är partitionerad.
 
### <a name="how-much-data-is-being-read"></a>Hur mycket data läses?

*   **InputEventsSourcesTotal** är hello antalet enheter läsa. Till exempel hello antal blobbar.
*   **InputEventsTotal** är hello antalet händelser som läsa. Det här måttet är tillgängligt per partition.
*   **InputEventsInBytesTotal** är hello antalet lästa byte.
*   **InputEventsLastArrivalTime** uppdateras med tiden för alla mottagna händelser i kö.
 
### <a name="is-time-moving-forward-if-actual-events-are-read-punctuation-might-not-be-issued"></a>Är tiden går vidare? Om faktiska händelser läses kanske inte skiljetecken utfärdas.

*   **InputEventsLastPunctuationTime** anger när en skiljetecken utfärdades tookeep tid flyttning framåt. Om skiljetecken inte är utfärdat kan dataflöde blockeras.
 
### <a name="are-there-any-errors-in-hello-input"></a>Finns det några fel i hello indata?

*   **InputEventsEventDataNullTotal** antal händelser som har null-data.
*   **InputEventsSerializerErrorsTotal** antal händelser som inte gick att deserialisera korrekt.
*   **InputEventsDegradedTotal** antal händelser som har haft problem än med deserialisering.
 
### <a name="are-events-being-dropped-or-adjusted"></a>Är händelser tas bort eller justeras?

*   **InputEventsEarlyTotal** är hello antalet händelser som har ett Programtidsstämpel innan hello övre gräns.
*   **InputEventsLateTotal** är hello antalet händelser som har ett Programtidsstämpel efter hello övre gräns.
*   **InputEventsDroppedBeforeApplicationStartTimeTotal** är hello antalet händelser tas bort innan hello jobbets starttid.
 
### <a name="are-we-falling-behind-in-reading-data"></a>Vi sjunker vid läsning av data?

*   **InputEventsSourcesBackloggedTotal** får du reda på hur många fler meddelanden behöver toobe läsa för Event Hubs och Azure IoT Hub-indata.


## <a name="get-help"></a>Få hjälp
Mer hjälp, försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooStream Analytics](stream-analytics-introduction.md)
* [Kom igång med Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skala Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Språkreferens för Stream Analytics-fråga](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strömma Analytics management REST API-referens](https://msdn.microsoft.com/library/azure/dn835031.aspx)
