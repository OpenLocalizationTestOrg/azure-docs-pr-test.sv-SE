---
title: Apache Spark streaming med Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur du använder för att sända data till eller från Apache Kafka med DStreams Spark Apache Spark. I det här exemplet strömma data med hjälp av en Jupyter-anteckningsbok från Spark på HDInsight."
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
ms.openlocfilehash: 81fa319f6fb94bdabacd8f68d14b9a1063a9749a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-preview-on-hdinsight"></a><span data-ttu-id="93553-105">Apache Spark streaming (DStream) exempel med Kafka (förhandsversion) på HDInsight</span><span class="sxs-lookup"><span data-stu-id="93553-105">Apache Spark streaming (DStream) example with Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="93553-106">Lär dig hur du använder Spark Apache Spark att sända data till eller från Apache Kafka på HDInsight med DStreams.</span><span class="sxs-lookup"><span data-stu-id="93553-106">Learn how to use Spark Apache Spark to stream data into or out of Apache Kafka on HDInsight using DStreams.</span></span> <span data-ttu-id="93553-107">Det här exemplet används en Jupyter-anteckningsbok som körs på Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="93553-107">This example uses a Jupyter notebook that runs on the Spark cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="93553-108">Stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både en Spark i HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-108">The steps in this document create an Azure resource group that contains both a Spark on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="93553-109">Dessa kluster finns både i ett Azure Virtual Network, vilket innebär att Spark-klustret kan kommunicera direkt med Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-109">These clusters are both located within an Azure Virtual Network, which allows the Spark cluster to directly communicate with the Kafka cluster.</span></span>
>
> <span data-ttu-id="93553-110">Kom ihåg att ta bort kluster för att undvika överdriven avgifter när du är klar med steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="93553-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="93553-111">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="93553-111">Create the clusters</span></span>

<span data-ttu-id="93553-112">Apache Kafka på HDInsight ger inte tillgång till Kafka mäklare via det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="93553-112">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="93553-113">Allt som kommunicerar Kafka måste finnas i samma virtuella Azure-nätverket som noder i klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="93553-113">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="93553-114">I det här exemplet finns både Kafka och Spark-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="93553-114">For this example, both the Kafka and Spark clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="93553-115">Följande diagram visar hur kommunikation som flödar mellan kluster:</span><span class="sxs-lookup"><span data-stu-id="93553-115">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram över Spark och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="93553-117">Även om Kafka själva är begränsad till kommunikation inom det virtuella nätverket, kan andra tjänster på klustret, till exempel SSH och Ambari nås via internet.</span><span class="sxs-lookup"><span data-stu-id="93553-117">Though Kafka itself is limited to communication within the virtual network, other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="93553-118">Mer information om de offentliga portarna som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="93553-118">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="93553-119">Du kan skapa en Azure-nätverk, Kafka, och Spark-kluster manuellt, men det är enklare att använda en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="93553-119">While you can create an Azure virtual network, Kafka, and Spark clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="93553-120">Använd följande steg för att distribuera ett Azure-nätverk, Kafka, och vidtar kluster på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="93553-120">Use the following steps to deploy an Azure virtual network, Kafka, and Spark clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="93553-121">Använd knappen följande för att logga in på Azure och öppna mallen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93553-121">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    <span data-ttu-id="93553-122">Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="93553-122">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="93553-123">Klustret måste innehålla minst tre arbetsnoder för att garantera tillgängligheten för Kafka i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="93553-123">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="93553-124">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="93553-124">This template creates a Kafka cluster that contains three worker nodes.</span></span>

    <span data-ttu-id="93553-125">Den här mallen skapar ett kluster i HDInsight 3,6 för både Kafka och Spark.</span><span class="sxs-lookup"><span data-stu-id="93553-125">This template creates an HDInsight 3.6 cluster for both Kafka and Spark.</span></span>

