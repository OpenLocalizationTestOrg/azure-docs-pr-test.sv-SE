---
title: aaaApache Spark streaming med Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Spark Apache Spark toostream data till eller från Apache Kafka med DStreams. I det här exemplet strömma data med hjälp av en Jupyter-anteckningsbok från Spark på HDInsight."
keywords: "kafka exempel kafka zookeeper Väck kafka, spark streaming kafka exempel för strömning"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: f48b37aadafa4979cd27af68e8417db6acc8a0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a>Apache Spark streaming (DStream) exempel med Kafka (förhandsversion) på HDInsight

Lär dig hur toouse Spark Apache Spark toostream data till eller från Apache Kafka på HDInsight med DStreams. Det här exemplet används en Jupyter-anteckningsbok som körs på hello Spark-kluster.
> [!NOTE]
> hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster. Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Spark-kluster toodirectly kommunicera med hello Kafka klustret.
>
> Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.

## <a name="create-hello-clusters"></a>Skapa hello kluster

Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet. Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret. I det här exemplet finns både hello Kafka och Spark-kluster i Azure-nätverk. hello följande diagram visar hur kommunikation som flödar mellan hello kluster:

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Även om Kafka själva begränsad toocommunication hello virtuellt nätverk, hello andra tjänster på hello klustret som SSH- och Ambari kan nås över internet. Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Du kan skapa en Azure-nätverk, Kafka och Spark kluster manuellt, men det är enklare toouse en Azure Resource Manager-mall. Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Spark-kluster tooyour Azure-prenumeration.

1. Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.

    > [!WARNING]
    > tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna. Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.

    Den här mallen skapar ett kluster i HDInsight 3,6 för både Kafka och Spark.

2. Använd hello efter poster för information om toopopulate hello hello **anpassad distribution** bladet:
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Resursgruppen**: skapa en grupp eller välj en befintlig. Den här gruppen innehåller hello HDInsight-kluster.

    * **Plats**: Välj en plats geografiskt nära tooyou.

    * **Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Spark och Kafka kluster. Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.

    * **Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Spark och Kafka kluster.

    * **Klustret inloggningslösenordet**: hello administratörslösenord för hello Spark och Kafka kluster.

    * **SSH-användarnamn**: hello SSH användaren toocreate för hello Spark och Kafka kluster.

    * **SSH-lösenordet**: hello användarlösenord hello SSH för hello Spark och Kafka kluster.

3. Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.

4. Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**. Det tar cirka 20 minuter toocreate hello kluster.

När hello resurserna har skapats, är du omdirigerade tooa bladet för hello resursgruppen som innehåller hello kluster och web instrumentpanelen.

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Observera att hello namnen på hello HDInsight-kluster är **spark BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall. Du kan använda dessa namn i senare steg när du ansluter toohello kluster.

## <a name="use-hello-notebooks"></a>Använd hello bärbara datorer

hello-koden för hello-exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Åtgärderna i hello hello `README.md` filen toocomplete det här exemplet.

## <a name="delete-hello-cluster"></a>Ta bort hello kluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen. Ta bort hello-gruppen tar du bort alla resurser som har skapats genom att följa det här dokumentet, hello Azure Virtual Network och storage-konto som används av hello kluster.

## <a name="next-steps"></a>Nästa steg

I det här exemplet har vi berättat hur toouse Väck tooread och skriva tooKafka. Använd följande länkar toodiscover hello andra sätt toowork med Kafka:

* [Kom igång med Apache Kafka på HDInsight](hdinsight-apache-kafka-get-started.md)
* [Använda MirrorMaker toocreate en replik av Kafka på HDInsight](hdinsight-apache-kafka-mirroring.md)
* [Använda Apache Storm med Kafka på HDInsight](hdinsight-apache-storm-with-kafka.md)

