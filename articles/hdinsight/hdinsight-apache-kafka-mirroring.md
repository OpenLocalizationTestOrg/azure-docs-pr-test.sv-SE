---
title: avsnitt om aaaMirror Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Kafka spegling funktion toomaintain en replik av en Kafka på HDInsight-kluster genom att spegla avsnitt tooa sekundära kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: 5ace0251d7402d4d7d9b28726e253ce7091a87ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a>Använd MirrorMaker tooreplicate Apache Kafka avsnitt med Kafka på HDInsight (förhandsgranskning)

Lär dig hur toouse Apache Kafka spegling funktionen tooreplicate avsnitt tooa sekundära kluster. Spegling kan kördes som en kontinuerlig process eller används ibland som en metod för att migrera data från ett kluster tooanother.

I det här exemplet är spegling används tooreplicate avsnitt mellan två HDInsight-kluster. Båda klustren finns i ett Azure Virtual Network i hello samma region.

> [!WARNING]
> Spegling ska inte ses som ett sätt tooachieve feltolerans. hello offset tooitems inom ett ämne skiljer sig mellan hello käll- och kluster, så att klienter inte kan använda hello två synonymt.
>
> Om du är orolig feltolerans, anger du replikering för hello avsnitt i klustret. Mer information finns i [Kom igång med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md).

## <a name="how-kafka-mirroring-works"></a>Så här fungerar Kafka spegling

Spegling fungerar genom att använda hello MirrorMaker verktyget (del av Apache Kafka) tooconsume poster från avsnitt på hello källklustret och sedan skapa en lokal kopia på hello målklustret. MirrorMaker använder (minst) *konsumenter* som läses från hello källklustret och en *producenten* som skriver toohello lokala (mål) klustret.

hello följande diagram illustrerar processen för spegling av hello:

