---
title: "aaaApache Spark strukturerade strömning med Kafka - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse Apache Spark strömmande (DStream) tooget data till eller från Apache Kafka. I det här exemplet strömma data med hjälp av en Jupyter-anteckningsbok från Spark på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/09/2017
ms.author: larryfr
ms.openlocfilehash: 0837e8fc5ea314e644daed029d596feeb2b02c68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a>Använda Spark strukturerade strömning med Kafka (förhandsversion) på HDInsight

Lär dig hur toouse tooread Spark Streaming strukturerade data från Apache Kafka på Azure HDInsight.

Spark strukturerad strömning är en dataström bearbetningsmotor som bygger på Spark SQL. Det gör du tooexpress strömmande beräkningar hello samma som batch beräkningar på statiska data. Mer information om strukturerade strömning finns hello [strukturerade strömning Programmeringsguide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) på Apache.org.

> [!IMPORTANT]
> Det här exemplet används 2.1 Spark på HDInsight 3,6. Strukturerade Streaming anses __alpha__ på Spark 2.1.
>
> hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster. Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Spark-kluster toodirectly kommunicera med hello Kafka klustret.
>
> Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.

## <a name="create-hello-clusters"></a>Skapa hello kluster

Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet. Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret. I det här exemplet finns både hello Kafka och Spark-kluster i Azure-nätverk. hello följande diagram visar hur kommunikation som flödar mellan hello kluster:

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Hej Kafka service är begränsad toocommunication hello virtuellt nätverk. Andra tjänster på hello klustret, hello t.ex. SSH- och Ambari, kan nås över internet. Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Du kan skapa en Azure-nätverk, Kafka och Spark kluster manuellt, men det är enklare toouse en Azure Resource Manager-mall. Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Spark-kluster tooyour Azure-prenumeration.

1. Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Den här mallen skapar hello följande resurser:

    * En Kafka på 3.5 HDInsight-klustret.
    * En Spark på HDInsight 3,6 klustret.
    * Ett Azure Virtual Network, som innehåller hello HDInsight-kluster.

    > [!IMPORTANT]
    > hello strukturerade strömmande anteckningsboken används i det här exemplet kräver Spark i HDInsight 3,6. Om du använder en tidigare version av Spark på HDInsight får du fel när du använder hello anteckningsboken.

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

När hello resurserna har skapats, är du omdirigerade toohello blad för resursgrupp.

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Observera att hello namnen på hello HDInsight-kluster är **spark BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall. Du kan använda dessa namn i senare steg när du ansluter toohello kluster.

## <a name="get-hello-kafka-brokers"></a>Hämta hello Kafka mäklare

hello koden i det här exemplet ansluter toohello Kafka broker värdar i hello Kafka klustret. toofind Hej Kafka broker värdar, Använd följande PowerShell- eller Bash exempel hello:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
$clusterName = Read-Host -Prompt "Enter hello Kafka cluster name"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/KAFKA/components/KAFKA_BROKER" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$brokerHosts = $respObj.host_components.HostRoles.host_name
($brokerHosts -join ":9092,") + ":9092"
```

```bash
curl -u admin:$PASSWORD -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER" | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")'
```

> [!NOTE]
> Det här exemplet förväntar sig `$PASSWORD` toocontain hello lösenordet för hello klusterinloggning och `$CLUSTERNAME` toocontain hello namnet på hello Kafka klustret.
>
> Det här exemplet används hello [jq](https://stedolan.github.io/jq/) verktyget tooparse data utanför hello JSON-dokumentet.

hello utdata är liknande toohello följande text:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Spara den här informationen eftersom den används i följande avsnitt i det här dokumentet hello.

## <a name="get-hello-notebooks"></a>Hämta hello bärbara datorer

hello-koden för hello-exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-hello-notebooks"></a>Överför hello bärbara datorer

Använd följande steg tooupload hello anteckningsböcker från hello projektet tooyour Spark på HDInsight-kluster hello:

1. Anslut toohello Jupyter notebook i Spark-kluster i din webbläsare. Följande URL, Ersätt i hello `CLUSTERNAME` med hello namnet på klustret Kafka:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    När du uppmanas ange hello klusterinloggning (admin) och lösenord som används när du skapade hello.

2. Använd hello från hello övre högra hörnet på sidan hello __överför__ knappen tooupload hello __Stream-Tweets-To_Kafka.ipynb__ toohello kluster. Välj __öppna__ toostart hello överföringen.

    ![Använd hello överför knappen tooselect och ladda upp en bärbar dator](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Välj hello KafkaStreaming.ipynb fil](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Hitta hello __Stream-Tweets-To_Kafka.ipynb__ post i hello lista över datorer och välj __överför__ bredvid den.

    ![Använd hello överför som knappen bredvid hello KafkaStreaming.ipynb post tooupload-den-toohello anteckningsboken server](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Upprepa steg 1 – 3 tooload hello __Spark-strukturerad-Streaming-från-Kafka.ipynb__ bärbar dator.

## <a name="load-tweets-into-kafka"></a>Läs in tweets till Kafka

När hello filer har överförts, välja hello __Stream-Tweets-To_Kafka.ipynb__ post tooopen hello bärbar dator. Åtgärderna hello i hello anteckningsboken tooload tweets till Kafka.

## <a name="process-tweets-using-spark-structured-streaming"></a>Processen tweets med Spark strukturerade strömning

Hello Jupyter-anteckningsbok startsidan, Välj hello __Spark-strukturerad-Streaming-från-Kafka.ipynb__ transaktionen. Åtgärderna hello i hello anteckningsboken tooload tweets från Kafka med Spark strukturerade strömning.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toouse Spark strukturerade strömning Se hello efter dokument toolearn mer om hur du arbetar med Spark och Kafka:

* [Hur toouse Väck strömmande (DStream) med Kafka](hdinsight-apache-spark-with-kafka.md).
* [Börja med Jupyter-anteckningsbok och Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md)