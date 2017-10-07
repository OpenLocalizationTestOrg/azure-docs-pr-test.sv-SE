---
title: "aaaUse Apache Kafka med Storm på HDInsight - Azure | Microsoft Docs"
description: "Apache Kafka har installerats med Apache Storm på HDInsight. Lär dig hur toowrite tooKafka och läs sedan från den, med hjälp av hello KafkaBolt och KafkaSpout komponenter medföljer Storm. Lär dig också hur toouse hello som framework toodefine och skicka Storm-topologier."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: e4941329-1580-4cd8-b82e-a2258802c1a7
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: larryfr
ms.openlocfilehash: 95701f51dfdf6f1a859dcde96d7053df4f21701f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a>Använda Apache Kafka (förhandsversion) med Storm på HDInsight

Lär dig hur toouse Apache Storm tooread från och skriva tooApache Kafka. Det här exemplet visar även hur toosave data från en Storm-topologi toohello HDFS-kompatibla filsystemet används av HDInsight.

> [!NOTE]
> hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både ett Storm på HDInsight och en Kafka på HDInsight-kluster. Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Storm-kluster toodirectly kommunicera med hello Kafka klustret.
> 
> Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.

## <a name="get-hello-code"></a>Hämta hello kod

hello koden hello exemplet i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).

toocompile det här projektet du behöver följande konfiguration för din utvecklingsmiljö hello:

* [Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre. HDInsight 3.5 eller högre krävs Java 8.

* [Maven 3.x](https://maven.apache.org/download.cgi)

* En SSH-klient (du behöver hello `ssh` och `scp` kommandon) – information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* En textredigerare eller IDE.

hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn. Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.

* `JAVA_HOME`-måste peka toohello katalog där hello JDK har installerats.
* `PATH`-bör innehålla hello följande sökvägar:
  
    * `JAVA_HOME`(eller motsvarande hello-sökväg).
    * `JAVA_HOME\bin`(eller motsvarande hello-sökväg).
    * hello katalog där Maven är installerat.

## <a name="create-hello-clusters"></a>Skapa hello kluster

Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet. Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret. I det här exemplet finns både hello Kafka och Storm-kluster i Azure-nätverk. hello följande diagram visar hur kommunikation som flödar mellan hello kluster:

![Diagram över Storm och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> Andra tjänster på hello klustret hello som SSH- och Ambari kan nås över internet. Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Du kan skapa en Azure-nätverk, Kafka, och Storm-kluster manuellt, är det enklare toouse en Azure Resource Manager-mall. Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Storm-kluster tooyour Azure-prenumeration.

1. Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**. Den skapar hello följande resurser:
    
    * Azure-resursgrupp
    * Azure Virtual Network
    * Azure-lagringskonto
    * Kafka på HDInsight version 3,6 (tre arbetarnoder)
    * Storm på HDInsight version 3,6 (tre arbetarnoder)

  > [!WARNING]
  > tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna. Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.

2. Använd hello följande riktlinjer toopopulate hello poster på hello **anpassad distribution** bladet:
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * **Resursgruppen**: skapa en grupp eller välj en befintlig. Den här gruppen innehåller hello HDInsight-kluster.
   
    * **Plats**: Välj en plats geografiskt nära tooyou.

    * **Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Storm och Kafka kluster. Ange till exempel **hdi** skapar en Storm-kluster med namnet **storm-hdi** och ett Kafka kluster med namnet **kafka hdi**.
   
    * **Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Storm och Kafka kluster.
   
    * **Klustret inloggningslösenordet**: hello administratörslösenord för hello Storm och Kafka kluster.
    
    * **SSH-användarnamn**: hello SSH användaren toocreate för hello Storm och Kafka kluster.
    
    * **SSH-lösenordet**: hello användarlösenord hello SSH för hello Storm och Kafka kluster.

3. Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.

4. Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**. Det tar cirka 20 minuter toocreate hello kluster.

När du har skapat hello resurser visas hello bladet för resursgruppen hello.

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> Observera att hello namnen på hello HDInsight-kluster är **storm-BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall. Du kan använda dessa namn i senare steg när du ansluter toohello kluster.

## <a name="understanding-hello-code"></a>Förstå hello kod

Det här projektet innehåller två topologier:

* **KafkaWriter**: definieras av hello **writer.yaml** filen, skriver den här topologin slumpmässiga meningar tooKafka med hello KafkaBolt med Apache Storm.

    Den här topologin använder en anpassad **SentenceSpout** komponenten toogenerate slumpmässiga meningar.

* **KafkaReader**: definieras av hello **reader.yaml** filen, den här topologin läser data från Kafka med hello KafkaSpout med Apache Storm och sedan loggar hello data toostdout.

    Den här topologin använder toodefault för hello Storm HdfsBolt toowrite datalagring för hello Storm-kluster.
### <a name="flux"></a>Som

hello topologier definieras med hjälp av [som](https://storm.apache.org/releases/1.1.0/flux.html). Som introducerades i Storm-0.10.x och låter dig tooseparate hello topologi configuration från hello kod. Topologier som använder hello som framework definieras hello topologi i en YAML-fil. Hej YAML filen kan vara ingår i hello-topologi. Det kan också vara en fristående fil som används när du skickar hello-topologi. Som stöder också variabeln ersättning vid körning, som används i det här exemplet.

hello anges följande parametrar vid körning för dessa topologier:

* `${kafka.topic}`: hello namnet på hello Kafka avsnitt som hello topologier läsning och skrivning till.

* `${kafka.broker.hosts}`: hello värd att hello Kafka förmedlar körs på. hello broker informationen används av hello KafkaBolt när du skriver tooKafka.

* `${kafka.zookeeper.hosts}`: hello-värdar som Zookeeper körs på i hello Kafka klustret.

Mer information om topologier som finns [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).

## <a name="download-and-compile-hello-project"></a>Hämta och kompilera hello-projekt

1. Hämta hello projektet från på din utvecklingsmiljö [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), öppna en kommandorad och ändra kataloger toohello plats som du hämtade hello-projekt.

2. Från hello **hdinsight-storm-java-kafka** katalog, Använd hello följande kommando toocompile hello projekt och skapa ett paket för distribution:

  ```bash
  mvn clean package
  ```

    hello paketet skapas en fil med namnet `KafkaTopology-1.0-SNAPSHOT.jar` i hello `target` directory.

3. Använd hello följande kommandon toocopy hello paketet tooyour Storm på HDInsight-kluster. Ersätt **användarnamn** med hello SSH-användarnamn för hello-kluster. Ersätt **BASENAME** med grundläggande hello-namn du använde när du skapar hello kluster.

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    När du uppmanas ange hello-lösenord som du använde när du skapar hello kluster.

## <a name="configure-hello-topology"></a>Konfigurera hello-topologi

1. Använd någon av följande metoder toodiscover hello hello Kafka broker värdar:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $brokerHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($brokerHosts -join ":9092,") + ":9092"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > hello Bash exempel förutsätter att `$CLUSTERNAME` innehåller hello namnet på hello HDInsight-kluster. Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad. När du uppmanas du ange hello lösenord för hello inloggningskonto för klustret.

    hello-värdet som returneras är liknande toohello följande text:

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > Det kan finnas fler än två broker värdar för klustret, behöver du inte tooprovide en fullständig lista över alla värdar tooclients. En eller två är tillräckligt.

2. Använd någon av följande metoder toodiscover hello Kafka Zookeeper värdar hello:

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $zookeeperHosts = $respObj.host_components.HostRoles.host_name[0..1]
    ($zookeeperHosts -join ":2181,") + ":2181"
    ```

    ```bash
    curl -su admin -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2
    ```

    > [!IMPORTANT]
    > hello Bash exempel förutsätter att `$CLUSTERNAME` innehåller hello namnet på hello HDInsight-kluster. Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad. När du uppmanas du ange hello lösenord för hello inloggningskonto för klustret.

    hello-värdet som returneras är liknande toohello följande text:

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > Det finns fler än två Zookeeper-noder, behöver du inte tooprovide en fullständig lista över alla värdar tooclients. En eller två är tillräckligt.

    Spara det här värdet eftersom det används senare.

3. Redigera hello `dev.properties` filen i projektet hello hello rot. Lägg till hello Broker och Zookeeper värdar information toohello matchande rader i den här filen. hello konfigureras följande exempel med hjälp av hello exempelvärden från hello föregående steg:

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. Spara hello `dev.properties` fil och sedan använda hello följande kommando tooupload den toohello Storm-kluster:

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    Ersätt **användarnamn** med hello SSH-användarnamn för hello-kluster. Ersätt **BASENAME** med grundläggande hello-namn du använde när du skapar hello kluster.

## <a name="start-hello-writer"></a>Starta hello skrivare

1. Använd följande tooconnect toohello Storm-kluster med SSH hello. Ersätt **användarnamn** med hello SSH-användarnamn används när du skapar hello kluster. Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    När du uppmanas ange hello-lösenord som du använde när du skapar hello kluster.
   
    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Från hello SSH-anslutning, använder du följande kommando toocreate hello Kafka avsnittet används av hello topologi hello:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    Ersätt `$KAFKAZKHOSTS` med hello Zookeeper värd för information som du hämtade i hello föregående avsnitt.

2. Använd hello efter kommandot toostart hello writer topologi från hello SSH-anslutning toohello Storm-kluster:

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    hello-parametrar som används med det här kommandot är:

    * `org.apache.storm.flux.Flux`: Använd som tooconfigure och kör den här topologin.

    * `--remote`: Skicka hello topologi tooNimbus. hello topologi distribueras över hello arbetarnoder i klustret hello.

    * `-R /writer.yaml`: Använd hello `writer.yaml` filen tooconfigure hello-topologi. `-R`Anger att den här resursen ingår i hello jar-filen. Det är i hello jar hello rot så `/writer.yaml` är hello sökvägen tooit.

    * `--filter`: Fylla poster i hello `writer.yaml` topologi med värden i hello `dev.properties` fil. Till exempel hello värdet för hello `kafka.topic` post i hello-filen är används tooreplace hello `${kafka.topic}` post i hello topologi definition.

5. När hello-topologi har börjat använda hello efter kommandot tooverify att skrivs data toohello Kafka avsnittet:

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    Ersätt `$KAFKAZKHOSTS` med hello Zookeeper värd för information som du hämtade i hello föregående avsnitt.

    Detta kommando använder ett skript som medföljer Kafka toomonitor hello avsnittet. Efter ett tag och bör börja returnera slumpmässiga meningar som har skrivits toohello avsnittet. hello utdata är liknande toohello följande exempel:

        i am at two with nature             
        an apple a day keeps hello doctor away
        snow white and hello seven dwarfs     
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        hello cow jumped over hello moon        
        an apple a day keeps hello doctor away
        an apple a day keeps hello doctor away
        four score and seven years ago      
        snow white and hello seven dwarfs     
        snow white and hello seven dwarfs     
        i am at two with nature             
        an apple a day keeps hello doctor away

    Tryck på Ctrl + c toostop hello skript.

## <a name="start-hello-reader"></a>Starta hello läsare

1. Använd hello efter kommandot toostart hello reader topologi från hello SSH-session toohello Storm-kluster:

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. När hello topologi startar öppna hello Storm-Användargränssnittet. Den här webbgränssnittet finns i https://storm-BASENAME.azurehdinsight.net/stormui. Ersätt __BASENAME__ med hello basnamn används när hello klustret har skapats. 

    Vid uppmaning används hello admin inloggningsnamnet (standard `admin`) och lösenord som används när hello klustret har skapats. Du ser en webbsida liknande toohello följande bild:

    ![Storm UI](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. Hello Storm-Användargränssnittet, Välj hello __kafka läsare__ länken i hello __Topology Summary__ avsnittet toodisplay information om hello __kafka läsare__ topologi.

    ![Topologi sammanfattningen i hello Storm webbgränssnittet](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. toodisplay information om hello instanser av hello loggaren bult komponent, Välj hello __loggaren bult__ länken i hello __bultar (hela tiden)__ avsnitt.

    ![Loggaren bult länk under hello bultar](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. I hello __Executors__ väljer du en länk i hello __Port__ kolumnen toodisplay logga information om den här instansen av hello-komponent.

    ![Executors länk](./media/hdinsight-apache-storm-with-kafka/executors.png)

    hello-loggen innehåller en logg över hello data läses från hello Kafka avsnittet. hello information i loggen för hello är liknande toohello följande text:

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a>Stoppa hello topologier

Från en SSH-session toohello Storm-kluster, använder du följande kommandon toostop hello Storm-topologier hello:

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen. Ta bort hello resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet.

## <a name="next-steps"></a>Nästa steg

Flera exempeltopologier som kan användas med Storm på HDInsight finns [exempel Storm-topologier och komponenter](hdinsight-storm-example-topology.md).

Mer information om distribuera och övervaka topologier på Linux-baserat HDInsight finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)