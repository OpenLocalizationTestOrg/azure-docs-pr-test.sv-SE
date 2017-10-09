---
title: aaaStart med Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate en Apache Kafka kluster på Azure HDInsight. Lär dig hur toocreate ämnen, prenumeranter och konsumenter."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a>Kom igång med Apache Kafka (förhandsversion) i HDInsight

Lär dig hur toocreate och använda en [Apache Kafka](https://kafka.apache.org) kluster på Azure HDInsight. Kafka är en distribuerad direktuppspelningsplattform med öppen källkod som är tillgänglig i HDInsight. Det används ofta som förhandlare meddelandet eftersom det ger liknande funktioner tooa publicera och prenumerera meddelandekön.

> [!NOTE]
> Det finns för närvarande två versioner av Kafka med HDInsight; 0.9.0 (HDInsight 3.4) och 0.10.0 (HDInsight 3.5 och 3.6). hello stegen i det här dokumentet förutsätter att du använder Kafka på HDInsight 3,6.

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a>Skapa ett Kafka-kluster

Använd hello följa steg toocreate en Kafka på HDInsight-kluster:

1. Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny**, **Intelligence + analys**, och välj sedan **HDInsight**.
   
    ![Skapa ett HDInsight-kluster](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. Från **grunderna**, ange hello följande information:

    * **Klusternamn**: hello namnet på hello HDInsight-kluster.
    * **Prenumerationen**: Välj hello prenumeration toouse.
    * **Klustret inloggning användarnamn** och **klustret inloggningslösenordet**: hello inloggning vid åtkomst till hello klustret via HTTPS. Du använder dessa autentiseringsuppgifter tooaccess tjänster, till exempel hello Ambari-Webbgränssnittet eller REST API.
    * **Secure Shell (SSH) användarnamn**: hello-inloggning som används vid åtkomst till hello klustret via SSH. Som standard är hello lösenord hello samma som hello inloggningslösenordet för klustret.
    * **Resursgruppen**: hello resurs grupp toocreate hello klustret i.
    * **Plats**: hello Azure-region toocreate hello klustret i.
   
 ![Välj en prenumeration](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. Välj **kluster typen**, och sedan ange hello följande värden från **klusterkonfigurationen**:
   
    * **Klustertyp**: Kafka

    * **Version**: Kafka 0.10.0 (HDI 3.6)

    * **Klusternivå**: Standard
     
 Använd slutligen hello **Välj** knappen toosave inställningar.
     
 ![Välj klustertyp](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. När du har valt hello typ av kluster, använda hello __Välj__ knappen tooset hello typ av kluster. Använd sedan hello __nästa__ toofinish grundläggande konfiguration.

5. Från **Lagring** ska du välja eller skapa ett lagringskonto. Hello stegen i det här dokumentet, lämna hello andra fält till hello standardvärden. Använd hello __nästa__ toosave konfiguration för lagring.

    ![Ange hello konto lagringsinställningarna för HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. Från __program (valfritt)__väljer __nästa__ toocontinue. Inga program krävs för det här exemplet.

7. Från __klusterstorleken__väljer __nästa__ toocontinue.

    > [!WARNING]
    > tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna.

    ![Ange hello Kafka klusterstorleken](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > Hej **diskar per arbetsnoden** post kontroller hello skalbarheten för Kafka på HDInsight. Mer information finns i [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (Konfigurera lagring och skalbarhet för Kafka i HDInsight).

8. Från __avancerade inställningar__väljer __nästa__ toocontinue.

9. Från hello **sammanfattning**, granska hello konfigurationen för hello kluster. Använd hello __redigera__ länkar toochange inställningar som är felaktiga. Slutligen Använd the__Create__ knappen toocreate hello-kluster.
   
    ![Sammanfattning av klusterkonfiguration](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > Det kan ta upp too20 minuter toocreate hello klustret.

## <a name="connect-toohello-cluster"></a>Ansluta toohello kluster

> [!IMPORTANT]
> När du utför följande steg hello, måste du använda en SSH-klient. Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

Använd SSH tooconnect toohello kluster från din klient:

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

Ersätt **SSHUSER** med hello SSH-användarnamn som du angav när klustret skapas. Ersätt **KLUSTERNAMN** med hello hello klustrets namn.

När du uppmanas ange hello-lösenord som du använde för hello SSH-konto.

Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

## <a id="getkafkainfo"></a>Hämta information om hello Zookeeper och Broker värden

När du arbetar med Kafka, måste du känna två värden värden. Hej *Zookeeper* värdar och hello *Broker* värdar. Dessa värdar används med hello Kafka API och många hello verktyg som levereras med Kafka.

Använd hello följande steg toocreate miljövariabler som innehåller information om hello värden. De här miljövariablerna används i hello stegen i det här dokumentet.

1. Från ett SSH-anslutning toohello kluster, Använd hello följande kommando tooinstall hello `jq` verktyget. Det här verktyget är används tooparse JSON-dokument och är användbart vid hämtning av information om hello broker värden:
   
    ```bash
    sudo apt -y install jq
    ```

2. tooset hello miljövariablerna med information som hämtas från Ambari, Använd hello följande kommandon:

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > Ange `CLUSTERNAME=` toohello namnet på hello Kafka klustret. Ange `PASSWORD=` toohello inloggning (admin) lösenord som du använde när du skapar hello kluster.

    hello följande är ett exempel på hello innehållet i `$KAFKAZKHOSTS`:
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    hello följande är ett exempel på hello innehållet i `$KAFKABROKERS`:
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > Hej `cut` kommandot är används tootrim hello listan över värdar tootwo värdposter. Du behöver inte tooprovide hello fullständig lista över värdar när du skapar en Kafka konsument eller tillverkare.
   
    > [!WARNING]
    > Förlita dig inte på hello informationen som returneras från den här sessionen tooalways är korrekt. Om du skalar hello kluster, nya mäklare läggs till eller tas bort. Om ett fel inträffar och har ersatts av en nod, kan hello värdnamn för hello nod ändras.
    >
    > Du bör hämta information om hello Zookeeper och broker värdar strax innan du använder tooensure som du har en giltig information.

## <a name="create-a-topic"></a>Skapa ett ämne

Kafka lagrar dataströmmar i kategorier som kallas *ämnen*. Använda ett skript som medföljer Kafka toocreate ett ämne från en SSH-anslutning tooa klustret headnode:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

Det här kommandot ansluter tooZookeeper med information om hello värden som lagras i `$KAFKAZKHOSTS`, och sedan skapa Kafka ämne med namnet **testa**. Du kan verifiera hello avsnittet skapades med hjälp av hello följande skript toolist avsnitt:

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

hello kommandots utdata visar Kafka avsnitt som innehåller hello **testa** avsnittet.

## <a name="produce-and-consume-records"></a>Skapa och använda poster

Kafka lagrar *poster* i ämnen. Poster produceras av *producenter*, och används av *konsumenter*. Producenter hämtar poster från asynkrona Kafka-*meddelandeköer*. Varje arbetsnod i HDInsight-klustret är en asynkron Kafka-meddelandekö.

Använd följande steg toostore poster i hello test avsnittet du skapade tidigare och läsa dem med hjälp av en konsument hello:

1. Använda ett skript som medföljer Kafka toowrite poster toohello avsnittet från hello SSH-session:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    Du returneras inte toohello fråga efter det här kommandot. I stället skriver några textmeddelanden och använder sedan **Ctrl + C** toostop skicka toohello avsnittet. Varje rad skickas som en separat post.

2. Använda ett skript som medföljer Kafka tooread poster från hello avsnittet:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    Detta kommando hämtar hello poster hello artikeln och visar dem. Med hjälp av `--from-beginning` anger hello konsumenten toostart från hello början av hello stream, så hämtas alla poster.

3. Använd __Ctrl + C__ toostop hello konsumenten.

## <a name="producer-and-consumer-api"></a>Producent- och konsument-API

Du kan också programmässigt skapa och använda poster med hello [Kafka API: er](http://kafka.apache.org/documentation#api). toobuild Java producent och konsumenten, använder du hello följande steg från din utvecklingsmiljö.

> [!IMPORTANT]
> Du måste ha följande komponenter installerade i din utvecklingsmiljö hello:
>
> * [Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, till exempel OpenJDK.
>
> * [Apache Maven](http://maven.apache.org/)
>
> * En SSH-klienten och hello `scp` kommando. Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.

1. Hämta hello exempel från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started). Använd hello-projekt i hello exempelvis hello producenten/konsumenten `Producer-Consumer` directory. Det här exemplet innehåller hello följande klasser:
   
    * **Kör** -startar hello konsumenten eller tillverkare.

    * **Producenten** -butiker 1 000 000 poster toohello avsnittet.

    * **Konsumenten** -läser poster från hello-avsnittet.

2. toocreate ett jar-paket, ändra kataloger toohello placeringen hello `Producer-Consumer` katalogen och Använd hello följande kommando:

    ```
    mvn clean package
    ```

    Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-producer-consumer-1.0-SNAPSHOT.jar`.

3. Använd hello följande kommandon toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` filen tooyour HDInsight-kluster:
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    Ersätt **SSHUSER** med hello SSH-användare för klustret och Ersätt **KLUSTERNAMN** med hello namnet på klustret. När du uppmanas ange hello lösenord för hello SSH-användare.

4. En gång hello `scp` kommandot slutförs kopiera hello-fil, ansluta toohello kluster med SSH. Använd följande kommando toowrite poster toohello testämne hello:

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. När hello processen är klar använder du hello efter kommandot tooread från hello avsnittet:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    hello poster läsas tillsammans med antalet poster visas. Du kan se några fler än 1 000 000 loggas som du har skickat flera poster toohello avsnitt med ett skript i ett tidigare steg.

6. Använd __Ctrl + C__ tooexit hello konsumenten.

### <a name="multiple-consumers"></a>Flera konsumenter

Kafka-konsumenter använder en konsumentgrupp vid läsning av poster. Hello samma grupp med flera konsumenter balanserade leder till att läsa in läsningar av ett ämne. Varje konsument i hello-gruppen tar emot en del av hello poster. toosee hur det fungerar, Använd hello följande steg:

1. Öppna ett nytt SSH-session toohello kluster, så att du har två av dem. I varje session använder hello följande toostart hello konsumenter med samma konsumenten grupp-ID:
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    Detta kommando startar konsumenter med hjälp av grupp-ID för hello `mygroup`.

    > [!NOTE]
    > Använd hello kommandon i hello [hämta hello Zookeeper och Broker värdinformation](#getkafkainfo) avsnittet tooset `$KAFKABROKERS` för den här SSH-sessionen.

2. Titta på som varje session antal hello-poster som tas emot från hello-avsnittet. hello totalt båda sessioner bör vara hello samma som du tidigare fått från en klient.

Användning av klienter i samma grupp hanteras via hello partitioner för hello avsnittet hello. För hello `test` avsnittet skapade tidigare, den har åtta partitioner. Om du öppnar åtta SSH-sessioner och starta en konsument i alla sessioner läser varje konsument poster från en enda partition för hello ämnet.

> [!IMPORTANT]
> Det får inte finnas flera instanser av konsumenten i en konsumentgrupp än partitioner. I det här exemplet kan en konsumentgrupp innehålla upp tooeight konsumenter eftersom hello antalet partitioner i hello-avsnittet. Du kan även ha flera konsumentgrupper med högst åtta konsumenter vardera.

Poster som lagras i Kafka lagras i hello ordning de tas emot inom en partition. tooachieve i sorterade levereras poster *inom en partition*, skapa en konsumentgrupp där hello antalet instanser av konsumenten matchar hello antalet partitioner. tooachieve i sorterade levereras poster *i hello avsnittet*, skapa en konsumentgrupp med endast en konsument-instans.

## <a name="streaming-api"></a>Strömmande API

hello streaming API har lagts till tooKafka i version 0.10.0; tidigare versioner förlitar sig på Apache Spark eller Storm för bearbetning av dataströmmen.

1. Om du inte redan har gjort hämta hello exempel från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour utvecklingsmiljö. För hello strömning exempel, använda hello projektet på hello `streaming` directory.
   
    Det här projektet innehåller endast en klass `Stream`, som läser poster från hello `test` ämne som skapats tidigare. Det antal hello ord läsa och genererar varje ord och räknas tooa avsnitt med namnet `wordcounts`. Hej `wordcounts` avsnittet skapas i ett senare steg i det här avsnittet.

2. Ändra kataloger toohello placeringen hello från kommandoraden i din utvecklingsmiljö hello `Streaming` katalogen och Använd hello efter kommandot toocreate ett jar-paket:

    ```bash
    mvn clean package
    ```

    Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-streaming-1.0-SNAPSHOT.jar`.

3. Använd hello följande kommandon toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` filen tooyour HDInsight-kluster:
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    Ersätt **SSHUSER** med hello SSH-användare för klustret och Ersätt **KLUSTERNAMN** med hello namnet på klustret. När du uppmanas ange hello lösenord för hello SSH-användare.

4. En gång hello `scp` kommandot slutförs kopiera hello-fil, ansluta toohello kluster med SSH och sedan använda följande kommando toocreate hello hello `wordcounts` avsnittet:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. Därefter starta hello strömning processen med hjälp av hello följande kommando:
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    Detta kommando startar hello strömning processen i hello bakgrund.

6. Använd hello följande kommando toosend meddelanden toohello `test` avsnittet. Dessa meddelanden bearbetas av hello strömning exempel:
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. Använd hello följande tooview hello kommandoutdata som skrivs toohello `wordcounts` ämne av hello strömning processen:
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > tooview hello data, måste du se hello konsumenten tooprint hello nyckel och hello funktionen för avserialisering toouse för hello nyckel och värde. hello nyckelnamn är hello word och hello nyckelvärdet innehåller hello count.
   
    hello utdata är liknande toohello följande text:
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > hello antal ökar varje gång ett ord har påträffats.

7. Använd hello __Ctrl + C__ tooexit hello konsumenten och Använd sedan hello `fg` kommandot toobring hello strömmande bakgrund uppgiften tillbaka toohello förgrunden. Använd __Ctrl + C__ tooexit den också.

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Felsöka

Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Nästa steg

I det här dokumentet har du lärt dig hello grunderna i att arbeta med Apache Kafka på HDInsight. Använd följande toolearn mer information om hur du arbetar med Kafka hello:

* [Säkerställ en hög tillgänglighet för dina data med Kafka i HDInsight](hdinsight-apache-kafka-high-availability.md)
* [Öka skalbarheten genom att konfigurera hanterade diskar med Kafka i HDInsight](hdinsight-apache-kafka-scalability.md)
* [Apache Kafka-dokumentation](http://kafka.apache.org/documentation.html) på kafka.apache.org.
* [Använda MirrorMaker toocreate en replik av Kafka på HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Använda Apache Storm med Kafka på HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Använda Apache Spark med Kafka på HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Ansluta tooKafka via ett virtuellt Azure-nätverk](hdinsight-apache-kafka-connect-vpn-gateway.md)
