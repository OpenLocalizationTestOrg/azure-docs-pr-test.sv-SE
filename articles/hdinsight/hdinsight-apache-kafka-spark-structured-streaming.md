---
title: "Apache Väck strukturerade strömning med Kafka - Azure HDInsight | Microsoft Docs"
description: "Lär dig använda Apache Spark streaming (DStream) för att hämta data till eller från Apache Kafka. I det här exemplet strömma data med hjälp av en Jupyter-anteckningsbok från Spark på HDInsight."
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
ms.openlocfilehash: 02b49e13e8f54c3d55310f4d2b21c7e09c91fe81
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a>Använda Spark strukturerade strömning med Kafka (förhandsversion) på HDInsight

Lär dig hur du använder Spark strukturerade strömning för att läsa data från Apache Kafka på Azure HDInsight.

Spark strukturerad strömning är en dataström bearbetningsmotor som bygger på Spark SQL. Det kan du express strömmande beräkningar samma som batch beräkningar på statiska data. Läs mer strukturerade direktuppspelning av [strukturerade strömning Programmeringsguide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) på Apache.org.

> [!IMPORTANT]
> Det här exemplet används 2.1 Spark på HDInsight 3,6. Strukturerade Streaming anses __alpha__ på Spark 2.1.
>
> Stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster. Dessa kluster finns både i ett Azure Virtual Network, vilket innebär att Spark-klustret kan kommunicera direkt med Kafka kluster.
>
> Kom ihåg att ta bort kluster för att undvika överdriven avgifter när du är klar med steg i det här dokumentet.

## <a name="create-the-clusters"></a>Skapa kluster

Apache Kafka på HDInsight ger inte tillgång till Kafka mäklare via det offentliga internet. Allt som kommunicerar Kafka måste finnas i samma virtuella Azure-nätverket som noder i klustret Kafka. I det här exemplet finns både Kafka och Spark-kluster i Azure-nätverk. Följande diagram visar hur kommunikation som flödar mellan kluster:

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Tjänsten Kafka är begränsad till kommunikation inom det virtuella nätverket. Andra tjänster på klustret, till exempel SSH och Ambari, kan nås via internet. Mer information om de offentliga portarna som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).

Du kan skapa en Azure-nätverk, Kafka, och Spark-kluster manuellt, men det är enklare att använda en Azure Resource Manager-mall. Använd följande steg för att distribuera ett Azure-nätverk, Kafka, och vidtar kluster på Azure-prenumerationen.

1. Använd knappen följande för att logga in på Azure och öppna mallen i Azure-portalen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    Den här mallen skapas i följande resurser:

    * En Kafka på 3.5 HDInsight-klustret.
    * En Spark på HDInsight 3,6 klustret.
    * Ett Azure Virtual Network, som innehåller HDInsight-kluster.

    > [!IMPORTANT]
    > Strukturerade strömmande anteckningsboken som används i det här exemplet kräver Spark i HDInsight 3,6. Om du använder en tidigare version av Spark på HDInsight får du fel när du använder den bärbara datorn.

2. Använd följande information för att fylla i posterna på den **anpassad distribution** bladet:
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Resursgruppen**: skapa en grupp eller välj en befintlig. Den här gruppen innehåller HDInsight-klustret.

    * **Plats**: Välj en plats geografiskt nära dig.

    * **Basera klusternamnet**: det här värdet används som det grundläggande namnet på Spark och Kafka kluster. Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.

    * **Klustrets inloggningsnamn**: admin användarnamn för Spark och Kafka kluster.

    * **Klustret inloggningslösenordet**: administratörslösenord för Spark och Kafka kluster.

    * **SSH-användarnamn**: SSH-användare skapa för Spark och Kafka kluster.

    * **SSH-lösenordet**: lösenord för SSH-användare för Spark och Kafka kluster.

3. Läs den **villkor**, och välj sedan **jag samtycker till villkoren som anges ovan**.

4. Kontrollera slutligen **fäst på instrumentpanelen** och välj sedan **inköp**. Det tar ungefär 20 minuter för att skapa kluster.

När resurserna som har skapats, omdirigeras du till bladet för resursgruppen.

![Blad för resursgrupp för virtuella nätverk och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Lägg märke till att namnen på HDInsight-kluster **spark BASENAME** och **kafka BASENAME**, där BASENAME är det namn du angav i mallen. Du kan använda dessa namn i senare steg när du ansluter till kluster.

## <a name="get-the-kafka-brokers"></a>Hämta Kafka mäklare

Koden i det här exemplet ansluter till Kafka broker värdar i klustret Kafka. Använd följande PowerShell- eller Bash-exempel för att hitta Kafka broker värdar:

```powershell
$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
$clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
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
> Det här exemplet förväntar sig `$PASSWORD` som innehåller lösenordet för inloggningen kluster och `$CLUSTERNAME` som innehåller namnet på klustret Kafka.
>
> Det här exemplet används den [jq](https://stedolan.github.io/jq/) verktyg för att analysera data från JSON-dokumentet.

De utdata som genereras liknar följande text:

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

Spara den här informationen eftersom den används i följande avsnitt i det här dokumentet.

## <a name="get-the-notebooks"></a>Hämta de bärbara datorerna

Koden för det exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>Överför de bärbara datorerna

Använd följande steg för att överföra anteckningsböcker från projektet till din Spark på HDInsight-kluster:

1. Anslut till Jupyter notebook i Spark-kluster i din webbläsare. I följande URL, ersätter `CLUSTERNAME` med namnet på klustret Kafka:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    När du uppmanas ange klusterinloggning (admin) och lösenord som används när du skapade klustret.

2. Använd från det övre högra hörnet på sidan, den __överför__ för att överföra den __Stream-Tweets-To_Kafka.ipynb__ filen till klustret. Välj __öppna__ så startas guiden Överför.

    ![Använd knappen Skicka för att välja och ladda upp en bärbar dator](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Markera filen KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Hitta de __Stream-Tweets-To_Kafka.ipynb__ post i listan över anteckningsböcker och välj __överför__ bredvid den.

    ![Använd knappen Överför bredvid posten KafkaStreaming.ipynb för att överföra den till servern anteckningsboken](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. Upprepa steg 1 – 3 för att läsa in den __Spark-strukturerad-Streaming-från-Kafka.ipynb__ bärbar dator.

## <a name="load-tweets-into-kafka"></a>Läs in tweets till Kafka

När filerna har överförts, välja den __Stream-Tweets-To_Kafka.ipynb__ post för att öppna den bärbara datorn. Följ stegen i den bärbara datorn att läsa in tweets till Kafka.

## <a name="process-tweets-using-spark-structured-streaming"></a>Processen tweets med Spark strukturerade strömning

Sidan Jupyter-anteckningsbok, Välj den __Spark-strukturerad-Streaming-från-Kafka.ipynb__ transaktionen. Följ stegen i den bärbara datorn att läsa in tweets från Kafka med Spark strukturerade strömning.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du använder Spark strukturerade strömning, finns i följande dokument vill lära dig mer om hur du arbetar med Spark och Kafka:

* [Hur du använder Spark streaming (DStream) med Kafka](hdinsight-apache-spark-with-kafka.md).
* [Börja med Jupyter-anteckningsbok och Spark i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md)