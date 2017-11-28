---
title: "aaaUse Apache Spark streaming med Händelsehubbar i Azure HDInsight | Microsoft Docs"
description: "Skapa ett Apache Spark streaming exempel på hur toosend data strömma tooAzure Event Hub och ta emot dessa händelser i HDInsight Spark-kluster med ett scala-program."
keywords: Apache spark streaming, spark streaming, spark-exemplet, apache spark streaming exempel event hub azure exempel, spark-exempel
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 10cc5884047b3b8249fe8a8822a16a19780a4af3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="ba502-104">Apache Spark streaming: bearbetning av data från Azure Event Hubs med Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-104">Apache Spark streaming: Process data from Azure Event Hubs with Spark cluster on HDInsight</span></span>

<span data-ttu-id="ba502-105">I den här artikeln kan du skapa ett Apache Spark streaming prov som inbegriper hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba502-105">In this article, you create an Apache Spark streaming sample that involves hello following steps:</span></span>

1. <span data-ttu-id="ba502-106">Du kan använda ett fristående program tooingest meddelanden i en Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="ba502-106">You use a standalone application tooingest messages into an Azure Event Hub.</span></span>

2. <span data-ttu-id="ba502-107">Med två olika sätt hämta hälsningsmeddelande från Event Hub i realtid med hjälp av ett program som körs i Spark-kluster på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ba502-107">With two different approaches, you retrieve hello messages from Event Hub in real-time using an application running in Spark cluster on Azure HDInsight.</span></span>

3. <span data-ttu-id="ba502-108">Du skapar strömmande analytiska pipelines toopersist data toodifferent lagringssystem eller kunskap genom statistik från data på hello direkt.</span><span class="sxs-lookup"><span data-stu-id="ba502-108">You build streaming analytic pipelines toopersist data toodifferent storage systems, or get insights from data on hello fly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ba502-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ba502-109">Prerequisites</span></span>

* <span data-ttu-id="ba502-110">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ba502-110">An Azure subscription.</span></span> <span data-ttu-id="ba502-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ba502-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="ba502-112">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ba502-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="ba502-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="spark-streaming-concepts"></a><span data-ttu-id="ba502-114">Spark Streaming begrepp</span><span class="sxs-lookup"><span data-stu-id="ba502-114">Spark Streaming concepts</span></span>

