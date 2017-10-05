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
# <a name="use-spark-structured-streaming-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="faf70-104">Använda Spark strukturerade strömning med Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf70-104">Use Spark Structured Streaming with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="faf70-105">Lär dig hur du använder Spark strukturerade strömning för att läsa data från Apache Kafka på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="faf70-105">Learn how to use Spark Structured Streaming to read data from Apache Kafka on Azure HDInsight.</span></span>

<span data-ttu-id="faf70-106">Spark strukturerad strömning är en dataström bearbetningsmotor som bygger på Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="faf70-106">Spark structured streaming is a stream processing engine built on Spark SQL.</span></span> <span data-ttu-id="faf70-107">Det kan du express strömmande beräkningar samma som batch beräkningar på statiska data.</span><span class="sxs-lookup"><span data-stu-id="faf70-107">It allows you to express streaming computations the same as batch computation on static data.</span></span> <span data-ttu-id="faf70-108">Läs mer strukturerade direktuppspelning av [strukturerade strömning Programmeringsguide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) på Apache.org.</span><span class="sxs-lookup"><span data-stu-id="faf70-108">For more information on Structured Streaming, see the [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.1.0/structured-streaming-programming-guide.html) at Apache.org.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="faf70-109">Det här exemplet används 2.1 Spark på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="faf70-109">This example used Spark 2.1 on HDInsight 3.6.</span></span> <span data-ttu-id="faf70-110">Strukturerade Streaming anses __alpha__ på Spark 2.1.</span><span class="sxs-lookup"><span data-stu-id="faf70-110">Structured Streaming is considered __alpha__ on Spark 2.1.</span></span>
>
> <span data-ttu-id="faf70-111">Stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-111">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="faf70-112">Dessa kluster finns både i ett Azure Virtual Network, vilket innebär att Spark-klustret kan kommunicera direkt med Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-112">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="faf70-113">Kom ihåg att ta bort kluster för att undvika överdriven avgifter när du är klar med steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="faf70-113">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="faf70-114">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="faf70-114">Create the clusters</span></span>

<span data-ttu-id="faf70-115">Apache Kafka på HDInsight ger inte tillgång till Kafka mäklare via det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="faf70-115">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="faf70-116">Allt som kommunicerar Kafka måste finnas i samma virtuella Azure-nätverket som noder i klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="faf70-116">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="faf70-117">I det här exemplet finns både Kafka och Spark-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="faf70-117">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="faf70-118">Följande diagram visar hur kommunikation som flödar mellan kluster:</span><span class="sxs-lookup"><span data-stu-id="faf70-118">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="faf70-120">Tjänsten Kafka är begränsad till kommunikation inom det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="faf70-120">The Kafka service is limited to communication within the virtual network.</span></span> <span data-ttu-id="faf70-121">Andra tjänster på klustret, till exempel SSH och Ambari, kan nås via internet.</span><span class="sxs-lookup"><span data-stu-id="faf70-121">Other services on the cluster, such as SSH and Ambari, can be accessed over the internet.</span></span> <span data-ttu-id="faf70-122">Mer information om de offentliga portarna som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="faf70-122">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="faf70-123">Du kan skapa en Azure-nätverk, Kafka, och Spark-kluster manuellt, men det är enklare att använda en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="faf70-123">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="faf70-124">Använd följande steg för att distribuera ett Azure-nätverk, Kafka, och vidtar kluster på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="faf70-124">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="faf70-125">Använd knappen följande för att logga in på Azure och öppna mallen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="faf70-125">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="faf70-126">Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span><span class="sxs-lookup"><span data-stu-id="faf70-126">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.</span></span>

    <span data-ttu-id="faf70-127">Den här mallen skapas i följande resurser:</span><span class="sxs-lookup"><span data-stu-id="faf70-127">This template creates the following resources:</span></span>

    * <span data-ttu-id="faf70-128">En Kafka på 3.5 HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="faf70-128">A Kafka on HDInsight 3.5 cluster.</span></span>
    * <span data-ttu-id="faf70-129">En Spark på HDInsight 3,6 klustret.</span><span class="sxs-lookup"><span data-stu-id="faf70-129">A Spark on HDInsight 3.6 cluster.</span></span>
    * <span data-ttu-id="faf70-130">Ett Azure Virtual Network, som innehåller HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-130">An Azure Virtual Network, which contains the HDInsight clusters.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="faf70-131">Strukturerade strömmande anteckningsboken som används i det här exemplet kräver Spark i HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="faf70-131">The structured streaming notebook used in this example requires Spark on HDInsight 3.6.</span></span> <span data-ttu-id="faf70-132">Om du använder en tidigare version av Spark på HDInsight får du fel när du använder den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="faf70-132">If you use an earlier version of Spark on HDInsight, you receive errors when using the notebook.</span></span>

2. <span data-ttu-id="faf70-133">Använd följande information för att fylla i posterna på den **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="faf70-133">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="faf70-135">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="faf70-135">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="faf70-136">Den här gruppen innehåller HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="faf70-136">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="faf70-137">**Plats**: Välj en plats geografiskt nära dig.</span><span class="sxs-lookup"><span data-stu-id="faf70-137">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="faf70-138">**Basera klusternamnet**: det här värdet används som det grundläggande namnet på Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-138">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="faf70-139">Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="faf70-139">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="faf70-140">**Klustrets inloggningsnamn**: admin användarnamn för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-140">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="faf70-141">**Klustret inloggningslösenordet**: administratörslösenord för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-141">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="faf70-142">**SSH-användarnamn**: SSH-användare skapa för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-142">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="faf70-143">**SSH-lösenordet**: lösenord för SSH-användare för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-143">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="faf70-144">Läs den **villkor**, och välj sedan **jag samtycker till villkoren som anges ovan**.</span><span class="sxs-lookup"><span data-stu-id="faf70-144">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="faf70-145">Kontrollera slutligen **fäst på instrumentpanelen** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="faf70-145">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="faf70-146">Det tar ungefär 20 minuter för att skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-146">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="faf70-147">När resurserna som har skapats, omdirigeras du till bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="faf70-147">Once the resources have been created, you are redirected to the resource group blade.</span></span>