![Diagram över hello spegling process](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

Apache Kafka på HDInsight ger inte tillgång toohello Kafka service över hello offentliga internet. Kafka producenter och konsumenter måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret. I det här exemplet finns både hello Kafka källa och målkluster i Azure-nätverk. hello följande diagram visar hur kommunikation som flödar mellan hello kluster:

![Diagram över käll- och Kafka-kluster i Azure-nätverk](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

hello käll- och kluster kan vara olika hello antalet noder och partitioner och Offset inom hello avsnitt skiljer sig också. Spegling underhåller hello nyckelvärdet som används för partitionering, så poster ordning bevaras på grundval av per nyckel.

### <a name="mirroring-across-network-boundaries"></a>Spegling över nätverksgränser

Om du behöver toomirror mellan Kafka kluster i olika nätverk, finns följande ytterligare överväganden hello:

* **Gateways**: hello nätverk måste vara kan toocommunicate på hello TCPIP-nivå.

* **Namnmatchning**: hello Kafka kluster i varje nätverk måste vara kan tooconnect tooeach andra med hjälp av värdnamn. Det här kan kräva en Domain Name System (DNS)-server i varje nätverk som är konfigurerad tooforward begäranden toohello andra nätverk.

    När du skapar ett virtuellt Azure-nätverk i stället för med hello nätverket hello automatisk DNS som måste du ange en anpassad DNS-server och hello IP-adress för hello-servern. Efter hello virtuella nätverket har skapats, måste du skapa en virtuell Azure-dator som använder den IP-adressen och sedan installera och konfigurera DNS-programvaran på den.

    > [!WARNING]
    > Skapa och konfigurera hello anpassad DNS-server innan du installerar HDInsight i hello virtuellt nätverk. Det finns ingen ytterligare konfiguration krävs för HDInsight toouse hello DNS-server som konfigurerats för hello virtuellt nätverk.

Mer information om hur du ansluter två virtuella Azure-nätverk finns [konfigurera VNet-till-VNet-anslutningen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).

## <a name="create-kafka-clusters"></a>Skapa Kafka kluster

Du kan skapa ett virtuellt Azure-nätverk och Kafka kluster manuellt, är det enklare toouse en Azure Resource Manager-mall. Använd följande steg toodeploy hello Azure-nätverk och två Kafka kluster tooyour Azure-prenumeration.

1. Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna. Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.

2. Använd hello efter poster för information om toopopulate hello hello **anpassad distribution** bladet:
    
    ![HDInsight anpassad distribution](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * **Resursgruppen**: skapa en grupp eller välj en befintlig. Den här gruppen innehåller hello HDInsight-kluster.

    * **Plats**: Välj en plats geografiskt nära tooyou.
     
    * **Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Kafka kluster. Ange till exempel **hdi** skapar kluster med namnet **källa hdi** och **dest hdi**.

    * **Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello käll- och Kafka-kluster.

    * **Klustret inloggningslösenordet**: hello administratörslösenord för hello käll- och Kafka-kluster.

    * **SSH-användarnamn**: hello SSH användaren toocreate för hello käll- och Kafka-kluster.

    * **SSH-lösenordet**: hello användarlösenord hello SSH för hello käll- och Kafka-kluster.

3. Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.

4. Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**. Det tar cirka 20 minuter toocreate hello kluster.

När hello resurserna har skapats, är du omdirigerade tooa bladet för hello resursgruppen som innehåller hello kluster och web instrumentpanelen.

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> Observera att hello namnen på hello HDInsight-kluster är **källa BASENAME** och **dest BASENAME**, där BASENAME är hello namn du angett toohello mall. Du kan använda dessa namn i senare steg när du ansluter toohello kluster.

## <a name="create-topics"></a>Skapa avsnitt

1. Ansluta toohello **källa** kluster med SSH:

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    Ersätt **sshuser** med hello SSH-användarnamn används när du skapar hello kluster. Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd hello följande kommandon toofind hello Zookeeper-värdar för hello källklustret:

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get hello zookeeper hosts for hello source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with hello password for hello cluster.

    Replace `$CLUSTERNAME` with hello name of hello source cluster.

3. toocreate a topic named `testtopic`, use hello following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. Använd hello efter kommandot tooverify som hello avsnittet skapades:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    hello svaret innehåller `testtopic`.

4. Använd hello följande tooview hello Zookeeper värdinformation för detta (hello **källa**) klustret:

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    Detta returnerar information liknande toohello följande text:

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    Spara den här informationen. Den används i nästa avsnitt om hello.

## <a name="configure-mirroring"></a>Konfigurera spegling

1. Ansluta toohello **mål** kluster med hjälp av en annan SSH-session:

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    Ersätt **sshuser** med hello SSH-användarnamn används när du skapar hello kluster. Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.

    Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Använd hello följande kommando toocreate en `consumer.properties` fil som beskriver hur toocommunicate med hello **källa** klustret:

    ```bash
    nano consumer.properties
    ```

    Använd hello följande text som hello hello `consumer.properties` fil:

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    Ersätt **SOURCE_ZKHOSTS** med hello Zookeeper värd information från hello **källa** klustret.

    Den här filen beskriver hello konsumenten information toouse vid läsning från hello källa Kafka klustret. Mer information konsumenten konfiguration finns i [konsumenten konfigurationerna](https://kafka.apache.org/documentation#consumerconfigs) på kafka.apache.org.

    toosave hello-fil, Använd **Ctrl + X**, **Y**, och sedan **RETUR**.

3. Innan du konfigurerar hello producenten som kommunicerar med hello målklustret du hitta hello broker värdar för hello **mål** klustret. Använd följande kommandon tooretrieve hello informationen:

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    Ersätt `$PASSWORD` med hello inloggning (admin) kontolösenord för hello-kluster.

    Ersätt `$CLUSTERNAME` med hello namnet hello målklustret.

    Dessa kommandon returnera information liknande toohello följande:

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. Använd hello följande toocreate en `producer.properties` fil som beskriver hur toocommunicate med hello **mål** klustret:

    ```bash
    nano producer.properties
    ```

    Använd hello följande text som hello hello `producer.properties` fil:

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    Ersätt **DEST_BROKERS** med hello broker information från hello föregående steg.

    Mer information producenten konfiguration finns i [producenten konfigurationerna](https://kafka.apache.org/documentation#producerconfigs) på kafka.apache.org.

## <a name="start-mirrormaker"></a>Starta MirrorMaker

1. Från hello SSH-anslutning toohello **mål** bör du använda följande kommando toostart hello MirrorMaker processen hello:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    hello-parametrar som används i det här exemplet är:

    * **--consumer.config**: Anger hello-fil som innehåller konsumenten egenskaper. Dessa egenskaper är används toocreate konsumenter som läser från hello *källa* Kafka klustret.

    * **--producer.config**: Anger hello-fil som innehåller producenten egenskaper. Dessa egenskaper är används toocreate producent som skriver toohello *mål* Kafka klustret.

    * **--godkända**: en lista över ämnen som MirrorMaker replikerar från hello källa klustret toohello mål.

    * **--num.streams**: hello antalet konsumenten trådar toocreate.

 Vid start returnerar MirrorMaker information liknande toohello följande text:

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. Från hello SSH-anslutning toohello **källa** kluster kan använda följande kommando toostart producent hello och skicka meddelanden toohello avsnittet:

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    Ersätt `$PASSWORD` med hello inloggning (admin) lösenord för hello källa kluster.

    Ersätt `$CLUSTERNAME` med hello hello källa klustrets namn.

     När du kommer till en tom rad med en markör Skriv i några textmeddelanden. Dessa skickas toohello avsnittet på hello **källa** klustret. När du är klar kan du använda **Ctrl + C** tooend hello producenten processen.

3. Från hello SSH-anslutning toohello **mål** klustret använder **Ctrl + C** tooend hello MirrorMaker process. Och sedan använda hello följande kommandon tooverify som hello `testtopic` artikeln skapades och informationen i avsnittet hello var replikerade toothis spegling:

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    Ersätt `$PASSWORD` med hello inloggning (admin) lösenord för hello målklustret.

    Ersätt `$CLUSTERNAME` med hello namnet hello målklustret.

    hello lista över ämnen innehåller nu `testtopic`, som skapas när MirrorMaster speglar hello avsnittet från hello källa klustret toohello mål. hälsningsmeddelande som hämtats från hello-avsnittet är hello samma som har angetts på hello källa klustret.

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen. Ta bort hello resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet, hello Azure Virtual Network och storage-konto som används av hello kluster.

## <a name="next-steps"></a>Nästa steg

I det här dokumentet du har lärt dig hur toouse MirrorMaker toocreate en replik av en Kafka kluster. Använd följande länkar toodiscover hello andra sätt toowork med Kafka:

* [Apache Kafka MirrorMaker dokumentationen](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) på cwiki.apache.org.
* [Kom igång med Apache Kafka på HDInsight](hdinsight-apache-kafka-get-started.md)
* [Använda Apache Spark med Kafka på HDInsight](hdinsight-apache-spark-with-kafka.md)
* [Använda Apache Storm med Kafka på HDInsight](hdinsight-apache-storm-with-kafka.md)
* [Ansluta tooKafka via ett virtuellt Azure-nätverk](hdinsight-apache-kafka-connect-vpn-gateway.md)
