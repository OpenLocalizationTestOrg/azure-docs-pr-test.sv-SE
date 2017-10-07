---
title: "aaaProcess händelser från Event Hubs med Storm på HDInsight använder Java | Microsoft Docs"
description: "Lär dig hur du skapar tooprocess Händelsehubbar data med en Java Storm-topologi med Maven."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.openlocfilehash: 6506f5bc8f6ab0e29350c071a3f84433382038e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a><span data-ttu-id="ab62d-103">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="ab62d-103">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>

<span data-ttu-id="ab62d-104">Lär dig hur toouse Händelsehubbar i Azure med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab62d-104">Learn how toouse Azure Event Hubs with Storm on HDInsight.</span></span> <span data-ttu-id="ab62d-105">Det här exemplet använder Java-baserade komponenter tooread och skriva data i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="ab62d-105">This example uses Java-based components tooread and write data in Azure Event Hubs.</span></span>

<span data-ttu-id="ab62d-106">Händelsehubbar i Azure kan du tooprocess enorma mängder data från webbplatser, appar och enheter.</span><span class="sxs-lookup"><span data-stu-id="ab62d-106">Azure Event Hubs allows you tooprocess massive amounts of data from websites, apps, and devices.</span></span> <span data-ttu-id="ab62d-107">hello Event Hub kanal gör det enkelt toouse Apache Storm på HDInsight tooanalyze dessa data i realtid.</span><span class="sxs-lookup"><span data-stu-id="ab62d-107">hello Event Hub spout makes it easy toouse Apache Storm on HDInsight tooanalyze this data in real time.</span></span> <span data-ttu-id="ab62d-108">Du kan också skriva data tooEvent NAV från Storm med hjälp av hello Händelsehubbar bultar.</span><span class="sxs-lookup"><span data-stu-id="ab62d-108">You can also write data tooEvent Hubs from Storm by using hello Event Hubs bolt.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ab62d-109">Krav</span><span class="sxs-lookup"><span data-stu-id="ab62d-109">Prerequisites</span></span>

