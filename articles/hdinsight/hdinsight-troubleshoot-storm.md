---
title: "aaaTroubleshoot Storm med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du använder Apache Storm med Azure HDInsight."
keywords: "Azure HDInsight, Storm, vanliga frågor och svar, felsökningsguide för vanliga problem"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a>Felsöka Storm med Azure HDInsight

Läs mer om hello de främsta problemen och sina lösningar för att arbeta med Apache Storm-nyttolaster i Apache Ambari.

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a>Hur kommer jag åt hello Storm-Användargränssnittet på ett kluster
Har du två alternativ för att komma åt hello Storm-Användargränssnittet från en webbläsare:

### <a name="ambari-ui"></a>Ambari UI
1. Gå toohello Ambari instrumentpanelen.
2. I hello lista över tjänster, Välj **Storm**.
3. I hello **snabblänkar** väljer du **Storm-Användargränssnittet**.

### <a name="direct-link"></a>Direktlänk
Du kan komma åt hello Storm-Användargränssnittet på hello följande URL:

https://\<klustrat DNS-namn\>/stormui

Exempel:

 https://stormcluster.azurehdinsight.NET/stormui

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a>Hur överföra Storm hubb kanal kontrollpunkt händelseinformation från en topologi tooanother

När du utvecklar topologier som läses från Azure Event Hubs genom att använda hello HDInsight Storm event hub kanal .jar-fil, måste du distribuera en topologi som har samma namn på ett nytt kluster hello. Du måste dock behålla hello kontrollpunktsdata som har utförts tooApache ZooKeeper på hello gamla klustret.

### <a name="where-checkpoint-data-is-stored"></a>Kontrollpunktsdata ska lagras
Kontrollpunktsdata för förskjutningarna lagras av hello event hub kanal i ZooKeeper i två rot-sökvägar:
- Icke-transaktionell kanal kontrollpunkter lagras i /eventhubspout.
- Transaktionell kanal kontrollpunktsdata lagras i / transaktionella.

### <a name="how-toorestore"></a>Hur toorestore
tooget hello skript och bibliotek om du använder tooexport data utanför ZooKeeper och sedan importera hello data tillbaka tooZooKeeper med ett nytt namn, se [HDInsight Storm exempel](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).

hello lib mappen innehåller .jar-filer som innehåller hello implementering för hello exporten/importen. hello bash har ett exempelskript som visar hur tooexport data från hello ZooKeeper-server på hello gamla klustret och sedan importera den bakre toohello ZooKeeper-server på hello nya klustret.

Kör hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skript från hello ZooKeeper-noder tooexport och därefter importera data. Uppdatera hello skriptet toohello rätt Hortonworks Data Platform (HDP) version. (Vi arbetar på att skripten generiska i HDInsight. Allmänt skript kan köras från en nod på hello kluster utan ändringar av hello användare.)

hello export-kommando skriver hello metadata tooan Apache Hadoop Distributed File System (HDFS) väg (i Azure Blob Storage eller Azure Data Lake Store store) på en plats som du anger.

### <a name="examples"></a>Exempel

#### <a name="export-offset-metadata"></a>Exportera offset metadata
1. Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.
2. Hello kör följande kommando (när du har uppdaterat hello HDP versionssträng) tooexport ZooKeeper förskjutning data toohello /stormmetadta/zkdata HDFS sökväg:

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a>Importera offset metadata
1. Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.
2. Kör hello följande kommando (när du har uppdaterat hello HDP versionssträng) tooimport ZooKeeper förskjutning data från hello HDFS sökväg/stormmetadata/zkdata toohello ZooKeeper-server på hello målkluster:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a>Ta bort offset metadata så att topologier kan börja bearbeta data från hello början eller från en tidsstämpel som hello-användaren väljer
1. Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.
2. Kör hello följande kommando (när du har uppdaterat hello HDP versionssträng) toodelete alla ZooKeeper förskjutning data i hello aktuella klustret:

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a>Hur jag för att hitta Storm binärfiler i ett kluster
Storm-binärfiler för hello aktuella HDP stack finns i /usr/hdp/current/storm-client. hello plats är hello samma både för huvudnoderna och arbetsnoder.
 
Det kan finnas flera binärfiler för specifika HDP versioner i /usr/hdp (till exempel /usr/hdp/2.5.0.1233/storm). Hej /usr/hdp/current/storm-client mappen är symlinked toohello senaste versionen som körs på hello klustret.

Mer information finns i [Anslut tooan HDInsight-kluster med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) och [Storm](http://storm.apache.org/).
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a>Hur vet hello topologi för distribution av ett Storm-kluster
Först identifiera alla komponenter som installeras med Storm på HDInsight. Ett Storm-kluster består av fyra nod:

* Gateway-noder
* HEAD-noder
* ZooKeeper-noder
* Arbetsnoder
 
### <a name="gateway-nodes"></a>Gateway-noder
En gateway-noden är en gateway och en omvänd proxy-tjänst som gör allmän åtkomst tooan active Ambari management-tjänsten. Den hanterar också Ambari ledande val.
 
### <a name="head-nodes"></a>HEAD-noder
Storm huvudnoderna kör hello följande tjänster:
* Nimbus
* Ambari-server
* Ambari mått server
* Ambari mått insamlaren
 
### <a name="zookeeper-nodes"></a>ZooKeeper-noder
HDInsight levereras med ett kvorum för ZooKeeper tre noder. hello kvorum storlek är fast och kan inte konfigureras.
 
Storm-tjänster i hello kluster är konfigurerade tooautomatically Använd hello ZooKeeper kvorum.
 
### <a name="worker-nodes"></a>Arbetsnoder
Storm arbetarnoder kör hello följande tjänster:
* Överordnad
* Worker Java virtuella datorer (JVMs) för topologier som körs
* Ambari-agent
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a>Hur jag för att hitta Storm event hub kanal binärfiler för utveckling
 
Mer information om hur du använder Storm event hub kanal .jar filer med din topologi finns hello följande resurser.
 
### <a name="java-based-topology"></a>Java-baserad topologi
[Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a>C#-baserade topologi (Mono på HDInsight 3.4 + Linux Storm-kluster)
[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology) (Bearbeta händelser från Azure Event Hubs med Storm i HDInsight (C#))
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a>Senaste Storm event hub-kanalen binärfiler för HDInsight 3.5 + Linux Storm-kluster
toolearn hur toouse hello senaste Storm event hub kanal som fungerar med HDInsight 3.5 + Linux Storm-kluster finns i hello mvn-lagringsplatsen [Readme-filen](https://github.com/hdinsight/mvn-repo/blob/master/README.md).
 
### <a name="source-code-examples"></a>Käll-kodexempel
Se [exempel](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) på hur tooread och skriva från Azure Event Hub med en Apache Storm-topologi (skriven i Java) på ett Azure HDInsight-kluster.
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a>Hur jag för att hitta Storm Log4J konfigurationsfiler på kluster
 
tooidentify Apache Log4J konfigurationsfiler för Storm-tjänster.
 
### <a name="on-head-nodes"></a>På huvudnoderna
Hej Nimbus Log4J konfiguration läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.
 
### <a name="on-worker-nodes"></a>På arbetsnoder
hello handledarens Log4J konfiguration läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.
 
hello worker Log4J-konfigurationsfilen har lästs från/usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.
 
Exempel: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml

