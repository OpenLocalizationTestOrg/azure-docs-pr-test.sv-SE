---
title: "aaaWhat är HDInsight, hello Hadoop-teknikstacken & kluster? - Azure | Microsoft Docs"
description: "En introduktion tooHDInsight och hello Hadoop teknikstacken och komponenter, inklusive Spark, Kafka, Hive, HBase för stordataanalys."
keywords: "Azure hadoop, hadoop azure, introduktion av hadoop, introduktion av hadoop, hadoop-teknikstacken, introduktion toohadoop, introduktion toohadoop, vad är ett hadoop-kluster, vilka är hadoop-kluster är det hadoop som används för"
services: hdinsight
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: e56a396a-1b39-43e0-b543-f2dee5b8dd3a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: cgronlun
ms.openlocfilehash: 0aed3d1a6cf3c0cdc3b036cfa4865a2e0b58e2c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-hdinsight-hello-hadoop-technology-stack-and-hadoop-clusters"></a>Introduktion tooAzure HDInsight, hello Hadoop-teknikstacken och Hadoop-kluster
 Den här artikeln innehåller en introduktion tooAzure HDInsight, en molnet fördelning av hello Hadoop-teknikstacken. Den omfattar också en förklaring av vad Hadoop-kluster är och när du ska använda dem. 