<span data-ttu-id="ba502-115">En detaljerad förklaring av Spark streaming finns [Apache Spark streaming översikt](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span><span class="sxs-lookup"><span data-stu-id="ba502-115">For a detailed explanation of Spark streaming, see [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span></span> <span data-ttu-id="ba502-116">HDInsight ger hello samma strömmande funktioner tooa Spark-kluster i Azure.</span><span class="sxs-lookup"><span data-stu-id="ba502-116">HDInsight brings hello same streaming features tooa Spark cluster on Azure.</span></span>  

## <a name="what-does-this-solution-do"></a><span data-ttu-id="ba502-117">Vad är den här lösningen?</span><span class="sxs-lookup"><span data-stu-id="ba502-117">What does this solution do?</span></span>

<span data-ttu-id="ba502-118">Utför hello följa stegen i den här artikeln toocreate en Spark streaming exempelvis:</span><span class="sxs-lookup"><span data-stu-id="ba502-118">In this article, toocreate a Spark streaming example, perform hello following steps:</span></span>

1. <span data-ttu-id="ba502-119">Skapa en Azure-Händelsehubb som tar emot en dataström med händelser.</span><span class="sxs-lookup"><span data-stu-id="ba502-119">Create an Azure Event Hub that will receive a stream of events.</span></span>

2. <span data-ttu-id="ba502-120">Kör ett lokala fristående program som genererar händelser och skickar den toohello Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="ba502-120">Run a local standalone application that generates events and pushes it toohello Azure Event Hub.</span></span> <span data-ttu-id="ba502-121">hello exempelprogram som gör detta publiceras på [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="ba502-121">hello sample application that does this is published at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span>

3. <span data-ttu-id="ba502-122">Kör en strömmande via fjärranslutning på ett Spark-kluster som läser strömningen av händelser från Azure Event Hub och utföra olika databearbetning/analys.</span><span class="sxs-lookup"><span data-stu-id="ba502-122">Run a streaming application remotely on a Spark cluster that reads streaming events from Azure Event Hub and perform various data processing/analysis.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="ba502-123">Skapa ett Azure Event Hub</span><span class="sxs-lookup"><span data-stu-id="ba502-123">Create an Azure Event Hub</span></span>

1. <span data-ttu-id="ba502-124">Logga in toohello [Azure Portal](https://ms.portal.azure.com), och klicka på **ny** på hello upp till vänster i hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="ba502-124">Log on toohello [Azure Portal](https://ms.portal.azure.com), and click **New** at hello top left of hello screen.</span></span>

2. <span data-ttu-id="ba502-125">Klicka på **Sakernas Internet** och sedan på **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="ba502-125">Click **Internet of Things**, then click **Event Hubs**.</span></span>

    <span data-ttu-id="ba502-126">![Skapa händelsehubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "skapa händelsehubb för Spark streaming exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-126">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")</span></span>

3. <span data-ttu-id="ba502-127">I hello **skapa namnområdet** bladet, ange ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="ba502-127">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="ba502-128">Välj hello prisnivån (Basic eller Standard).</span><span class="sxs-lookup"><span data-stu-id="ba502-128">choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="ba502-129">Också välja en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="ba502-129">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="ba502-130">Klicka på **skapa** toocreate hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="ba502-130">Click **Create** toocreate hello namespace.</span></span>

      <span data-ttu-id="ba502-131">![Ange ett händelsenamn hubb Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "ger en händelsehubbens namn Spark streaming exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-131">![Provide an event hub name for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")</span></span>

    > [!NOTE]
    > <span data-ttu-id="ba502-132">Du ska markera hello samma **plats** som Apache Spark-kluster i HDInsight tooreduce svarstid och kostnader.</span><span class="sxs-lookup"><span data-stu-id="ba502-132">You should select hello same **Location** as your Apache Spark cluster in HDInsight tooreduce latency and costs.</span></span>
    >
    >

4. <span data-ttu-id="ba502-133">Klicka på hello nyskapade namnområde i hello Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="ba502-133">In hello Event Hubs namespace list, click hello newly-created namespace.</span></span>      


5. <span data-ttu-id="ba502-134">I hello namnområde bladet, klickar du på **Händelsehubbar**, och klicka sedan på **+ Event Hub** toocreate en ny Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="ba502-134">In hello namespace blade, click **Event Hubs**, and then click **+ Event Hub** toocreate a new Event Hub.</span></span>
   
    <span data-ttu-id="ba502-135">![Skapa händelsehubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "skapa händelsehubb för Spark streaming exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-135">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Create event hub for Spark streaming example")</span></span>

6. <span data-ttu-id="ba502-136">Ange ett namn för din Event Hub, ange hello partition antal too10 och meddelandet kvarhållning too1.</span><span class="sxs-lookup"><span data-stu-id="ba502-136">Type a name for your Event Hub, set hello partition count too10, and message retention too1.</span></span> <span data-ttu-id="ba502-137">Vi inte arkiverar hälsningsmeddelande i den här lösningen så att du kan lämna hello rest som standard och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="ba502-137">We are not archiving hello messages in this solution so you can leave hello rest as default, and then click **Create**.</span></span>
   
    <span data-ttu-id="ba502-138">![Ange händelseinformationen hubb för Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "ange händelseinformationen hubb för Spark streaming exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-138">![Provide event hub details for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")</span></span>

7. <span data-ttu-id="ba502-139">hello nyskapad Event Hub listas i hello Event Hub-blad.</span><span class="sxs-lookup"><span data-stu-id="ba502-139">hello newly created Event Hub is listed in hello Event Hub blade.</span></span>
    
     <span data-ttu-id="ba502-140">![Visa Event Hub hello Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub för hello Väck strömmande exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-140">![View Event Hub for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub for hello Spark streaming example")</span></span>

8. <span data-ttu-id="ba502-141">Klicka på tillbaka i hello namnområde bladet (inte hello specifik Händelsehubb bladet) **principer för delad åtkomst**, och klicka sedan på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="ba502-141">Back in hello namespace blade (not hello specific Event Hub blade), click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
     <span data-ttu-id="ba502-142">![Ange Event Hub-principer t.ex hello Spark streaming](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "ange Event Hub principer för hello Väck strömmande exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-142">![Set Event Hub policies for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for hello Spark streaming example")</span></span>

9. <span data-ttu-id="ba502-143">Klicka på hello Kopiera knappen toocopy hello **RootManageSharedAccessKey** primära nyckel och anslutningen sträng toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="ba502-143">Click hello copy button toocopy hello **RootManageSharedAccessKey** primary key and connection string toohello clipboard.</span></span> <span data-ttu-id="ba502-144">Spara dessa toouse senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="ba502-144">Save these toouse later in hello tutorial.</span></span>
    
     <span data-ttu-id="ba502-145">![Visa Event Hub princip nycklar hello Spark streaming exempel](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub princip nycklar för hello Väck strömmande exempel")</span><span class="sxs-lookup"><span data-stu-id="ba502-145">![View Event Hub policy keys for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for hello Spark streaming example")</span></span>

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a><span data-ttu-id="ba502-146">Skicka meddelanden tooAzure Händelsehubb med hjälp av ett exempelprogram Scala</span><span class="sxs-lookup"><span data-stu-id="ba502-146">Send messages tooAzure Event Hub using a sample Scala application</span></span>

<span data-ttu-id="ba502-147">I det här avsnittet använder du en fristående-lokala Scala-program som genererar en dataström med händelser och skickar den tooAzure Händelsehubb som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ba502-147">In this section you use a standalone local Scala application that generates a stream of events and sends it tooAzure Event Hub that you created earlier.</span></span> <span data-ttu-id="ba502-148">Det här programmet är tillgängligt på GitHub på [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span><span class="sxs-lookup"><span data-stu-id="ba502-148">This application is available on GitHub at [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span></span> <span data-ttu-id="ba502-149">hello stegen här förutsätter att du redan har forked GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ba502-149">hello steps here assume that you have already forked this GitHub repository.</span></span>

1. <span data-ttu-id="ba502-150">Kontrollera att du har hello följande installerat på hello datorn där du kör det här programmet.</span><span class="sxs-lookup"><span data-stu-id="ba502-150">Make sure you have hello following installed on hello computer where you run this application.</span></span>

    * <span data-ttu-id="ba502-151">Oracle Java Development kit.</span><span class="sxs-lookup"><span data-stu-id="ba502-151">Oracle Java Development kit.</span></span> <span data-ttu-id="ba502-152">Du kan installera den från [här](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="ba502-152">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
    * <span data-ttu-id="ba502-153">Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="ba502-153">Apache Maven.</span></span> <span data-ttu-id="ba502-154">Du kan ladda ned det från [här](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="ba502-154">You can download it from [here](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="ba502-155">Instruktioner tooinstall Maven finns [här](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="ba502-155">Instructions tooinstall Maven are available [here](https://maven.apache.org/install.html).</span></span>

2. <span data-ttu-id="ba502-156">Öppna en kommandotolk och navigera toohello plats som du har klonat hello GitHub-repo hello Scala exempelprogram och kör följande kommando toobuild hello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="ba502-156">Open a command prompt and navigate toohello location you cloned hello GitHub repo for hello sample Scala application and run hello following command toobuild hello application.</span></span>

        mvn package

3. <span data-ttu-id="ba502-157">hello utdata jar för programmet hello **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, skapas under **/target** directory.</span><span class="sxs-lookup"><span data-stu-id="ba502-157">hello output jar for hello application, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, is created under **/target** directory.</span></span> <span data-ttu-id="ba502-158">Du kan använda den här JAR senare i den här artikeln tootest hello komplett lösning.</span><span class="sxs-lookup"><span data-stu-id="ba502-158">You use this JAR later in this article tootest hello complete solution.</span></span>

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a><span data-ttu-id="ba502-159">Skapa program tooreceive meddelanden från Event Hub i ett Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba502-159">Create application tooreceive messages from Event Hub into a Spark cluster</span></span> 

<span data-ttu-id="ba502-160">Vi har två metoder tooconnect Spark Streaming och Azure Event Hubs, mottagar-baserade anslutningen och Direct-DStream-baserad anslutning.</span><span class="sxs-lookup"><span data-stu-id="ba502-160">We have two approaches tooconnect Spark Streaming and Azure Event Hubs, Receiver-based connection and Direct-DStream-based connection.</span></span> <span data-ttu-id="ba502-161">Direct-DStream-baserade introduceras på januari 2017 i hello 2.0.3 versionen.</span><span class="sxs-lookup"><span data-stu-id="ba502-161">Direct-DStream-based is introduced on Jan of 2017, in hello 2.0.3 release.</span></span> <span data-ttu-id="ba502-162">Den bör tooreplace hello ursprungliga mottagar-baserade anslutningen eftersom det är mer performant och resurseffektiv.</span><span class="sxs-lookup"><span data-stu-id="ba502-162">It is supposed tooreplace hello original receiver-based connection as it is more performant and resource-efficient.</span></span> <span data-ttu-id="ba502-163">Mer information finns i [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span><span class="sxs-lookup"><span data-stu-id="ba502-163">More details found in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span></span> <span data-ttu-id="ba502-164">Direkt DStream stöder endast Spark 2.0 +.</span><span class="sxs-lookup"><span data-stu-id="ba502-164">Direct DStream only supports Spark 2.0+.</span></span>

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a><span data-ttu-id="ba502-165">Skapa program med hello beroende toospark eventhubs connector</span><span class="sxs-lookup"><span data-stu-id="ba502-165">Build applications with hello dependency toospark-eventhubs connector</span></span>

<span data-ttu-id="ba502-166">Vi kommer även att publicera hello mellanlagring version av Spark-EventHubs i GitHub.</span><span class="sxs-lookup"><span data-stu-id="ba502-166">We will also publish hello staging version of Spark-EventHubs in GitHub.</span></span> <span data-ttu-id="ba502-167">toouse hello fristående versionen av Spark EventHubs, hello första steget är tooindicate GitHub som hello källa lagringsplatsen genom att lägga till följande post toopom.xml hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-167">toouse hello staging version of Spark-EventHubs, hello first step is tooindicate GitHub as hello source repo by adding hello following entry toopom.xml:</span></span>

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

<span data-ttu-id="ba502-168">Du kan sedan lägga till hello följande beroende tooyour projekt tootake hello före utgivna versionen.</span><span class="sxs-lookup"><span data-stu-id="ba502-168">You can then add hello following dependency tooyour project tootake hello pre-released version.</span></span>

<span data-ttu-id="ba502-169">Maven beroende</span><span class="sxs-lookup"><span data-stu-id="ba502-169">Maven Dependency</span></span>

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

<span data-ttu-id="ba502-170">Segregerade Barlasttankar beroende</span><span class="sxs-lookup"><span data-stu-id="ba502-170">SBT Dependency</span></span>

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a><span data-ttu-id="ba502-171">Direkt DStream anslutning</span><span class="sxs-lookup"><span data-stu-id="ba502-171">Direct DStream Connection</span></span>

<span data-ttu-id="ba502-172">En förskapad jar-fil som innehåller exempel som använder direkt DStream kan hämtas i [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span><span class="sxs-lookup"><span data-stu-id="ba502-172">A pre-built jar file containing examples using Direct DStream can be downloaded in [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span></span>

<span data-ttu-id="ba502-173">hello jar-filen innehåller tre exempel vars källkoden finns på [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span><span class="sxs-lookup"><span data-stu-id="ba502-173">hello jar file contains three examples whose source code are available at [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span></span>

<span data-ttu-id="ba502-174">Tar [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) som exempel:</span><span class="sxs-lookup"><span data-stu-id="ba502-174">Taking [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) as an example:</span></span>

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

<span data-ttu-id="ba502-175">I hello ovan exempelvis `eventhubParameters` hello parametrar specifika tooa EventHubs instans och du har toopass den toohello `createDirectStreams` API som skapar ett direkt DStream objektet mappning tooa Händelsehubbar namnområde.</span><span class="sxs-lookup"><span data-stu-id="ba502-175">In hello above example, `eventhubParameters` are hello parameters specific tooa single EventHubs instance and you have toopass it toohello `createDirectStreams` API which constructs a Direct DStream object mapping tooa Event Hubs namespace.</span></span> <span data-ttu-id="ba502-176">Du kan anropa DStream API: er som tillhandahålls av Spark Streaming API framework över hello direkt DStream objekt.</span><span class="sxs-lookup"><span data-stu-id="ba502-176">Over hello Direct DStream object, you can call any DStream API provided by Spark Streaming API framework.</span></span> <span data-ttu-id="ba502-177">I det här exemplet beräkna vi hello frekvensen för varje ord i hello sista 3 micro batch intervall.</span><span class="sxs-lookup"><span data-stu-id="ba502-177">In this example, we calculate hello frequency of each word within hello last 3 micro batch intervals.</span></span>

### <a name="receiver-based-connection"></a><span data-ttu-id="ba502-178">Mottagaren-baserade anslutningen</span><span class="sxs-lookup"><span data-stu-id="ba502-178">Receiver-based Connection</span></span>

<span data-ttu-id="ba502-179">En Spark streaming exempelprogram som skrivits i Scala som tar emot händelser och väg hello toodifferent mål, finns på [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="ba502-179">A Spark streaming example application written in Scala, which receives events and route hello toodifferent destinations, is available at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span> <span data-ttu-id="ba502-180">Gör hello under tooupdate hello programmet för din Event Hub-konfiguration och skapa hello utdata jar.</span><span class="sxs-lookup"><span data-stu-id="ba502-180">Follow hello steps below tooupdate hello application for your Event Hub configuration and create hello output jar.</span></span>

1. <span data-ttu-id="ba502-181">Starta IntelliJ IDEA och från hello Starta skärmen Välj **checka ut från versionskontroll** och klicka sedan på **Git**.</span><span class="sxs-lookup"><span data-stu-id="ba502-181">Launch IntelliJ IDEA and from hello launch screen select **Check out from Version Control** and then click **Git**.</span></span>
   
    <span data-ttu-id="ba502-182">![Apache Spark streaming exempel – get källor från Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming exempel – get källor från Git")</span><span class="sxs-lookup"><span data-stu-id="ba502-182">![Apache Spark streaming example - get sources from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming example - get sources from Git")</span></span>

2. <span data-ttu-id="ba502-183">I hello **klona databasen** dialogrutan Ange hello URL toohello Git-lagringsplatsen tooclone från ange hello directory tooclone till och klicka sedan på **klona**.</span><span class="sxs-lookup"><span data-stu-id="ba502-183">In hello **Clone Repository** dialog box, provide hello URL toohello Git repository tooclone from, specify hello directory tooclone to, and then click **Clone**.</span></span>
   
    <span data-ttu-id="ba502-184">![Apache Spark streaming exempel – klona från Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming exempel – klona från Git")</span><span class="sxs-lookup"><span data-stu-id="ba502-184">![Apache Spark streaming example - clone from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming example - clone from Git")</span></span>
3. <span data-ttu-id="ba502-185">Följ hello prompter till hello projekt helt klonas.</span><span class="sxs-lookup"><span data-stu-id="ba502-185">Follow hello prompts till hello project is completely cloned.</span></span> <span data-ttu-id="ba502-186">Tryck på **Alt + 1** tooopen hello **projektvyn**.</span><span class="sxs-lookup"><span data-stu-id="ba502-186">Press **Alt + 1** tooopen hello **Project View**.</span></span> <span data-ttu-id="ba502-187">Det bör likna följande hello.</span><span class="sxs-lookup"><span data-stu-id="ba502-187">It should resemble hello following.</span></span>
   
    <span data-ttu-id="ba502-188">![Apache Spark streaming exempel – projektvyn](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming exempel – projektvyn")</span><span class="sxs-lookup"><span data-stu-id="ba502-188">![Apache Spark streaming example - Project View](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming example - Project View")</span></span>
4. <span data-ttu-id="ba502-189">Kontrollera att hello programkod kompileras med Java8.</span><span class="sxs-lookup"><span data-stu-id="ba502-189">Make sure hello application code is compiled with Java8.</span></span> <span data-ttu-id="ba502-190">tooensure, genom att välja **filen**, klickar du på **projektstruktur**, och på hello **projekt** kontrollerar du att projektet språket är inställd för**8 - Lambdas, typ anteckningar, etc.**.</span><span class="sxs-lookup"><span data-stu-id="ba502-190">tooensure this, click **File**, click **Project Structure**, and on hello **Project** tab, make sure Project language level is set too**8 - Lambdas, type annotations, etc.**.</span></span>
   
    <span data-ttu-id="ba502-191">![Apache Spark streaming exempel - Set-kompilatorn](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming exempel - Set-kompilatorn")</span><span class="sxs-lookup"><span data-stu-id="ba502-191">![Apache Spark streaming example - Set compiler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming example - Set compiler")</span></span>
5. <span data-ttu-id="ba502-192">Öppna hello **pom.xml** och kontrollera hello Spark-versionen är korrekt.</span><span class="sxs-lookup"><span data-stu-id="ba502-192">Open hello **pom.xml** and make sure hello Spark version is correct.</span></span> <span data-ttu-id="ba502-193">Under `<properties>` , leta efter hello följande fragment och verifiera hello Spark-version.</span><span class="sxs-lookup"><span data-stu-id="ba502-193">Under `<properties>` node, look for hello following snippet and verify hello Spark version.</span></span>

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. <span data-ttu-id="ba502-194">hello-programmet kräver en beroende jar kallas **JDBC driver jar**.</span><span class="sxs-lookup"><span data-stu-id="ba502-194">hello application requires a dependency jar called **JDBC driver jar**.</span></span> <span data-ttu-id="ba502-195">Detta är obligatorisk toowrite hälsningsmeddelande togs emot från Event Hub till en Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ba502-195">This is required toowrite hello messages received from Event Hub into an Azure SQL database.</span></span> <span data-ttu-id="ba502-196">Du kan hämta den här jar (v4.1 eller senare) från [här](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span><span class="sxs-lookup"><span data-stu-id="ba502-196">You can download this jar (v4.1 or later) from [here](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span></span> <span data-ttu-id="ba502-197">Lägg till referens toothis jar i hello projekt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ba502-197">Add reference toothis jar in hello project library.</span></span> <span data-ttu-id="ba502-198">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-198">Perform hello following steps:</span></span>
     
     1. <span data-ttu-id="ba502-199">IntelliJ IDEA fönster där du har hello-programmet öppnas, klickar du på **filen**, klickar du på **projektstruktur**, och klicka sedan på **bibliotek**.</span><span class="sxs-lookup"><span data-stu-id="ba502-199">From IntelliJ IDEA window where you have hello application open, click **File**, click **Project Structure**, and then click **Libraries**.</span></span> 
     2. <span data-ttu-id="ba502-200">Klicka på hello lägga till ikonen (![lägga till ikonen](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), klickar du på **Java**, och gå sedan toohello plats där du sparade hello JDBC driver jar.</span><span class="sxs-lookup"><span data-stu-id="ba502-200">Click hello add icon (![add icon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), click **Java**, and then navigate toohello location where you downloaded hello JDBC driver jar.</span></span> <span data-ttu-id="ba502-201">Följ hello prompter tooadd hello jar-filen toohello projekt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ba502-201">Follow hello prompts tooadd hello jar file toohello project library.</span></span>

         <span data-ttu-id="ba502-202">![lägga till beroenden som saknas](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "lägga till den saknade beroende burkar")</span><span class="sxs-lookup"><span data-stu-id="ba502-202">![add missing dependencies](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Add missing dependency jars")</span></span>
     3. <span data-ttu-id="ba502-203">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="ba502-203">Click **Apply**.</span></span>

7. <span data-ttu-id="ba502-204">Skapa hello utdata jar-filen.</span><span class="sxs-lookup"><span data-stu-id="ba502-204">Create hello output jar file.</span></span> <span data-ttu-id="ba502-205">Utför följande steg hello.</span><span class="sxs-lookup"><span data-stu-id="ba502-205">Perform hello following steps.</span></span>

   1. <span data-ttu-id="ba502-206">I hello **projektstruktur** dialogrutan klickar du på **artefakter** och klicka sedan på hello plus symbolen.</span><span class="sxs-lookup"><span data-stu-id="ba502-206">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="ba502-207">Hello popup-menyn klickar du på **JAR**, och klicka sedan på **från moduler med beroenden**.</span><span class="sxs-lookup"><span data-stu-id="ba502-207">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>      
       
       <span data-ttu-id="ba502-208">![Apache Spark streaming exempel – skapa JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming exempel – skapa JAR")</span><span class="sxs-lookup"><span data-stu-id="ba502-208">![Apache Spark streaming example - create JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming example - create JAR")</span></span>
   2. <span data-ttu-id="ba502-209">I hello **skapa JAR från moduler** dialogrutan klickar du på ellipsknappen hello (![tre punkter](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) mot hello **Main klassen**.</span><span class="sxs-lookup"><span data-stu-id="ba502-209">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) against hello **Main Class**.</span></span>
   3. <span data-ttu-id="ba502-210">I hello **Välj Main klassen** dialogrutan väljer någon av hello tillgängliga klasser och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba502-210">In hello **Select Main Class** dialog box, select any of hello available classes and then click **OK**.</span></span>
      
       <span data-ttu-id="ba502-211">![Apache Spark streaming exempel – Välj klass för jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming exempel – Välj klass för jar")</span><span class="sxs-lookup"><span data-stu-id="ba502-211">![Apache Spark streaming example - select class for jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming example - select class for jar")</span></span>
   4. <span data-ttu-id="ba502-212">I hello **skapa JAR från moduler** dialogrutan Kontrollera hello alternativet för**extrahera toohello mål JAR** är markerad och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba502-212">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="ba502-213">Detta skapar en enda JAR med alla beroenden.</span><span class="sxs-lookup"><span data-stu-id="ba502-213">This creates a single JAR with all dependencies.</span></span>
      
       <span data-ttu-id="ba502-214">![Apache Spark streaming exempel – skapa jar från moduler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming exempel – skapa jar från moduler")</span><span class="sxs-lookup"><span data-stu-id="ba502-214">![Apache Spark streaming example - create jar from modules](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming example - create jar from modules")</span></span>
   5. <span data-ttu-id="ba502-215">Hej **utdata Layout** fliken listar alla hello burkar som ingår som en del av hello Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="ba502-215">hello **Output Layout** tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="ba502-216">Du kan välja och ta bort hello sådana där hello Scala program har ingen direkt beroende.</span><span class="sxs-lookup"><span data-stu-id="ba502-216">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="ba502-217">För hello program skapar vi här, kan du ta bort alla utom hello senast (**spark-streaming-data-beständiga-exempel kompilera utdata**).</span><span class="sxs-lookup"><span data-stu-id="ba502-217">For hello application we are creating here, you can remove all but hello last one (**spark-streaming-data-persistence-examples compile output**).</span></span> <span data-ttu-id="ba502-218">Välj hello burkar toodelete och klicka sedan på hello **ta bort** ikon (![ikonen Ta bort](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span><span class="sxs-lookup"><span data-stu-id="ba502-218">Select hello jars toodelete and then click hello **Delete** icon (![delete icon](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span></span>
      
       <span data-ttu-id="ba502-219">![Apache Spark streaming exempel – ta bort extraherade burkar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming exempel – ta bort extraherade burkar")</span><span class="sxs-lookup"><span data-stu-id="ba502-219">![Apache Spark streaming example - delete extracted jars](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming example - delete extracted jars")</span></span>
      
       <span data-ttu-id="ba502-220">Kontrollera att **bygger på Kontrollera** är markerad, vilket säkerställer att hello jar skapas varje gång hello projektet skapats eller uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="ba502-220">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="ba502-221">Klicka på **Använd**.</span><span class="sxs-lookup"><span data-stu-id="ba502-221">Click **Apply**.</span></span>
   6. <span data-ttu-id="ba502-222">I hello **utdata Layout** fliken höger längst hello hello **element** finns hello SQL JDBC jar som du lagt till tidigare toohello projekt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="ba502-222">In hello **Output Layout** tab, right at hello bottom of hello **Available Elements** box, you have hello SQL JDBC jar that you added earlier toohello project library.</span></span> <span data-ttu-id="ba502-223">Du måste lägga till den här toohello **utdata Layout** fliken. Högerklicka på hello jar-filen och klicka sedan på **extrahera roten till utdata**.</span><span class="sxs-lookup"><span data-stu-id="ba502-223">You must add this toohello **Output Layout** tab. Right-click hello jar file, and then click **Extract Into Output Root**.</span></span>
      
       <span data-ttu-id="ba502-224">![Apache Spark streaming exempel – extrahera beroende jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming exempel – extrahera beroende jar")</span><span class="sxs-lookup"><span data-stu-id="ba502-224">![Apache Spark streaming example - extract dependency jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming example - extract dependency jar")</span></span>  
      
       <span data-ttu-id="ba502-225">Hej **utdata Layout** fliken bör nu se ut så här.</span><span class="sxs-lookup"><span data-stu-id="ba502-225">hello **Output Layout** tab should now look like this.</span></span>
      
       <span data-ttu-id="ba502-226">![Apache Spark streaming exempel – fliken slutversionen](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming exempel – fliken slutversionen")</span><span class="sxs-lookup"><span data-stu-id="ba502-226">![Apache Spark streaming example - final output tab](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming example - final output tab")</span></span>        
      
       <span data-ttu-id="ba502-227">I hello **projektstruktur** dialogrutan klickar du på **tillämpa** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ba502-227">In hello **Project Structure** dialog box, click **Apply** and then click **OK**.</span></span>    
   7. <span data-ttu-id="ba502-228">Hello menyraden klickar du på **skapa**, och klicka sedan på **Se projektet**.</span><span class="sxs-lookup"><span data-stu-id="ba502-228">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="ba502-229">Du kan också klicka på **skapa artefakter** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="ba502-229">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="ba502-230">hello utdata jar skapas under **\classes\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="ba502-230">hello output jar is created under **\classes\artifacts**.</span></span>
      
       <span data-ttu-id="ba502-231">![Apache Spark streaming exempel - utdata JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming exempel - utdata JAR")</span><span class="sxs-lookup"><span data-stu-id="ba502-231">![Apache Spark streaming example - output JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming example - output JAR")</span></span>

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a><span data-ttu-id="ba502-232">Kör hello via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="ba502-232">Run hello application remotely on a Spark cluster using Livy</span></span>

<span data-ttu-id="ba502-233">I den här artikeln använder du Livius toorun hello Apache Spark streaming program via fjärranslutning på ett Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="ba502-233">In this article you use Livy toorun hello Apache Spark streaming application remotely on a Spark cluster.</span></span> <span data-ttu-id="ba502-234">Detaljerad information om hur toouse Livius med HDInsight Spark-kluster finns i [skicka jobb via fjärranslutning tooan Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-234">For detailed discussion on how toouse Livy with HDInsight Spark cluster, see [Submit jobs remotely tooan Apache Spark cluster on Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span> <span data-ttu-id="ba502-235">Innan du kan börja hello Spark streaming programmet körs, finns det några saker du bör göra:</span><span class="sxs-lookup"><span data-stu-id="ba502-235">Before you can start running hello Spark streaming application, there are a couple of things you should do:</span></span>

1. <span data-ttu-id="ba502-236">Starta hello lokala fristående toogenerate programhändelser och skickas tooEvent hubb.</span><span class="sxs-lookup"><span data-stu-id="ba502-236">Start hello local standalone application toogenerate events and sent tooEvent Hub.</span></span> <span data-ttu-id="ba502-237">Använd följande kommando toodo så hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-237">Use hello following command toodo so:</span></span>

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. <span data-ttu-id="ba502-238">Kopiera hello strömning jar (**spark-streaming-data-beständiga-examples.jar**) toohello Azure Blob-lagring som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="ba502-238">Copy hello streaming jar (**spark-streaming-data-persistence-examples.jar**) toohello Azure Blob storage associated with hello cluster.</span></span> <span data-ttu-id="ba502-239">Detta gör hello jar tillgänglig tooLivy.</span><span class="sxs-lookup"><span data-stu-id="ba502-239">This makes hello jar accessible tooLivy.</span></span> <span data-ttu-id="ba502-240">Du kan använda [ **AzCopy**](../storage/common/storage-use-azcopy.md), ett kommando rad toodo-verktyget så.</span><span class="sxs-lookup"><span data-stu-id="ba502-240">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="ba502-241">Det finns många andra klienter kan du använda tooupload data.</span><span class="sxs-lookup"><span data-stu-id="ba502-241">There are a lot of other clients you can use tooupload data.</span></span> <span data-ttu-id="ba502-242">Du kan hitta mer om dem i [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-242">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
3. <span data-ttu-id="ba502-243">Installera CURL på hello datorn där du kör dessa program från.</span><span class="sxs-lookup"><span data-stu-id="ba502-243">Install CURL on hello computer where you are running these applications from.</span></span> <span data-ttu-id="ba502-244">Vi använder CURL tooinvoke hello Livius slutpunkter toorun hello jobb via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="ba502-244">We use CURL tooinvoke hello Livy endpoints toorun hello jobs remotely.</span></span>

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a><span data-ttu-id="ba502-245">Kör hello Spark streaming tooreceive hello programhändelser till en Azure Storage Blob som text</span><span class="sxs-lookup"><span data-stu-id="ba502-245">Run hello Spark streaming application tooreceive hello events into an Azure Storage Blob as text</span></span>

<span data-ttu-id="ba502-246">Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-246">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="ba502-247">Hej parametrar i hello filen **inputBlob.txt** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-247">hello parameters in hello file **inputBlob.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="ba502-248">Låt oss förstå hello parametrar i hello indatafilen är:</span><span class="sxs-lookup"><span data-stu-id="ba502-248">Let us understand what hello parameters in hello input file are:</span></span>

* <span data-ttu-id="ba502-249">**filen** är hello sökvägen toohello programmet jar-filen på hello Azure storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ba502-249">**file** is hello path toohello application jar file on hello Azure storage account associated with hello cluster.</span></span>
* <span data-ttu-id="ba502-250">**className** är hello klassen i hello jar hello namn.</span><span class="sxs-lookup"><span data-stu-id="ba502-250">**className** is hello name of hello class in hello jar.</span></span>
* <span data-ttu-id="ba502-251">**argument** är hello lista över de argument som krävs av hello-klass</span><span class="sxs-lookup"><span data-stu-id="ba502-251">**args** is hello list of arguments required by hello class</span></span>
* <span data-ttu-id="ba502-252">**numExecutors** är hello antal kärnor som används av Spark toorun hello direktuppspelning av program.</span><span class="sxs-lookup"><span data-stu-id="ba502-252">**numExecutors** is hello number of cores used by Spark toorun hello streaming application.</span></span> <span data-ttu-id="ba502-253">Detta ska alltid vara minst två gånger hello antalet partitioner i Händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="ba502-253">This should always be at least twice hello number of Event Hub partitions.</span></span>
* <span data-ttu-id="ba502-254">**executorMemory**, **executorCores**, **driverMemory** är parametrar som används tooassign krävs resurser toohello strömmande program.</span><span class="sxs-lookup"><span data-stu-id="ba502-254">**executorMemory**, **executorCores**, **driverMemory** are parameters used tooassign required resources toohello streaming application.</span></span>

> [!NOTE]
> <span data-ttu-id="ba502-255">Du behöver inte toocreate hello utdatamappar (EventCheckpoint, EventCount/EventCount10) som används som parametrar.</span><span class="sxs-lookup"><span data-stu-id="ba502-255">You do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="ba502-256">direktuppspelning av programmet hello skapar dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="ba502-256">hello streaming application creates them for you.</span></span>
>
>

<span data-ttu-id="ba502-257">Du bör se utdata som liknar hello följande när du kör hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="ba502-257">When you run hello command, you should see an output like hello following:</span></span>

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact

<span data-ttu-id="ba502-258">Anteckna hello batch-ID i hello sista raden i hello utdata (i det här exemplet är det '1').</span><span class="sxs-lookup"><span data-stu-id="ba502-258">Make a note of hello batch ID in hello last line of hello output (in this example it is '1').</span></span> <span data-ttu-id="ba502-259">tooverify som hello program har körts, titta på ditt Azure storage-konto som är associerade med hello kluster och du bör se hello **/EventCount/EventCount10** mapp som skapade det.</span><span class="sxs-lookup"><span data-stu-id="ba502-259">tooverify that hello application runs successfully, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventCount/EventCount10** folder created there.</span></span> <span data-ttu-id="ba502-260">Den här mappen bör innehålla blobbar som samlar in hello antalet händelser som bearbetas inom hello tidsperiod som angetts för parametern hello **batch intervall i sekunder**.</span><span class="sxs-lookup"><span data-stu-id="ba502-260">This folder should contain blobs that captures hello number of events processed within hello time period specified for hello parameter **batch-interval-in-seconds**.</span></span>

<span data-ttu-id="ba502-261">hello Spark streaming programmet fortsätter toorun tills du avsluta den.</span><span class="sxs-lookup"><span data-stu-id="ba502-261">hello Spark streaming application will continue toorun until you kill it.</span></span> <span data-ttu-id="ba502-262">toodo så Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba502-262">toodo so, use hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a><span data-ttu-id="ba502-263">Kör hello program tooreceive hello händelser till en Azure Storage Blob som JSON</span><span class="sxs-lookup"><span data-stu-id="ba502-263">Run hello applications tooreceive hello events into an Azure Storage Blob as JSON</span></span>
<span data-ttu-id="ba502-264">Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-264">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="ba502-265">Hej parametrar i hello filen **inputJSON.txt** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-265">hello parameters in hello file **inputJSON.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="ba502-266">hello parametrar är liknande toowhat som du angav för hello text-utdata i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ba502-266">hello parameters are similar toowhat you specified for hello text output, in hello previous step.</span></span> <span data-ttu-id="ba502-267">Igen och behöver du inte toocreate hello utdatamappar (EventCheckpoint, EventCount/EventCount10) som används som parametrar.</span><span class="sxs-lookup"><span data-stu-id="ba502-267">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="ba502-268">direktuppspelning av programmet hello skapar dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="ba502-268">hello streaming application creates them for you.</span></span>

 <span data-ttu-id="ba502-269">När du kör kommandot hello kan du titta på ditt Azure storage-konto som är associerade med hello kluster och du bör se hello **/EventStore10** mapp som skapade det.</span><span class="sxs-lookup"><span data-stu-id="ba502-269">After you run hello command, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventStore10** folder created there.</span></span> <span data-ttu-id="ba502-270">Öppna en fil med prefixet **del -** och du bör se hello händelser som har bearbetats i en JSON-format.</span><span class="sxs-lookup"><span data-stu-id="ba502-270">Open any file prefixed with **part-** and you should see hello events processed in a JSON format.</span></span>

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a><span data-ttu-id="ba502-271">Köra hello program tooreceive hello händelser till en Hive-tabell</span><span class="sxs-lookup"><span data-stu-id="ba502-271">Run hello applications tooreceive hello events into a Hive table</span></span>
<span data-ttu-id="ba502-272">toorun hello Spark streaming program som dataströmmar händelser till en annan registreringsdatafil tabell du måste vissa ytterligare komponenter.</span><span class="sxs-lookup"><span data-stu-id="ba502-272">toorun hello Spark streaming application that streams events into a Hive table you need some additional components.</span></span> <span data-ttu-id="ba502-273">Dessa är:</span><span class="sxs-lookup"><span data-stu-id="ba502-273">These are:</span></span>

* <span data-ttu-id="ba502-274">datanucleus-api-jdo-3.2.6.jar</span><span class="sxs-lookup"><span data-stu-id="ba502-274">datanucleus-api-jdo-3.2.6.jar</span></span>
* <span data-ttu-id="ba502-275">datanucleus-rdbms-3.2.9.jar</span><span class="sxs-lookup"><span data-stu-id="ba502-275">datanucleus-rdbms-3.2.9.jar</span></span>
* <span data-ttu-id="ba502-276">datanucleus-core-3.2.10.jar</span><span class="sxs-lookup"><span data-stu-id="ba502-276">datanucleus-core-3.2.10.jar</span></span>
* <span data-ttu-id="ba502-277">hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="ba502-277">hive-site.xml</span></span>

<span data-ttu-id="ba502-278">Hej **.jar** filer är tillgängliga på HDInsight Spark-kluster på `/usr/hdp/current/spark-client/lib`.</span><span class="sxs-lookup"><span data-stu-id="ba502-278">hello **.jar** files are available on your HDInsight Spark cluster at `/usr/hdp/current/spark-client/lib`.</span></span> <span data-ttu-id="ba502-279">Hej **hive-site.xml** finns på `/usr/hdp/current/spark-client/conf`.</span><span class="sxs-lookup"><span data-stu-id="ba502-279">hello **hive-site.xml** is available at `/usr/hdp/current/spark-client/conf`.</span></span>

<span data-ttu-id="ba502-280">Du kan använda [WinScp](http://winscp.net/eng/download.php) toocopy över de här filerna från hello klustret tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="ba502-280">You can use [WinScp](http://winscp.net/eng/download.php) toocopy over these files from hello cluster tooyour local computer.</span></span> <span data-ttu-id="ba502-281">Du kan sedan använda verktyg toocopy filerna över tooyour storage-konto som är associerade med hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ba502-281">You can then use tools toocopy these files over tooyour storage account associated with hello cluster.</span></span> <span data-ttu-id="ba502-282">Mer information om hur tooupload filer toohello storage-konto finns [överföra data för Hadoop-jobb i HDInsight](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-282">For more information on how tooupload files toohello storage account, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

<span data-ttu-id="ba502-283">När du har kopierat över hello filer tooyour Azure storage-konto kan öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando (Ersätt användarnamn/lösenord och kluster namn) hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-283">Once you have copied over hello files tooyour Azure storage account, open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="ba502-284">Hej parametrar i hello filen **inputHive.txt** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-284">hello parameters in hello file **inputHive.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="ba502-285">hello parametrar är liknande toowhat som du angav för hello text-utdata i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ba502-285">hello parameters are similar toowhat you specified for hello text output, in hello previous steps.</span></span> <span data-ttu-id="ba502-286">Igen och du behöver inte toocreate hello utdata mappar (EventCheckpoint, EventCount/EventCount10) eller hello utdata Hive-tabell (EventHiveTable10) som används som parametrar.</span><span class="sxs-lookup"><span data-stu-id="ba502-286">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) or hello output Hive table (EventHiveTable10) that are used as parameters.</span></span> <span data-ttu-id="ba502-287">direktuppspelning av programmet hello skapar dem åt dig.</span><span class="sxs-lookup"><span data-stu-id="ba502-287">hello streaming application creates them for you.</span></span> <span data-ttu-id="ba502-288">Observera att hello **burkar** och **filer** alternativet innehåller sökvägar toohello .jar filer och hello hive-site.xml som du kopierade över toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="ba502-288">Note that hello **jars** and **files** option includes paths toohello .jar files and hello hive-site.xml that you copied over toohello storage account.</span></span>

<span data-ttu-id="ba502-289">tooverify som hello hive-tabellen har skapats kan du SSH till hello klustret och köra Hive-frågor.</span><span class="sxs-lookup"><span data-stu-id="ba502-289">tooverify that hello hive table was successfully created, you can SSH into hello cluster and run Hive queries.</span></span> <span data-ttu-id="ba502-290">Instruktioner finns i [använda Hive med Hadoop i HDInsight med SSH](hdinsight-hadoop-use-hive-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-290">For instructions, see [Use Hive with Hadoop in HDInsight with SSH](hdinsight-hadoop-use-hive-ssh.md).</span></span> <span data-ttu-id="ba502-291">När du är ansluten med SSH, du kan köra följande kommando tooverify hello som hello Hive-tabell **EventHiveTable10**, skapas.</span><span class="sxs-lookup"><span data-stu-id="ba502-291">Once you are connected using SSH, you can run hello following command tooverify that hello Hive table, **EventHiveTable10**, is created.</span></span>

    show tables;

<span data-ttu-id="ba502-292">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-292">You should see an output similar toohello following:</span></span>

    OK
    eventhivetable10
    hivesampletable

<span data-ttu-id="ba502-293">Du kan också köra en SELECT-frågan tooview hello tabell hello innehåll.</span><span class="sxs-lookup"><span data-stu-id="ba502-293">You can also run a SELECT query tooview hello contents of hello table.</span></span>

    SELECT * FROM eventhivetable10 LIMIT 10;

<span data-ttu-id="ba502-294">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-294">You should see an output like hello following:</span></span>

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a><span data-ttu-id="ba502-295">Köra hello program tooreceive hello händelser till en Azure SQL-databastabell</span><span class="sxs-lookup"><span data-stu-id="ba502-295">Run hello applications tooreceive hello events into an Azure SQL database table</span></span>
<span data-ttu-id="ba502-296">Kontrollera att du har en Azure SQL-databas som har skapats innan du kör det här steget.</span><span class="sxs-lookup"><span data-stu-id="ba502-296">Before running this step, make sure you have an Azure SQL database created.</span></span> <span data-ttu-id="ba502-297">Instruktioner finns i [skapa en SQL-databas i minuter](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-297">For instructions, see [Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span> <span data-ttu-id="ba502-298">toocomplete det här avsnittet, behöver du värden för databasens namn, Databasservernamnet och Hej administratör Databasautentiseringsuppgifter som parametrar.</span><span class="sxs-lookup"><span data-stu-id="ba502-298">toocomplete this section, you need values for database name, database server name, and hello database administrator credentials as parameters.</span></span> <span data-ttu-id="ba502-299">Du behöver inte toocreate hello databastabell om.</span><span class="sxs-lookup"><span data-stu-id="ba502-299">You do not need toocreate hello database table though.</span></span> <span data-ttu-id="ba502-300">hello Spark streaming programmet skapas som.</span><span class="sxs-lookup"><span data-stu-id="ba502-300">hello Spark streaming application creates that for you.</span></span>

<span data-ttu-id="ba502-301">Öppna en kommandotolk, navigera toohello katalog där du installerade CURL och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-301">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="ba502-302">Hej parametrar i hello filen **inputSQL.txt** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-302">hello parameters in hello file **inputSQL.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="ba502-303">tooverify som hello programmet körs utan problem, kan du ansluta toohello Azure SQL database med SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="ba502-303">tooverify that hello application runs successfully, you can connect toohello Azure SQL database using SQL Server Management Studio.</span></span> <span data-ttu-id="ba502-304">Anvisningar för hur toodo som finns i [ansluta tooSQL databasen med SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="ba502-304">For instructions on how toodo that, see [Connect tooSQL Database with SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span></span> <span data-ttu-id="ba502-305">När du är ansluten toohello databasen kan du navigera toohello **EventContent** tabell som har skapats av hello direktuppspelning av program.</span><span class="sxs-lookup"><span data-stu-id="ba502-305">Once you are connected toohello database, you can navigate toohello **EventContent** table that was created by hello streaming application.</span></span> <span data-ttu-id="ba502-306">Du kan köra en snabb fråga tooget hello data från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="ba502-306">You can run a quick query tooget hello data from hello table.</span></span> <span data-ttu-id="ba502-307">Kör följande fråga hello:</span><span class="sxs-lookup"><span data-stu-id="ba502-307">Run hello following query:</span></span>

    SELECT * FROM EventCount

<span data-ttu-id="ba502-308">Du bör se utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ba502-308">You should see output similar toohello following:</span></span>

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <span data-ttu-id="ba502-309"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="ba502-309"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="ba502-310">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-310">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)
* [<span data-ttu-id="ba502-311">Design av mottagare-baserade anslutningen och direkt DStream</span><span class="sxs-lookup"><span data-stu-id="ba502-311">Design of Receiver-based Connection and Direct DStream</span></span>](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a><span data-ttu-id="ba502-312">Scenarier</span><span class="sxs-lookup"><span data-stu-id="ba502-312">Scenarios</span></span>
* [<span data-ttu-id="ba502-313">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="ba502-313">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="ba502-314">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="ba502-314">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="ba502-315">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="ba502-315">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="ba502-316">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-316">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="ba502-317">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="ba502-317">Create and run applications</span></span>
* [<span data-ttu-id="ba502-318">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="ba502-318">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="ba502-319">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="ba502-319">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="ba502-320">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="ba502-320">Tools and extensions</span></span>
* [<span data-ttu-id="ba502-321">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="ba502-321">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="ba502-322">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="ba502-322">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="ba502-323">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-323">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="ba502-324">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-324">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="ba502-325">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="ba502-325">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="ba502-326">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ba502-326">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="ba502-327">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="ba502-327">Manage resources</span></span>
* [<span data-ttu-id="ba502-328">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-328">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="ba502-329">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ba502-329">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
