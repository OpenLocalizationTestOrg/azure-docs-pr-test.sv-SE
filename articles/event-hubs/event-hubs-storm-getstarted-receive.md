---
title: "aaaReceive händelser från Azure Event Hubs använder Apache Storm | Microsoft Docs"
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
ms.openlocfilehash: a0ab860ee8d504a28aac380c504c928f0d6dbc1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a><span data-ttu-id="a0447-103">Ta emot händelser från Event Hubs använder Apache Storm</span><span class="sxs-lookup"><span data-stu-id="a0447-103">Receive events from Event Hubs using Apache Storm</span></span>

<span data-ttu-id="a0447-104">[Apache Storm](https://storm.incubator.apache.org) är ett system för distribuerade beräkningar i realtid som förenklar tillförlitliga bearbetningen av unbounded dataströmmar.</span><span class="sxs-lookup"><span data-stu-id="a0447-104">[Apache Storm](https://storm.incubator.apache.org) is a distributed real-time computation system that simplifies reliable processing of unbounded streams of data.</span></span> <span data-ttu-id="a0447-105">Det här avsnittet visar hur toouse ett Azure Event Hubs Storm prata tooreceive händelser från Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a0447-105">This section shows how toouse an Azure Event Hubs Storm spout tooreceive events from Event Hubs.</span></span> <span data-ttu-id="a0447-106">Med Apache Storm kan du dela upp händelser över flera processer som ligger på olika noder.</span><span class="sxs-lookup"><span data-stu-id="a0447-106">Using Apache Storm, you can split events across multiple processes hosted in different nodes.</span></span> <span data-ttu-id="a0447-107">Hej Händelsehubbar integrering med Storm förenklar händelsekonsumtion genom att transparent kontrollpunkter förloppet använder Storm's Zookeeper installation, hantera permanenta kontrollpunkter och parallella mottaganden från Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="a0447-107">hello Event Hubs integration with Storm simplifies event consumption by transparently checkpointing its progress using Storm's Zookeeper installation, managing persistent checkpoints and parallel receives from Event Hubs.</span></span>

<span data-ttu-id="a0447-108">Mer information om Händelsehubbar får mönster, se hello [översikt av Händelsehubbar][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="a0447-108">For more information about Event Hubs receive patterns, see hello [Event Hubs overview][Event Hubs overview].</span></span>

## <a name="create-project-and-add-code"></a><span data-ttu-id="a0447-109">Skapa projektet och Lägg till kod</span><span class="sxs-lookup"><span data-stu-id="a0447-109">Create project and add code</span></span>

<span data-ttu-id="a0447-110">Den här kursen använder en [HDInsight Storm] [ HDInsight Storm] installation som levereras med hello Händelsehubbar kanal som redan är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="a0447-110">This tutorial uses an [HDInsight Storm][HDInsight Storm] installation, which comes with hello Event Hubs spout already available.</span></span>

1. <span data-ttu-id="a0447-111">Följ hello [HDInsight Storm - komma igång](../hdinsight/hdinsight-storm-overview.md) proceduren toocreate nya HDInsight-kluster och ansluta tooit via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="a0447-111">Follow hello [HDInsight Storm - Get Started](../hdinsight/hdinsight-storm-overview.md) procedure toocreate a new HDInsight cluster, and connect tooit via Remote Desktop.</span></span>
2. <span data-ttu-id="a0447-112">Kopiera hello `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` filen tooyour lokala utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0447-112">Copy hello `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` file tooyour local development environment.</span></span> <span data-ttu-id="a0447-113">Innehåller hello händelser-storm-kanal.</span><span class="sxs-lookup"><span data-stu-id="a0447-113">This contains hello events-storm-spout.</span></span>
3. <span data-ttu-id="a0447-114">Använd följande kommando tooinstall hello paketet till hello lokala Maven Arkiv hello.</span><span class="sxs-lookup"><span data-stu-id="a0447-114">Use hello following command tooinstall hello package into hello local Maven store.</span></span> <span data-ttu-id="a0447-115">Detta gör att du tooadd den som en referens i hello Storm-projekt i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="a0447-115">This enables you tooadd it as a reference in hello Storm project in a later step.</span></span>

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. <span data-ttu-id="a0447-116">Skapa ett nytt Maven-projekt i Eclipse (klicka på **filen**, sedan **ny**, sedan **projekt**).</span><span class="sxs-lookup"><span data-stu-id="a0447-116">In Eclipse, create a new Maven project (click **File**, then **New**, then **Project**).</span></span>
   
    ![][12]
5. <span data-ttu-id="a0447-117">Välj **använda standardplatsen för arbetsytan**, klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="a0447-117">Select **Use default Workspace location**, then click **Next**</span></span>
6. <span data-ttu-id="a0447-118">Välj hello **maven-archetype-Snabbstart** archetype, klicka på **nästa**</span><span class="sxs-lookup"><span data-stu-id="a0447-118">Select hello **maven-archetype-quickstart** archetype, then click **Next**</span></span>
7. <span data-ttu-id="a0447-119">Infoga en **GroupId** och **artefakt-ID**, klicka på **Slutför**</span><span class="sxs-lookup"><span data-stu-id="a0447-119">Insert a **GroupId** and **ArtifactId**, then click **Finish**</span></span>
8. <span data-ttu-id="a0447-120">I **pom.xml**, Lägg till följande beroenden i hello hello `<dependency>` nod.</span><span class="sxs-lookup"><span data-stu-id="a0447-120">In **pom.xml**, add hello following dependencies in hello `<dependency>` node.</span></span>

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

9. <span data-ttu-id="a0447-121">I hello **src** mapp, skapa en fil med namnet **Config.properties** och kopiera hello efter innehåll, ersätter hello `receive rule key` och `event hub name` värden:</span><span class="sxs-lookup"><span data-stu-id="a0447-121">In hello **src** folder, create a file called **Config.properties** and copy hello following content, substituting hello `receive rule key` and `event hub name` values:</span></span>

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
    <span data-ttu-id="a0447-122">Hej värde för **eventhub.receiver.credits** avgör hur många händelser grupperas innan du släpper dem toohello Storm pipeline.</span><span class="sxs-lookup"><span data-stu-id="a0447-122">hello value for **eventhub.receiver.credits** determines how many events are batched before releasing them toohello Storm pipeline.</span></span> <span data-ttu-id="a0447-123">Det här exemplet anger det här värdet too10 för hello dig ut av enkelhet.</span><span class="sxs-lookup"><span data-stu-id="a0447-123">For hello sake of simplicity, this example sets this value too10.</span></span> <span data-ttu-id="a0447-124">I produktion, vara det vanligtvis konfigurerat toohigher värden. till exempel 1024.</span><span class="sxs-lookup"><span data-stu-id="a0447-124">In production, it should usually be set toohigher values; for example, 1024.</span></span>
10. <span data-ttu-id="a0447-125">Skapa en ny klass med namnet **LoggerBolt** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a0447-125">Create a new class called **LoggerBolt** with hello following code:</span></span>
    
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
    
    <span data-ttu-id="a0447-126">Storm bulten loggar hello innehåll hello emot händelser.</span><span class="sxs-lookup"><span data-stu-id="a0447-126">This Storm bolt logs hello content of hello received events.</span></span> <span data-ttu-id="a0447-127">Detta kan enkelt utökas toostore tupplar i en storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a0447-127">This can easily be extended toostore tuples in a storage service.</span></span> <span data-ttu-id="a0447-128">Hej [HDInsight sensor analys kursen] använder samma metod toostore data i HBase.</span><span class="sxs-lookup"><span data-stu-id="a0447-128">hello [HDInsight sensor analysis tutorial] uses this same approach toostore data into HBase.</span></span>
11. <span data-ttu-id="a0447-129">Skapa en klass med namnet **LogTopology** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="a0447-129">Create a class called **LogTopology** with hello following code:</span></span>
    
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
        
            // set hello number of workers toobe hello same as partition number.
            // hello idea is toohave a spout and a logger bolt co-exist in one
            // worker tooavoid shuffling messages across workers in storm cluster.
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

    <span data-ttu-id="a0447-130">Den här klassen skapas en ny kanal för Händelsehubbar, med hjälp av hello egenskaper i hello configuration file tooinstantiate den.</span><span class="sxs-lookup"><span data-stu-id="a0447-130">This class creates a new Event Hubs spout, using hello properties in hello configuration file tooinstantiate it.</span></span> <span data-ttu-id="a0447-131">Det är viktigt toonote som det här exemplet skapar så många spouts uppgifter som hello antalet partitioner i hello händelsehubb, i ordning toouse hello maximala parallellitet tillåts av den händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="a0447-131">It is important toonote that this example creates as many spouts tasks as hello number of partitions in hello event hub, in order toouse hello maximum parallelism allowed by that event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0447-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0447-132">Next steps</span></span>
<span data-ttu-id="a0447-133">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="a0447-133">You can learn more about Event Hubs by visiting hello following links:</span></span>

* <span data-ttu-id="a0447-134">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="a0447-134">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="a0447-135">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="a0447-135">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a0447-136">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a0447-136">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/hdinsight-storm-overview.md
[HDInsight sensor analys kursen]: ../hdinsight/hdinsight-storm-sensor-data-analysis.md

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