![Blad för resursgrupp för virtuella nätverk och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="faf70-149">Lägg märke till att namnen på HDInsight-kluster **spark BASENAME** och **kafka BASENAME**, där BASENAME är det namn du angav i mallen.</span><span class="sxs-lookup"><span data-stu-id="faf70-149">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="faf70-150">Du kan använda dessa namn i senare steg när du ansluter till kluster.</span><span class="sxs-lookup"><span data-stu-id="faf70-150">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="get-the-kafka-brokers"></a><span data-ttu-id="faf70-151">Hämta Kafka mäklare</span><span class="sxs-lookup"><span data-stu-id="faf70-151">Get the Kafka brokers</span></span>

<span data-ttu-id="faf70-152">Koden i det här exemplet ansluter till Kafka broker värdar i klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="faf70-152">The code in this example connects to the Kafka broker hosts in the Kafka cluster.</span></span> <span data-ttu-id="faf70-153">Använd följande PowerShell- eller Bash-exempel för att hitta Kafka broker värdar:</span><span class="sxs-lookup"><span data-stu-id="faf70-153">To find the Kafka broker hosts, use the following PowerShell or Bash example:</span></span>

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
> <span data-ttu-id="faf70-154">Det här exemplet förväntar sig `$PASSWORD` som innehåller lösenordet för inloggningen kluster och `$CLUSTERNAME` som innehåller namnet på klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="faf70-154">This example expects `$PASSWORD` to contain the password for the cluster login, and `$CLUSTERNAME` to contain the name of the Kafka cluster.</span></span>
>
> <span data-ttu-id="faf70-155">Det här exemplet används den [jq](https://stedolan.github.io/jq/) verktyg för att analysera data från JSON-dokumentet.</span><span class="sxs-lookup"><span data-stu-id="faf70-155">This example uses the [jq](https://stedolan.github.io/jq/) utility to parse data out of the JSON document.</span></span>

<span data-ttu-id="faf70-156">De utdata som genereras liknar följande text:</span><span class="sxs-lookup"><span data-stu-id="faf70-156">The output is similar to the following text:</span></span>

`wn0-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn1-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn2-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092,wn3-kafka.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net:9092`

<span data-ttu-id="faf70-157">Spara den här informationen eftersom den används i följande avsnitt i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="faf70-157">Save this information, as it is used in the following sections of this document.</span></span>

## <a name="get-the-notebooks"></a><span data-ttu-id="faf70-158">Hämta de bärbara datorerna</span><span class="sxs-lookup"><span data-stu-id="faf70-158">Get the notebooks</span></span>

<span data-ttu-id="faf70-159">Koden för det exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span><span class="sxs-lookup"><span data-stu-id="faf70-159">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).</span></span>

## <a name="upload-the-notebooks"></a><span data-ttu-id="faf70-160">Överför de bärbara datorerna</span><span class="sxs-lookup"><span data-stu-id="faf70-160">Upload the notebooks</span></span>

<span data-ttu-id="faf70-161">Använd följande steg för att överföra anteckningsböcker från projektet till din Spark på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="faf70-161">Use the following steps to upload the notebooks from the project to your Spark on HDInsight cluster:</span></span>

1. <span data-ttu-id="faf70-162">Anslut till Jupyter notebook i Spark-kluster i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="faf70-162">In your web browser, connect to the Jupyter notebook on your Spark cluster.</span></span> <span data-ttu-id="faf70-163">I följande URL, ersätter `CLUSTERNAME` med namnet på klustret Kafka:</span><span class="sxs-lookup"><span data-stu-id="faf70-163">In the following URL, replace `CLUSTERNAME` with the name of your Kafka cluster:</span></span>

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    <span data-ttu-id="faf70-164">När du uppmanas ange klusterinloggning (admin) och lösenord som används när du skapade klustret.</span><span class="sxs-lookup"><span data-stu-id="faf70-164">When prompted, enter the cluster login (admin) and password used when you created the cluster.</span></span>

2. <span data-ttu-id="faf70-165">Använd från det övre högra hörnet på sidan, den __överför__ för att överföra den __Stream-Tweets-To_Kafka.ipynb__ filen till klustret.</span><span class="sxs-lookup"><span data-stu-id="faf70-165">From the upper right side of the page, use the __Upload__ button to upload the __Stream-Tweets-To_Kafka.ipynb__ file to the cluster.</span></span> <span data-ttu-id="faf70-166">Välj __öppna__ så startas guiden Överför.</span><span class="sxs-lookup"><span data-stu-id="faf70-166">Select __Open__ to start the upload.</span></span>

    ![Använd knappen Skicka för att välja och ladda upp en bärbar dator](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Markera filen KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. <span data-ttu-id="faf70-169">Hitta de __Stream-Tweets-To_Kafka.ipynb__ post i listan över anteckningsböcker och välj __överför__ bredvid den.</span><span class="sxs-lookup"><span data-stu-id="faf70-169">Find the __Stream-Tweets-To_Kafka.ipynb__ entry in the list of notebooks, and select __Upload__ button beside it.</span></span>

    ![Använd knappen Överför bredvid posten KafkaStreaming.ipynb för att överföra den till servern anteckningsboken](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)

4. <span data-ttu-id="faf70-171">Upprepa steg 1 – 3 för att läsa in den __Spark-strukturerad-Streaming-från-Kafka.ipynb__ bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="faf70-171">Repeat steps 1-3 to load the __Spark-Structured-Streaming-From-Kafka.ipynb__ notebook.</span></span>

## <a name="load-tweets-into-kafka"></a><span data-ttu-id="faf70-172">Läs in tweets till Kafka</span><span class="sxs-lookup"><span data-stu-id="faf70-172">Load tweets into Kafka</span></span>

<span data-ttu-id="faf70-173">När filerna har överförts, välja den __Stream-Tweets-To_Kafka.ipynb__ post för att öppna den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="faf70-173">Once the files have been uploaded, select the __Stream-Tweets-To_Kafka.ipynb__ entry to open the notebook.</span></span> <span data-ttu-id="faf70-174">Följ stegen i den bärbara datorn att läsa in tweets till Kafka.</span><span class="sxs-lookup"><span data-stu-id="faf70-174">Follow the steps in the notebook to load tweets into Kafka.</span></span>

## <a name="process-tweets-using-spark-structured-streaming"></a><span data-ttu-id="faf70-175">Processen tweets med Spark strukturerade strömning</span><span class="sxs-lookup"><span data-stu-id="faf70-175">Process tweets using Spark Structured Streaming</span></span>

<span data-ttu-id="faf70-176">Sidan Jupyter-anteckningsbok, Välj den __Spark-strukturerad-Streaming-från-Kafka.ipynb__ transaktionen.</span><span class="sxs-lookup"><span data-stu-id="faf70-176">From the Jupyter Notebook home page, select the __Spark-Structured-Streaming-From-Kafka.ipynb__ entry.</span></span> <span data-ttu-id="faf70-177">Följ stegen i den bärbara datorn att läsa in tweets från Kafka med Spark strukturerade strömning.</span><span class="sxs-lookup"><span data-stu-id="faf70-177">Follow the steps in the notebook to load tweets from Kafka using Spark Structured Streaming.</span></span>

## <a name="next-steps"></a><span data-ttu-id="faf70-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="faf70-178">Next steps</span></span>

<span data-ttu-id="faf70-179">Nu när du har lärt dig hur du använder Spark strukturerade strömning, finns i följande dokument vill lära dig mer om hur du arbetar med Spark och Kafka:</span><span class="sxs-lookup"><span data-stu-id="faf70-179">Now that you have learned how to use Spark Structured Streaming, see the following documents to learn more about working with Spark and Kafka:</span></span>

* <span data-ttu-id="faf70-180">[Hur du använder Spark streaming (DStream) med Kafka](hdinsight-apache-spark-with-kafka.md).</span><span class="sxs-lookup"><span data-stu-id="faf70-180">[How to use Spark streaming (DStream) with Kafka](hdinsight-apache-spark-with-kafka.md).</span></span>
* [<span data-ttu-id="faf70-181">Börja med Jupyter-anteckningsbok och Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="faf70-181">Start with Jupyter Notebook and Spark on HDInsight</span></span>](hdinsight-apache-spark-jupyter-spark-sql.md)