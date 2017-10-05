---
title: "Använda Apache Kafka med Storm på HDInsight - Azure | Microsoft Docs"
description: "Apache Kafka har installerats med Apache Storm på HDInsight. Lär dig mer om att skriva till Kafka och läses sedan från den, använder de KafkaBolt och KafkaSpout med Storm. Också lära dig hur du använder som framework att definiera och skicka Storm-topologier."
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
ms.openlocfilehash: e8895ef3c11aea48513e4060a20f5f49b11fc961
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-apache-kafka-preview-with-storm-on-hdinsight"></a><span data-ttu-id="4287d-105">Använda Apache Kafka (förhandsversion) med Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4287d-105">Use Apache Kafka (preview) with Storm on HDInsight</span></span>

<span data-ttu-id="4287d-106">Lär dig hur du använder Apache Storm att läsa från och skriva till Apache Kafka.</span><span class="sxs-lookup"><span data-stu-id="4287d-106">Learn how to use Apache Storm to read from and write to Apache Kafka.</span></span> <span data-ttu-id="4287d-107">Det här exemplet visar även hur du sparar data från en Storm-topologi till HDFS-kompatibla filsystem som används av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4287d-107">This example also demonstrates how to save data from a Storm topology to the HDFS-compatible file system used by HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="4287d-108">Stegen i det här dokumentet skapa ett Azure-resursgrupp som innehåller både ett Storm på HDInsight och en Kafka på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-108">The steps in this document create an Azure resource group that contains both a Storm on HDInsight and a Kafka on HDInsight cluster.</span></span> <span data-ttu-id="4287d-109">Dessa kluster finns både i ett Azure Virtual Network, vilket innebär att Storm-klustret kan kommunicera direkt med Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-109">These clusters are both located within an Azure Virtual Network, which allows the Storm cluster to directly communicate with the Kafka cluster.</span></span>
> 
> <span data-ttu-id="4287d-110">Kom ihåg att ta bort kluster för att undvika överdriven avgifter när du är klar med steg i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4287d-110">When you are done with the steps in this document, remember to delete the clusters to avoid excess charges.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="4287d-111">Hämta koden</span><span class="sxs-lookup"><span data-stu-id="4287d-111">Get the code</span></span>

