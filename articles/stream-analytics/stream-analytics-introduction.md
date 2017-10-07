---
title: aaaIntroduction tooStream Analytics | Microsoft Docs
description: "Läs mer om Stream Analytics, en hanterad tjänst som hjälper dig att analysera data som skickats från hello Sakernas Internet (IoT) i realtid."
keywords: "analytics som en tjänst, hanterade tjänster, stream bearbetning, streaming analytics vad är stream analytics"
services: stream-analytics
documentationcenter: 
author: jenniehubbard
manager: jhubbard
editor: cgronlun
ms.assetid: 613c9b01-d103-46e0-b0ca-0839fee94ca8
ms.service: stream-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/08/2017
ms.author: jhubbard
ms.openlocfilehash: 6dd7ea1d358bcc94e927a3e699a2771a25104d72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-stream-analytics"></a>Vad är Stream Analytics?

Azure Stream Analytics är en helt hanterad händelsebearbetningsmotor som låter dig ställa in analytiska beräkningar i realtid på streamingdata. hello data kan komma från enheter, sensorer, webbplatser, sociala media feeds, program, infrastruktursystem och mer. 

## <a name="what-can-i-do-with-stream-analytics"></a>Vad kan jag göra med Stream Analytics?

Använda Stream Analytics tooexamine stora mängder data som flödar från enheter eller processer, extrahera information från hello dataström och leta efter mönster, trender och relationer. Baserat på vad som finns i hello data kan utföra du sedan uppgifter för programmet. Du kan till exempel generera aviseringar, startar automation arbetsflöden, feed information tooa reporting verktyg som till exempel Power BI eller lagra data för senare undersökning. 

Exempel:

* Anpassade börshandelanalys i realtid och aviseringar som erbjuds av företag för finansiella tjänster.
* Upptäck bedrägerier i realtid baserat på transaktionsdata. 
* Tjänster för data- och identitetsskydd.
* Analys av data som genereras av sensorer och motstånd som är inbäddade i fysiska objekt (Sakernas internet eller IoT).
* Webbklickanalys.
* Kundrealtionshanteringsprogram (CRM), till exempel varningar när kundupplevelsen inom ett tidsintervall har försämrats.

## <a name="how-does-stream-analytics-work"></a>Hur fungerar Stream Analytics?

Det här diagrammet visar hello Stream Analytics pipeline, visar hur data inhämtas, analyseras och skickas sedan till presentation eller åtgärd. 

![Arbetsflöde för Stream Analytics](./media/stream-analytics-introduction/stream_analytics_intro_pipeline.png)

Stream Analytics börjar med en källa för dataöverföring. kan inhämtas hello data till Azure från en enhet med hjälp av en Azure-händelsehubb eller IoT-hubb. hello data kan också hämtas från ett dataarkiv som Azure Blob Storage. 

tooexamine hello stream, skapa ett Stream Analytics *jobbet* som anger var hello data kommer från. hello jobb anger också en *omvandling*&mdash;hur toolook av data, mönster eller relationer. För den här uppgifter stöder Stream Analytics ett SQL-liknande frågespråk som låter dig filtrera, sortera, sammanställa och koppla strömmande data över en tidsperiod.

Slutligen anger hello jobb en toosend hello omvandlas utdata till. På så sätt kan du styra vilka toodo i svaret toohello information som du har analyserats. I svaret tooanalysis kan du t.ex.:

* Skicka ett kommando toochange inställningar för en enhet. 
* Skicka data tooa kö som övervakas av en process som vidtar åtgärder baserat på den hittar. 
* Skicka data tooa Power BI-instrumentpanel för rapportering.
* Skicka data toostorage som Data Lake Store, SQL Server-databas eller Azure Blob eller Table storage.

Du kan övervaka det hela och justera hur många händelser som bearbetas per sekund. Du kan också ha jobb som producerar diagnostiska loggar för felsökning.

## <a name="key-capabilities-and-benefits"></a>Viktiga funktioner och fördelar

Stream Analytics är utformad toobe enkelt toouse, flexibel och skalbar tooany jobbet storlek och ekonomisk.

### <a name="connectivity-toomany-inputs-and-outputs"></a>Anslutningen toomany indata och utdata

