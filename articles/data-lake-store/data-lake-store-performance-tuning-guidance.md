---
title: aaaAzure Data Lake Store prestanda justera riktlinjer | Microsoft Docs
description: "Riktlinjer för prestandajustering Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: 939faa068c0f81d18d9533956f4d336bc4d0cbe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-azure-data-lake-store-for-performance"></a>Justera Azure Data Lake Store för prestanda

Data Lake Store stöder hög genomströmning för i/o-intensiva analyser och dataförflyttning.  I Azure Data Lake Store är med hjälp av alla tillgängliga genomflödet – hello mängden data som kan läsas eller skrivas per sekund – viktiga tooget hello bästa prestanda.  Detta uppnås genom att utföra så många läsningar och skrivningar parallellt som möjligt.

![Data Lake Store-prestanda](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Azure Data Lake Store kan skala tooprovide hello nödvändiga genomströmning för alla analytics scenario. Som standard innehåller ett Azure Data Lake Store-konto automatiskt tillräckligt med genomflödet toomeet hello behov av en bred kategori användningsfall. Hello fall där kunder stöter på hello Standardgränsen hello i ADLS-konto kan vara konfigurerade tooprovide mer genomströmning genom att kontakta Microsoft support.

## <a name="data-ingestion"></a>Datainhämtning

När du vill föra in data från en källa system tooADLS, är det viktigt tooconsider som hello källa maskinvara, nätverksmaskinvara för källa och network connectivity tooADLS kan vara hello flaskhalsar.  

![Data Lake Store-prestanda](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Det är viktigt tooensure som hello dataflyttning inte påverkas av dessa faktorer.

### <a name="source-hardware"></a>Käll-maskinvara

Om du använder lokala datorer eller virtuella datorer i Azure, bör du noggrant välja hello lämplig maskinvara. Föredrar SSD tooHDDs för källa maskinvara, och Välj maskinvara med snabbare axlar. Använd hello snabbast möjligt för nätverkskort för källa nätverksmaskinvara.  På Azure rekommenderar vi Azure D14 virtuella datorer som har hello korrekt kraftfulla disk- och nätverksmaskinvara.

### <a name="network-connectivity-tooazure-data-lake-store"></a>Network Connectivity tooAzure Data Lake Store

hello nätverksanslutningen mellan datakällan och Azure Data Lake store kan ibland vara hello flaskhalsar. När datakällan är lokalt, Överväg att använda en dedikerad länk med [Azure ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) . Om datakällan finns i Azure, hello prestanda som är bäst när hello data är i hello samma Azure-region som hello Data Lake Store.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Konfigurera Datapåfyllning verktyg för maximal parallellisering

När du har åtgärdat hello källa maskinvara och nätverk anslutning flaskhalsar ovan, är du redo tooconfigure införandet-verktyg. hello följande tabell sammanfattar hello inställningar för flera populära införandet verktyg och djupgående prestandajustering artiklar för dem.  Mer om vilka verktyget toouse för ditt scenario toolearn finns [artikel](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-data-scenarios).

| Verktyget               | Inställningar     | Mer information                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount ConcurrentFileCount |  [Länk](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-powershell#performance-guidance-while-using-powershell)   |
| AdlCopy    | Azure Data Lake Analytics-enheter  |   [Länk](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (mapper)   | [Länk](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Länk](../data-factory/data-factory-copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.block.size -m (mapper)    |   [Länk](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Struktur datauppsättningen

När data lagras i Data Lake Store, hello filstorlek, antal filer och mappstrukturer kan påverka prestanda.  hello beskrivs följande avsnitt bästa praxis inom följande områden.  

### <a name="file-size"></a>Filstorlek

Analytics-motorer, till exempel HDInsight och Azure Data Lake Analytics har vanligtvis en kostnader per fil.  Om du lagrar data så många små filer kan detta påverka prestanda negativt.  

I allmänhet ordna dina data i större storlek filer för bättre prestanda.  Som en tumregel ordna datauppsättningar i filer på 256MB eller större. I vissa fall, till exempel bilder och binära data, är det inte möjligt tooprocess dem parallellt.  I dessa fall bör tookeep enskilda filer under 2GB.

Ibland begränsad data pipelines kontroll över hello rådata som har mycket små filer.  Det rekommenderas toohave en ”matlagning” process som genererar större filer toouse för underordnade program.  

### <a name="organizing-time-series-data-in-folders"></a>Ordna Time Series-data i mappar

Partitionen rensas time series-data kan hjälpa några frågor läsa endast en delmängd av hello data, vilket förbättrar prestanda för Hive och ADLA arbetsbelastningar.    

Dessa pipelines som mata in time series-data, placera ofta sina filer med en mycket strukturerat namngivning av filer och mappar. Nedan visas ett vanligt exempel som visas för data som är strukturerad efter datum:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Observera att hello datetime information visas som mappar och hello filnamn.

Datum och klockslag följer hello vanliga mönstret

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Hello val som du gör med hello mapp- och organisation ska igen, optimeras för hello större filstorlek och ett rimligt antal filer i varje mapp.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>Optimera beräkningsintensiva i/o-jobb på Hadoop och Spark arbetsbelastningar i HDInsight

Jobb indelas i följande tre kategorier hello:

* **CPU-intensiv.**  Dessa jobb har lång beräkning gånger med minimal i/o-tider.  Exempel är machine learning och naturligt språk bearbeta jobb.  
* **Minnesintensiva.**  Dessa jobb använder mycket minne.  Exempel: PageRank och analys i realtid jobb.  
* **I/o-intensiva.**  Dessa jobb tillbringar större delen av tiden gör i/o.  Ett vanligt exempel är en kopieringsjobbet som endast Läs- och skrivåtgärder.  Andra exempel är data förberedelse jobb som läsa stora mängder data, utför omvandlingen vissa data och sedan skriver hello tillbaka toohello datalagret.  

hello följande riktlinjer är endast tillämplig tooI i/O beräkningsintensiva jobb.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Allmänna överväganden för ett HDInsight-kluster

* **HDInsight-versioner.** Använd hello senaste versionen av HDInsight för bästa prestanda.
* **Regioner.** Placera hello Data Lake Store i hello samma region som hello HDInsight-kluster.  

Ett HDInsight-kluster består av två huvudnoderna och vissa arbetsnoder. Varje arbetsnod innehåller ett specifikt antal kärnor och minne som bestäms av hello VM-typen.  När du kör ett jobb är YARN hello resurs förhandlare som allokerar hello tillgängligt minne och kärnor toocreate behållare.  Hello uppgifter behövs toocomplete hello jobbet körs varje behållare.  Behållare köra parallella tooprocess uppgifter snabbt. Därför bättre prestanda genom att köra så många parallella behållare som möjligt.

Det finns tre lager i ett HDInsight-kluster som kan vara ögonen öppna tooincrease hello antal behållare och använda alla tillgängliga genomflöde.  

* **Fysiska lagret**
* **YARN-lager**
* **Arbetsbelastningen lager**

### <a name="physical-layer"></a>Fysiska lagret

**Kör kluster med flera noder och/eller större storlek virtuella datorer.**  Större aktiverar du toorun fler YARN-behållare som hello bilden nedan.

![Data Lake Store-prestanda](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Använda virtuella datorer med mer bandbredd i nätverket.**  hello mängden nätverksbandbredd kan utgöra en flaskhals om det finns mindre bandbredd än Data Lake Store dataflöde.  Olika virtuella datorer har olika storlekar för nätverksbandbredd.  Välj en VM-typ som har hello största möjliga nätverkets bandbredd.

### <a name="yarn-layer"></a>YARN-lager

**Använda mindre YARN-behållare.**  Minska hello storleken för varje YARN behållare toocreate flera behållare med hello samma mängd resurser.

![Data Lake Store-prestanda](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

Beroende på din arbetsbelastning, kommer det alltid att en YARN behållare minimistorleken som krävs. Om du väljer för liten för en behållare körs jobb i minnet är slut problem. Vanligtvis vara YARN behållare mindre än 1GB. Det är vanligt toosee 3GB YARN behållare. Du kanske måste större YARN-behållare för vissa arbetsbelastningar.  

**Öka kärnor per YARN-behållare.**  Öka hello antal kärnor som allokerats tooeach behållare tooincrease hello antalet parallella aktiviteter som körs i varje behållare.  Detta fungerar för program som Spark som kör flera uppgifter per behållare.  För program som Hive som kör en enda tråd i varje behållare, är det bättre toohave fler behållare i stället för flera kärnor per behållare.   

### <a name="workload-layer"></a>Arbetsbelastningen lager

**Använd alla tillgängliga behållare.**  Ange hello antalet aktiviteter toobe lika mycket eller större än hello antalet tillgängliga behållare så att alla resurser används.

![Data Lake Store-prestanda](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Misslyckade aktiviteter är kostsamma.** Om varje aktivitet har en stor mängd data tooprocess, resulterar fel på en aktivitet i en dyr försök igen.  Därför är det bättre toocreate fler aktiviteter, som bearbetar en liten mängd data.

I tillägg toohello allmänna riktlinjer ovan har alla program olika parametrar tillgängliga tootune för det specifika programmet. hello tabellen nedan visas några av hello parametrar och länkar tooget igång med prestandajustering för varje program.

| Arbetsbelastning               | Parametern tooset uppgifter                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [Spark i HDInisight](data-lake-store-performance-tuning-spark.md)       | <ul><li>Antal executors</li><li>Utföraren-minne</li><li>Utföraren kärnor</li></ul> |
| [Hive i HDInsight](data-lake-store-performance-tuning-hive.md)    | <ul><li>hive.tez.container.size</li></ul>         |
| [MapReduce på HDInsight](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>Mapreduce.Map.Memory</li><li>Mapreduce.Job.Maps</li><li>Mapreduce.Reduce.Memory</li><li>Mapreduce.Job.reduces</li></ul> |
| [Storm på HDInsight](data-lake-store-performance-tuning-storm.md)| <ul><li>Antalet arbetsprocesser</li><li>Antalet instanser för kanal utförare</li><li>Antalet instanser av bulten utförare </li><li>Antalet aktiviteter som kanal</li><li>Antal bult uppgifter</li></ul>|

## <a name="see-also"></a>Se även
* [Översikt över Azure Data Lake Store](data-lake-store-overview.md)
* [Kom igång med Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
