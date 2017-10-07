---
title: "aaaWhat är Apache Storm - Azure HDInsight | Microsoft Docs"
description: "Apache Storm kan tooprocess dataströmmar i realtid. Azure HDInsight kan du tooeasily skapa Storm-kluster på hello Azure-molnet. Med Visual Studio kan du skapa Storm-lösningar med hjälp av C# och sedan distribuera tooyour HDInsight Storm-kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "apache storm användarfall,stormkluster,vad är apache storm"
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 6c6b2925ef3e5666dfecc3fb3c835bb362902c51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-storm-on-azure-hdinsight"></a>Vad är Apache Storm på Azure HDInsight?

[Apache Storm](http://storm.apache.org/) är ett distribuerat, feltolerant beräkningssystem med öppen källkod. Du kan använda Storm tooprocess dataströmmar i realtid med Hadoop. Storm-lösningar kan även ge garanterad bearbetning av data, med hello möjlighet tooreplay data som inte har bearbetats hello första gången.

Storm på HDInsight ger hello följande viktiga fördelar:

* Fungerar som en hanterad tjänst med ett SLA med 99,9 procent drifttid.

* Stöder enkel anpassning genom skriptkörning mot ett Storm-kluster när det skapas eller i efterhand. Mer information finns i [Customize HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md) (Anpassa HDInsight-kluster med skriptåtgärder).

* Använder olika språk. Du kan skriva Storm-komponenter i hello språket du föredrar, till exempel Java, C# och Python.

    * Integrerar Visual Studio med HDInsight för hello utveckling, hantering och övervakning av C#-topologier. Mer information finns i [utveckla C# Storm-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * Stöder hello Trident Java-gränssnitt. Du kan skapa Storm-topologier som stöder engångsbearbetning av meddelanden, transaktionell datalagringsbeständighet och en uppsättning vanliga Stream Analytics-åtgärder.

*  Enkelt skala upp eller ned Storm-kluster. Du kan lägga till eller ta bort worker noder med ingen påverkan toorunning Storm-topologier.

* Kan integreras med hello följande Azure-tjänster:

    * Azure Event Hubs

    * Azure Virtual Network

    * Azure SQL Database

    * Azure Storage

    * Azure Cosmos DB

* Kombinerar hello funktionerna i flera HDInsight-kluster på ett säkert sätt med hjälp av virtuellt nätverk. Du kan skapa analytiska pipelines med Storm, Kafka, Spark, HBase- eller Hadoop-kluster.

En lista över företag som använder Apache Storm för sina lösningar för analys i realtid finns i [Företag som använder Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

tooget igång med Storm, se [Kom igång med Storm på HDInsight][gettingstarted].

## <a name="how-does-storm-work"></a>Så här fungerar Storm

Storm kör topologier i stället för hello MapReduce-jobb som du kanske känner till. Storm-topologier består av flera komponenter som är ordnade i en riktad acyklisk graf (DAG). Data flödar mellan hello komponenter i hello graph. Varje komponent använder en eller flera dataströmmar och kan du generera en eller flera strömmar. hello följande diagram illustrerar hur data flödar mellan komponenter i en grundläggande ordräkningstopologin:

![Exempel på hur komponenter är ordnade i en Storm-topologi](./media/hdinsight-storm-overview/example-apache-storm-topology-diagram.png)

* Kanalkomponenter överför data till en topologi. De genererar en eller flera strömmar till hello-topologi.

* Bultkomponenter förbrukar strömmar som sänts ut från kanaler eller andra bultar. Bultar kan du generera dataströmmar i hello-topologi. Bultar ansvarar även för att skriva data tooexternal tjänster eller lagring, till exempel HDFS, Kafka eller HBase.

## <a name="ease-of-creation"></a>Enkelt att skapa

Du kan etablera ett nytt Storm-kluster i HDInsight på några minuter. Mer information om hur du skapar ett Storm-kluster finns i [Kom igång med Storm på HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

## <a name="ease-of-use"></a>Användarvänlighet

* __Secure Shell (SSH) anslutningen__: du kan komma åt hello huvudnoderna av Storm-kluster via hello Internet med hjälp av SSH. Du kan köra kommandon direkt i klustret med hjälp av SSH.

  Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

* __Web anslutningen__: alla HDInsight-kluster tillhandahåller hello Ambari-webbgränssnittet. Du kan enkelt kan övervaka, konfigurera och hantera tjänster på ditt kluster med hjälp av hello Ambari-webbgränssnittet. Storm-kluster ger också hello Storm-Användargränssnittet. Du kan övervaka och hantera Storm-topologier som körs från webbläsaren med hjälp av hello Storm-Användargränssnittet.

  Mer information finns i hello [hantera HDInsight med hello Ambari-Webbgränssnittet](hdinsight-hadoop-manage-ambari.md) och [övervaka och hantera med hjälp av hello Storm-Användargränssnittet](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-storm-ui) dokument.

* __Azure PowerShell och Azure CLI__: PowerShell och CLI båda ger kommandoradsverktyg som du kan använda från din klient system toowork med HDInsight och andra Azure-tjänster.

* __Visual Studio-integrationen__: Azure Data Lake-verktyg för Visual Studio innehåller projektmallar för att skapa C# Storm-topologier med hjälp av hello SCP.Net framework. Data Lake-verktyg ger också verktyg toodeploy, övervaka och hantera lösningar med Storm på HDInsight.

  Mer information finns i [utveckla C# Storm-topologier med hello HDInsight Tools för Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

## <a name="integration-with-other-azure-services"></a>Integration med andra Azure-tjänster

* __Azure Data Lake Store__: Ett exempel på hur du använder Data Lake Store med Storm-kluster finns i [Use Azure Data Lake Store with Apache Storm on HDInsight](hdinsight-storm-write-data-lake-store.md) (Använda Azure Data Lake Store med Apache Storm i HDInsight).

* __Händelsehubbar__: ett exempel på Händelsehubbar med ett Storm-kluster, se hello följande dokument:

    * [Develop a Java-based topology for Storm on HDInsight](hdinsight-storm-develop-java-topology.md) (Utveckla en Java-baserad topologi för Storm i HDInsight)

    * [Process events from Azure Event Hubs with Storm on HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md) (Bearbeta händelser från Azure Event Hubs med Storm i HDInsight (C#))

* __SQL-databas__, __Cosmos DB__, __Händelsehubbar__, och __HBase__: mall exempel ingår i hello Data Lake-verktyg för Visual Studio. Mer information finns i [Develop a C# topology for Storm on HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Utveckla en C#-topologi för Storm i HDInsight).

## <a name="reliability"></a>Tillförlitlighet

Apache Storm garanterar att varje inkommande meddelande alltid fullständigt bearbetas, även om hello dataanalysen är utspridd på hundratals noder.

Hej Nimbus-noden ger funktioner liknande toohello Hadoop JobTracker och tilldelar aktiviteter tooother noder i ett kluster via Zookeeper. Zookeeper-noder samordnar för ett kluster och underlättar kommunikationen mellan Nimbus och hello handledarens processen på arbetsnoderna hello. Om en nod för bearbetning kraschar informeras Nimbus-noden hello och tilldelar hello uppgiften och tillhörande data tooanother nod.

hello standardkonfigurationen för Apache Storm-kluster är toohave endast en Nimbus-noden. Storm på HDInsight kör två Nimbus-noder. Om hello primära noden kraschar växlar hello Storm-kluster toohello sekundära noden medan hello primära noden återställs. hello illustrerar följande diagram hello flödet aktivitetskonfigurationen för Storm på HDInsight:

![Diagram över nimbus och zookeeper och övervakaren](./media/hdinsight-storm-overview/nimbus.png)

## <a name="scale"></a>Skala

HDInsight-kluster kan skalas dynamiskt genom att lägga till eller ta bort arbetsnoder. Den här åtgärden kan utföras medan data bearbetas.

> [!IMPORTANT]
> tootake nytta av nya noder lagts till via skalning behöver du toorebalance Storm-topologier som startats innan hello klusterstorleken ökades.

## <a name="support"></a>Support

Storm i HDInsight levereras med fullständig, fortlöpande support på företagsnivå. Storm i HDInsight har även ett SLA med 99,9 % drifttid. Det innebär att vi garanterar att ett Storm-kluster har extern anslutning minst 99,9 procent hello tid.

Mer information finns på [Azure-supporten](https://azure.microsoft.com/support/options/).

## <a name="apache-storm-use-cases"></a>Apache Storm användningsområden

hello följande är några vanliga scenarier som du kan använda Storm på HDInsight:

* Sakernas Internet (IoT)
* Upptäckt av bedrägerier
* Sociala analyser
* Extrahering, transformering och inläsning (ETL)
* Nätverksövervakning
* Söka
* Mobile engagement

Information om verkliga scenarier finns hello [hur företag använder Storm](https://storm.apache.org/documentation/Powered-By.html) dokumentet.

## <a name="development"></a>Utveckling

.NET-utvecklare kan utforma och implementera topologier i C# med hjälp av Data Lake Tools för Visual Studio. Du kan också skapa hybridtopologier som använder Java- och C#-komponenter.

Mer information finns i [Develop C# topologies for Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md) (Utveckla en C#-topologi för Storm i HDInsight).

Du kan också utveckla Java-lösningar med hjälp av hello IDE önskat. Mer information finns i [Develop Java topologies for Storm on HDInsight](hdinsight-storm-develop-java-topology.md) (Utveckla Java-topologier för Storm i HDInsight).

Python kan också vara används toodevelop Storm-komponenter. Mer information finns i [Develop Storm topologies using Python on HDInsight](hdinsight-storm-develop-python-topology.md) (Utveckla Storm-topologier med Python i HDInsight).

## <a name="common-development-patterns"></a>Vanliga utvecklingsmönster

### <a name="guaranteed-message-processing"></a>Garanterad meddelandebehandling

Apache Storm kan ge olika nivåer av garanterad meddelandebehandling. Ett grundläggande Storm-program kan till exempel garantera bearbetning minst en gång och Trident kan garantera bearbetning exakt en gång.

Mer information finns i [Garantier om databearbetning](https://storm.apache.org/about/guarantees-data-processing.html) på apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

Hej mönster läsa en inkommande tuppel, sända noll eller flera tupplar och sedan kvittera hello inkommande tuppeln omedelbart i hello slutet av hello execute-metoden är vanligt. Storm tillhandahåller hello [IBasicBolt](https://storm.apache.org/releases/1.0.3/javadocs/org/apache/storm/topology/IBasicBolt.html) gränssnitt tooautomate detta mönster.

### <a name="joins"></a>Kopplingar

Hur dataströmmar kopplas varierar mellan olika program. Du kan till exempel koppla samman varje tuppel från flera strömmar till en ny ström eller så kan du koppla bara batchar av tupplar för ett specifikt fönster. Oavsett vilket kan du ansluta med hjälp av [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-). Fältet gruppering är ett sätt att definiera hur tupplar är routade toobolts.

I följande Java-exemplet hello, är fieldsGrouping används tooroute tupplar som kommer från komponenterna ”1”, ”2” och ”3” toohello MyJoiner bult:

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batches"></a>Batchar

Apache Storm tillhandahåller en intern tidsmekanism som kallas ”tidstuppel”. Du kan ange hur ofta en tidstuppel har genererats i topologin.

Ett exempel på hur du använder en tidstuppel från en C#-komponent finns i [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

### <a name="caches"></a>Caches

Minnesintern cachelagring används ofta som en mekanism för snabbare bearbetning eftersom den bevarar ofta använda tillgångar i minnet. Eftersom en topologi distribueras över flera noder, och har flera processer i varje nod, bör du överväga att använda [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-). Använd `fieldsGrouping` tooensure att tupplar som innehåller hello-fält som används för cache-sökning alltid är routade toohello samma process. Denna grupperingsfunktion förhindrar duplicering av cacheposter mellan processer.

### <a name="stream-top-n"></a>Dataström för ”högsta N”

När topologin beror på att beräkna ett värde för övre N, beräkna hello högsta N-värdet parallellt. Sedan merge hello resultatet av dessa beräkningar i ett globalt värde. Den här åtgärden kan utföras med hjälp av [fieldsGrouping](http://storm.apache.org/releases/current/javadocs/org/apache/storm/topology/InputDeclarer.html#fieldsGrouping-java.lang.String-org.apache.storm.tuple.Fields-) tooroute av fält för parallell bearbetning. Du kan sedan vidarebefordra tooa bult som globalt avgör hello högsta N-värdet.

Ett exempel på beräkning av högsta N-värdet finns hello [RollingTopWords](https://github.com/apache/storm/blob/master/examples/storm-starter/src/jvm/org/apache/storm/starter/RollingTopWords.java) exempel.

## <a name="logging"></a>Loggning

Storm använder Apache Log4j toolog information. En stor mängd data loggas som standard och det kan vara svårt toosort via hello information. Du kan inkludera en konfigurationsfil för loggning som en del av din Storm-topologi toocontrol loggningsbeteende.

För en exempeltopologi som visar hur tooconfigure loggning, se [Java-baserad WordCount](hdinsight-storm-develop-java-topology.md) exempel för Storm på HDInsight.

## <a name="next-steps"></a>Nästa steg

Lär dig mer om lösningar för realtidsanalys med Storm i HDInsight:

* [Komma igång med Apache Storm i HDInsight][gettingstarted]
* [Exempeltopologier för Apache Storm på HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md