2. <span data-ttu-id="93553-126">Använd följande information för att fylla i posterna på den **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="93553-126">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * <span data-ttu-id="93553-128">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="93553-128">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="93553-129">Den här gruppen innehåller HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="93553-129">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="93553-130">**Plats**: Välj en plats geografiskt nära dig.</span><span class="sxs-lookup"><span data-stu-id="93553-130">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="93553-131">**Basera klusternamnet**: det här värdet används som det grundläggande namnet på Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-131">**Base Cluster Name**: This value is used as the base name for the Spark and Kafka clusters.</span></span> <span data-ttu-id="93553-132">Ange till exempel **hdi** skapar ett Spark-kluster med namnet spark hdi__ och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="93553-132">For example, entering **hdi** creates a Spark cluster named spark-hdi__ and a Kafka cluster named **kafka-hdi**.</span></span>

    * <span data-ttu-id="93553-133">**Klustrets inloggningsnamn**: admin användarnamn för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-133">**Cluster Login User Name**: The admin user name for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="93553-134">**Klustret inloggningslösenordet**: administratörslösenord för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-134">**Cluster Login Password**: The admin user password for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="93553-135">**SSH-användarnamn**: SSH-användare skapa för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-135">**SSH User Name**: The SSH user to create for the Spark and Kafka clusters.</span></span>

    * <span data-ttu-id="93553-136">**SSH-lösenordet**: lösenord för SSH-användare för Spark och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-136">**SSH Password**: The password for the SSH user for the Spark and Kafka clusters.</span></span>

3. <span data-ttu-id="93553-137">Läs den **villkor**, och välj sedan **jag samtycker till villkoren som anges ovan**.</span><span class="sxs-lookup"><span data-stu-id="93553-137">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="93553-138">Kontrollera slutligen **fäst på instrumentpanelen** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="93553-138">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="93553-139">Det tar ungefär 20 minuter för att skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-139">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="93553-140">När resurserna som har skapats, omdirigeras till ett blad för resursgruppen som innehåller kluster och web instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="93553-140">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![Blad för resursgrupp för virtuella nätverk och kluster](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="93553-142">Lägg märke till att namnen på HDInsight-kluster **spark BASENAME** och **kafka BASENAME**, där BASENAME är det namn du angav i mallen.</span><span class="sxs-lookup"><span data-stu-id="93553-142">Notice that the names of the HDInsight clusters are **spark-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="93553-143">Du kan använda dessa namn i senare steg när du ansluter till kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-143">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="use-the-notebooks"></a><span data-ttu-id="93553-144">Använd de bärbara datorerna</span><span class="sxs-lookup"><span data-stu-id="93553-144">Use the notebooks</span></span>

<span data-ttu-id="93553-145">Koden för det exempel som beskrivs i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span><span class="sxs-lookup"><span data-stu-id="93553-145">The code for the example described in this document is available at [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).</span></span>

<span data-ttu-id="93553-146">Följ stegen i den `README.md` filen för att slutföra det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="93553-146">Follow the steps in the `README.md` file to complete this example.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="93553-147">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="93553-147">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="93553-148">Eftersom stegen i det här dokumentet skapa båda klustren i samma Azure resursgrupp måste du ta bort resursgruppen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="93553-148">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="93553-149">Ta bort gruppen tar du bort alla resurser som har skapats genom att följa det här dokumentet, Azure Virtual Network och storage-konto som används av kluster.</span><span class="sxs-lookup"><span data-stu-id="93553-149">Deleting the group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="93553-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93553-150">Next steps</span></span>

<span data-ttu-id="93553-151">I det här exemplet du har lärt dig hur du använder Spark att läsa och skriva till Kafka.</span><span class="sxs-lookup"><span data-stu-id="93553-151">In this example, you learned how to use Spark to read and write to Kafka.</span></span> <span data-ttu-id="93553-152">Använd följande länkar för att identifiera andra sätt att arbeta med Kafka:</span><span class="sxs-lookup"><span data-stu-id="93553-152">Use the following links to discover other ways to work with Kafka:</span></span>

* [<span data-ttu-id="93553-153">Kom igång med Apache Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="93553-153">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="93553-154">Använd MirrorMaker för att skapa en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="93553-154">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="93553-155">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="93553-155">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)

