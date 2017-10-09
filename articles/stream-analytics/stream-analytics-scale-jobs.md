---
title: "aaaScale Stream Analytics-jobb tooincrease genomströmning | Microsoft Docs"
description: "Lär dig hur tooscale Stream Analytics-jobb genom att konfigurera inkommande partitioner, justera hello frågedefinitionen och ange jobbet enheter för strömning."
keywords: "data som strömmas, finjustera strömning databehandling analytics"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 7e857ddb-71dd-4537-b7ab-4524335d7b35
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/22/2017
ms.author: jeffstok
ms.openlocfilehash: 4ba8f6b2f8bfebd52cfa07696b501b42cda21f75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scale-azure-stream-analytics-jobs-tooincrease-stream-data-processing-throughput"></a>Skala Azure Stream Analytics-jobb tooincrease dataströmmen databearbetning genomflöde
Den här artikeln visar hur tootune en Stream Analytics fråga tooincrease genomströmning för Streaming Analytics-jobb. Du lär dig hur tooscale Stream Analytics-jobb genom att konfigurera inkommande partitioner, prestandajustering hello analytics fråga definition och beräkna och ange jobbet *strömningsenheter* (SUs). 

## <a name="what-are-hello-parts-of-a-stream-analytics-job"></a>Vad är hello delar av ett Stream Analytics-jobb?
En Stream Analytics-jobbdefinition innehåller indata, en fråge- och utdata. Indata är där hello jobbet läser hello dataström från. hello frågan är hello används tootransform data Indataströmmen och hello utdata är där hello jobbet skickar hello jobbet resultatet till.  

Ett jobb kräver minst en Indatakällan för strömmande data. hello kan-dataströmmen inkommande datakälla lagras i en Azure händelsehubb eller i Azure blob storage. Mer information finns i [introduktion tooAzure Stream Analytics](stream-analytics-introduction.md) och [komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md).

## <a name="partitions-in-event-hubs-and-azure-storage"></a>Partitioner i händelsehubbar och Azure-lagring
Skalning Stream Analytics-jobbet utnyttjar partitioner i hello indata eller utdata. Partitionering kan du dela upp data i delmängder baserat på en partitionsnyckel. En process som förbrukar hello data (till exempel ett Streaming Analytics-jobb) kan använda och skriva olika partitioner parallellt, vilket ökar genomströmningen. När du arbetar med Streaming Analytics kan dra du nytta av partitionering i händelsehubbar och i Blob storage. 

Mer information om partitioner finns hello följande artiklar:

* [Översikt över Event Hubs funktioner](../event-hubs/event-hubs-features.md#partitions)
* [Data partitionering](https://docs.microsoft.com/azure/architecture/best-practices/data-partitioning#partitioning-azure-blob-storage)


## <a name="streaming-units-sus"></a>Enheter för strömning (SUs)
Strömmande enheter (SUs) representerar hello resurser och datorkraft som krävs i ordning tooexecute Azure Stream Analytics-jobbet. SUs ge ett sätt toodescribe hello relativa händelsebearbetning kapacitet baserat på ett blandat mått av CPU, minne, läser och skriver priser. Varje SU motsvarar tooroughly 1 MB per sekund för genomströmning. 

Om du väljer hur många SUs krävs för ett specifikt jobb beror på hello partition konfiguration för hello indata och hello frågan för hello jobb. Du kan välja upp tooyour kvoten i SUs för ett jobb. Som standard har varje Azure-prenumeration en kvot på upp too50 SUs för alla hello analytics-jobb i en viss region. tooincrease SUs för dina prenumerationer utöver den här kvoten Kontakta [Microsoft-supporten](http://support.microsoft.com). Giltiga värden för SUs per jobb är 1, 3, 6, och upp i steg 6.

## <a name="embarrassingly-parallel-jobs"></a>Embarrassingly parallella jobb
En *embarrassingly parallella* jobbet är hello mest skalbara scenario som vi har i Azure Stream Analytics. Ansluter en partition av hello inkommande tooone instans av hello frågan tooone partition hello utdata. Den här parallellitet har hello följande krav:

1. Om din fråga logik som är beroende av hello samma nyckel som bearbetas av hello samma fråga instans, måste du se till att hello händelser gå toohello samma partition som indata. För händelsehubbar, innebär detta att hello händelsedata måste ha hello **PartitionKey** värdet uppsättningen. Du kan också använda partitionerade avsändare. För blob-lagring innebär detta att hello händelser skickas toohello samma partition mapp. Om din fråga logik inte kräver hello samma nyckel toobe bearbetas av hello samma fråga instans kan du ignorera det här kravet. Ett exempel på den här logiken skulle vara en enkel fråga väljer project filter.  

2. När hello data är placerade på hello inkommande sida, måste du se till att frågan är partitionerad. Detta kräver toouse **Partition By** i alla hello steg. Flera steg tillåts, men de måste vara partitionerad hello samma nyckel. För närvarande hello partitionering nyckeln måste anges för**PartitionId** för hello jobbet toobe fullständigt parallellt.  

3. För närvarande stöder endast händelsehubbar och blob-lagring partitionerade utdata. Event hub utdata, måste du konfigurera hello partition viktiga toobe **PartitionId**. För blob storage-utdata har du inte toodo något.  

4. hello antalet inkommande partitioner måste vara lika med hello antalet partitioner för utdata. BLOB storage-utdata stöds inte för närvarande partitioner. Men det är OK, eftersom den ärver hello partitioneringsschema av hello överordnad fråga. Här följer exempel på partitionen värden som tillåter ett fullständigt parallella jobb:  

   * 8 event hub inkommande partitioner och 8 händelsehubb utdata partitioner
   * 8 event hub inkommande partitioner och blob storage-utdata  
   * 8 blob storage inkommande partitioner och blob storage-utdata  
   * 8 blob storage inkommande partitioner och 8 event hub utdata partitioner  

hello beskrivs följande avsnitt några exempelscenarier som embarrassingly parallella.

### <a name="simple-query"></a>Enkel fråga

* Indata: Event hub med 8 partitioner
* Utdata: Event hub med 8 partitioner

Fråga:

    SELECT TollBoothId
    FROM Input1 Partition By PartitionId
    WHERE TollBoothId > 100

Den här frågan är ett enkelt filter. Vi behöver därför inte tooworry om partitionering hello indata som skickas toohello händelsehubb. Observera att hello-frågan inkluderar **Partition av PartitionId**, så att den uppfyller krav #2 från tidigare. Hello utdata vi måste tooconfigure hello event hub utdata i hello jobbet toohave hello partitionera nyckeluppsättning för**PartitionId**. En senaste kontrollen är toomake att hello antalet inkommande partitioner är lika toohello antalet partitioner för utdata.

### <a name="query-with-a-grouping-key"></a>Fråga med en grupperingsnyckel för

* Indata: Event hub med 8 partitioner
* Utdata: Blob storage

Fråga:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Den här frågan har en gruppering. Därför hello toobe för samma nyckel måste bearbetas av hello samma fråga instans, vilket innebär att händelser måste skickas som toohello händelsehubb på ett sätt som är partitionerad. Men vilken nyckel ska användas? **PartitionId** är ett begrepp som jobbet logik. hello nyckeln faktiskt värnar om är **TollBoothId**, så hello **PartitionKey** värdet av hello händelsedata ska **TollBoothId**. Vi göra detta i hello frågan genom att ange **Partition By** för**PartitionId**. Eftersom hello utdata är blob storage, behöver vi inte tooworry om hur du konfigurerar en partitionsnyckelvärde, enligt kravet #4.

### <a name="multi-step-query-with-a-grouping-key"></a>Flera steg frågan med en grupperingsnyckel för
* Indata: Event hub med 8 partitioner
* Utdata: Händelsen NAV-instans med 8 partitioner

Fråga:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

Den här frågan har en grupperingsnyckel, så hello toobe för samma nyckel måste bearbetas av hello samma fråga-instans. Vi kan använda hello samma strategi som hello föregående exempel. I det här fallet har hello-frågan flera steg. Har varje steg **Partition av PartitionId**? Ja, så hello fråga uppfyller kravet #3. Hello utdata vi måste tooset hello partitionsnyckel för**PartitionId**, enligt tidigare diskussion. Vi kan också se att den har hello samma antal partitioner som hello indata.

## <a name="example-scenarios-that-are-not-embarrassingly-parallel"></a>Exempelscenarier som *inte* embarrassingly parallellt

I föregående avsnitt hello visade vi några embarrassingly parallella scenarier. I det här avsnittet diskuterar vi scenarier som inte uppfyller alla hello krav toobe embarrassingly parallellt. 

### <a name="mismatched-partition-count"></a>Antal matchningar partitioner
* Indata: Event hub med 8 partitioner
* Utdata: Event hub med 32 partitioner

I det här fallet det spelar ingen roll vilken hello-frågan är. Om hello inkommande partitionsantal inte matchar antalet för hello utdata partitioner, inte hello topologi embarrassingly parallellt.

### <a name="not-using-event-hubs-or-blob-storage-as-output"></a>Inte använder händelsehubbar eller blob-lagring som utdata
* Indata: Event hub med 8 partitioner
* Utdata: PowerBI

PowerBI utdata stöder för närvarande inte partitionering. Det här scenariot är därför inte embarrassingly parallellt.

### <a name="multi-step-query-with-different-partition-by-values"></a>Flera steg fråga med olika Partition By-värden
* Indata: Event hub med 8 partitioner
* Utdata: Event hub med 8 partitioner

Fråga:

    WITH Step1 AS (
    SELECT COUNT(*) AS Count, TollBoothId, PartitionId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1 Partition By TollBoothId
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Som du ser hello andra steget använder **TollBoothId** som hello partitionering nyckel. Det här steget är inte hello samma hello första steg, och därför kräver oss toodo en blandad. 

hello Visar föregående exempel några Stream Analytics-jobb som överensstämmer för (inte eller) en embarrassingly parallella topologi. Om de uppfyller har hello risken för maximal skala. För jobb som inte passar något av dessa profiler vägledning skalning kommer att vara tillgänglig i framtida uppdateringar. Använd hello allmänna riktlinjer för tillfället i hello följande avsnitt.

## <a name="calculate-hello-maximum-streaming-units-of-a-job"></a>Beräkna Hej max strömmande enheter för ett jobb
hello totala antalet strömmande enheter som kan användas av ett Stream Analytics-jobb är beroende av hello antalet steg i hello frågan för hello jobbet och hello antalet partitioner för varje steg.

### <a name="steps-in-a-query"></a>Stegen i en fråga
En fråga kan ha en eller flera steg. Varje steg finns en underfråga som definieras av hello **WITH** nyckelord. hello-fråga som är utanför hello **WITH** nyckelordet (enbart en fråga) också räknas som ett steg, till exempel hello **Välj** instruktionen i hello följande fråga:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute,3), TollBoothId

Den här frågan har två steg.

> [!NOTE]
> Den här frågan diskuteras i detalj senare i hello artikeln.
>  

### <a name="partition-a-step"></a>Partitionera ett steg
Partitionering ett steg kräver hello följande villkor:

* hello indatakälla partitioneras. 
* Hej **Välj** hello frågas uttryck måste läsa från en partitionerad Indatakällan.
* hello query hello steg måste ha hello **Partition By** nyckelord.

När en fråga är partitionerad hello inkommande händelser är bearbetade och aggregerade i separat partitionsgrupper och utdata händelser genereras för varje hello grupper. Om du vill att en kombinerad mängd måste du skapa en andra partitionerade steg tooaggregate.

### <a name="calculate-hello-max-streaming-units-for-a-job"></a>Beräkna Hej max strömningsenheter för ett jobb
Alla åtgärder för partitionerade kan tillsammans skala upp toosix strömningsenheter (SUs) för ett Stream Analytics-jobb. vara måste partitionerade tooadd SUs, ett steg. Varje partition får ha sex SUs.

<table border="1">
<tr><th>Fråga</th><th>Max SUs för hello jobbet</th></td>

<tr><td>
<ul>
<li>hello frågan innehåller ett steg.</li>
<li>hello steg inte har partitionerats.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>hello inkommande dataströmmen är partitionerad med 3.</li>
<li>hello frågan innehåller ett steg.</li>
<li>hello steg är partitionerad.</li>
</ul>
</td>
<td>18</td></tr>

<tr><td>
<ul>
<li>hello frågan innehåller två steg.</li>
<li>Ingen av hello stegen är partitionerad.</li>
</ul>
</td>
<td>6</td></tr>

<tr><td>
<ul>
<li>hello inkommande dataströmmen är partitionerad med 3.</li>
<li>hello frågan innehåller två steg. hello inkommande steg är partitionerad och hello andra steget är inte.</li>
<li>Hej <strong>Välj</strong> instruktionen läser från hello partitionerad indata.</li>
</ul>
</td>
<td>24 (18 för partitionerade steg) + 6 partitionerade anvisningar</td></tr>
</table>

### <a name="examples-of-scaling"></a>Exempel på skalning

hello beräknar följande fråga hello många bilar inom en minut tre period gå via en avgift station som har tre tollbooths. Den här frågan kan skalas upp toosix SUs.

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

toouse flera SUs för hello frågan, både hello inkommande dataström och hello frågan vara partitionerade. Eftersom hello data dataströmmen partition anges too3 kan hello följande ändrade frågan skalas upp too18 SUs:

    SELECT COUNT(*) AS Count, TollBoothId
    FROM Input1 Partition By PartitionId
    GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId

När en fråga är partitionerad hello inkommande händelser bearbetas och aggregeras i en separat partitionsgrupper. Utdatahändelserna genereras även för hello grupper. Partitionering kan orsaka vissa oväntade resultat när hello **GROUP BY** fältet är inte hello partitionsnyckel i hello inkommande dataströmmen. Till exempel hello **TollBoothId** fält i hello föregående frågan är inte hello Partitionsnyckeln för **Input1**. hello resultatet är att hello data från vaktkur #1 kan spridas i flera partitioner.

Var och en av hello **Input1** partitionerna bearbetas separat av Stream Analytics. Flera poster antalet hello bil för hello därför samma vaktkur i hello samma rullande fönster kommer att skapas. Det här problemet kan åtgärdas genom att lägga till ett icke-partition steg, som i följande exempel hello om hello inkommande partitionsnyckel inte kan ändras:

    WITH Step1 AS (
        SELECT COUNT(*) AS Count, TollBoothId
        FROM Input1 Partition By PartitionId
        GROUP BY TumblingWindow(minute, 3), TollBoothId, PartitionId
    )

    SELECT SUM(Count) AS Count, TollBoothId
    FROM Step1
    GROUP BY TumblingWindow(minute, 3), TollBoothId

Den här frågan kan vara skalade too24 SUs.

> [!NOTE]
> Om du ansluter till två dataströmmar, kontrollerar du att hello dataströmmar partitioneras av hello partitionsnyckel för hello kolumnen att du använder toocreate hello kopplingar. Kontrollera också att du har hello samma antal partitioner i båda dataströmmar.
> 
> 

## <a name="configure-stream-analytics-streaming-units"></a>Konfigurera Stream Analytics enheter för strömning

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello lista över resurser, hitta hello Stream Analytics-jobbet att tooscale och sedan öppna den.
3. I hello jobb-bladet under **konfigurera**, klickar du på **skala**.

    ![Azure Stream Analytics-jobbet Portalkonfiguration][img.stream.analytics.preview.portal.settings.scale]

4. Använd hello skjutreglaget tooset hello SUs för hello jobbet. Observera att du är begränsad toospecific SU inställningar.


## <a name="monitor-job-performance"></a>Övervaka jobb prestanda
Med hello Azure-portalen kan spåra du hello genomflödet i ett jobb:

![Azure Stream Analytics övervaka jobb][img.stream.analytics.monitor.job]

Beräkna hello förväntades genomflödet för hello arbetsbelastning. Om hello genomströmning är mindre än förväntat, finjustera hello inkommande partition, finjustera hello fråga och lägga till SUs tooyour jobb.


## <a name="visualize-stream-analytics-throughput-at-scale-hello-raspberry-pi-scenario"></a>Visualisera Stream Analytics genomflöde i skala: hello hallon Pi scenario
toohelp du förstår hur Stream Analytics-jobb skala, vi utföra ett experiment baserat på indata från en hallon Pi-enhet. Experimentet Låt oss se hello påverkar genomflöde för flera strömmande enheter och partitioner.

I det här scenariot skickar hello enheten sensor data (klienter) tooan händelsehubb. Streaming Analytics bearbetar hello data och skickar en avisering eller statistik som en utdata tooanother händelsehubb. 

hello klienten skickar sensordata i JSON-format. hello utdata är också i JSON-format. hello data ser ut så här:

    {"devicetime":"2014-12-11T02:24:56.8850110Z","hmdt":42.7,"temp":72.6,"prss":98187.75,"lght":0.38,"dspl":"R-PI Olivier's Office"}

hello följande fråga är används toosend en avisering när en enstaka stängs av:

    SELECT AVG(lght),
     "LightOff" as AlertText
    FROM input TIMESTAMP
    BY devicetime
     WHERE
        lght< 0.05 GROUP BY TumblingWindow(second, 1)

### <a name="measure-throughput"></a>Mäta dataflöde

I den här kontexten är genomströmning hello inkommande data som bearbetas av Stream Analytics i en fast mängd tid. (Vi mätt i 10 minuter.) tooachieve hello bästa bearbetning genomströmning för hello mata in data, både hello dataströmmen indata och hello frågan har partitionerats. Vi inkluderat **COUNT()** i hello frågan toomeasure hur många inkommande händelser har bearbetats. toomake att hello jobbet väntar inte på bara inkommande händelser toocome, varje partition för hello inkommande händelsehubb har förinstallerats på 300 MB som indata.

hello visar följande tabell hello resultat som vi såg när vi har ökat hello antalet strömningsenheter och hello motsvarande partition räknar i händelsehubbar.  

<table border="1">
<tr><th>Inkommande partitioner</th><th>Utdata partitioner</th><th>Enheter för strömning</th><th>Varaktigt dataflöde
</th></td>

<tr><td>12</td>
<td>12</td>
<td>6</td>
<td>4.06 MB/s</td>
</tr>

<tr><td>12</td>
<td>12</td>
<td>12</td>
<td>8.06 MB/s</td>
</tr>

<tr><td>48</td>
<td>48</td>
<td>48</td>
<td>38.32 MB/s</td>
</tr>

<tr><td>192</td>
<td>192</td>
<td>192</td>
<td>172.67 MB/s</td>
</tr>

<tr><td>480</td>
<td>480</td>
<td>480</td>
<td>454.27 MB/s</td>
</tr>

<tr><td>720</td>
<td>720</td>
<td>720</td>
<td>609.69 MB/s</td>
</tr>
</table>

Och hello följande diagram visar en visualisering av hello relation mellan SUs och genomflöde.

![img.stream.Analytics.perfgraph][img.stream.analytics.perfgraph]

## <a name="get-help"></a>Få hjälp
För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Image references-->

[img.stream.analytics.monitor.job]: ./media/stream-analytics-scale-jobs/StreamAnalytics.job.monitor-NewPortal.png
[img.stream.analytics.configure.scale]: ./media/stream-analytics-scale-jobs/StreamAnalytics.configure.scale.png
[img.stream.analytics.perfgraph]: ./media/stream-analytics-scale-jobs/perf.png
[img.stream.analytics.streaming.units.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsStreamingUnitsExample.jpg
[img.stream.analytics.preview.portal.settings.scale]: ./media/stream-analytics-scale-jobs/StreamAnalyticsPreviewPortalJobSettings-NewPortal.png   

<!--Link references-->

[microsoft.support]: http://support.microsoft.com
[azure.management.portal]: http://manage.windowsazure.com
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

