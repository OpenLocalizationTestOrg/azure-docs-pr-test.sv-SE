---
title: "Compute-kontexten alternativ för R Server på HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om olika beräkning kontexten alternativen för användare med R Server på HDInsight"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0deb0b1c-4094-459b-94fc-ec9b774c1f8a
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: 47f4441612be4f363ba82cc22b09786a6f3bfdc3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Compute-kontexten alternativ för R Server på HDInsight

Microsoft R Server på Azure HDInsight styr hur anropen genom att ange beräknings-kontexten. Den här artikeln beskrivs de alternativ som är tillgängliga för att ange om och hur körningen paralleliserad över kärnor kantnod eller HDInsight-kluster.

Edge-nod i ett kluster ger en lämplig plats att ansluta till klustret och köra R-skript. Med en kantnod har möjlighet att köra parallelized distribuerade funktioner ScaleR över kärnor på noden gränsservern. Du kan också köra dem mellan noder i klustret med hjälp av Scaler's Hadoop kartan minska eller compute kontexter för Spark.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Microsoft R Server på Azure HDInsight
[Microsoft R Server på Azure HDInsight](hdinsight-hadoop-r-server-overview.md) innehåller de senaste funktionerna för analys av R-baserade. Det kan använda data som lagras i en HDFS-behållare i din [Azure Blob](../storage/common/storage-introduction.md "Azure Blob storage") storage-konto, ett Data Lake store eller lokala Linux-filsystem. Eftersom R Server bygger på öppen källkod R gäller R-baserade program som du skapar öppen källkod R-paket 8000 +. De kan också använda rutiner i [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler), Microsofts big analytics datapaketet som ingår i R Server.  

## <a name="compute-contexts-for-an-edge-node"></a>Beräkna kontexter för en kantnod
I allmänhet körs ett R-skript som körs i R Server på kantnoden inom R-tolken på noden. Undantagen är de steg som anropar en ScaleR funktion. ScaleR anropen köras i en miljö för beräkning som bestäms av hur du ställer in ScaleR beräknings-kontexten.  När du kör din R-skriptet från en kantnod är beräknings-kontexten möjliga värden:

- lokal sekventiella (*'lokala'*)
- lokala parallell (*'localpar'*)
- Minska karta
- Spark

Den *'lokala'* och *'localpar'* alternativ skiljer sig i hur **rxExec** anropen. De båda köra andra rx funktionsanrop på ett sätt som parallellt över alla tillgängliga kärnor om inget annat anges med hjälp av ScaleR **numCoresToUse** alternativ, till exempel `rxOptions(numCoresToUse=6)`. Parallell körning alternativ ger optimala prestanda.

I följande tabell sammanfattas de olika beräkning kontext alternativen för att ange hur anropen:

| Compute-kontexten  | Hur du ställer in                      | Körningskontext                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Lokal sekventiella | rxSetComputeContext('local')    | Paralleliserad körning över kärnor av noden gränsservern förutom rxExec anrop, som utförs i följd |
| Lokala parallellt   | rxSetComputeContext('localpar') | Paralleliserad körning av noden gränsservern över kärnor |
| Spark            | RxSpark()                       | Paralleliserad distribuerade körning via Spark mellan noderna av HDI-klustret |
| Minska karta       | RxHadoopMR()                    | Paralleliserad distribuerade utförs via minska karta över noderna i HDI-klustret |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Riktlinjer för att besluta om en beräknings-kontext

Som av de tre alternativen du väljer som tillhandahåller paralleliserad körning beror på vilka slags analytics arbetet, storlek och plats för dina data. Det finns ingen enkel formeln som anger vilket compute kontext för att använda. Det finns dock vissa övergripande riktlinjer som hjälper dig att fatta rätt beslut eller åtminstone hjälpa dig att begränsa dina val innan du kör en prestandamått. Dessa riktlinjer omfattar:

- Lokal Linux-filsystemet är snabbare än HDFS.
- Upprepade analys är snabbare om data är lokala och om den är i XDF.
- Är det bättre att strömma små mängder data från en datakälla för text. Om mängden data som är större, konvertera du det till XDF innan analys.
- Arbetet med att kopiera eller strömmande data till kantnoden för analys blir svårhanterligt för mycket stora mängder data.
- Spark är snabbare än kartan minska för analys i Hadoop.

I följande avsnitt erbjuder ges dessa principer vissa allmänna råden för att välja en beräknings-kontext.

### <a name="local"></a>Lokal
* Om mängden data att analysera är liten och kräver inte upprepade analys, sedan strömmas direkt i den analysis rutinunderhåll med hjälp av *'lokala'* eller *'localpar'*.
* Om mängden data att analysera är liten eller medelstor och kräver upprepade analys, sedan kopiera den till det lokala filsystemet, importera den till XDF och analysera den via *'lokala'* eller *'localpar'*.

### <a name="hadoop-spark"></a>Hadoop, Spark
* Om mängden data att analysera är stor kan sedan importera det till ett Spark-DataFrame med **RxHiveData** eller **RxParquetData**, eller XDF i HDFS (om inte lagring är ett problem), och analysera den med hjälp av Spark-beräkning kontexten.

### <a name="hadoop-map-reduce"></a>Minska Hadoop-karta
* Använda context kartan minska beräkning endast om det uppstår ett oöverstigliga problem med Spark beräkning kontexten eftersom det är vanligtvis långsammare.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Infogade hjälp om rxSetComputeContext
Mer information och exempel på ScaleR beräkning kontexter finns infogat hjälp i R om metoden rxSetComputeContext, till exempel:

    > ?rxSetComputeContext

Du kan också gå till den ”[ScaleR distribuerad datoranvändning guiden](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing)” som är tillgängliga från den [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server på MSDN") bibliotek.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig om de alternativ som är tillgängliga för att ange om och hur körningen paralleliserad över kärnor kantnod eller HDInsight-kluster. Mer information om hur du använder R Server med HDInsight-kluster finns i följande avsnitt:

* [Översikt över R Server för Hadoop](hdinsight-hadoop-r-server-overview.md)
* [Komma igång med R Server för Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Lägg till RStudio Server HDInsight (om inte läggas till när klustret skapas)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Alternativ för Azure Storage för R Server på HDInsight](hdinsight-hadoop-r-server-storage.md)

