---
title: "alternativ för aaaCompute kontext för R Server på HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om hello olika beräkning kontexten alternativ tillgängliga toousers med R Server på HDInsight"
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
ms.openlocfilehash: b3b0d0cc3caa390797dcff8c73d66cd3ad78bcaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="compute-context-options-for-r-server-on-hdinsight"></a>Compute-kontexten alternativ för R Server på HDInsight

Microsoft R Server på Azure HDInsight styr hur anropen genom att ange hello beräknings-kontexten. Den här artikeln beskrivs hello-alternativ som är tillgängliga toospecify om och hur körningen paralleliserad över kärnor hello kantnod eller HDInsight-kluster.

hello edge nod i ett kluster ger en behändig plats tooconnect toohello klustret och toorun R-skript. Med en kantnod har hello möjlighet att köra hello paralleliserad distribuerade funktioner ScaleR över hello kärnor av hello gränsservern nod. Du kan också köra dem över hello klusternoder hello med hjälp av Scaler's Hadoop kartan minska eller Spark beräkning sammanhang.

## <a name="microsoft-r-server-on-azure-hdinsight"></a>Microsoft R Server på Azure HDInsight
[Microsoft R Server på Azure HDInsight](hdinsight-hadoop-r-server-overview.md) ger hello senaste funktioner för R-baserade analytics. Det kan använda data som lagras i en HDFS-behållare i din [Azure Blob](../storage/common/storage-introduction.md "Azure Blob storage") storage-konto, ett Data Lake store eller hello lokala Linux-filsystem. Eftersom R Server bygger på öppen källkod R gäller hello R-baserade program som du skapar hello 8000 + öppen källkod R-paket. De kan också använda hello-rutiner i [RevoScaleR](https://msdn.microsoft.com/microsoft-r/scaler/scaler), Microsofts big analytics datapaketet som ingår i R Server.  

## <a name="compute-contexts-for-an-edge-node"></a>Beräkna kontexter för en kantnod
I allmänhet körs ett R-skript som körs i R Server på hello kantnod inom hello R-tolken på noden. hello undantag är de steg som anropar en ScaleR funktion. Hej ScaleR anrop köras i en miljö för beräkning som bestäms av hur du ställer in hello ScaleR beräkning kontext.  När du kör din R-skriptet från en kantnod beräkna hello möjliga värden för hello kontext är:

- lokal sekventiella (*'lokala'*)
- lokala parallell (*'localpar'*)
- Minska karta
- Spark

Hej *'lokala'* och *'localpar'* alternativ skiljer sig i hur **rxExec** anropen. De båda köra andra rx funktionsanrop på ett sätt som parallellt över alla tillgängliga kärnor om inget annat anges med hjälp av hello ScaleR **numCoresToUse** alternativ, till exempel `rxOptions(numCoresToUse=6)`. Parallell körning alternativ ger optimala prestanda.

hello följande tabell sammanfattas hello olika compute kontexten alternativ tooset hur anropen:

| Compute-kontexten  | Hur tooset                      | Körningskontext                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Lokal sekventiella | rxSetComputeContext('local')    | Paralleliserad körning över hello kärnor hello nod gränsservern förutom rxExec anrop, som utförs i följd |
| Lokala parallellt   | rxSetComputeContext('localpar') | Paralleliserad körning av hello gränsservern nod över hello kärnor |
| Spark            | RxSpark()                       | Paralleliserad distribuerade körning via Spark över hello noder av hello HDI-kluster |
| Minska karta       | RxHadoopMR()                    | Paralleliserad distribuerade körning via minska karta över hello noder av hello HDI-kluster |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Riktlinjer för att besluta om en beräknings-kontext

Vilket hello tre alternativ du väljer anger paralleliserad körning beror på hello natur analytics arbete, hello storlek och hello platsen för dina data. Det finns ingen enkel formeln som anger vilket compute kontexten toouse. Det finns dock vissa övergripande riktlinjer som hjälper dig att göra rätt val av hello eller åtminstone hjälpa dig att begränsa dina val innan du kör en benchmark. Dessa riktlinjer omfattar:

- hello lokala Linux-filsystemet är snabbare än HDFS.
- Upprepade analys är snabbare hello data är lokala och om den är i XDF.
- Det är bättre toostream små mängder data från en datakälla för text. Om hello mängden data är större, konvertera den tooXDF före analys.
- hello arbetet med att kopiera eller strömmande data hello toohello kantnod för analys blir svårhanterligt för mycket stora mängder data.
- Spark är snabbare än kartan minska för analys i Hadoop.

Dessa principer får erbjuder hello avsnitten vissa allmänna råden för att välja en beräknings-kontext.

### <a name="local"></a>Lokal
* Om hello mängden data tooanalyze är liten och kräver inte upprepade analys, sedan strömma den direkt till hello analys rutinunderhåll med *'lokala'* eller *'localpar'*.
* Om hello mängden data tooanalyze kräver upprepade analys är små och medelstora och sedan kopiera den toohello lokalt filsystem, importera den tooXDF och analysera den via *'lokala'* eller *'localpar'*.

### <a name="hadoop-spark"></a>Hadoop, Spark
* Om hello mängden data tooanalyze är stor kan sedan importera den tooa Spark DataFrame med **RxHiveData** eller **RxParquetData**, eller tooXDF i HDFS (om inte lagring är ett problem), och analysera den med hjälp av hello Spark Compute-kontexten.

### <a name="hadoop-map-reduce"></a>Minska Hadoop-karta
* Använd hello kartan minska beräkning kontexten endast om det uppstår ett oöverstigliga problem med hello Spark beräkning kontexten eftersom det är vanligtvis långsammare.  

## <a name="inline-help-on-rxsetcomputecontext"></a>Infogade hjälp om rxSetComputeContext
Mer information och exempel på ScaleR compute kontexter, se hello infogade hjälp i R i hello rxSetComputeContext metod, till exempel:

    > ?rxSetComputeContext

Du kan också se toohello ”[ScaleR distribuerad datoranvändning guiden](https://msdn.microsoft.com/microsoft-r/scaler-distributed-computing)” som är tillgängliga från hello [R Server MSDN](https://msdn.microsoft.com/library/mt674634.aspx "R Server på MSDN") bibliotek.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig om hello-alternativ som är tillgängliga toospecify om och hur körningen paralleliserad över kärnor hello kantnod eller HDInsight-kluster. toolearn mer information om hur toouse R Server med HDInsight-kluster, se hello följande avsnitt:

* [Översikt över R Server för Hadoop](hdinsight-hadoop-r-server-overview.md)
* [Komma igång med R Server för Hadoop](hdinsight-hadoop-r-server-get-started.md)
* [Lägg till RStudio Server tooHDInsight (om inte läggs till när klustret skapas)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Alternativ för Azure Storage för R Server på HDInsight](hdinsight-hadoop-r-server-storage.md)

