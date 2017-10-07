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
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="b2686-105">Apache Spark streaming (DStream) exempel med Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b2686-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="b2686-106">Lär dig hur toouse Spark Apache Spark toostream data till eller från Apache Kafka på HDInsight med DStreams.</span><span class="sxs-lookup"><span data-stu-id="b2686-106">Learn how toouse Spark Apache Spark toostream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="b2686-107">Det här exemplet används en Jupyter-anteckningsbok som körs på hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-107">This example uses a Jupyter notebook that runs on hello Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="b2686-108">hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-108">hello steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="b2686-109">Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Spark-kluster toodirectly kommunicera med hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="b2686-109">These clusters are both located within an Azure Virtual Network, which allows hello Spark cluster toodirectly communicate with hello Kafka cluster.</span></span>
>
> <span data-ttu-id="b2686-110">Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="b2686-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="b2686-111">Skapa hello kluster</span><span class="sxs-lookup"><span data-stu-id="b2686-111">Create hello clusters</span></span>

<span data-ttu-id="b2686-112">Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="b2686-112">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="b2686-113">Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="b2686-113">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="b2686-114">I det här exemplet finns både hello Kafka och Spark-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="b2686-114">For this example, both hello Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="b2686-115">hello följande diagram visar hur kommunikation som flödar mellan hello kluster:</span><span class="sxs-lookup"><span data-stu-id="b2686-115">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="b2686-117">Även om Kafka själva begränsad toocommunication hello virtuellt nätverk, hello andra tjänster på hello klustret som SSH- och Ambari kan nås över internet.</span><span class="sxs-lookup"><span data-stu-id="b2686-117">Though Kafka itself is limited toocommunication within hello virtual network, other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="b2686-118">Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="b2686-118">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="b2686-119">Du kan skapa en Azure-nätverk, Kafka och Spark kluster manuellt, men det är enklare toouse en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="b2686-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="b2686-120">Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Spark-kluster tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b2686-120">Use hello following steps toodeploy an Azure virtual network, Kafka, and Spark clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="b2686-121">Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2686-121">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
    
    <span data-ttu-id="b2686-122">hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="b2686-122">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="b2686-123">tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="b2686-123">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="b2686-124">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="b2686-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="b2686-125">Den här mallen skapar ett kluster i HDInsight 3,6 för både Kafka och Spark.</span><span class="sxs-lookup"><span data-stu-id="b2686-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="b2686-126">Använd hello efter poster för information om toopopulate hello hello **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="b2686-126">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="b2686-128">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="b2686-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="b2686-129">Den här gruppen innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-129">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="b2686-130">**Plats**: Välj en plats geografiskt nära tooyou.</span><span class="sxs-lookup"><span data-stu-id="b2686-130">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="b2686-131">**Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-131">**Base Cluster Name**: This value is used as hello base name for hello Spark and Kafka clusters.</span></span> <span data-ttu-id="b2686-132">Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="b2686-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="b2686-133">**Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-133">**Cluster Login User Name**: hello admin user name for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="b2686-134">**Klustret inloggningslösenordet**: hello administratörslösenord för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-134">**Cluster Login Password**: hello admin user password for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="b2686-135">**SSH-användarnamn**: hello SSH användaren toocreate för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-135">**SSH User Name**: hello SSH user toocreate for hello Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="b2686-136">**SSH-lösenordet**: hello användarlösenord hello SSH för hello Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-136">**SSH Password**: hello password for hello SSH user for hello Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="b2686-137">Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.</span><span class="sxs-lookup"><span data-stu-id="b2686-137">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="b2686-138">Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="b2686-138">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="b2686-139">Det tar cirka 20 minuter toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-139">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="b2686-140">När hello resurserna har skapats, är du omdirigerade tooa bladet för hello resursgruppen som innehåller hello kluster och web instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b2686-140">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="b2686-142">Observera att hello namnen på hello HDInsight-kluster är **spark BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall.</span><span class="sxs-lookup"><span data-stu-id="b2686-142">Notice that hello names of hello HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="b2686-143">Du kan använda dessa namn i senare steg när du ansluter toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-143">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="use-hello-notebooks"></a><span data-ttu-id="b2686-144">Använd hello bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="b2686-144">Use hello notebooks</span></span>

<span data-ttu-id="b2686-145">hello-koden för hello-exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="b2686-145">hello code for hello example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="b2686-146">Åtgärderna i hello hello `README.md` filen toocomplete det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b2686-146">Follow hello steps in hello `README.md` file toocomplete this example.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="b2686-147">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="b2686-147">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="b2686-148">Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b2686-148">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="b2686-149">Ta bort hello-gruppen tar du bort alla resurser som har skapats genom att följa det här dokumentet, hello Azure Virtual Network och storage-konto som används av hello kluster.</span><span class="sxs-lookup"><span data-stu-id="b2686-149">Deleting hello group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b2686-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2686-150">Next steps</span></span>

<span data-ttu-id="b2686-151">I det här exemplet har vi berättat hur toouse Väck tooread och skriva tooKafka.</span><span class="sxs-lookup"><span data-stu-id="b2686-151">In this example, you learned how toouse Spark tooread and write tooKafka.</span></span> <span data-ttu-id="b2686-152">Använd följande länkar toodiscover hello andra sätt toowork med Kafka:</span><span class="sxs-lookup"><span data-stu-id="b2686-152">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* [<span data-ttu-id="b2686-153">Kom igång med Apache Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b2686-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="b2686-154">Använda MirrorMaker toocreate en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b2686-154">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="b2686-155">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="b2686-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

