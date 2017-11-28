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
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="a4351-104">Använda Spark strukturerade strömning med Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a4351-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="a4351-105">Lär dig hur toouse tooread Spark Streaming strukturerade data från Apache Kafka på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a4351-105">Learn how toouse Spark Structured Streaming tooread data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="a4351-106">Spark strukturerad strömning är en dataström bearbetningsmotor som bygger på Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="a4351-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="a4351-107">Det gör du tooexpress strömmande beräkningar hello samma som batch beräkningar på statiska data.</span><span class="sxs-lookup"><span data-stu-id="a4351-107">It allows you tooexpress streaming computations hello same as batch computation on static data.</span></span> <span data-ttu-id="a4351-108">Mer information om strukturerade strömning finns hello [strukturerade strömning Programmeringsguide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) på Apache.org.</span><span class="sxs-lookup"><span data-stu-id="a4351-108">For more information on Structured Streaming, see hello [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4351-109">Det här exemplet används 2.1 Spark på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="a4351-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="a4351-110">Strukturerade Streaming anses __alpha__ på Spark 2.1.</span><span class="sxs-lookup"><span data-stu-id="a4351-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="a4351-111">hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-111">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="a4351-112">Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Spark-kluster toodirectly kommunicera med hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-112">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="a4351-113">Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a4351-113">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="a4351-114">Skapa hello kluster</span><span class="sxs-lookup"><span data-stu-id="a4351-114">Create hello clusters</span></span>

<span data-ttu-id="a4351-115">Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="a4351-115">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="a4351-116">Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-116">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="a4351-117">I det här exemplet finns både hello Kafka och Spark-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4351-117">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="a4351-118">hello följande diagram visar hur kommunikation som flödar mellan hello kluster:</span><span class="sxs-lookup"><span data-stu-id="a4351-118">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="a4351-120">Hej Kafka service är begränsad toocommunication hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a4351-120">hello Kafka service is limited toocommunication within hello virtual network.</span></span> <span data-ttu-id="a4351-121">Andra tjänster på hello klustret, hello t.ex. SSH- och Ambari, kan nås över internet.</span><span class="sxs-lookup"><span data-stu-id="a4351-121">Other services on hello cluster, such as SSH and Ambari, can be accessed over hello internet.</span></span> <span data-ttu-id="a4351-122">Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="a4351-122">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="a4351-123">Du kan skapa en Azure-nätverk, Kafka och Spark kluster manuellt, men det är enklare toouse en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a4351-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="a4351-124">Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Spark-kluster tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a4351-124">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="a4351-125">Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a4351-125">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="a4351-126">hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="a4351-126">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="a4351-127">Den här mallen skapar hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a4351-127">This template creates hello following resources:</span></span>

    * <span data-ttu-id="a4351-128">En Kafka på 3.5 HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="a4351-129">En Spark på HDInsight 3,6 klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="a4351-130">Ett Azure Virtual Network, som innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-130">An Azure Virtual Network, which contains hello HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a4351-131">hello strukturerade strömmande anteckningsboken används i det här exemplet kräver Spark i HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="a4351-131">hello structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="a4351-132">Om du använder en tidigare version av Spark på HDInsight får du fel när du använder hello anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="a4351-132">If you use an earlier version of Spark on HDInsight, you receive errors when using hello notebook.</span></span>

2. <span data-ttu-id="a4351-133">Använd hello efter poster för information om toopopulate hello hello **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="a4351-133">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="a4351-135">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="a4351-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="a4351-136">Den här gruppen innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-136">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="a4351-137">**Plats**: Välj en plats geografiskt nära tooyou.</span><span class="sxs-lookup"><span data-stu-id="a4351-137">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="a4351-138">**Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-138">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="a4351-139">Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="a4351-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="a4351-140">**Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-140">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="a4351-141">**Klustret inloggningslösenordet**: hello administratörslösenord för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-141">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="a4351-142">**SSH-användarnamn**: hello SSH användaren toocreate för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-142">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="a4351-143">**SSH-lösenordet**: hello användarlösenord hello SSH för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-143">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="a4351-144">Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.</span><span class="sxs-lookup"><span data-stu-id="a4351-144">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="a4351-145">Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="a4351-145">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="a4351-146">Det tar cirka 20 minuter toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-146">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="a4351-147">När hello resurserna har skapats, är du omdirigerade toohello blad för resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a4351-147">Once hello resources have been created, you are redirected toohello resource group blade.</span></span>

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="a4351-149">Observera att hello namnen på hello HDInsight-kluster är **spark BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall.</span><span class="sxs-lookup"><span data-stu-id="a4351-149">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="a4351-150">Du kan använda dessa namn i senare steg när du ansluter toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-150">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="get-hello-kafka-brokers"></a><span data-ttu-id="a4351-151">Hämta hello Kafka mäklare</span><span class="sxs-lookup"><span data-stu-id="a4351-151">Get hello Kafka brokers</span></span>

<span data-ttu-id="a4351-152">hello koden i det här exemplet ansluter toohello Kafka broker värdar i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-152">hello code in this example connects toohello Kafka broker hosts in hello Kafka cluster.</span></span> <span data-ttu-id="a4351-153">toofind Hej Kafka broker värdar, Använd följande PowerShell- eller Bash exempel hello:</span><span class="sxs-lookup"><span data-stu-id="a4351-153">toofind hello Kafka broker hosts, use hello following PowerShell or Bash example:</span></span>

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
> <span data-ttu-id="a4351-154">Det här exemplet förväntar sig `$PASSWORD` toocontain hello lösenordet för hello klusterinloggning och `$CLUSTERNAME` toocontain hello namnet på hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="a4351-154">This example expects `$PASSWORD` toocontain hello password for hello cluster login, and `$CLUSTERNAME` toocontain hello name of hello Kafka cluster.</span></span>
>
> <span data-ttu-id="a4351-155">Det här exemplet används hello [jq](https://stedolan.github.io/jq/) verktyget tooparse data utanför hello JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a4351-155">This example uses hello [jq](https://stedolan.github.io/jq/) utility tooparse data out of hello JSON document.</span></span>

<span data-ttu-id="a4351-156">hello utdata är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="a4351-156">hello output is similar toohello following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="a4351-157">Spara den här informationen eftersom den används i följande avsnitt i det här dokumentet hello.</span><span class="sxs-lookup"><span data-stu-id="a4351-157">Save this information, as it is used in hello following sections of this document.</span></span>

## <a name="get-hello-notebooks"></a><span data-ttu-id="a4351-158">Hämta hello bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="a4351-158">Get hello notebooks</span></span>

<span data-ttu-id="a4351-159">hello-koden för hello-exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="a4351-159">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-hello-notebooks"></a><span data-ttu-id="a4351-160">Överför hello bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="a4351-160">Upload hello notebooks</span></span>

<span data-ttu-id="a4351-161">Använd följande steg tooupload hello anteckningsböcker från hello projektet tooyour Spark på HDInsight-kluster hello:</span><span class="sxs-lookup"><span data-stu-id="a4351-161">Use hello following steps tooupload hello notebooks from hello project tooyour Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="a4351-162">Anslut toohello Jupyter notebook i Spark-kluster i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a4351-162">In your web browser, connect toohello Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="a4351-163">Följande URL, Ersätt i hello `CLUSTERNAME` med hello namnet på klustret Kafka:</span><span class="sxs-lookup"><span data-stu-id="a4351-163">In hello following URL, replace `CLUSTERNAME` with hello name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="a4351-164">När du uppmanas ange hello klusterinloggning (admin) och lösenord som används när du skapade hello.</span><span class="sxs-lookup"><span data-stu-id="a4351-164">When prompted, enter hello cluster login (admin) and password used when you created hello cluster.</span></span>

2. <span data-ttu-id="a4351-165">Använd hello från hello övre högra hörnet på sidan hello __överför__ knappen tooupload hello __Stream-Tweets-To_Kafka.ipynb__ toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="a4351-165">From hello upper right side of hello page, use hello __Upload__ button tooupload hello __Stream-Tweets-To_Kafka.ipynb__ file toohello cluster.</span></span> <span data-ttu-id="a4351-166">Välj __öppna__ toostart hello överföringen.</span><span class="sxs-lookup"><span data-stu-id="a4351-166">Select __Open__ toostart hello upload.</span></span>

    ![Använd hello överför knappen tooselect och ladda upp en bärbar dator](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Välj hello KafkaStreaming.ipynb fil](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="a4351-169">Hitta hello __Stream-Tweets-To_Kafka.ipynb__ post i hello lista över datorer och välj __överför__ bredvid den.</span><span class="sxs-lookup"><span data-stu-id="a4351-169">Find hello __Stream-Tweets-To_Kafka.ipynb__ entry in hello list of notebooks, and select __Upload__ button beside it.</span></span>

    ![Använd hello överför som knappen bredvid hello KafkaStreaming.ipynb post tooupload-den-toohello anteckningsboken server](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="a4351-171">Upprepa steg 1 – 3 tooload hello __Spark-strukturerad-Streaming-från-Kafka.ipynb__ bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="a4351-171">Repeat steps 1-3 tooload hello __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="a4351-172">Läs in tweets till Kafka</span><span class="sxs-lookup"><span data-stu-id="a4351-172">Load tweets into Kafka</span></span>

<span data-ttu-id="a4351-173">När hello filer har överförts, välja hello __Stream-Tweets-To_Kafka.ipynb__ post tooopen hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="a4351-173">Once hello files have been uploaded, select hello __Stream-Tweets-To_Kafka.ipynb__ entry tooopen hello notebook.</span></span> <span data-ttu-id="a4351-174">Åtgärderna hello i hello anteckningsboken tooload tweets till Kafka.</span><span class="sxs-lookup"><span data-stu-id="a4351-174">Follow hello steps in hello notebook tooload tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="a4351-175">Processen tweets med Spark strukturerade strömning</span><span class="sxs-lookup"><span data-stu-id="a4351-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="a4351-176">Hello Jupyter-anteckningsbok startsidan, Välj hello __Spark-strukturerad-Streaming-från-Kafka.ipynb__ transaktionen.</span><span class="sxs-lookup"><span data-stu-id="a4351-176">From hello Jupyter Notebook home page, select hello __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="a4351-177">Åtgärderna hello i hello anteckningsboken tooload tweets från Kafka med Spark strukturerade strömning.</span><span class="sxs-lookup"><span data-stu-id="a4351-177">Follow hello steps in hello notebook tooload tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a4351-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4351-178">Next steps</span></span>

<span data-ttu-id="a4351-179">Nu när du har lärt dig hur toouse Spark strukturerade strömning Se hello efter dokument toolearn mer om hur du arbetar med Spark och Kafka:</span><span class="sxs-lookup"><span data-stu-id="a4351-179">Now that you have learned how toouse Spark Structured Streaming, see hello following documents toolearn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="a4351-180">[Hur toouse Väck strömmande (DStream) med Kafka](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="a4351-180">[How toouse Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="a4351-181">Börja med Jupyter-anteckningsbok och Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="a4351-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)