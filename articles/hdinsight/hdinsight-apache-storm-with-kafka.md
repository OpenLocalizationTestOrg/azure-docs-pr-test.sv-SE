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
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="143cb-105">Använda Apache Kafka (förhandsversion) med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="143cb-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="143cb-106">Lär dig hur toouse Apache Storm tooread från och skriva tooApache Kafka.</span><span class="sxs-lookup"><span data-stu-id="143cb-106">Learn how toouse Apache Storm tooread from and write tooApache Kafka.</span></span> <span data-ttu-id="143cb-107">Det här exemplet visar även hur toosave data från en Storm-topologi toohello HDFS-kompatibla filsystemet används av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="143cb-107">This example also demonstrates how toosave data from a Storm topology toohello HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="143cb-108">hello stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både ett Storm på HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-108">hello steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="143cb-109">Dessa kluster finns både i ett Azure Virtual Network, vilket gör att hello Storm-kluster toodirectly kommunicera med hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="143cb-109">These clusters are both located within an Azure Virtual Network, which allows hello Storm cluster toodirectly communicate with hello Kafka cluster.</span></span>
> 
> <span data-ttu-id="143cb-110">Kom ihåg toodelete hello kluster tooavoid överdriven avgifter när du är klar med hello steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="143cb-110">When you are done with hello steps in this document, remember toodelete hello clusters tooavoid excess charges.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="143cb-111">Hämta hello kod</span><span class="sxs-lookup"><span data-stu-id="143cb-111">Get hello code</span></span>

<span data-ttu-id="143cb-112">hello koden hello exemplet i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="143cb-112">hello code for hello example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="143cb-113">toocompile det här projektet du behöver följande konfiguration för din utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="143cb-113">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="143cb-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.</span><span class="sxs-lookup"><span data-stu-id="143cb-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="143cb-115">HDInsight 3.5 eller högre krävs Java 8.</span><span class="sxs-lookup"><span data-stu-id="143cb-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="143cb-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="143cb-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="143cb-117">En SSH-klient (du behöver hello `ssh` och `scp` kommandon) – information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="143cb-117">An SSH client (you need hello `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="143cb-118">En textredigerare eller IDE.</span><span class="sxs-lookup"><span data-stu-id="143cb-118">A text editor or IDE.</span></span>

<span data-ttu-id="143cb-119">hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="143cb-119">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="143cb-120">Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="143cb-120">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="143cb-121">`JAVA_HOME`-måste peka toohello katalog där hello JDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="143cb-121">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="143cb-122">`PATH`-bör innehålla hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="143cb-122">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="143cb-123">`JAVA_HOME`(eller motsvarande hello-sökväg).</span><span class="sxs-lookup"><span data-stu-id="143cb-123">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="143cb-124">`JAVA_HOME\bin`(eller motsvarande hello-sökväg).</span><span class="sxs-lookup"><span data-stu-id="143cb-124">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="143cb-125">hello katalog där Maven är installerat.</span><span class="sxs-lookup"><span data-stu-id="143cb-125">hello directory where Maven is installed.</span></span>

## <a name="create-hello-clusters"></a><span data-ttu-id="143cb-126">Skapa hello kluster</span><span class="sxs-lookup"><span data-stu-id="143cb-126">Create hello clusters</span></span>