Stream Analytics ansluter direkt för[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) och [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) för inhämtning av dataströmmar och hello [Azure Blob storage-tjänst](https://docs.microsoft.com/azure/storage/storage-introduction#blob-storage-accounts) tooingest historiska data. Om du hämtar data från händelsehubbar kan du kombinera Stream Analytics med andra datakällor och bearbetningsmotorer.

Jobbindata kan även inkludera referensdata (statiska eller långsamt föränderliga data). Du kan delta i strömmande data toothis referens data tooperform sökning operations hello samma sätt som med databasfrågor.

Dirigera Stream Analytics-jobbutdata i många riktningar. Du kan skriva toostorage, till exempel Azure Storage-blobbar eller tabeller, Azure SQL DB, Azure Data Lake lagrar eller Azure Cosmos DB. Därifrån kan hello data gå för batchanalyser via Azure HDInsight. Du kan skicka hello utdata tooanother tjänsten för användning av en annan process, till exempel händelsehubbar, Azure Service Bus-ämnen eller köer. Du kan skicka hello utdata tooPower BI för visualisering.

### <a name="ease-of-use"></a>Användarvänlighet

toodefine omvandlingar du använder en enkel, deklarativ [Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx) som hjälper dig att skapa avancerade analyser med ingen programmering. hello-frågespråket tar strömmande data som indata. Därefter kan du filtrera och sortera data för hello Aggregera värden, utföra beräkningar, ansluta till data (inom en stream eller tooreference data) och använda geospatiala funktioner. Du kan redigera frågor i hello-portalen med IntelliSense och syntaxkontroll av och du kan testa frågor med exempeldata som du kan extrahera från hello direktsänd dataström.

### <a name="extensible-query-language"></a>Omfattande frågespråk

Du kan utöka hello funktionerna i hello frågespråket genom att definiera och anropar ytterligare funktioner. Du kan definiera funktionsanrop i hello Azure Machine Learning service tootake nytta av Azure Machine Learning-lösningar. Du kan också integrera JavaScript användardefinierade funktioner (UDF) i ordning tooperform komplexa beräkningar som en del en Stream Analytics-fråga.

### <a name="scalability"></a>Skalbarhet

Stream Analytics kan hantera upp too1 GB för inkommande data per sekund. Integrering med [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) och [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) tillåter jobb tooingest miljontals händelser per sekund från anslutna enheter, klickströmsdata och loggfiler, tooname några. Funktionen hello partition i händelsehubbar, du kan också partitionera beräkningar i logiska steg, var och en med ytterligare hello möjlighet toobe partitionerad tooincrease skalbarhet.

### <a name="low-cost"></a>Låg kostnad

Som en molntjänst är Stream Analytics optimerad toolet du komma igång med låg kostnad. Du betalar allteftersom baserat på streaming unit användnings- och hello mängden data som bearbetas av hello system. Användningen beräknas baserat på hello mängden bearbetade händelser och hello mycket datorkraft som tilldelats i hello klustret toohandle Stream Analytics-jobb.

### <a name="reliability-quick-recovery-and-repeatability"></a>Tillförlitlighet och snabb återställning och repeterbarhet

Som en hanterad tjänst i molnet hello Stream Analytics förhindra dataförlust och ger kontinuitet för företag. Om fel uppstår tillhandahåller hello tjänsten inbyggda återställningsfunktioner. Med hello förmåga toointernally upprätthålla tillstånd, tillhandahåller hello tjänsten repeterbara resultat som det är möjligt tooarchive händelser och tillämpar bearbetning i hello framtiden kan alltid komma hello samma resultat. Detta gör att du toogo bakåt i tiden och undersöka beräkningar i samband med rotorsaksanalyser, konsekvensanalyser och så vidare.

## <a name="next-steps"></a>Nästa steg

* Kom igång genom att [experimentera med indata- och frågor från IoT-enheter](stream-analytics-get-started-with-azure-stream-analytics-to-process-data-from-iot-devices.md).
* Skapa en [slutpunkt till slutpunkt Stream Analytics lösning](stream-analytics-real-time-fraud-detection.md) som undersöker telefon metadata toolook för bedrägliga anrop.
* Lär dig mer om hello SQL-liknande frågespråk för Stream Analytics och unika begrepp som [fönstrets funktioner](stream-analytics-window-functions.md).
* Lär dig hur för[skala Stream Analytics-jobb](stream-analytics-scale-jobs.md). 
* Lär dig hur för[integrera Stream Analytics och Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).
* Hitta svar tooyour Stream Analytics-frågor i hello [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

