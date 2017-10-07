---
title: "aaaOptimize Hive-frågor i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toooptimize dina Hive-frågor för Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d6174c08-06aa-42ac-8e9b-8b8718d9978e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/26/2016
ms.author: jgao
ms.openlocfilehash: d27f8100e1e9f4823040ff9f693e7b78d6192c6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-hive-queries-in-azure-hdinsight"></a>Optimera Hive-frågor i Azure HDInsight

Hadoop-kluster är som standard inte optimerats för prestanda. Den här artikeln beskriver vissa vanligaste Hive prestanda optimeringsmetoder som du kan använda tooyour frågor.

## <a name="scale-out-worker-nodes"></a>Skala ut arbetsnoder

Öka hello antalet arbetarnoder i ett kluster kan utnyttja mer mappers och förminskningsapparater toobe körs parallellt. Det finns två sätt som du kan öka skala ut i HDInsight:

* Du kan ange hello antalet arbetarnoder använder hello Azure-portalen, Azure PowerShell eller plattformsoberoende kommandoradsgränssnittet samtidigt hello etablera.  Mer information finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md). hello följande skärmbild visar hello worker nodkonfiguration på hello Azure-portalen:
  
    ![scaleout_1][image-hdi-optimize-hive-scaleout_1]
* Vid hello körningstiden, kan du även skala ut ett kluster utan att återskapa en:

    ![scaleout_1][image-hdi-optimize-hive-scaleout_2]