* <span data-ttu-id="ab62d-110">En Apache Storm på HDInsight-kluster av version 3,6.</span><span class="sxs-lookup"><span data-stu-id="ab62d-110">An Apache Storm on HDInsight cluster version 3.6.</span></span> <span data-ttu-id="ab62d-111">Mer information finns i [Kom igång med Storm på HDInsight-kluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="ab62d-111">For more information, see [Get started with Storm on HDInsight cluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="ab62d-112">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ab62d-112">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab62d-113">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab62d-113">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ab62d-114">En [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="ab62d-114">An [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="ab62d-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, såsom [OpenJDK](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="ab62d-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or equivalent, such as [OpenJDK](http://openjdk.java.net/).</span></span>

* <span data-ttu-id="ab62d-116">[Maven](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="ab62d-116">[Maven](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="ab62d-117">En textredigerare eller integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="ab62d-117">A text editor or integrated development environment (IDE).</span></span>

    > [!NOTE]
    > <span data-ttu-id="ab62d-118">Editor- eller IDE kan ha specifika funktioner för att arbeta med Maven som inte riktar sig i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ab62d-118">Your editor or IDE may have specific functionality for working with Maven that is not addressed in this document.</span></span> <span data-ttu-id="ab62d-119">Information om hello funktioner för din miljö för redigering av dokumentationen hello för hello produkt som du använder.</span><span class="sxs-lookup"><span data-stu-id="ab62d-119">For information about hello capabilities of your editing environment, see hello documentation for hello product you are using.</span></span>

    * <span data-ttu-id="ab62d-120">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="ab62d-120">An SSH client.</span></span> <span data-ttu-id="ab62d-121">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="ab62d-121">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="ab62d-122">Hej `ssh` och `scp` kommandon.</span><span class="sxs-lookup"><span data-stu-id="ab62d-122">hello `ssh` and `scp` commands.</span></span> <span data-ttu-id="ab62d-123">Dessa är används toocopy filer toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab62d-123">These are used toocopy files toohello HDInsight cluster.</span></span> <span data-ttu-id="ab62d-124">Du kan hämta dessa via Bash på Windows 10 på Windows.</span><span class="sxs-lookup"><span data-stu-id="ab62d-124">On Windows, you can get these through Bash on Windows 10.</span></span>

## <a name="understanding-hello-example"></a><span data-ttu-id="ab62d-125">Förstå hello-exempel</span><span class="sxs-lookup"><span data-stu-id="ab62d-125">Understanding hello example</span></span>

<span data-ttu-id="ab62d-126">Hej [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) exempel innehåller två topologier:</span><span class="sxs-lookup"><span data-stu-id="ab62d-126">hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) example contains two topologies:</span></span>

<span data-ttu-id="ab62d-127">Hej `resources/writer.yaml` topologi skriver slumpmässiga data tooan Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="ab62d-127">hello `resources/writer.yaml` topology writes random data tooan Azure Event Hub.</span></span> <span data-ttu-id="ab62d-128">hello data genereras hello `DeviceSpout` komponent, och är en slumpmässig enhets-ID och ett värde för enheten.</span><span class="sxs-lookup"><span data-stu-id="ab62d-128">hello data is generated by hello `DeviceSpout` component, and is a random device ID and device value.</span></span> <span data-ttu-id="ab62d-129">Det därför simulera vissa maskinvara som genererar ett sträng-ID och ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="ab62d-129">So it's simulating some hardware that emits a string ID and a numeric value.</span></span>

<span data-ttu-id="ab62d-130">Dig `resources/reader.yaml` topologi läser data från Event Hub (hello data skrivits av EventHubWriter,) Parsar hello JSON-data och sedan loggar hello `deviceId` och `deviceValue` data.</span><span class="sxs-lookup"><span data-stu-id="ab62d-130">Thee `resources/reader.yaml` topology reads data from Event Hub (hello data written by EventHubWriter,) parses hello JSON data, and then logs hello `deviceId` and `deviceValue` data.</span></span>

<span data-ttu-id="ab62d-131">hello data formateras som JSON-dokument innan den kan skrivas tooEvent hubb och när läses av hello reader tolkas utanför JSON och i tupplar.</span><span class="sxs-lookup"><span data-stu-id="ab62d-131">hello data is formatted as a JSON document before it is written tooEvent Hub, and when read by hello reader it is parsed out of JSON and into tuples.</span></span> <span data-ttu-id="ab62d-132">hello JSON-format är:</span><span class="sxs-lookup"><span data-stu-id="ab62d-132">hello JSON format is as follows:</span></span>

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a><span data-ttu-id="ab62d-133">Konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="ab62d-133">Project configuration</span></span>

<span data-ttu-id="ab62d-134">Hej `POM.xml` filen innehåller konfigurationsinformation för det här Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="ab62d-134">hello `POM.xml` file contains configuration information for this Maven project.</span></span> <span data-ttu-id="ab62d-135">hello intressanta delar är:</span><span class="sxs-lookup"><span data-stu-id="ab62d-135">hello interesting pieces are:</span></span>

#### <a name="event-hub-components"></a><span data-ttu-id="ab62d-136">Event Hub komponenter</span><span class="sxs-lookup"><span data-stu-id="ab62d-136">Event Hub components</span></span>

<span data-ttu-id="ab62d-137">hello-komponent som läser och skriver tooAzure Händelsehubbar finns i hello [HDInsight databasen](https://github.com/hdinsight/mvn-rep).</span><span class="sxs-lookup"><span data-stu-id="ab62d-137">hello component that reads and writes tooAzure Event Hubs is located in hello [HDInsight repository](https://github.com/hdinsight/mvn-rep).</span></span> <span data-ttu-id="ab62d-138">följande avsnitt i hello hello `POM.xml` filen belastningen hello komponenter från den här lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="ab62d-138">hello following sections in hello `POM.xml` file load hello components from this repository</span></span>

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a><span data-ttu-id="ab62d-139">Hej EventHubs Storm-kanalen beroende</span><span class="sxs-lookup"><span data-stu-id="ab62d-139">hello EventHubs Storm Spout dependency</span></span>

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

<span data-ttu-id="ab62d-140">Den här xml definierar ett beroende för hello eventhubs paket, som innehåller både en kanal för att läsa från Event Hubs och en bult för att skriva tooit.</span><span class="sxs-lookup"><span data-stu-id="ab62d-140">This xml defines a dependency for hello eventhubs package, which contains both a spout for reading from Event Hubs, and a bolt for writing tooit.</span></span>

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

<span data-ttu-id="ab62d-141">Den här xml konfigurerar hello projektet toogenerate utdata för Java 8, som används av HDInsight 3.5 eller högre.</span><span class="sxs-lookup"><span data-stu-id="ab62d-141">This xml configures hello project toogenerate output for Java 8, which is used by HDInsight 3.5 or higher.</span></span>

#### <a name="hello-maven-shade-plugin"></a><span data-ttu-id="ab62d-142">Hej maven-skugga-plugin-program</span><span class="sxs-lookup"><span data-stu-id="ab62d-142">hello maven-shade-plugin</span></span>

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

<span data-ttu-id="ab62d-143">Den här xml konfigurerar hello lösning toopackage hello utdata till en uber jar.</span><span class="sxs-lookup"><span data-stu-id="ab62d-143">This xml configures hello solution toopackage hello output into an uber jar.</span></span> <span data-ttu-id="ab62d-144">hello jar innehåller både hello Projektkod och nödvändiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="ab62d-144">hello jar contains both hello project code and required dependencies.</span></span> <span data-ttu-id="ab62d-145">Det används också för att:</span><span class="sxs-lookup"><span data-stu-id="ab62d-145">It is also used to:</span></span>

* <span data-ttu-id="ab62d-146">Byt namn på licensfiler för hello beroenden.</span><span class="sxs-lookup"><span data-stu-id="ab62d-146">Rename license files for hello dependencies.</span></span>
* <span data-ttu-id="ab62d-147">Exkludera security-signaturer.</span><span class="sxs-lookup"><span data-stu-id="ab62d-147">Exclude security/signatures.</span></span>
* <span data-ttu-id="ab62d-148">Se till att flera implementeringar av hello samma gränssnittet kombineras till en transaktion.</span><span class="sxs-lookup"><span data-stu-id="ab62d-148">Ensure that multiple implementations of hello same interface are merged into one entry.</span></span>

<span data-ttu-id="ab62d-149">Dessa konfigurationsinställningar undvika fel vid körning.</span><span class="sxs-lookup"><span data-stu-id="ab62d-149">These configuration settings prevent errors at runtime.</span></span>

#### <a name="topology-definitions"></a><span data-ttu-id="ab62d-150">Topologi definitioner</span><span class="sxs-lookup"><span data-stu-id="ab62d-150">Topology definitions</span></span>

<span data-ttu-id="ab62d-151">Det här exemplet används hello [som](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span><span class="sxs-lookup"><span data-stu-id="ab62d-151">This example uses hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span></span> <span data-ttu-id="ab62d-152">Det här ramverket använder YAML toodefine hello topologier.</span><span class="sxs-lookup"><span data-stu-id="ab62d-152">This framework uses YAML toodefine hello topologies.</span></span> <span data-ttu-id="ab62d-153">hello största fördelen är att du inte hårddisken kodning hello topologi i Java-kod.</span><span class="sxs-lookup"><span data-stu-id="ab62d-153">hello primary benefit is that you aren't hard coding hello topology in Java code.</span></span> <span data-ttu-id="ab62d-154">Du kan ändra den innan du skickar hello topologi, utan att behöva toorecompile allt eftersom hello definition YAML.</span><span class="sxs-lookup"><span data-stu-id="ab62d-154">Since hello definition is YAML, you can change it before submitting hello topology, without having toorecompile everything.</span></span>

<span data-ttu-id="ab62d-155">__Writer.yaml__:</span><span class="sxs-lookup"><span data-stu-id="ab62d-155">__writer.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure hello Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through hello components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

<span data-ttu-id="ab62d-156">__Reader.yaml__:</span><span class="sxs-lookup"><span data-stu-id="ab62d-156">__reader.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure hello Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through hello components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-hello-topology-about-event-hub"></a><span data-ttu-id="ab62d-157">Berätta Event Hub hello-topologi</span><span class="sxs-lookup"><span data-stu-id="ab62d-157">Tell hello topology about Event Hub</span></span>

<span data-ttu-id="ab62d-158">Vid körning hello `dev.properties` filen är används toopass hello Event Hub configuration toohello topologi.</span><span class="sxs-lookup"><span data-stu-id="ab62d-158">At run time, hello `dev.properties` file is used toopass hello Event Hub configuration toohello topology.</span></span> <span data-ttu-id="ab62d-159">hello är följande exempel hello standardinnehållet i hello-fil:</span><span class="sxs-lookup"><span data-stu-id="ab62d-159">hello following example is hello default contents of hello file:</span></span>

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a><span data-ttu-id="ab62d-160">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="ab62d-160">Configure environment variables</span></span>

<span data-ttu-id="ab62d-161">hello kan följande miljövariabler anges när du installerar Java och hello JDK på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="ab62d-161">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="ab62d-162">Dock bör du kontrollera att de finns och att de innehåller hello rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="ab62d-162">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="ab62d-163">**JAVA_HOME** -ska peka toohello katalog där hello Java runtime environment (JRE) har installerats.</span><span class="sxs-lookup"><span data-stu-id="ab62d-163">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="ab62d-164">Till exempel i en Unix- eller Linux-distribution, den måste ha ett värde liknande för`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="ab62d-164">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="ab62d-165">I Windows, skulle det ha ett värde som är liknande för`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="ab62d-165">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>
* <span data-ttu-id="ab62d-166">**SÖKVÄGEN** -bör innehålla hello följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="ab62d-166">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="ab62d-167">**JAVA_HOME** (eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="ab62d-167">**JAVA_HOME** (or hello equivalent path)</span></span>
  * <span data-ttu-id="ab62d-168">**JAVA_HOME\bin** (eller motsvarande hello-sökväg)</span><span class="sxs-lookup"><span data-stu-id="ab62d-168">**JAVA_HOME\bin** (or hello equivalent path)</span></span>
  * <span data-ttu-id="ab62d-169">hello katalog där Maven har installerats</span><span class="sxs-lookup"><span data-stu-id="ab62d-169">hello directory where Maven is installed</span></span>

## <a name="configure-event-hub"></a><span data-ttu-id="ab62d-170">Konfigurera Event Hub</span><span class="sxs-lookup"><span data-stu-id="ab62d-170">Configure Event Hub</span></span>

<span data-ttu-id="ab62d-171">Händelsehubbar är hello datakälla för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ab62d-171">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="ab62d-172">Använd följande steg toocreate en Händelsehubb hello.</span><span class="sxs-lookup"><span data-stu-id="ab62d-172">Use hello following steps toocreate a Event Hub.</span></span>

1. <span data-ttu-id="ab62d-173">Från hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **ny** > **Service Bus** > **Händelsehubb**  >  **Skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="ab62d-173">From hello [Azure Classic Portal](https://manage.windowsazure.com), select **NEW** > **Service Bus** > **Event Hub** > **Custom Create**.</span></span>

2. <span data-ttu-id="ab62d-174">På hello **lägga till en ny Händelsehubb** anger en **Händelsehubbens namn**.</span><span class="sxs-lookup"><span data-stu-id="ab62d-174">On hello **Add a new Event Hub** screen, enter an **Event Hub Name**.</span></span> <span data-ttu-id="ab62d-175">Välj hello **Region** toocreate hello hubben i, och sedan skapa ett namnområde eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="ab62d-175">Select hello **Region** toocreate hello hub in, and then create a namespace or select an existing one.</span></span> <span data-ttu-id="ab62d-176">Klicka slutligen på hello **pilen** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="ab62d-176">Finally, click hello **Arrow** toocontinue.</span></span>

    ![sida 1 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > <span data-ttu-id="ab62d-178">Välj hello samma **plats** som ditt Storm på HDInsight server tooreduce svarstid och kostnader.</span><span class="sxs-lookup"><span data-stu-id="ab62d-178">Select hello same **Location** as your Storm on HDInsight server tooreduce latency and costs.</span></span>

3. <span data-ttu-id="ab62d-179">På hello **konfigurera Event Hub** anger hello **partitions antal** och **meddelandet kvarhållning** värden.</span><span class="sxs-lookup"><span data-stu-id="ab62d-179">On hello **Configure Event Hub** screen, enter hello **Partition count** and **Message Retention** values.</span></span> <span data-ttu-id="ab62d-180">I det här exemplet använder du en partitionsantal 10 och ett meddelande kvarhållning av 1.</span><span class="sxs-lookup"><span data-stu-id="ab62d-180">For this example, use a partition count of 10 and a message retention of 1.</span></span> <span data-ttu-id="ab62d-181">Observera hello partitionsantal eftersom du behöver det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="ab62d-181">Note hello partition count because you need this value later.</span></span>

    ![sida 2 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. <span data-ttu-id="ab62d-183">Efter hello händelsehubb har skapats, Välj hello namnområdet, Välj **Händelsehubbar**, och välj sedan hello händelsehubb som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="ab62d-183">After hello event hub has been created, select hello namespace, select **Event Hubs**, and then select hello event hub that you created earlier.</span></span>
5. <span data-ttu-id="ab62d-184">Välj **konfigurera**, sedan skapa två nya åtkomstprinciper för med hjälp av hello följande information:</span><span class="sxs-lookup"><span data-stu-id="ab62d-184">Select **Configure**, then create two new access policies by using hello following information:</span></span>

    <table>
    <tr><th><span data-ttu-id="ab62d-185">Namn</span><span class="sxs-lookup"><span data-stu-id="ab62d-185">Name</span></span></th><th><span data-ttu-id="ab62d-186">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="ab62d-186">Permissions</span></span></th></tr>
    <tr><td><span data-ttu-id="ab62d-187">Skrivare</span><span class="sxs-lookup"><span data-stu-id="ab62d-187">Writer</span></span></td><td><span data-ttu-id="ab62d-188">Skicka</span><span class="sxs-lookup"><span data-stu-id="ab62d-188">Send</span></span></td></tr>
    <tr><td><span data-ttu-id="ab62d-189">Läsare</span><span class="sxs-lookup"><span data-stu-id="ab62d-189">Reader</span></span></td><td><span data-ttu-id="ab62d-190">Lyssna</span><span class="sxs-lookup"><span data-stu-id="ab62d-190">Listen</span></span></td></tr>
    </table>

    <span data-ttu-id="ab62d-191">När du har skapat hello behörigheter väljer hello **spara** ikon på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="ab62d-191">After You create hello permissions, select hello **Save** icon at hello bottom of hello page.</span></span> <span data-ttu-id="ab62d-192">Dessa principer för delad åtkomst används tooread och skriva tooEvent hubb.</span><span class="sxs-lookup"><span data-stu-id="ab62d-192">These shared access policies are used tooread and write tooEvent Hub.</span></span>

    ![principer](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. <span data-ttu-id="ab62d-194">När du har sparat hello principer använda hello **nyckelgenerator för delad åtkomst** längst hello hello sidan tooretrieve hello nyckel för hello **writer** och **reader** principer.</span><span class="sxs-lookup"><span data-stu-id="ab62d-194">After you save hello policies, use hello **Shared access key generator** at hello bottom of hello page tooretrieve hello key for hello **writer** and **reader** policies.</span></span> <span data-ttu-id="ab62d-195">Spara de här nycklarna.</span><span class="sxs-lookup"><span data-stu-id="ab62d-195">Save these keys.</span></span>

## <a name="download-and-build-hello-project"></a><span data-ttu-id="ab62d-196">Hämta och skapa hello-projekt</span><span class="sxs-lookup"><span data-stu-id="ab62d-196">Download and build hello project</span></span>

1. <span data-ttu-id="ab62d-197">Hämta hello projektet från GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="ab62d-197">Download hello project from GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span></span> <span data-ttu-id="ab62d-198">Du kan hämta hello paketet som ett zip-arkiv, eller använda [git](https://git-scm.com/) tooclone hello-projekt lokalt.</span><span class="sxs-lookup"><span data-stu-id="ab62d-198">You can either download hello package as a zip archive, or use [git](https://git-scm.com/) tooclone hello project locally.</span></span>

2. <span data-ttu-id="ab62d-199">Ändra hello `dev.properties` fil med hello konfigurationen för din Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="ab62d-199">Modify hello `dev.properties` file with hello configuration for your Event Hub.</span></span>

3. <span data-ttu-id="ab62d-200">Använd hello följande toobuild och paketet hello-projektet:</span><span class="sxs-lookup"><span data-stu-id="ab62d-200">Use hello following toobuild and package hello project:</span></span>

        mvn package

    <span data-ttu-id="ab62d-201">Det här kommandot laddar ned nödvändiga beroendena, versioner, och sedan paket hello projektet.</span><span class="sxs-lookup"><span data-stu-id="ab62d-201">This command downloads required dependencies, builds, and then packages hello project.</span></span> <span data-ttu-id="ab62d-202">hello utdata lagras i hello **/target** katalogen som **EventHubExample-1.0-SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="ab62d-202">hello output is stored in hello **/target** directory as **EventHubExample-1.0-SNAPSHOT.jar**.</span></span>

## <a name="test-locally"></a><span data-ttu-id="ab62d-203">Testa lokalt</span><span class="sxs-lookup"><span data-stu-id="ab62d-203">Test locally</span></span>

<span data-ttu-id="ab62d-204">Eftersom dessa topologier bara läsa och skriva tooEvent Hubs, kan du testa dem lokalt om du har en [Storm utvecklingsmiljö](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="ab62d-204">Since these topologies just read and write tooEvent Hubs, you can test them locally if you have a [Storm development environment](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span></span> <span data-ttu-id="ab62d-205">Använd följande steg toorun lokalt i hello utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="ab62d-205">Use hello following steps toorun locally in hello dev environment:</span></span>

1. <span data-ttu-id="ab62d-206">Kör hello writer:</span><span class="sxs-lookup"><span data-stu-id="ab62d-206">Run hello writer:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. <span data-ttu-id="ab62d-207">Kör hello läsare:</span><span class="sxs-lookup"><span data-stu-id="ab62d-207">Run hello reader:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * <span data-ttu-id="ab62d-208">`--local`: Kör hello topologi i lokalt läge (icke-distribuerade).</span><span class="sxs-lookup"><span data-stu-id="ab62d-208">`--local`: Run hello topology in local mode (non-distributed).</span></span>
> * <span data-ttu-id="ab62d-209">`-R /writer.yaml`: Läsa in definitionen av hello topologi från hello `resources` paketeras i hello jar.</span><span class="sxs-lookup"><span data-stu-id="ab62d-209">`-R /writer.yaml`: Load hello topology definition from hello `resources` packaged in hello jar.</span></span> <span data-ttu-id="ab62d-210">Ange hello sökvägen tooit som sista parameter i hello om hello topologin är en fil på hello lokala filsystem.</span><span class="sxs-lookup"><span data-stu-id="ab62d-210">If hello topology is a file on hello local file system, specify hello path tooit as hello last parameter instead.</span></span>
> * <span data-ttu-id="ab62d-211">`--filter dev.properties`: Använd hello innehållet i `dev.properties` toofill i hello värden i hello topologi definitioner.</span><span class="sxs-lookup"><span data-stu-id="ab62d-211">`--filter dev.properties`: Use hello contents of `dev.properties` toofill in hello values in hello topology definitions.</span></span> <span data-ttu-id="ab62d-212">Till exempel `${eventhub.read.policy.name}`.</span><span class="sxs-lookup"><span data-stu-id="ab62d-212">For example, `${eventhub.read.policy.name}`.</span></span>

<span data-ttu-id="ab62d-213">Utdata är loggade toohello konsolen när du kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="ab62d-213">Output is logged toohello console when running locally.</span></span> <span data-ttu-id="ab62d-214">Använd __Ctrl + C__ toostop hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="ab62d-214">Use __Ctrl+C__ toostop hello topology.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="ab62d-215">Distribuera hello topologier</span><span class="sxs-lookup"><span data-stu-id="ab62d-215">Deploy hello topologies</span></span>

1. <span data-ttu-id="ab62d-216">Använd SCP toocopy hello jar paketet tooyour HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab62d-216">Use SCP toocopy hello jar package tooyour HDInsight cluster.</span></span> <span data-ttu-id="ab62d-217">Ersätt användarnamn med hello SSH-användare för klustret.</span><span class="sxs-lookup"><span data-stu-id="ab62d-217">Replace USERNAME with hello SSH user for your cluster.</span></span> <span data-ttu-id="ab62d-218">Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="ab62d-218">Replace CLUSTERNAME with hello name of your HDInsight cluster:</span></span>

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    <span data-ttu-id="ab62d-219">Om du använder ett lösenord för SSH-konto kan du ange tooenter hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ab62d-219">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="ab62d-220">Om du har använt en SSH-nyckel med hello-konto kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="ab62d-220">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="ab62d-221">Till exempel, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span><span class="sxs-lookup"><span data-stu-id="ab62d-221">For example, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span></span>

    <span data-ttu-id="ab62d-222">Det här kommandot kopieras hello filen toohello-hemkataloger SSH-användare på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="ab62d-222">This command copies hello file toohello home directory of your SSH user on hello cluster.</span></span>

2. <span data-ttu-id="ab62d-223">När hello-filen har överförts, kan du använda SSH tooconnect toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab62d-223">Once hello file has finished uploading, use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="ab62d-224">Ersätt **användarnamn** hello namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="ab62d-224">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="ab62d-225">Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="ab62d-225">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > <span data-ttu-id="ab62d-226">Om du använder ett lösenord för SSH-konto kan du ange tooenter hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="ab62d-226">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="ab62d-227">Om du har använt en SSH-nyckel med hello-konto kan du behöva toouse hello `-i` parametern toospecify hello sökvägen toohello nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="ab62d-227">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="ab62d-228">hello följande exempel laddas hello privata nyckel från `~/.ssh/id_rsa`:</span><span class="sxs-lookup"><span data-stu-id="ab62d-228">hello following example loads hello private key from `~/.ssh/id_rsa`:</span></span>
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. <span data-ttu-id="ab62d-229">Använd följande kommando toostart hello topologier hello:</span><span class="sxs-lookup"><span data-stu-id="ab62d-229">Use hello following command toostart hello topologies:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * <span data-ttu-id="ab62d-230">`--remote`: Skickar hello topologi toohello Nimbus-tjänsten, som börjar på hello arbetarnoder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="ab62d-230">`--remote`: Submits hello topology toohello Nimbus service, which starts it on hello worker nodes in hello cluster.</span></span>

4. <span data-ttu-id="ab62d-231">tooview hello loggade data gå toohttps://CLUSTERNAME.azurehdinsight.net/stormui, där __KLUSTERNAMN__ är hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab62d-231">tooview hello logged data, go toohttps://CLUSTERNAME.azurehdinsight.net/stormui, where __CLUSTERNAME__ is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="ab62d-232">Välj hello topologier och detaljnivån toohello komponenter.</span><span class="sxs-lookup"><span data-stu-id="ab62d-232">Select hello topologies and drill down toohello components.</span></span> <span data-ttu-id="ab62d-233">Välj hello __port__ post för en instans av en komponent tooview loggade information.</span><span class="sxs-lookup"><span data-stu-id="ab62d-233">Select hello __port__ entry for an instance of a component tooview logged information.</span></span>

5. <span data-ttu-id="ab62d-234">Använd följande kommandon toostop hello topologier hello:</span><span class="sxs-lookup"><span data-stu-id="ab62d-234">Use hello following commands toostop hello topologies:</span></span>

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a><span data-ttu-id="ab62d-235">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="ab62d-235">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="ab62d-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ab62d-236">Next steps</span></span>

* [<span data-ttu-id="ab62d-237">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab62d-237">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
