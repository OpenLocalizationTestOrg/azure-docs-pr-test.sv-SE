---
title: "Ta emot händelser från Azure Event Hubs använder Apache Storm | Microsoft Docs"
description: "Börja ta emot från Event Hubs använder Apache Storm"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 3e15370c7602276ef323708632b324fe05497f41
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a><span data-ttu-id="63bfa-103">Ta emot händelser från Event Hubs använder Apache Storm</span><span class="sxs-lookup"><span data-stu-id="63bfa-103">Receive events from Event Hubs using Apache Storm</span></span>

<span data-ttu-id="63bfa-104">[Apache Storm](https://storm.incubator.apache.org) är ett system för distribuerade beräkningar i realtid som förenklar tillförlitliga bearbetningen av unbounded dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="63bfa-104">[Apache Storm](https://storm.incubator.apache.org) is a distributed real-time computation system that simplifies reliable processing of unbounded streams of data.</span></span> <span data-ttu-id="63bfa-105">Det här avsnittet visar hur du använder en Azure Event Hubs Storm-kanal för att ta emot händelser från Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="63bfa-105">This section shows how to use an Azure Event Hubs Storm spout to receive events from Event Hubs.</span></span> <span data-ttu-id="63bfa-106">Med Apache Storm kan du dela upp händelser över flera processer som ligger på olika noder.</span><span class="sxs-lookup"><span data-stu-id="63bfa-106">Using Apache Storm, you can split events across multiple processes hosted in different nodes.</span></span> <span data-ttu-id="63bfa-107">Händelsehubbar integrering med Storm förenklar händelsekonsumtion genom att transparent kontrollpunkter förloppet använder Storm's Zookeeper installation, hantera permanenta kontrollpunkter och parallella mottaganden från Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="63bfa-107">The Event Hubs integration with Storm simplifies event consumption by transparently checkpointing its progress using Storm's Zookeeper installation, managing persistent checkpoints and parallel receives from Event Hubs.</span></span>

<span data-ttu-id="63bfa-108">Mer information om Händelsehubbar får mönster, finns det [översikt av Händelsehubbar][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="63bfa-108">For more information about Event Hubs receive patterns, see the [Event Hubs overview][Event Hubs overview].</span></span>

## <a name="create-project-and-add-code"></a><span data-ttu-id="63bfa-109">Skapa projektet och Lägg till kod</span><span class="sxs-lookup"><span data-stu-id="63bfa-109">Create project and add code</span></span>

<span data-ttu-id="63bfa-110">Den här kursen använder en [HDInsight Storm] [ HDInsight Storm] installation som medföljer den Händelsehubbar kanal som redan finns.</span><span class="sxs-lookup"><span data-stu-id="63bfa-110">This tutorial uses an [HDInsight Storm][HDInsight Storm] installation, which comes with the Event Hubs spout already available.</span></span>

1. <span data-ttu-id="63bfa-111">Följ den [HDInsight Storm - komma igång](../hdinsight/hdinsight-storm-overview.md) proceduren för att skapa ett nytt HDInsight-kluster och ansluta till den via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="63bfa-111">Follow the [HDInsight Storm - Get Started](../hdinsight/hdinsight-storm-overview.md) procedure to create a new HDInsight cluster, and connect to it via Remote Desktop.</span></span>
2. <span data-ttu-id="63bfa-112">Kopiera den `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` filen till din lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="63bfa-112">Copy the `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` file to your local development environment.</span></span> <span data-ttu-id="63bfa-113">Innehåller händelser-storm-kanal.</span><span class="sxs-lookup"><span data-stu-id="63bfa-113">This contains the events-storm-spout.</span></span>
3. <span data-ttu-id="63bfa-114">Använd följande kommando för att installera paketet i det lokala arkivet Maven.</span><span class="sxs-lookup"><span data-stu-id="63bfa-114">Use the following command to install the package into the local Maven store.</span></span> <span data-ttu-id="63bfa-115">På så sätt kan du lägga till den som en referens i projektet Storm i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="63bfa-115">This enables you to add it as a reference in the Storm project in a later step.</span></span>

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. <span data-ttu-id="63bfa-116">Skapa ett nytt Maven-projekt i Eclipse (klicka på **filen**, sedan **ny**, sedan **projekt**).</span><span class="sxs-lookup"><span data-stu-id="63bfa-116">In Eclipse, create a new Maven project (click **File**, then **New**, then **Project**).</span></span>
   
    ![][12]
5. <span data-ttu-id="63bfa-117">Välj **använda standardplatsen för arbetsytan**, klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="63bfa-117">Select **Use default Workspace location**, then click **Next**</span></span>
6. <span data-ttu-id="63bfa-118">Välj den **maven-archetype-Snabbstart** archetype, klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="63bfa-118">Select the **maven-archetype-quickstart** archetype, then click **Next**</span></span>
7. <span data-ttu-id="63bfa-119">Infoga en **GroupId** och **artefakt-ID**, klicka på **Slutför**</span><span class="sxs-lookup"><span data-stu-id="63bfa-119">Insert a **GroupId** and **ArtifactId**, then click **Finish**</span></span>
8. <span data-ttu-id="63bfa-120">I **pom.xml**, Lägg till följande beroenden i den `<dependency>` nod.</span><span class="sxs-lookup"><span data-stu-id="63bfa-120">In **pom.xml**, add the following dependencies in the `<dependency>` node.</span></span>

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. <span data-ttu-id="63bfa-121">I den **src** mapp, skapa en fil med namnet **Config.properties** och kopiera följande innehåll, ersätter den `receive rule key` och `event hub name` värden:</span><span class="sxs-lookup"><span data-stu-id="63bfa-121">In the **src** folder, create a file called **Config.properties** and copy the following content, substituting the `receive rule key` and `event hub name` values:</span></span>

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    <span data-ttu-id="63bfa-122">Värdet för **eventhub.receiver.credits** avgör hur många händelser grupperas innan du lanserar Storm-pipeline.</span><span class="sxs-lookup"><span data-stu-id="63bfa-122">The value for **eventhub.receiver.credits** determines how many events are batched before releasing them to the Storm pipeline.</span></span> <span data-ttu-id="63bfa-123">För enkelhetens skull anger det här exemplet du värdet till 10.</span><span class="sxs-lookup"><span data-stu-id="63bfa-123">For the sake of simplicity, this example sets this value to 10.</span></span> <span data-ttu-id="63bfa-124">I produktion sättas det vanligtvis till högre värden. till exempel 1024.</span><span class="sxs-lookup"><span data-stu-id="63bfa-124">In production, it should usually be set to higher values; for example, 1024.</span></span>
10. <span data-ttu-id="63bfa-125">Skapa en ny klass med namnet **LoggerBolt** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="63bfa-125">Create a new class called **LoggerBolt** with the following code:</span></span>
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    <span data-ttu-id="63bfa-126">Storm bulten loggar innehållet i de mottagna händelserna.</span><span class="sxs-lookup"><span data-stu-id="63bfa-126">This Storm bolt logs the content of the received events.</span></span> <span data-ttu-id="63bfa-127">Detta kan enkelt utökas för att lagra tupplar i en storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="63bfa-127">This can easily be extended to store tuples in a storage service.</span></span> <span data-ttu-id="63bfa-128">Den [HDInsight sensor analys kursen] använder samma metod för att lagra data i HBase.</span><span class="sxs-lookup"><span data-stu-id="63bfa-128">The [HDInsight sensor analysis tutorial] uses this same approach to store data into HBase.</span></span>
11. <span data-ttu-id="63bfa-129">Skapa en klass med namnet **LogTopology** med följande kod:</span><span class="sxs-lookup"><span data-stu-id="63bfa-129">Create a class called **LogTopology** with the following code:</span></span>
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set the number of workers to be the same as partition number.
            // the idea is to have a spout and a logger bolt co-exist in one
            // worker to avoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    <span data-ttu-id="63bfa-130">Den här klassen skapas en ny kanal för Händelsehubbar, med hjälp av egenskaperna i konfigurationsfilen för att skapa en instans av den.</span><span class="sxs-lookup"><span data-stu-id="63bfa-130">This class creates a new Event Hubs spout, using the properties in the configuration file to instantiate it.</span></span> <span data-ttu-id="63bfa-131">Det är viktigt att Observera att det här exemplet skapar så många aktiviteter för kanaler som antalet partitioner i händelsehubben, för att kunna använda maximala parallellitet tillåts av den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="63bfa-131">It is important to note that this example creates as many spouts tasks as the number of partitions in the event hub, in order to use the maximum parallelism allowed by that event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="63bfa-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="63bfa-132">Next steps</span></span>
<span data-ttu-id="63bfa-133">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="63bfa-133">You can learn more about Event Hubs by visiting the following links:</span></span>

* <span data-ttu-id="63bfa-134">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="63bfa-134">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="63bfa-135">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="63bfa-135">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="63bfa-136">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="63bfa-136">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
<span data-ttu-id="63bfa-137">[HDInsight sensor analys kursen]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md</span><span class="sxs-lookup"><span data-stu-id="63bfa-137">[HDInsight sensor analysis tutorial]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md</span></span>

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