Läs mer om hello olika virtuella datorer som stöds av HDInsight [HDInsight priser](https://azure.microsoft.com/pricing/details/hdinsight/).

## <a name="enable-tez"></a>Aktivera Tez

[Apache Tez](http://hortonworks.com/hadoop/tez/) är en alternativ motorn toohello MapReduce Körningsmotor:

![tez_1][image-hdi-optimize-hive-tez_1]

Tez går snabbare eftersom:

* **Köra dirigeras acykliska diagram (DAG) som ett enda jobb i hello-motorn MapReduce**. hello DAG kräver varje uppsättning mappers toobe följt av en uppsättning förminskningsapparater. Detta medför flera MapReduce-jobb toobe roterade för varje Hive-fråga. Tez har inte sådan begränsning och kan bearbeta komplexa DAG som ett jobb, vilket minimerar jobbet startade kostnader.
* **Undviker onödig skrivningar**. På grund av toomultiple jobb som skapas för hello skrivs samma Hive-fråga i motorn MapReduce hello, hello utdata för varje jobb tooHDFS för mellanliggande data. Eftersom Tez minimerar antalet jobb för varje Hive-fråga är kan tooavoid onödiga skrivåtgärder.
* **Minimerar uppstart fördröjningar**. Tez är bättre kan toominimize uppstart fördröjning genom att minska hello antal mappers måste toostart och även förbättra optimering i hela.
* **Återanvänder behållare**. När möjliga Tez kan tooreuse behållare tooensure som fördröjning på grund av minskas toostarting upp behållare.
* **Kontinuerlig optimeringstekniker**. Traditionellt gjordes optimering under kompilering fasen. Men det finns mer information om hello indata som innebär att bättre optimering under körningen. Tez använder kontinuerlig optimeringstekniker som tillåter toooptimize hello planen ytterligare i hello runtime-fasen.

Mer information om dessa koncept finns [Apache TEZ](http://hortonworks.com/hadoop/tez/).

Du kan göra en Hive-fråga Tez aktiverad med hello frågan med hello inställningen nedan:

    set hive.execution.engine=tez;

Linux-baserade HDInsight-kluster har Tez aktiverad som standard.


## <a name="hive-partitioning"></a>Hive partitionering

I/o-åtgärden är hello större flaskhalsar för att köra Hive-frågor. hello prestanda förbättras om hello mängden data som behöver toobe läsa kan minskas. Sök igenom hela Hive-tabeller som standard Hive-frågor. Det här är bra för frågor som tabellsökningar. Men för frågor som behöver bara tooscan en liten mängd data (till exempel frågor med filtrering), detta skapar onödiga omkostnader. Hive partitionering tillåter Hive-frågor tooaccess endast hello nödvändiga mängden data i Hive-tabeller.

Hive partitionering implementeras genom att ordna hello rådata i nya kataloger med varje partition har en egen katalog - där hello definieras av hello användare. hello följande diagram illustrerar partitionering en Hive-tabell som hello kolumnen *år*. En ny katalog skapas för varje år.

![Partitionering][image-hdi-optimize-hive-partitioning_1]

Vissa partitionering att tänka på:

* **Gör inte under partition** -partitionering på kolumner med bara några värden kan orsaka några partitioner. Till exempel bara partitionering på kön skapas två partitioner toobe skapade (han och hondjur), därför bara minska hello svarstid med högst hälften.
* **Gör inte över partition** - på Hej andra extreme, skapa en partition på en kolumn med ett unikt värde (t.ex, användar-ID) medför flera partitioner. Gör mycket stress på hello klustret namenode över partitionen eftersom den har toohandle hello stort antal mappar.
* **Undvika data skeva** -Välj din partitionsnyckel klokt så att alla partitioner har även storlek. Ett exempel partitionering på *tillstånd* kan orsaka hello antalet poster under California toobe nästan 30 x som Vermont på grund av toohello skillnad i populationen.

toocreate en partitionstabell använda hello *partitioneras av* sats:

    CREATE TABLE lineitem_part
        (L_ORDERKEY INT, L_PARTKEY INT, L_SUPPKEY INT,L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE            STRING, L_SHIPINSTRUCT STRING, L_SHIPMODE STRING,
         L_COMMENT STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    STORED AS TEXTFILE;

Du kan antingen skapa partitionering statisk eller dynamisk partitionering när hello partitionerad tabell skapas.

* **Statisk partitionering** innebär att du har redan shardade data i hello lämpliga kataloger och du kan be Hive partitioner manuellt utifrån hello katalogplatsen. hello följande kodavsnitt är ett exempel.
  
        INSERT OVERWRITE TABLE lineitem_part
        PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’)
        SELECT * FROM lineitem 
        WHERE lineitem.L_SHIPDATE = ‘5/23/1996 12:00:00 AM’
  
        ALTER TABLE lineitem_part ADD PARTITION (L_SHIPDATE = ‘5/23/1996 12:00:00 AM’))
        LOCATION ‘wasb://sampledata@ignitedemo.blob.core.windows.net/partitions/5_23_1996/'
* **Dynamisk partitionering** innebär det att du vill Hive toocreate partitioner automatiskt åt dig. Eftersom det redan har skapat hello partitionering tabell från hello mellanlagringstabell finns allt vi behöver toodo infoga data toohello partitionerad tabell:
  
        SET hive.exec.dynamic.partition = true;
        SET hive.exec.dynamic.partition.mode = nonstrict;
        INSERT INTO TABLE lineitem_part
        PARTITION (L_SHIPDATE)
        SELECT L_ORDERKEY as L_ORDERKEY, L_PARTKEY as L_PARTKEY , 
             L_SUPPKEY as L_SUPPKEY, L_LINENUMBER as L_LINENUMBER,
              L_QUANTITY as L_QUANTITY, L_EXTENDEDPRICE as L_EXTENDEDPRICE,
             L_DISCOUNT as L_DISCOUNT, L_TAX as L_TAX, L_RETURNFLAG as           L_RETURNFLAG, L_LINESTATUS as L_LINESTATUS, L_SHIPDATE as           L_SHIPDATE_PS, L_COMMITDATE as L_COMMITDATE, L_RECEIPTDATE as      L_RECEIPTDATE, L_SHIPINSTRUCT as L_SHIPINSTRUCT, L_SHIPMODE as      L_SHIPMODE, L_COMMENT as L_COMMENT, L_SHIPDATE as L_SHIPDATE FROM lineitem;

Mer information finns i [partitionerade tabeller](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL#LanguageManualDDL-PartitionedTables).

## <a name="use-hello-orcfile-format"></a>Använd hello ORCFile format
Hive stöder olika filformat. Exempel:

* **Text**: Detta är hello standardfilformat och fungerar med de flesta scenarier
* **Avro**: fungerar bra för heterogena miljöer
* **ORC/parkettgolv**: bäst prestanda

ORC (optimerad raden kolumner)-formatet är ett mycket effektivt sätt toostore Hive-data. Jämfört med tooother format, ORC har hello följande fördelar:

* stöd för komplexa typer inklusive DateTime och komplicerade och halvstrukturerade typer
* Konfigurera too70% komprimering
* indexerar var 10 000 rader, som tillåter hoppar över rader
* en betydande minskning av körning körning

tooenable ORC format du först skapa en tabell med hello-satsen *lagras som ORC*:

    CREATE TABLE lineitem_orc_part
        (L_ORDERKEY INT, L_PARTKEY INT,L_SUPPKEY INT, L_LINENUMBER INT,
         L_QUANTITY DOUBLE, L_EXTENDEDPRICE DOUBLE, L_DISCOUNT DOUBLE,
         L_TAX DOUBLE, L_RETURNFLAG STRING, L_LINESTATUS STRING,
         L_SHIPDATE_PS STRING, L_COMMITDATE STRING, L_RECEIPTDATE STRING,
         L_SHIPINSTRUCT STRING, L_SHIPMODE STRING, L_COMMENT      STRING)
    PARTITIONED BY(L_SHIPDATE STRING)
    STORED AS ORC;

Sätt datatabell toohello ORC från hello mellanlagringstabell. Exempel:

    INSERT INTO TABLE lineitem_orc
    SELECT L_ORDERKEY as L_ORDERKEY, 
           L_PARTKEY as L_PARTKEY , 
           L_SUPPKEY as L_SUPPKEY,
           L_LINENUMBER as L_LINENUMBER,
            L_QUANTITY as L_QUANTITY, 
           L_EXTENDEDPRICE as L_EXTENDEDPRICE,
           L_DISCOUNT as L_DISCOUNT,
           L_TAX as L_TAX,
           L_RETURNFLAG as L_RETURNFLAG,
           L_LINESTATUS as L_LINESTATUS,
           L_SHIPDATE as L_SHIPDATE,
           L_COMMITDATE as L_COMMITDATE,
           L_RECEIPTDATE as L_RECEIPTDATE, 
           L_SHIPINSTRUCT as L_SHIPINSTRUCT,
           L_SHIPMODE as L_SHIPMODE,
           L_COMMENT as L_COMMENT
    FROM lineitem;

Du kan läsa mer om hello ORC format [här](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+ORC).

## <a name="vectorization"></a>Vectorization

Vectorization gör Hive tooprocess en batch med 1024 rader tillsammans i stället för bearbetning av en rad i taget. Det innebär att enkla åtgärder utförs snabbare eftersom mindre intern kod måste toorun.

tooenable vectorization prefixet Hive-fråga med följande inställning hello:

    set hive.vectorized.execution.enabled = true;

Mer information finns i [Vectorized Frågekörningen](https://cwiki.apache.org/confluence/display/Hive/Vectorized+Query+Execution).

## <a name="other-optimization-methods"></a>Andra optimeringsmetoder för
Det finns flera optimeringsmetoder som du kan tänka på, till exempel:

* **Hive bucketing:** en teknik som möjliggör toocluster eller segment stora mängder data toooptimize frågeprestanda.
* **Anslut optimering:** optimering av registreringsdata Frågekörningen planera tooimprove hello effektiviteten för kopplingar och minska hello behovet av att användaren tips. Mer information finns i [ansluta optimering](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+JoinOptimization#LanguageManualJoinOptimization-JoinOptimization).
* **Öka förminskningsapparater**.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig flera vanliga Hive-fråga optimeringsmetoder. toolearn se fler hello följande artiklar:

* [Använda Apache Hive i HDInsight](hdinsight-use-hive.md)
* [Analysera svarta fördröjning data med hjälp av Hive i HDInsight](hdinsight-analyze-flight-delay-data.md)
* [Analysera Twitter-data med Hive i HDInsight](hdinsight-analyze-twitter-data.md)
* [Analysera sensordata med hello Hive-fråga konsolen på Hadoop i HDInsight](hdinsight-hive-analyze-sensor-data.md)
* [Använda Hive med HDInsight tooanalyze loggar från webbplatser](hdinsight-hive-analyze-website-log.md)

[image-hdi-optimize-hive-scaleout_1]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_1.png
[image-hdi-optimize-hive-scaleout_2]: ./media/hdinsight-hadoop-optimize-hive-query/scaleout_2.png
[image-hdi-optimize-hive-tez_1]: ./media/hdinsight-hadoop-optimize-hive-query/tez_1.png
[image-hdi-optimize-hive-partitioning_1]: ./media/hdinsight-hadoop-optimize-hive-query/partitioning_1.png