<span data-ttu-id="4287d-112">Koden för exemplet i det här dokumentet finns på [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span><span class="sxs-lookup"><span data-stu-id="4287d-112">The code for the example used in this document is available at [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka).</span></span>

<span data-ttu-id="4287d-113">För att kompilera det här projektet, behöver du följande konfiguration för din utvecklingsmiljö:</span><span class="sxs-lookup"><span data-stu-id="4287d-113">To compile this project, you need the following configuration for your development environment:</span></span>

* <span data-ttu-id="4287d-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) eller högre.</span><span class="sxs-lookup"><span data-stu-id="4287d-114">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="4287d-115">HDInsight 3.5 eller högre krävs Java 8.</span><span class="sxs-lookup"><span data-stu-id="4287d-115">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="4287d-116">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="4287d-116">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

* <span data-ttu-id="4287d-117">En SSH-klient (du behöver den `ssh` och `scp` kommandon) – information finns i [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="4287d-117">An SSH client (you need the `ssh` and `scp` commands) - For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="4287d-118">En textredigerare eller IDE.</span><span class="sxs-lookup"><span data-stu-id="4287d-118">A text editor or IDE.</span></span>

<span data-ttu-id="4287d-119">Följande miljövariabler kan anges när du installerar Java och JDK på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="4287d-119">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="4287d-120">Dock bör du kontrollera att de finns och att de innehåller rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="4287d-120">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="4287d-121">`JAVA_HOME`-måste peka på den katalog där JDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="4287d-121">`JAVA_HOME` - should point to the directory where the JDK is installed.</span></span>
* <span data-ttu-id="4287d-122">`PATH`-bör innehålla följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="4287d-122">`PATH` - should contain the following paths:</span></span>
  
    * <span data-ttu-id="4287d-123">`JAVA_HOME`(eller motsvarande sökväg).</span><span class="sxs-lookup"><span data-stu-id="4287d-123">`JAVA_HOME` (or the equivalent path).</span></span>
    * <span data-ttu-id="4287d-124">`JAVA_HOME\bin`(eller motsvarande sökväg).</span><span class="sxs-lookup"><span data-stu-id="4287d-124">`JAVA_HOME\bin` (or the equivalent path).</span></span>
    * <span data-ttu-id="4287d-125">Den katalog där Maven är installerat.</span><span class="sxs-lookup"><span data-stu-id="4287d-125">The directory where Maven is installed.</span></span>

## <a name="create-the-clusters"></a><span data-ttu-id="4287d-126">Skapa kluster</span><span class="sxs-lookup"><span data-stu-id="4287d-126">Create the clusters</span></span>

<span data-ttu-id="4287d-127">Apache Kafka på HDInsight ger inte tillgång till Kafka mäklare via det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="4287d-127">Apache Kafka on HDInsight does not provide access to the Kafka brokers over the public internet.</span></span> <span data-ttu-id="4287d-128">Allt som kommunicerar Kafka måste finnas i samma virtuella Azure-nätverket som noder i klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="4287d-128">Anything that talks to Kafka must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="4287d-129">I det här exemplet finns både Kafka och Storm-kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4287d-129">For this example, both the Kafka and Storm clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="4287d-130">Följande diagram visar hur kommunikation som flödar mellan kluster:</span><span class="sxs-lookup"><span data-stu-id="4287d-130">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram över Storm och Kafka kluster i Azure-nätverk](./media/hdinsight-apache-storm-with-kafka/storm-kafka-vnet.png)

> [!NOTE]
> <span data-ttu-id="4287d-132">Andra tjänster på klustret, till exempel SSH och Ambari kan nås via internet.</span><span class="sxs-lookup"><span data-stu-id="4287d-132">Other services on the cluster such as SSH and Ambari can be accessed over the internet.</span></span> <span data-ttu-id="4287d-133">Mer information om de offentliga portarna som är tillgängliga med HDInsight finns [portar och URI: er som används av HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span><span class="sxs-lookup"><span data-stu-id="4287d-133">For more information on the public ports available with HDInsight, see [Ports and URIs used by HDInsight](hdinsight-hadoop-port-settings-for-services.md).</span></span>

<span data-ttu-id="4287d-134">Du kan skapa en Azure-nätverk, Kafka, och Storm-kluster manuellt, men det är enklare att använda en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="4287d-134">While you can create an Azure virtual network, Kafka, and Storm clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="4287d-135">Använd följande steg för att distribuera ett Azure-nätverk, Kafka, och Storm-kluster till Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4287d-135">Use the following steps to deploy an Azure virtual network, Kafka, and Storm clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="4287d-136">Använd knappen följande för att logga in på Azure och öppna mallen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4287d-136">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-storm-cluster-in-vnet-v2.json" target="_blank"><img src="./media/hdinsight-apache-storm-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="4287d-137">Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span><span class="sxs-lookup"><span data-stu-id="4287d-137">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-storm-cluster-in-vnet-v1.json**.</span></span> <span data-ttu-id="4287d-138">Den skapar följande resurser:</span><span class="sxs-lookup"><span data-stu-id="4287d-138">It creates the following resources:</span></span>
    
    * <span data-ttu-id="4287d-139">Azure-resursgrupp</span><span class="sxs-lookup"><span data-stu-id="4287d-139">Azure resource group</span></span>
    * <span data-ttu-id="4287d-140">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="4287d-140">Azure Virtual Network</span></span>
    * <span data-ttu-id="4287d-141">Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="4287d-141">Azure Storage account</span></span>
    * <span data-ttu-id="4287d-142">Kafka på HDInsight version 3,6 (tre arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="4287d-142">Kafka on HDInsight version 3.6 (three worker nodes)</span></span>
    * <span data-ttu-id="4287d-143">Storm på HDInsight version 3,6 (tre arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="4287d-143">Storm on HDInsight version 3.6 (three worker nodes)</span></span>

  > [!WARNING]
  > <span data-ttu-id="4287d-144">Klustret måste innehålla minst tre arbetsnoder för att garantera tillgängligheten för Kafka i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="4287d-144">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="4287d-145">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="4287d-145">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="4287d-146">Använd följande riktlinjer för att fylla posterna på den **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="4287d-146">Use the following guidance to populate the entries on the **Custom deployment** blade:</span></span>
   
    ![HDInsight anpassad distribution](./media/hdinsight-apache-storm-with-kafka/parameters.png)

    * <span data-ttu-id="4287d-148">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="4287d-148">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="4287d-149">Den här gruppen innehåller HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-149">This group contains the HDInsight cluster.</span></span>
   
    * <span data-ttu-id="4287d-150">**Plats**: Välj en plats geografiskt nära dig.</span><span class="sxs-lookup"><span data-stu-id="4287d-150">**Location**: Select a location geographically close to you.</span></span>

    * <span data-ttu-id="4287d-151">**Basera klusternamnet**: det här värdet används som huvudnamnet för Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-151">**Base Cluster Name**: This value is used as the base name for the Storm and Kafka clusters.</span></span> <span data-ttu-id="4287d-152">Ange till exempel **hdi** skapar en Storm-kluster med namnet **storm-hdi** och ett Kafka kluster med namnet **kafka hdi**.</span><span class="sxs-lookup"><span data-stu-id="4287d-152">For example, entering **hdi** creates a Storm cluster named **storm-hdi** and a Kafka cluster named **kafka-hdi**.</span></span>
   
    * <span data-ttu-id="4287d-153">**Klustrets inloggningsnamn**: admin användarnamn Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-153">**Cluster Login User Name**: The admin user name for the Storm and Kafka clusters.</span></span>
   
    * <span data-ttu-id="4287d-154">**Klustret inloggningslösenordet**: administratörslösenord för Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-154">**Cluster Login Password**: The admin user password for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="4287d-155">**SSH-användarnamn**: SSH-användare skapa för Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-155">**SSH User Name**: The SSH user to create for the Storm and Kafka clusters.</span></span>
    
    * <span data-ttu-id="4287d-156">**SSH-lösenordet**: lösenord för SSH-användare för Storm och Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-156">**SSH Password**: The password for the SSH user for the Storm and Kafka clusters.</span></span>

3. <span data-ttu-id="4287d-157">Läs den **villkor**, och välj sedan **jag samtycker till villkoren som anges ovan**.</span><span class="sxs-lookup"><span data-stu-id="4287d-157">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="4287d-158">Kontrollera slutligen **fäst på instrumentpanelen** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="4287d-158">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="4287d-159">Det tar ungefär 20 minuter för att skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-159">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="4287d-160">När resurserna som har skapats visas bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="4287d-160">Once the resources have been created, the blade for the resource group is displayed.</span></span>

![Blad för resursgrupp för virtuella nätverk och kluster](./media/hdinsight-apache-storm-with-kafka/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="4287d-162">Lägg märke till att namnen på HDInsight-kluster **storm-BASENAME** och **kafka BASENAME**, där BASENAME är det namn du angav i mallen.</span><span class="sxs-lookup"><span data-stu-id="4287d-162">Notice that the names of the HDInsight clusters are **storm-BASENAME** and **kafka-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="4287d-163">Du kan använda dessa namn i senare steg när du ansluter till kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-163">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="understanding-the-code"></a><span data-ttu-id="4287d-164">Förstå koden</span><span class="sxs-lookup"><span data-stu-id="4287d-164">Understanding the code</span></span>

<span data-ttu-id="4287d-165">Det här projektet innehåller två topologier:</span><span class="sxs-lookup"><span data-stu-id="4287d-165">This project contains two topologies:</span></span>

* <span data-ttu-id="4287d-166">**KafkaWriter**: definieras av den **writer.yaml** filen, den här topologin skriver slumpmässiga meningar till Kafka med KafkaBolt med Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="4287d-166">**KafkaWriter**: Defined by the **writer.yaml** file, this topology writes random sentences to Kafka using the KafkaBolt provided with Apache Storm.</span></span>

    <span data-ttu-id="4287d-167">Den här topologin använder en anpassad **SentenceSpout** komponenten som ska generera en slumpmässig meningar.</span><span class="sxs-lookup"><span data-stu-id="4287d-167">This topology uses a custom **SentenceSpout** component to generate random sentences.</span></span>

* <span data-ttu-id="4287d-168">**KafkaReader**: definieras av den **reader.yaml** filen, den här topologin läser data från Kafka med KafkaSpout med Apache Storm och sedan loggar data STDOUT.</span><span class="sxs-lookup"><span data-stu-id="4287d-168">**KafkaReader**: Defined by the **reader.yaml** file, this topology reads data from Kafka using the KafkaSpout provided with Apache Storm, then logs the data to stdout.</span></span>

    <span data-ttu-id="4287d-169">Den här topologin använder Storm-HdfsBolt för att skriva data till standardlagring för Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-169">This topology uses the Storm HdfsBolt to write data to default storage for the Storm cluster.</span></span>
### <a name="flux"></a><span data-ttu-id="4287d-170">Som</span><span class="sxs-lookup"><span data-stu-id="4287d-170">Flux</span></span>

<span data-ttu-id="4287d-171">Topologierna definieras med hjälp av [som](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="4287d-171">The topologies are defined using [Flux](https://storm.apache.org/releases/1.1.0/flux.html).</span></span> <span data-ttu-id="4287d-172">Som introducerades i Storm-0.10.x och du kan avgränsa topologi konfigurationen från koden.</span><span class="sxs-lookup"><span data-stu-id="4287d-172">Flux was introduced in Storm 0.10.x and allows you to separate the topology configuration from the code.</span></span> <span data-ttu-id="4287d-173">Topologier som använder ramverket som definieras topologin i en YAML-fil.</span><span class="sxs-lookup"><span data-stu-id="4287d-173">For Topologies that use the Flux framework, the topology is defined in a YAML file.</span></span> <span data-ttu-id="4287d-174">YAML-filen kan vara ingår i topologin.</span><span class="sxs-lookup"><span data-stu-id="4287d-174">The YAML file can be included as part of the topology.</span></span> <span data-ttu-id="4287d-175">Det kan också vara en fristående fil som används när du skickar in topologin.</span><span class="sxs-lookup"><span data-stu-id="4287d-175">It can also be a standalone file used when you submit the topology.</span></span> <span data-ttu-id="4287d-176">Som stöder också variabeln ersättning vid körning, som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="4287d-176">Flux also supports variable substitution at run-time, which is used in this example.</span></span>

<span data-ttu-id="4287d-177">Följande parametrar anges vid körning för dessa topologier:</span><span class="sxs-lookup"><span data-stu-id="4287d-177">The following parameters are set at run time for these topologies:</span></span>

* <span data-ttu-id="4287d-178">`${kafka.topic}`: Namnet på avsnittet Kafka som den topologier läsning och skrivning till.</span><span class="sxs-lookup"><span data-stu-id="4287d-178">`${kafka.topic}`: The name of the Kafka topic that the topologies read/write to.</span></span>

* <span data-ttu-id="4287d-179">`${kafka.broker.hosts}`: De värdar som Kafka förmedlar köras på.</span><span class="sxs-lookup"><span data-stu-id="4287d-179">`${kafka.broker.hosts}`: The hosts that the Kafka brokers run on.</span></span> <span data-ttu-id="4287d-180">Service broker-informationen används av KafkaBolt vid skrivning till Kafka.</span><span class="sxs-lookup"><span data-stu-id="4287d-180">The broker information is used by the KafkaBolt when writing to Kafka.</span></span>

* <span data-ttu-id="4287d-181">`${kafka.zookeeper.hosts}`: De värdar som Zookeeper körs på i Kafka-klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-181">`${kafka.zookeeper.hosts}`: The hosts that Zookeeper runs on in the Kafka cluster.</span></span>

<span data-ttu-id="4287d-182">Mer information om topologier som finns [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="4287d-182">For more information on Flux topologies, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="download-and-compile-the-project"></a><span data-ttu-id="4287d-183">Hämta och kompileras projektet</span><span class="sxs-lookup"><span data-stu-id="4287d-183">Download and compile the project</span></span>

1. <span data-ttu-id="4287d-184">Hämta projektet från på din utvecklingsmiljö [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), öppna en kommandorad och ändra kataloger till platsen som du hämtade projektet.</span><span class="sxs-lookup"><span data-stu-id="4287d-184">On your development environment, download the project from [https://github.com/Azure-Samples/hdinsight-storm-java-kafka](https://github.com/Azure-Samples/hdinsight-storm-java-kafka), open a command-line, and change directories to the location that you downloaded the project.</span></span>

2. <span data-ttu-id="4287d-185">Från den **hdinsight-storm-java-kafka** directory, Använd följande kommando för att kompilera projektet och skapa ett paket för distribution:</span><span class="sxs-lookup"><span data-stu-id="4287d-185">From the **hdinsight-storm-java-kafka** directory, use the following command to compile the project and create a package for deployment:</span></span>

  ```bash
  mvn clean package
  ```

    <span data-ttu-id="4287d-186">Paketet skapas en fil med namnet `KafkaTopology-1.0-SNAPSHOT.jar` i den `target` directory.</span><span class="sxs-lookup"><span data-stu-id="4287d-186">The package process creates a file named `KafkaTopology-1.0-SNAPSHOT.jar` in the `target` directory.</span></span>

3. <span data-ttu-id="4287d-187">Använd följande kommandon för att kopiera paketet till ditt Storm på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-187">Use the following commands to copy the package to your Storm on HDInsight cluster.</span></span> <span data-ttu-id="4287d-188">Ersätt **användarnamn** med SSH-användarnamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-188">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="4287d-189">Ersätt **BASENAME** med det grundläggande namnet som du använde när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-189">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

  ```bash
  scp ./target/KafkaTopology-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
  ```

    <span data-ttu-id="4287d-190">När du uppmanas, ange lösenordet som du använde när du skapar kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-190">When prompted, enter the password you used when creating the clusters.</span></span>

## <a name="configure-the-topology"></a><span data-ttu-id="4287d-191">Konfigurera topologin</span><span class="sxs-lookup"><span data-stu-id="4287d-191">Configure the topology</span></span>

1. <span data-ttu-id="4287d-192">Använd någon av följande metoder för att identifiera Kafka broker värdar:</span><span class="sxs-lookup"><span data-stu-id="4287d-192">Use one of the following methods to discover the Kafka broker hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
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
    > <span data-ttu-id="4287d-193">Bash-exemplet förutsätter att `$CLUSTERNAME` innehåller namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-193">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="4287d-194">Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad.</span><span class="sxs-lookup"><span data-stu-id="4287d-194">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="4287d-195">När du uppmanas, ange lösenordet för inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-195">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="4287d-196">Det värde som returneras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="4287d-196">The value returned is similar to the following text:</span></span>

        wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092

    > [!IMPORTANT]
    > <span data-ttu-id="4287d-197">Det kan finnas fler än två broker värdar för klustret, behöver du inte ange en fullständig lista över alla värdar till klienter.</span><span class="sxs-lookup"><span data-stu-id="4287d-197">While there may be more than two broker hosts for your cluster, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="4287d-198">En eller två är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="4287d-198">One or two is enough.</span></span>

2. <span data-ttu-id="4287d-199">Använd någon av följande metoder för att identifiera Kafka Zookeeper-värdar:</span><span class="sxs-lookup"><span data-stu-id="4287d-199">Use one of the following methods to discover the Kafka Zookeeper hosts:</span></span>

    ```powershell
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $clusterName = Read-Host -Prompt "Enter the Kafka cluster name"
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
    > <span data-ttu-id="4287d-200">Bash-exemplet förutsätter att `$CLUSTERNAME` innehåller namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-200">The Bash example assumes that `$CLUSTERNAME` contains the name of the HDInsight cluster.</span></span> <span data-ttu-id="4287d-201">Det förutsätts även att som [jq](https://stedolan.github.io/jq/) är installerad.</span><span class="sxs-lookup"><span data-stu-id="4287d-201">It also assumes that [jq](https://stedolan.github.io/jq/) is installed.</span></span> <span data-ttu-id="4287d-202">När du uppmanas, ange lösenordet för inloggningskontot för klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-202">When prompted, enter the password for the cluster login account.</span></span>

    <span data-ttu-id="4287d-203">Det värde som returneras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="4287d-203">The value returned is similar to the following text:</span></span>

        zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181

    > [!IMPORTANT]
    > <span data-ttu-id="4287d-204">Det finns fler än två Zookeeper-noder, behöver du inte ange en fullständig lista över alla värdar till klienter.</span><span class="sxs-lookup"><span data-stu-id="4287d-204">While there are more than two Zookeeper nodes, you do not need to provide a full list of all hosts to clients.</span></span> <span data-ttu-id="4287d-205">En eller två är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="4287d-205">One or two is enough.</span></span>

    <span data-ttu-id="4287d-206">Spara det här värdet eftersom det används senare.</span><span class="sxs-lookup"><span data-stu-id="4287d-206">Save this value, as it is used later.</span></span>

3. <span data-ttu-id="4287d-207">Redigera den `dev.properties` filen i roten av projektet.</span><span class="sxs-lookup"><span data-stu-id="4287d-207">Edit the `dev.properties` file in the root of the project.</span></span> <span data-ttu-id="4287d-208">Lägga till den Service Broker och Zookeeper värdar i matchande rader i den här filen.</span><span class="sxs-lookup"><span data-stu-id="4287d-208">Add the Broker and Zookeeper hosts information to the matching lines in this file.</span></span> <span data-ttu-id="4287d-209">I följande exempel konfigureras med hjälp av exempelvärden från föregående steg:</span><span class="sxs-lookup"><span data-stu-id="4287d-209">The following example is configured using the sample values from the previous steps:</span></span>

        kafka.zookeeper.hosts: zk0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181,zk2-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:2181
        kafka.broker.hosts: wn0-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092,wn1-kafka.53qqkiavjsoeloiq3y1naf4hzc.ex.internal.cloudapp.net:9092
        kafka.topic: stormtopic

4. <span data-ttu-id="4287d-210">Spara den `dev.properties` filen och sedan använda följande kommando för att överföra den till Storm-kluster:</span><span class="sxs-lookup"><span data-stu-id="4287d-210">Save the `dev.properties` file and then use the following command to upload it to the Storm cluster:</span></span>

     ```bash
    scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:KafkaTopology-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="4287d-211">Ersätt **användarnamn** med SSH-användarnamn för klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-211">Replace **USERNAME** with the SSH user name for the cluster.</span></span> <span data-ttu-id="4287d-212">Ersätt **BASENAME** med det grundläggande namnet som du använde när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-212">Replace **BASENAME** with the base name you used when creating the cluster.</span></span>

## <a name="start-the-writer"></a><span data-ttu-id="4287d-213">Starta skrivaren</span><span class="sxs-lookup"><span data-stu-id="4287d-213">Start the writer</span></span>

1. <span data-ttu-id="4287d-214">Använd följande för att ansluta till Storm-kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="4287d-214">Use the following to connect to the Storm cluster using SSH.</span></span> <span data-ttu-id="4287d-215">Ersätt **användarnamn** med SSH-användarnamn som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-215">Replace **USERNAME** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="4287d-216">Ersätt **BASENAME** med det grundläggande namnet som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-216">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

  ```bash
  ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net
  ```

    <span data-ttu-id="4287d-217">När du uppmanas, ange lösenordet som du använde när du skapar kluster.</span><span class="sxs-lookup"><span data-stu-id="4287d-217">When prompted, enter the password you used when creating the clusters.</span></span>
   
    <span data-ttu-id="4287d-218">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4287d-218">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4287d-219">Använder du följande kommando från SSH-anslutning för att skapa avsnittet Kafka används av topologi:</span><span class="sxs-lookup"><span data-stu-id="4287d-219">From the SSH connection, use the following command to create the Kafka topic used by the topology:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic stormtopic --zookeeper $KAFKAZKHOSTS
    ```

    <span data-ttu-id="4287d-220">Ersätt `$KAFKAZKHOSTS` med Zookeeper värd för information som du hämtade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4287d-220">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

2. <span data-ttu-id="4287d-221">Från SSH-anslutning till Storm-kluster använder du följande kommando för att starta writer topologi:</span><span class="sxs-lookup"><span data-stu-id="4287d-221">From the SSH connection to the Storm cluster, use the following command to start the writer topology:</span></span>

    ```bash
    storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
    ```

    <span data-ttu-id="4287d-222">De parametrar som används med det här kommandot är:</span><span class="sxs-lookup"><span data-stu-id="4287d-222">The parameters used with this command are:</span></span>

    * <span data-ttu-id="4287d-223">`org.apache.storm.flux.Flux`: Använd som du konfigurerar och kör den här topologin.</span><span class="sxs-lookup"><span data-stu-id="4287d-223">`org.apache.storm.flux.Flux`: Use Flux to configure and run this topology.</span></span>

    * <span data-ttu-id="4287d-224">`--remote`: Skicka topologi och Nimbus.</span><span class="sxs-lookup"><span data-stu-id="4287d-224">`--remote`: Submit the topology to Nimbus.</span></span> <span data-ttu-id="4287d-225">Topologi distribueras över arbetarnoder i klustret.</span><span class="sxs-lookup"><span data-stu-id="4287d-225">The topology is distributed across the worker nodes in the cluster.</span></span>

    * <span data-ttu-id="4287d-226">`-R /writer.yaml`: Använd den `writer.yaml` filen om du vill konfigurera topologin.</span><span class="sxs-lookup"><span data-stu-id="4287d-226">`-R /writer.yaml`: Use the `writer.yaml` file to configure the topology.</span></span> <span data-ttu-id="4287d-227">`-R`Anger att den här resursen ingår i jar-filen.</span><span class="sxs-lookup"><span data-stu-id="4287d-227">`-R` indicates that this resource is included in the jar file.</span></span> <span data-ttu-id="4287d-228">Det är i roten av jar så `/writer.yaml` är sökvägen till den.</span><span class="sxs-lookup"><span data-stu-id="4287d-228">It's in the root of the jar, so `/writer.yaml` is the path to it.</span></span>

    * <span data-ttu-id="4287d-229">`--filter`: Fylla poster i den `writer.yaml` topologi med värdena i den `dev.properties` filen.</span><span class="sxs-lookup"><span data-stu-id="4287d-229">`--filter`: Populate entries in the `writer.yaml` topology using values in the `dev.properties` file.</span></span> <span data-ttu-id="4287d-230">Exempelvis värdet för den `kafka.topic` post i filen används för att ersätta den `${kafka.topic}` post i topologin definition.</span><span class="sxs-lookup"><span data-stu-id="4287d-230">For example, the value of the `kafka.topic` entry in the file is used to replace the `${kafka.topic}` entry in the topology definition.</span></span>

5. <span data-ttu-id="4287d-231">När topologin har börjat använder du följande kommando för att kontrollera att den data skrivs till avsnittet Kafka:</span><span class="sxs-lookup"><span data-stu-id="4287d-231">Once the topology has started, use the following command to verify that it is writing data to the Kafka topic:</span></span>

  ```bash
  /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $KAFKAZKHOSTS --from-beginning --topic stormtopic
  ```

    <span data-ttu-id="4287d-232">Ersätt `$KAFKAZKHOSTS` med Zookeeper värd för information som du hämtade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4287d-232">Replace `$KAFKAZKHOSTS` with the Zookeeper host information you retrieved in the previous section.</span></span>

    <span data-ttu-id="4287d-233">Det här kommandot använder ett skript som medföljer Kafka för att övervaka avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4287d-233">This command uses a script shipped with Kafka to monitor the topic.</span></span> <span data-ttu-id="4287d-234">Efter ett tag och bör börja returnera slumpmässiga meningar som har skrivits till ämnet.</span><span class="sxs-lookup"><span data-stu-id="4287d-234">After a moment, it should start returning random sentences that have been written to the topic.</span></span> <span data-ttu-id="4287d-235">Utdata ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="4287d-235">The output is similar to the following example:</span></span>

        i am at two with nature             
        an apple a day keeps the doctor away
        snow white and the seven dwarfs     
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        the cow jumped over the moon        
        an apple a day keeps the doctor away
        an apple a day keeps the doctor away
        four score and seven years ago      
        snow white and the seven dwarfs     
        snow white and the seven dwarfs     
        i am at two with nature             
        an apple a day keeps the doctor away

    <span data-ttu-id="4287d-236">Använd Ctrl + c för att stoppa skriptet.</span><span class="sxs-lookup"><span data-stu-id="4287d-236">Use Ctrl+c to stop the script.</span></span>

## <a name="start-the-reader"></a><span data-ttu-id="4287d-237">Starta läsaren</span><span class="sxs-lookup"><span data-stu-id="4287d-237">Start the reader</span></span>

1. <span data-ttu-id="4287d-238">Från SSH-session till Storm-kluster använder du följande kommando för att starta reader-topologi:</span><span class="sxs-lookup"><span data-stu-id="4287d-238">From the SSH session to the Storm cluster, use the following command to start the reader topology:</span></span>

  ```bash
  storm jar KafkaTopology-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties
  ```

2. <span data-ttu-id="4287d-239">När topologin startar öppna Storm-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="4287d-239">Once the topology starts, open the Storm UI.</span></span> <span data-ttu-id="4287d-240">Den här webbgränssnittet finns i https://storm-BASENAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="4287d-240">This web UI is located at https://storm-BASENAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="4287d-241">Ersätt __BASENAME__ med det grundläggande namnet som används när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="4287d-241">Replace __BASENAME__ with the base name used when the cluster was created.</span></span> 

    <span data-ttu-id="4287d-242">När du uppmanas, Använd inloggningsnamnet admin (standard `admin`) och lösenord som används när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="4287d-242">When prompted, use the admin login name (default, `admin`) and password used when the cluster was created.</span></span> <span data-ttu-id="4287d-243">Du ser en webbsida som liknar följande bild:</span><span class="sxs-lookup"><span data-stu-id="4287d-243">You see a web page similar to the following image:</span></span>

    ![Storm UI](./media/hdinsight-apache-storm-with-kafka/stormui.png)

3. <span data-ttu-id="4287d-245">Storm-Användargränssnittet, Välj den __kafka läsare__ länken i den __Topology Summary__ avsnitt för att visa information om den __kafka läsare__ topologi.</span><span class="sxs-lookup"><span data-stu-id="4287d-245">From the Storm UI, select the __kafka-reader__ link in the __Topology Summary__ section to display information about the __kafka-reader__ topology.</span></span>

    ![Topologi sammanfattningen i Storm webbgränssnittet](./media/hdinsight-apache-storm-with-kafka/topology-summary.png)

4. <span data-ttu-id="4287d-247">Om du vill visa information om instanser av komponenten loggaren bult, Välj den __loggaren bult__ länken i den __bultar (hela tiden)__ avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4287d-247">To display information about the instances of the logger-bolt component, select the __logger-bolt__ link in the __Bolts (All time)__ section.</span></span>

    ![Loggaren bult länk i avsnittet bultar](./media/hdinsight-apache-storm-with-kafka/bolts.png)

5. <span data-ttu-id="4287d-249">I den __Executors__ väljer du en länk i den __Port__ kolumn som ska visas logga information om den här instansen av komponenten.</span><span class="sxs-lookup"><span data-stu-id="4287d-249">In the __Executors__ section, select a link in the __Port__ column to display logging information about this instance of the component.</span></span>

    ![Executors länk](./media/hdinsight-apache-storm-with-kafka/executors.png)

    <span data-ttu-id="4287d-251">Loggen innehåller en logg över data läses från avsnittet Kafka.</span><span class="sxs-lookup"><span data-stu-id="4287d-251">The log contains a log of the data read from the Kafka topic.</span></span> <span data-ttu-id="4287d-252">Informationen i loggen liknar följande:</span><span class="sxs-lookup"><span data-stu-id="4287d-252">The information in the log is similar to the following text:</span></span>

        2016-11-04 17:47:14.907 c.m.e.LoggerBolt [INFO] Received data: four score and seven years ago
        2016-11-04 17:47:14.907 STDIO [INFO] the cow jumped over the moon
        2016-11-04 17:47:14.908 c.m.e.LoggerBolt [INFO] Received data: the cow jumped over the moon
        2016-11-04 17:47:14.911 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.911 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.932 STDIO [INFO] snow white and the seven dwarfs
        2016-11-04 17:47:14.932 c.m.e.LoggerBolt [INFO] Received data: snow white and the seven dwarfs
        2016-11-04 17:47:14.969 STDIO [INFO] an apple a day keeps the doctor away
        2016-11-04 17:47:14.970 c.m.e.LoggerBolt [INFO] Received data: an apple a day keeps the doctor away

## <a name="stop-the-topologies"></a><span data-ttu-id="4287d-253">Stoppa topologierna</span><span class="sxs-lookup"><span data-stu-id="4287d-253">Stop the topologies</span></span>

<span data-ttu-id="4287d-254">Använd följande kommandon från en SSH-session till Storm-kluster för att stoppa Storm-topologier:</span><span class="sxs-lookup"><span data-stu-id="4287d-254">From an SSH session to the Storm cluster, use the following commands to stop the Storm topologies:</span></span>

  ```bash
  storm kill kafka-writer
  storm kill kafka-reader
  ```

## <a name="delete-the-cluster"></a><span data-ttu-id="4287d-255">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="4287d-255">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="4287d-256">Eftersom stegen i det här dokumentet skapa båda klustren i samma Azure resursgrupp måste du ta bort resursgruppen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4287d-256">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="4287d-257">Ta bort resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="4287d-257">Deleting the resource group removes all resources created by following this document.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4287d-258">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4287d-258">Next steps</span></span>

<span data-ttu-id="4287d-259">Flera exempeltopologier som kan användas med Storm på HDInsight finns [exempel Storm-topologier och komponenter](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="4287d-259">For more example topologies that can be used with Storm on HDInsight, see [Example Storm topologies and components](hdinsight-storm-example-topology.md).</span></span>

<span data-ttu-id="4287d-260">Mer information om distribuera och övervaka topologier på Linux-baserat HDInsight finns [distribuera och hantera Apache Storm-topologier på Linux-baserat HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="4287d-260">For information on deploying and monitoring topologies on Linux-based HDInsight, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>