## <a name="what-is-hdinsight-and-hello-hadoop-technology-stack"></a>Vad är HDInsight och hello Hadoop-teknikstacken? 
Azure HDInsight är en cloud fördelning av hello Hadoop-komponenter från hello [Hortonworks Data Platform (HDP)](https://hortonworks.com/products/data-center/hdp/). [Apache Hadoop](http://hadoop.apache.org/) var hello ursprungliga öppen källkod ramverk för distribuerad bearbetning och analys av stora datamängder på datorkluster. 


Hej Hadoop-teknikstacken innehåller relaterade program och verktyg, inklusive Apache Hive, HBase, Spark, Kafka och många andra. toosee tillgängliga teknik stack komponenterna i Hadoop i HDInsight, se [komponenter och versioner som är tillgängliga med HDInsight][component-versioning]. tooread mer information om Hadoop i HDInsight, se hello [Azure-funktioner för HDInsight sidan](https://azure.microsoft.com/services/hdinsight/).

## <a name="what-is-a-hadoop-cluster-and-when-do-you-use-it"></a>Vad är ett Hadoop-kluster och när använder du dem?
*Hadoop* är också en typ av kluster som har följande:

* YARN för jobbschemaläggning och resurshantering
* MapReduce för parallellbearbetning
* Hej Hadoop distributed file system (HDFS)
  
Hadoop-kluster används oftast för batchbearbetning av lagrade data. Andra typer av kluster i HDInsight har ytterligare funktioner: Spark har blivit allt mer populärt tack vare dessa snabbare bearbetning i minnet. Se [Klustertyper r i HDInsight](#overview) för mer information.

## <a name="what-is-big-data"></a>Vad är stordata?
Stordata kan avse en stor samling digital information som:

* Sensordata från industriell utrustning
* Kundaktivitet som samlas in från en webbplats
* Ett Twitter-nyhetsflöde

Stordata samlas in i ständigt växande volymer, med allt högre hastighet och i ett växande antal olika format. Det kan vara historiska (betydelse lagrade) eller realtid (betydelse strömmas från hello källa). 

## <a name="overview"></a>Klustertyper i HDInsight
HDInsight omfattar specifika klustertyper och anpassningsmöjligheter för klustret, till exempel att lägga till komponenter, verktyg och språk.

### <a name="spark-kafka-interactive-hive-hbase-customized-and-other-cluster-types"></a>HBase, Spark, Kafka, Interactive Hive, Storm, anpassade och andra klustertyper
HDInsight erbjuder hello följande klustertyper:

* **[Apache Hadoop](https://wiki.apache.org/hadoop)**: använder [HDFS](#hdfs), [YARN](#yarn) resurshantering och en enkel [MapReduce](#mapreduce) programmering modellen tooprocess och analysera batch parallellt.
* **[Apache Spark](http://spark.apache.org/)**: ett ramverk för parallellbearbetning som stöder minnesintern bearbetning tooboost hello prestanda för program för stordataanalys, Väck works för SQL, strömning av data och maskininlärning. Se [Vad är Apache Spark i HDInsight?](hdinsight-apache-spark-overview.md)
* **[Apache HBase](http://hbase.apache.org/)**: En NoSQL-databas som bygger på Hadoop och ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och delstrukturerade data, potentiellt miljarder rader gånger miljoner kolumner. Se [Vad är HBase på HDInsight?](hdinsight-hbase-overview.md)
* **[Microsoft R Server](https://msdn.microsoft.com/microsoft-r/rserver)**: En server som fungerar som värd och hanterar parallella, distribuerade R-processer. Det ger dataanalytiker, statistiker och R-programmerare på begäran åtkomst tooscalable, distribuerade metoder för analyser på HDInsight. Se [Översikt över R Server på HDInsight](hdinsight-hadoop-r-server-overview.md).
* **[Apache Storm](https://storm.incubator.apache.org/)**: Ett distribuerat system för beräkningar i realtid för snabb bearbetning av stora dataströmmar. Storm finns som ett hanterat kluster i HDInsight. Se [Analysera sensordata i realtid med Storm och Hadoop](hdinsight-storm-sensor-data-analysis.md).
* **[Apache Interactive Hive-förhandsgranskning (även känt som: Live Long and Process)](https://cwiki.apache.org/confluence/display/Hive/LLAP)**: Minnesintern cachelagring för interaktiva och snabba Hive-frågor. Se [Använda Interactive Hive in HDInsight](hdinsight-hadoop-use-interactive-hive.md).
* **[Apache Kafka](https://kafka.apache.org/)**: En öppen källkodsplattform som används för att skapa strömmande datapipelines och program. Kafka ger också meddelandekö funktioner som gör att du toopublish och prenumerera toodata dataströmmar. Se [introduktion tooApache Kafka på HDInsight](hdinsight-apache-kafka-introduction.md).

Du kan också konfigurera kluster med hello följande metoder:
* **[Domänanslutna kluster preview](hdinsight-domain-joined-introduction.md)**: ett kluster ansluten tooan Active Directory-domän så att du kan styra åtkomsten och tillhandahålla styrning för data.
* **[Anpassade kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md)**: Kluster med skript som körs under etableringen och som installerar ytterligare komponenter.

### <a name="example-cluster-customization-scripts"></a>Exempelskript för klusteranpassning
Skriptåtgärder är Bash-skript på Linux som körs under klusteretablering och som kan vara används tooinstall ytterligare komponenter på hello klustret. 

hello tillhandahålls följande exempelskript av HDInsight-teamet hello:

* **[Hue](hdinsight-hadoop-hue-linux.md)**: en uppsättning toointeract för web-program som används med ett kluster. Endast för Linux-kluster.
* **[Giraph](hdinsight-hadoop-giraph-install-linux.md)**: bearbetning toomodel relationer mellan saker eller personer.
* **[Solr](hdinsight-hadoop-solr-install-linux.md)**: En sökplattform för stora företag som tillåter fulltextsökning i data.

Information om hur du utvecklar dina egna skriptåtgärder finns i [Utveckling av skriptåtgärder med HDInsight](hdinsight-hadoop-script-actions-linux.md).

## <a name="components-and-utilities-on-hdinsight-clusters"></a>Komponenter och verktyg i HDInsight-kluster
hello ingår följande komponenter och verktyg i HDInsight-kluster:

* **[Ambari](#ambari)**: Klusteretablering, hantering, övervakning och verktyg.
* **[Avro](#avro)**  (Microsoft .NET-bibliotek för Avro): dataserialisering för hello Microsoft .NET-miljön. 
* **[Hive och HCatalog](#hive)**: SQL-liknande frågefunktioner och ett tabell- och lagringshanteringslager.
* **[Mahout](#mahout)**: För skalbara program med maskininlärning.
* **[MapReduce](#mapreduce)**: Äldre ramverk för distribuerad Hadoop-bearbetning och resurshantering. See [YARN](#yarn).
* **[Oozie](#oozie)**: Arbetsflödeshantering.
* **[Phoenix](#phoenix)**: Relationsdatabaslager över HBase.
* **[Pig](#pig)**: Enklare skriptfunktioner för MapReduce-omformningar.
* **[Sqoop](#sqoop)**: Import och export av data.
* **[Tez](#tez)**: låter dataintensiva processer toorun effektivt i större skala.
* **[YARN](#yarn)**: resurshantering som ingår i hello Hadoop-kärnbiblioteket.
* **[ZooKeeper](#zookeeper)**: Samordning av processer i distribuerade system.

> [!NOTE]
> Information om hello specifika komponenter och versionsinformation finns [Hadoop-komponenter och versioner i HDInsight][component-versioning]
>
>

### <a name="ambari"></a>Ambari
Apache Ambari används för etablering, hantering och övervakning av Apache Hadoop-kluster. Det innehåller en intuitiv samling operatörsverktyg och en stabil uppsättning API: er som döljer hello komplexiteten hos Hadoop och förenklar hello åtgärden kluster. HDInsight-kluster på Linux tillhandahåller både Ambari-webbgränssnittet hello och hello Ambari REST API. Ambari Views i HDInsight-kluster ger möjlighet att använda plugin-UI-funktioner.
Se [Hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md) och <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API-referens</a>.

### <a name="avro"></a>Avro (Microsoft .NET-bibliotek för Avro)
hello Microsoft .NET-bibliotek för Avro implementerar hello Apache Avros kompakta binära dataöverföringsformat för serialisering för hello Microsoft .NET-miljön. Den definierar en språkoberoende schema så att data serialiseras på ett språk som kan läsas på andra språk. Detaljerad information om hello-format finns i Hej < target = _ ”blank” href = ”http://avro.apache.org/docs/current/spec.html” > specifikation för Apache Avro</a>. hello format för Avro filer stöder hello distribuerade MapReduce-programmeringsmodellen: filerna är ”delbara”, vilket innebär att du kan söka efter valfri punkt i en fil och börja läsa från ett visst block. toofind reda på hur, se [serialisera data med hello Microsoft .NET-bibliotek för Avro](hdinsight-dotnet-avro-serialization.md). Linux-baserade kluster support toocome.

### <a name="hdfs"></a>HDFS
Hadoop Distributed File System (HDFS) är ett filsystem som, med YARN och MapReduce, hello kärnan i Hadoop-teknik. Det är hello standardfilsystemet för Hadoop-kluster i HDInsight. Se [Fråga efter data från HDFS-kompatibel lagring](hdinsight-hadoop-use-blob-storage.md).

### <a name="hive"></a>Hive och HCatalog
<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> är data warehouse bygger på Hadoop och som gör att du tooquery programvara och hantera stora datamängder i distribuerade lagringsutrymmen med hjälp av en SQL-liknande språk som kallas HiveQL. Hive, liksom Pig, är en abstraktion ovanpå MapReduce och den översätter frågor till en serie MapReduce-jobb. Hive är närmare tooa system för relationsdatabashantering än Pig och används för mer strukturerade data. För Ostrukturerade data är Pig hello bättre val. Se [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md).

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> är ett tabell och lagringshanteringslager för Hadoop som ger dig en relationsbaserad datavy. I HCatalog kan du läsa och skriva filer i alla format som fungerar med en Hive SerDe (serialiserare-deserialiserare).

### <a name="mahout"></a>Mahout
<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> är ett bibliotek med maskininlärningsalgoritmer som körs på Hadoop. Med principerna för statistik maskininlärningsprogrammen systemen toolearn från data och toouse tidigare resultat toodetermine framtida beteende. Se [Generera filmrekommendationer med hjälp av Mahout på Hadoop](hdinsight-mahout.md).

### <a name="mapreduce"></a>MapReduce
MapReduce är hello äldre programvaruramverket för Hadoop för att skriva program toobatch stordatauppsättningar parallellt. Ett MapReduce-jobb delar upp stora datauppsättningar och organiserar hello data i nyckel-värdepar för bearbetning. MapReduce-jobb körs på [YARN](#yarn). Se <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> i hello Hadoop-wikin.

### <a name="oozie"></a>Oozie
<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> är ett system för att koordinera arbetsflöden som hanterar Hadoop-jobb. Den är integrerad med hello Hadoop-stacken och stöder Hadoop-jobb för MapReduce, Pig, Hive och Sqoop. Det kan också vara används tooschedule jobb specifika tooa system, som Java-program eller kommandoskript. Se [Använda Oozie med Hadoop](hdinsight-use-oozie-linux-mac.md).

### <a name="phoenix"></a>Phoenix
<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> är ett relationsdatabaslager över HBase. Phoenix omfattar en JDBC-drivrutin som låter dig tooquery och hantera SQL-tabeller direkt. Phoenix översätter frågor och andra instruktioner till egna NoSQL API-anrop (i stället för att använda MapReduce) vilket möjliggör snabbare program ovanpå NoSQL-lagringsplatser. Se [Använda Apache Phoenix och SQuirreL med HBase-kluster](hdinsight-hbase-phoenix-squirrel.md).

### <a name="pig"></a>Pig
<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> är en plattform på hög nivå som gör att du tooperform komplexa MapReduce-omformningar på stora datauppsättningar genom att använda ett enkelt skriptspråk som kallas Pig Latin. Pig översätter hello Pig Latin-skripten så att de kan köras inom Hadoop. Du kan skapa användardefinierade funktioner (UDF) tooextend Pig Latin. Se [Använda Pig med Hadoop](hdinsight-use-pig.md).

### <a name="sqoop"></a>Sqoop
<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> är ett verktyg som överför bulkdata mellan Hadoop och relationsdatabaser som SQL eller andra strukturerade datalager så effektivt som möjligt. Se [Använda Sqoop med Hadoop](hdinsight-use-sqoop.md).

### <a name="tez"></a>Tez
<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> är ett programramverk som bygger på Hadoop YARN och som kör komplicerade acykliska diagram över allmän databearbetning. Det är en mer flexibel och kraftfull efterföljare toohello MapReduce-ramverket som gör att dataintensiva processer som Hive, toorun mer effektivt i större skala. Se ["Använda Apache Tez för bättre prestanda" i Använda Hive och HiveQL](hdinsight-use-hive.md#usetez).

### <a name="yarn"></a>YARN
Apache YARN är hello nästa generation av MapReduce (MapReduce 2.0 eller MRv2) och stöder databehandlingsscenarier utöver MapReduce-batchbearbetning med bättre skalbarhet och realtidsbearbetning. YARN tillhandahåller resurshantering och ett distribuerat programramverk. MapReduce-jobb körs på YARN. Se <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Nästa generation av Apache Hadoop MapReduce (YARN)</a>.

### <a name="zookeeper"></a>ZooKeeper
<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> koordinerar processer i stora distribuerade system med hjälp av ett delat hierarkiskt namnområde med dataregister (znodes). Znodes innehåller små mängder meta information som behövs för toocoordinate processer: status, plats, konfiguration och så vidare. Se ett exempel på [ZooKeeper med ett HBase-kluster och Apache Phoenix](hdinsight-hbase-phoenix-squirrel-linux.md). 

## <a name="programming-languages-on-hdinsight"></a>Programmeringsspråk i HDInsight
HDInsight-kluster – Hadoop-, HBase-, Kafka-, Spark- och andrakluster – stöder ett antal programmeringsspråk, men vissa är inte installerade som standard. För bibliotek, moduler eller paket som inte installeras som standard, [använda åtgärden tooinstall hello skriptkomponenter](hdinsight-hadoop-script-actions-linux.md). 

### <a name="default-programming-language-support"></a>Programmeringsspråk som stöds som standard
Som standard stöder HDInsight-kluster:

* Java
* Python

Ytterligare språk kan installeras med [skriptåtgärder](hdinsight-hadoop-script-actions-linux.md).

### <a name="java-virtual-machine-jvm-languages"></a>Java Virtual Machine-språk (JVM)
Många andra språk än Java kan köras på en Java virtual machine (JVM) dock kan köra vissa av dessa språk ytterligare komponenter behöva installeras på hello-kluster.

Följande JVM-baserade språk stöds i HDInsight-kluster:

* Clojure
* Jython (Python för Java)
* Scala

### <a name="hadoop-specific-languages"></a>Hadoop-specifika språk
HDInsight-kluster stöder följande språk som är specifika toohello Hadoop-teknikstacken hello:

* Pig Latin för Pig-jobb
* HiveQL för Hive-jobb och SparkSQL

## <a name="hdinsight-standard-and-hdinsight-premium"></a>HDInsight Standard och HDInsight Premium
HDInsight erbjuder molntjänster för stordata i två kategorier, Standard och Premium. HDInsight Standard tillhandahåller ett kluster för företag som organisationer kan använda toorun sina stordataarbetsbelastningar. HDInsight Premium är en ytterligare utbyggnad av standardkapaciteten med avancerade analys- och säkerhetsfunktioner för ett HDInsight-kluster. Mer information finns i [Azure HDInsight Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium)

## <a name="microsoft-business-intelligence-and-hdinsight"></a>Microsoft Business Intelligence och HDInsight
Verktyg för bekant business intelligence (BI) Hämta, analysera och rapportera data som integreras med HDInsight med hjälp av antingen hello Power Query-tillägget eller hello Microsoft Hive ODBC-drivrutinen:

* [Ansluta Excel tooHadoop med Power Query](hdinsight-connect-excel-power-query.md): Lär dig hur tooconnect Excel toohello Azure Storage-konto som lagrar hello data från ditt HDInsight-kluster med hjälp av Microsoft Power Query för Excel. En Windows-arbetsstation krävs. 
* [Ansluta Excel tooHadoop med hello Microsoft Hive ODBC-drivrutinen](hdinsight-connect-excel-hive-odbc-driver.md): Lär dig hur tooimport data från HDInsight med hello Microsoft Hive ODBC-drivrutinen. En Windows-arbetsstation krävs. 
* [Microsoft Cloud Platform](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Lär dig mer om Power BI för Office 365, ladda ned utvärderingsversionen för hello SQL Server och konfigurera SharePoint Server 2013 och SQL Server BI.
* [SQL Server Analysis Services](http://msdn.microsoft.com/library/hh231701.aspx)
* [SQL Server Reporting Services](http://msdn.microsoft.com/library/ms159106.aspx)


## <a name="next-steps"></a>Nästa steg

* [Komma igång med Hadoop i HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md): En snabbkurs i hur du etablerar HDInsight Hadoop-kluster och kör Hive-exempelfrågor.
* [Komma igång med Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md): En snabbkurs i hur du skapar ett Spark-kluster och kör interaktiva Spark SQL-frågor.
* [Använda R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md): Börja använda R Server i HDInsight Premium.
* [Etablera HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md): Lär dig hur tooprovision en HDInsight Hadoop-kluster via hello Azure-portalen, Azure CLI eller Azure PowerShell.


[component-versioning]: hdinsight-component-versioning.md
[zookeeper]: http://zookeeper.apache.org/