<span data-ttu-id="143cb-127">Apache Kafka på HDInsight ger inte tillgång toohello Kafka mäklare över hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="143cb-127">Apache Kafka on HDInsight does not provide access toohello Kafka brokers over hello public internet.</span></span> <span data-ttu-id="143cb-128">Allt som pratar tooKafka måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="143cb-128">Anything that talks tooKafka must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="143cb-129">I det här exemplet finns både hello Kafka och Storm-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="143cb-129">For this example, both hello Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="143cb-130">hello följande diagram visar hur kommunikation som flödar mellan hello kluster:</span><span class="sxs-lookup"><span data-stu-id="143cb-130">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram över Storm och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="143cb-132">Andra tjänster på hello klustret hello som SSH- och Ambari kan nås över internet.</span><span class="sxs-lookup"><span data-stu-id="143cb-132">Other services on hello cluster such as SSH and Ambari can be accessed over hello internet.</span></span> <span data-ttu-id="143cb-133">Mer information om hello offentliga portar som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="143cb-133">For more information on hello public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="143cb-134">Du kan skapa en Azure-nätverk, Kafka, och Storm-kluster manuellt, är det enklare toouse en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="143cb-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="143cb-135">Använd hello följande steg toodeploy virtuellt Azure-nätverk, Kafka, och Storm-kluster tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="143cb-135">Use hello following steps toodeploy an Azure virtual network, Kafka, and Storm clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="143cb-136">Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="143cb-136">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="143cb-137">hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="143cb-137">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="143cb-138">Den skapar hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="143cb-138">It creates hello following resources:</span></span>
    
    * <span data-ttu-id="143cb-139">Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="143cb-139">Azure resource group</span></span>
    * <span data-ttu-id="143cb-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="143cb-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="143cb-141">Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="143cb-141">Azure Storage account</span></span>
    * <span data-ttu-id="143cb-142">Kafka på HDInsight version 3,6 (tre arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="143cb-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="143cb-143">Storm på HDInsight version 3,6 (tre arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="143cb-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="143cb-144">tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="143cb-144">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="143cb-145">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="143cb-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="143cb-146">Använd hello följande riktlinjer toopopulate hello poster på hello **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="143cb-146">Use hello following guidance toopopulate hello entries on hello **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="143cb-148">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="143cb-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="143cb-149">Den här gruppen innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-149">This group contains hello HDInsight cluster.</span></span>
   
    * <span data-ttu-id="143cb-150">**Plats**: Välj en plats geografiskt nära tooyou.</span><span class="sxs-lookup"><span data-stu-id="143cb-150">**Location**: Select a location geographically close tooyou.</span></span>

    * <span data-ttu-id="143cb-151">**Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-151">**Base Cluster Name**: This value is used as hello base name for hello Storm and Kafka clusters.</span></span> <span data-ttu-id="143cb-152">Ange till exempel **hdi** skapar en Storm-kluster med namnet **storm-hdi** och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="143cb-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="143cb-153">**Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-153">**Cluster Login User Name**: hello admin user name for hello Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="143cb-154">**Klustret inloggningslösenordet**: hello administratörslösenord för hello Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-154">**Cluster Login Password**: hello admin user password for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="143cb-155">**SSH-användarnamn**: hello SSH användaren toocreate för hello Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-155">**SSH User Name**: hello SSH user toocreate for hello Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="143cb-156">**SSH-lösenordet**: hello användarlösenord hello SSH för hello Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-156">**SSH Password**: hello password for hello SSH user for hello Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="143cb-157">Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.</span><span class="sxs-lookup"><span data-stu-id="143cb-157">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="143cb-158">Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="143cb-158">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="143cb-159">Det tar cirka 20 minuter toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-159">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="143cb-160">När du har skapat hello resurser visas hello bladet för resursgruppen hello.</span><span class="sxs-lookup"><span data-stu-id="143cb-160">Once hello resources have been created, hello blade for hello resource group is displayed.</span></span>

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="143cb-162">Observera att hello namnen på hello HDInsight-kluster är **storm-BASENAME** och **kafka BASENAME**, där BASENAME är hello namn du angett toohello mall.</span><span class="sxs-lookup"><span data-stu-id="143cb-162">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="143cb-163">Du kan använda dessa namn i senare steg när du ansluter toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-163">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="understanding-hello-code"></a><span data-ttu-id="143cb-164">Förstå hello kod</span><span class="sxs-lookup"><span data-stu-id="143cb-164">Understanding hello code</span></span>

<span data-ttu-id="143cb-165">Det här projektet innehåller två topologier:</span><span class="sxs-lookup"><span data-stu-id="143cb-165">This project contains two topologies:</span></span>

* <span data-ttu-id="143cb-166">**KafkaWriter**: definieras av hello **writer.yaml** filen, skriver den här topologin slumpmässiga meningar tooKafka med hello KafkaBolt med Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="143cb-166">**KafkaWriter**: Defined by hello **writer.yaml** file, this topology writes random sentences tooKafka using hello KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="143cb-167">Den här topologin använder en anpassad **SentenceSpout** komponenten toogenerate slumpmässiga meningar.</span><span class="sxs-lookup"><span data-stu-id="143cb-167">This topology uses a custom **SentenceSpout** component toogenerate random sentences.</span></span>

* <span data-ttu-id="143cb-168">**KafkaReader**: definieras av hello **reader.yaml** filen, den här topologin läser data från Kafka med hello KafkaSpout med Apache Storm och sedan loggar hello data toostdout.</span><span class="sxs-lookup"><span data-stu-id="143cb-168">**KafkaReader**: Defined by hello **reader.yaml** file, this topology reads data from Kafka using hello KafkaSpout provided with Apache Storm, then logs hello data toostdout.</span></span>

    <span data-ttu-id="143cb-169">Den här topologin använder toodefault för hello Storm HdfsBolt toowrite datalagring för hello Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-169">This topology uses hello Storm HdfsBolt toowrite data toodefault storage for hello Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="143cb-170">Som</span><span class="sxs-lookup"><span data-stu-id="143cb-170">Flux</span></span>

<span data-ttu-id="143cb-171">hello topologier definieras med hjälp av [som](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="143cb-171">hello topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="143cb-172">Som introducerades i Storm-0.10.x och låter dig tooseparate hello topologi configuration från hello kod.</span><span class="sxs-lookup"><span data-stu-id="143cb-172">Flux was introduced in Storm 0.10.x and allows you tooseparate hello topology configuration from hello code.</span></span> <span data-ttu-id="143cb-173">Topologier som använder hello som framework definieras hello topologi i en YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="143cb-173">For Topologies that use hello Flux framework, hello topology is defined in a YAML file.</span></span> <span data-ttu-id="143cb-174">Hej YAML filen kan vara ingår i hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="143cb-174">hello YAML file can be included as part of hello topology.</span></span> <span data-ttu-id="143cb-175">Det kan också vara en fristående fil som används när du skickar hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="143cb-175">It can also be a standalone file used when you submit hello topology.</span></span> <span data-ttu-id="143cb-176">Som stöder också variabeln ersättning vid körning, som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="143cb-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="143cb-177">hello anges följande parametrar vid körning för dessa topologier:</span><span class="sxs-lookup"><span data-stu-id="143cb-177">hello following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="143cb-178">`${kafka.topic}`: hello namnet på hello Kafka avsnitt som hello topologier läsning och skrivning till.</span><span class="sxs-lookup"><span data-stu-id="143cb-178">`${kafka.topic}`: hello name of hello Kafka topic that hello topologies read/write to.</span></span>

* <span data-ttu-id="143cb-179">`${kafka.broker.hosts}`: hello värd att hello Kafka förmedlar körs på.</span><span class="sxs-lookup"><span data-stu-id="143cb-179">`${kafka.broker.hosts}`: hello hosts that hello Kafka brokers run on.</span></span> <span data-ttu-id="143cb-180">hello broker informationen används av hello KafkaBolt när du skriver tooKafka.</span><span class="sxs-lookup"><span data-stu-id="143cb-180">hello broker information is used by hello KafkaBolt when writing tooKafka.</span></span>

* <span data-ttu-id="143cb-181">`${kafka.zookeeper.hosts}`: hello-värdar som Zookeeper körs på i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="143cb-181">`${kafka.zookeeper.hosts}`: hello hosts that Zookeeper runs on in hello Kafka cluster.</span></span>

<span data-ttu-id="143cb-182">Mer information om topologier som finns [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="143cb-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-hello-project"></a><span data-ttu-id="143cb-183">Hämta och kompilera hello-projekt</span><span class="sxs-lookup"><span data-stu-id="143cb-183">Download and compile hello project</span></span>

1. <span data-ttu-id="143cb-184">Hämta hello projektet från på din utvecklingsmiljö [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), öppna en kommandorad och ändra kataloger toohello plats som du hämtade hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="143cb-184">On your development environment, download hello project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories toohello location that you downloaded hello project.</span></span>

2. <span data-ttu-id="143cb-185">Från hello **hdinsight-storm-java-kafka** katalog, Använd hello följande kommando toocompile hello projekt och skapa ett paket för distribution:</span><span class="sxs-lookup"><span data-stu-id="143cb-185">From hello **hdinsight-storm-java-kafka** directory, use hello following command toocompile hello project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="143cb-186">hello paketet skapas en fil med namnet `KafkaTopology-1.0-SNAPSHOT.jar` i hello `target` directory.</span><span class="sxs-lookup"><span data-stu-id="143cb-186">hello package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in hello `target` directory.</span></span>

3. <span data-ttu-id="143cb-187">Använd hello följande kommandon toocopy hello paketet tooyour Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-187">Use hello following commands toocopy hello package tooyour Storm on HDInsight cluster.</span></span> <span data-ttu-id="143cb-188">Ersätt **användarnamn** med hello SSH-användarnamn för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-188">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="143cb-189">Ersätt **BASENAME** med grundläggande hello-namn du använde när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-189">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="143cb-190">När du uppmanas ange hello-lösenord som du använde när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-190">When prompted, enter hello password you used when creating hello clusters.</span></span>

## <a name="configure-hello-topology"></a><span data-ttu-id="143cb-191">Konfigurera hello-topologi</span><span class="sxs-lookup"><span data-stu-id="143cb-191">Configure hello topology</span></span>

1. <span data-ttu-id="143cb-192">Använd någon av följande metoder toodiscover hello hello Kafka broker värdar:</span><span class="sxs-lookup"><span data-stu-id="143cb-192">Use one of hello following methods toodiscover hello Kafka broker hosts:</span></span>

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
    > <span data-ttu-id="143cb-193">hello Bash exempel förutsätter att `$CLUSTERNAME` innehåller hello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-193">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="143cb-194">Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad.</span><span class="sxs-lookup"><span data-stu-id="143cb-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="143cb-195">När du uppmanas du ange hello lösenord för hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="143cb-195">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="143cb-196">hello-värdet som returneras är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="143cb-196">hello value returned is similar toohello following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="143cb-197">Det kan finnas fler än två broker värdar för klustret, behöver du inte tooprovide en fullständig lista över alla värdar tooclients.</span><span class="sxs-lookup"><span data-stu-id="143cb-197">While there may be more than two broker hosts for your cluster, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="143cb-198">En eller två är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="143cb-198">One or two is enough.</span></span>

2. <span data-ttu-id="143cb-199">Använd någon av följande metoder toodiscover hello Kafka Zookeeper värdar hello:</span><span class="sxs-lookup"><span data-stu-id="143cb-199">Use one of hello following methods toodiscover hello Kafka Zookeeper hosts:</span></span>

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
    > <span data-ttu-id="143cb-200">hello Bash exempel förutsätter att `$CLUSTERNAME` innehåller hello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-200">hello Bash example assumes that `$CLUSTERNAME` contains hello name of hello HDInsight cluster.</span></span> <span data-ttu-id="143cb-201">Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad.</span><span class="sxs-lookup"><span data-stu-id="143cb-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="143cb-202">När du uppmanas du ange hello lösenord för hello inloggningskonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="143cb-202">When prompted, enter hello password for hello cluster login account.</span></span>

    <span data-ttu-id="143cb-203">hello-värdet som returneras är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="143cb-203">hello value returned is similar toohello following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="143cb-204">Det finns fler än två Zookeeper-noder, behöver du inte tooprovide en fullständig lista över alla värdar tooclients.</span><span class="sxs-lookup"><span data-stu-id="143cb-204">While there are more than two Zookeeper nodes, you do not need tooprovide a full list of all hosts tooclients.</span></span> <span data-ttu-id="143cb-205">En eller två är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="143cb-205">One or two is enough.</span></span>

    <span data-ttu-id="143cb-206">Spara det här värdet eftersom det används senare.</span><span class="sxs-lookup"><span data-stu-id="143cb-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="143cb-207">Redigera hello `dev.properties` filen i projektet hello hello rot.</span><span class="sxs-lookup"><span data-stu-id="143cb-207">Edit hello `dev.properties` file in hello root of hello project.</span></span> <span data-ttu-id="143cb-208">Lägg till hello Broker och Zookeeper värdar information toohello matchande rader i den här filen.</span><span class="sxs-lookup"><span data-stu-id="143cb-208">Add hello Broker and Zookeeper hosts information toohello matching lines in this file.</span></span> <span data-ttu-id="143cb-209">hello konfigureras följande exempel med hjälp av hello exempelvärden från hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="143cb-209">hello following example is configured using hello sample values from hello previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="143cb-210">Spara hello `dev.properties` fil och sedan använda hello följande kommando tooupload den toohello Storm-kluster:</span><span class="sxs-lookup"><span data-stu-id="143cb-210">Save hello `dev.properties` file and then use hello following command tooupload it toohello Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="143cb-211">Ersätt **användarnamn** med hello SSH-användarnamn för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-211">Replace **USERNAME** with hello SSH user name for hello cluster.</span></span> <span data-ttu-id="143cb-212">Ersätt **BASENAME** med grundläggande hello-namn du använde när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-212">Replace **BASENAME** with hello base name you used when creating hello cluster.</span></span>

## <a name="start-hello-writer"></a><span data-ttu-id="143cb-213">Starta hello skrivare</span><span class="sxs-lookup"><span data-stu-id="143cb-213">Start hello writer</span></span>

1. <span data-ttu-id="143cb-214">Använd följande tooconnect toohello Storm-kluster med SSH hello.</span><span class="sxs-lookup"><span data-stu-id="143cb-214">Use hello following tooconnect toohello Storm cluster using SSH.</span></span> <span data-ttu-id="143cb-215">Ersätt **användarnamn** med hello SSH-användarnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-215">Replace **USERNAME** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="143cb-216">Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-216">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="143cb-217">När du uppmanas ange hello-lösenord som du använde när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="143cb-217">When prompted, enter hello password you used when creating hello clusters.</span></span>
   
    <span data-ttu-id="143cb-218">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="143cb-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="143cb-219">Från hello SSH-anslutning, använder du följande kommando toocreate hello Kafka avsnittet används av hello topologi hello:</span><span class="sxs-lookup"><span data-stu-id="143cb-219">From hello SSH connection, use hello following command toocreate hello Kafka topic used by hello topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="143cb-220">Ersätt `$KAFKAZKHOSTS` med hello Zookeeper värd för information som du hämtade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="143cb-220">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

2. <span data-ttu-id="143cb-221">Använd hello efter kommandot toostart hello writer topologi från hello SSH-anslutning toohello Storm-kluster:</span><span class="sxs-lookup"><span data-stu-id="143cb-221">From hello SSH connection toohello Storm cluster, use hello following command toostart hello writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="143cb-222">hello-parametrar som används med det här kommandot är:</span><span class="sxs-lookup"><span data-stu-id="143cb-222">hello parameters used with this command are:</span></span>

    * <span data-ttu-id="143cb-223">`org.apache.storm.flux.Flux`: Använd som tooconfigure och kör den här topologin.</span><span class="sxs-lookup"><span data-stu-id="143cb-223">`org.apache.storm.flux.Flux`: Use Flux tooconfigure and run this topology.</span></span>

    * <span data-ttu-id="143cb-224">`--remote`: Skicka hello topologi tooNimbus.</span><span class="sxs-lookup"><span data-stu-id="143cb-224">`--remote`: Submit hello topology tooNimbus.</span></span> <span data-ttu-id="143cb-225">hello topologi distribueras över hello arbetarnoder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="143cb-225">hello topology is distributed across hello worker nodes in hello cluster.</span></span>

    * <span data-ttu-id="143cb-226">`-R /writer.yaml`: Använd hello `writer.yaml` filen tooconfigure hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="143cb-226">`-R /writer.yaml`: Use hello `writer.yaml` file tooconfigure hello topology.</span></span> <span data-ttu-id="143cb-227">`-R`Anger att den här resursen ingår i hello jar-filen.</span><span class="sxs-lookup"><span data-stu-id="143cb-227">`-R` indicates that this resource is included in hello jar file.</span></span> <span data-ttu-id="143cb-228">Det är i hello jar hello rot så `/writer.yaml` är hello sökvägen tooit.</span><span class="sxs-lookup"><span data-stu-id="143cb-228">It's in hello root of hello jar, so `/writer.yaml` is hello path tooit.</span></span>

    * <span data-ttu-id="143cb-229">`--filter`: Fylla poster i hello `writer.yaml` topologi med värden i hello `dev.properties` fil.</span><span class="sxs-lookup"><span data-stu-id="143cb-229">`--filter`: Populate entries in hello `writer.yaml` topology using values in hello `dev.properties` file.</span></span> <span data-ttu-id="143cb-230">Till exempel hello värdet för hello `kafka.topic` post i hello-filen är används tooreplace hello `${kafka.topic}` post i hello topologi definition.</span><span class="sxs-lookup"><span data-stu-id="143cb-230">For example, hello value of hello `kafka.topic` entry in hello file is used tooreplace hello `${kafka.topic}` entry in hello topology definition.</span></span>

5. <span data-ttu-id="143cb-231">När hello-topologi har börjat använda hello efter kommandot tooverify att skrivs data toohello Kafka avsnittet:</span><span class="sxs-lookup"><span data-stu-id="143cb-231">Once hello topology has started, use hello following command tooverify that it is writing data toohello Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="143cb-232">Ersätt `$KAFKAZKHOSTS` med hello Zookeeper värd för information som du hämtade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="143cb-232">Replace `$KAFKAZKHOSTS` with hello Zookeeper host information you retrieved in hello previous section.</span></span>

    <span data-ttu-id="143cb-233">Detta kommando använder ett skript som medföljer Kafka toomonitor hello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="143cb-233">This command uses a script shipped with Kafka toomonitor hello topic.</span></span> <span data-ttu-id="143cb-234">Efter ett tag och bör börja returnera slumpmässiga meningar som har skrivits toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="143cb-234">After a moment, it should start returning random sentences that have been written toohello topic.</span></span> <span data-ttu-id="143cb-235">hello utdata är liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="143cb-235">hello output is similar toohello following example:</span></span>

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

    <span data-ttu-id="143cb-236">Tryck på Ctrl + c toostop hello skript.</span><span class="sxs-lookup"><span data-stu-id="143cb-236">Use Ctrl+c toostop hello script.</span></span>

## <a name="start-hello-reader"></a><span data-ttu-id="143cb-237">Starta hello läsare</span><span class="sxs-lookup"><span data-stu-id="143cb-237">Start hello reader</span></span>

1. <span data-ttu-id="143cb-238">Använd hello efter kommandot toostart hello reader topologi från hello SSH-session toohello Storm-kluster:</span><span class="sxs-lookup"><span data-stu-id="143cb-238">From hello SSH session toohello Storm cluster, use hello following command toostart hello reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="143cb-239">När hello topologi startar öppna hello Storm-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="143cb-239">Once hello topology starts, open hello Storm UI.</span></span> <span data-ttu-id="143cb-240">Den här webbgränssnittet finns i https://storm-BASENAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="143cb-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="143cb-241">Ersätt __BASENAME__ med hello basnamn används när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="143cb-241">Replace __BASENAME__ with hello base name used when hello cluster was created.</span></span> 

    <span data-ttu-id="143cb-242">Vid uppmaning används hello admin inloggningsnamnet (standard `admin`) och lösenord som används när hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="143cb-242">When prompted, use hello admin login name (default, `admin`) and password used when hello cluster was created.</span></span> <span data-ttu-id="143cb-243">Du ser en webbsida liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="143cb-243">You see a web page similar toohello following image:</span></span>

    ![Storm UI](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="143cb-245">Hello Storm-Användargränssnittet, Välj hello __kafka läsare__ länken i hello __Topology Summary__ avsnittet toodisplay information om hello __kafka läsare__ topologi.</span><span class="sxs-lookup"><span data-stu-id="143cb-245">From hello Storm UI, select hello __kafka-reader__ link in hello __Topology Summary__ section toodisplay information about hello __kafka-reader__ topology.</span></span>

    ![Topologi sammanfattningen i hello Storm webbgränssnittet](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="143cb-247">toodisplay information om hello instanser av hello loggaren bult komponent, Välj hello __loggaren bult__ länken i hello __bultar (hela tiden)__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="143cb-247">toodisplay information about hello instances of hello logger-bolt component, select hello __logger-bolt__ link in hello __Bolts (All time)__ section.</span></span>

    ![Loggaren bult länk under hello bultar](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="143cb-249">I hello __Executors__ väljer du en länk i hello __Port__ kolumnen toodisplay logga information om den här instansen av hello-komponent.</span><span class="sxs-lookup"><span data-stu-id="143cb-249">In hello __Executors__ section, select a link in hello __Port__ column toodisplay logging information about this instance of hello component.</span></span>

    ![Executors länk](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="143cb-251">hello-loggen innehåller en logg över hello data läses från hello Kafka avsnittet.</span><span class="sxs-lookup"><span data-stu-id="143cb-251">hello log contains a log of hello data read from hello Kafka topic.</span></span> <span data-ttu-id="143cb-252">hello information i loggen för hello är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="143cb-252">hello information in hello log is similar toohello following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] hello cow jumped over hello moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: hello cow jumped over hello moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and hello seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and hello seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps hello doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps hello doctor away

## <a name="stop-hello-topologies"></a><span data-ttu-id="143cb-253">Stoppa hello topologier</span><span class="sxs-lookup"><span data-stu-id="143cb-253">Stop hello topologies</span></span>

<span data-ttu-id="143cb-254">Från en SSH-session toohello Storm-kluster, använder du följande kommandon toostop hello Storm-topologier hello:</span><span class="sxs-lookup"><span data-stu-id="143cb-254">From an SSH session toohello Storm cluster, use hello following commands toostop hello Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-hello-cluster"></a><span data-ttu-id="143cb-255">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="143cb-255">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="143cb-256">Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="143cb-256">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="143cb-257">Ta bort hello resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="143cb-257">Deleting hello resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="143cb-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="143cb-258">Next steps</span></span>

<span data-ttu-id="143cb-259">Flera exempeltopologier som kan användas med Storm på HDInsight finns [exempel Storm-topologier och komponenter](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="143cb-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="143cb-260">Mer information om distribuera och övervaka topologier på Linux-baserat HDInsight finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="143cb-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>