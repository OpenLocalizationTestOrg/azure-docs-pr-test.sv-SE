---
title: "Bearbeta händelser från Event Hubs med Storm på HDInsight använder Java | Microsoft Docs"
description: "Lär dig mer om att bearbeta data i Händelsehubbar med en Java Storm-topologi som skapats med Maven."
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
ms.openlocfilehash: 2e8ebbdab2be7bed224a67facec798820615bb22
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a><span data-ttu-id="19656-103">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="19656-103">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>

<span data-ttu-id="19656-104">Lär dig hur du använder Azure Event Hubs med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="19656-104">Learn how to use Azure Event Hubs with Storm on HDInsight.</span></span> <span data-ttu-id="19656-105">Det här exemplet använder Java-baserade komponenter för att läsa och skriva data i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="19656-105">This example uses Java-based components to read and write data in Azure Event Hubs.</span></span>

<span data-ttu-id="19656-106">Händelsehubbar i Azure kan du bearbetar stora mängder data från webbplatser, appar och enheter.</span><span class="sxs-lookup"><span data-stu-id="19656-106">Azure Event Hubs allows you to process massive amounts of data from websites, apps, and devices.</span></span> <span data-ttu-id="19656-107">Event Hub-kanal gör det enkelt att använda Apache Storm på HDInsight för att analysera data i realtid.</span><span class="sxs-lookup"><span data-stu-id="19656-107">The Event Hub spout makes it easy to use Apache Storm on HDInsight to analyze this data in real time.</span></span> <span data-ttu-id="19656-108">Du kan också skriva data till Händelsehubbar från Storm med hjälp av bulten Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="19656-108">You can also write data to Event Hubs from Storm by using the Event Hubs bolt.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="19656-109">Krav</span><span class="sxs-lookup"><span data-stu-id="19656-109">Prerequisites</span></span>

* <span data-ttu-id="19656-110">En Apache Storm på HDInsight-kluster av version 3,6.</span><span class="sxs-lookup"><span data-stu-id="19656-110">An Apache Storm on HDInsight cluster version 3.6.</span></span> <span data-ttu-id="19656-111">Mer information finns i [Kom igång med Storm på HDInsight-kluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="19656-111">For more information, see [Get started with Storm on HDInsight cluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="19656-112">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="19656-112">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="19656-113">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="19656-113">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="19656-114">En [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="19656-114">An [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="19656-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, såsom [OpenJDK](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="19656-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or equivalent, such as [OpenJDK](http://openjdk.java.net/).</span></span>

* <span data-ttu-id="19656-116">[Maven](https://maven.apache.org/download.cgi): Maven är ett projekt build-system för Java-projekt.</span><span class="sxs-lookup"><span data-stu-id="19656-116">[Maven](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="19656-117">En textredigerare eller integrerad utvecklingsmiljö (IDE).</span><span class="sxs-lookup"><span data-stu-id="19656-117">A text editor or integrated development environment (IDE).</span></span>

    > [!NOTE]
    > <span data-ttu-id="19656-118">Editor- eller IDE kan ha specifika funktioner för att arbeta med Maven som inte riktar sig i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="19656-118">Your editor or IDE may have specific functionality for working with Maven that is not addressed in this document.</span></span> <span data-ttu-id="19656-119">Information om funktionerna i din miljö för redigering finns i dokumentationen för produkten som du använder.</span><span class="sxs-lookup"><span data-stu-id="19656-119">For information about the capabilities of your editing environment, see the documentation for the product you are using.</span></span>

    * <span data-ttu-id="19656-120">En SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="19656-120">An SSH client.</span></span> <span data-ttu-id="19656-121">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="19656-121">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="19656-122">Den `ssh` och `scp` kommandon.</span><span class="sxs-lookup"><span data-stu-id="19656-122">The `ssh` and `scp` commands.</span></span> <span data-ttu-id="19656-123">Dessa används för att kopiera filer till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="19656-123">These are used to copy files to the HDInsight cluster.</span></span> <span data-ttu-id="19656-124">Du kan hämta dessa via Bash på Windows 10 på Windows.</span><span class="sxs-lookup"><span data-stu-id="19656-124">On Windows, you can get these through Bash on Windows 10.</span></span>

## <a name="understanding-the-example"></a><span data-ttu-id="19656-125">Förstå exemplet</span><span class="sxs-lookup"><span data-stu-id="19656-125">Understanding the example</span></span>

<span data-ttu-id="19656-126">Den [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) exempel innehåller två topologier:</span><span class="sxs-lookup"><span data-stu-id="19656-126">The [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) example contains two topologies:</span></span>

<span data-ttu-id="19656-127">Den `resources/writer.yaml` topologi skriver slumpmässiga data till en Azure-Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="19656-127">The `resources/writer.yaml` topology writes random data to an Azure Event Hub.</span></span> <span data-ttu-id="19656-128">Data som genereras av den `DeviceSpout` komponent, och är en slumpmässig enhets-ID och ett värde för enheten.</span><span class="sxs-lookup"><span data-stu-id="19656-128">The data is generated by the `DeviceSpout` component, and is a random device ID and device value.</span></span> <span data-ttu-id="19656-129">Det därför simulera vissa maskinvara som genererar ett sträng-ID och ett numeriskt värde.</span><span class="sxs-lookup"><span data-stu-id="19656-129">So it's simulating some hardware that emits a string ID and a numeric value.</span></span>

<span data-ttu-id="19656-130">Dig `resources/reader.yaml` topologi läser data från Event Hub (data som skrivits av EventHubWriter,) Parsar JSON-data och sedan loggar den `deviceId` och `deviceValue` data.</span><span class="sxs-lookup"><span data-stu-id="19656-130">Thee `resources/reader.yaml` topology reads data from Event Hub (the data written by EventHubWriter,) parses the JSON data, and then logs the `deviceId` and `deviceValue` data.</span></span>

<span data-ttu-id="19656-131">Data formateras som JSON-dokument innan den kan skrivas till Händelsehubben och när läses av läsaren tolkas utanför JSON och i tupplar.</span><span class="sxs-lookup"><span data-stu-id="19656-131">The data is formatted as a JSON document before it is written to Event Hub, and when read by the reader it is parsed out of JSON and into tuples.</span></span> <span data-ttu-id="19656-132">JSON-formatet är:</span><span class="sxs-lookup"><span data-stu-id="19656-132">The JSON format is as follows:</span></span>

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a><span data-ttu-id="19656-133">Konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="19656-133">Project configuration</span></span>

<span data-ttu-id="19656-134">Den `POM.xml` filen innehåller konfigurationsinformation för det här Maven-projekt.</span><span class="sxs-lookup"><span data-stu-id="19656-134">The `POM.xml` file contains configuration information for this Maven project.</span></span> <span data-ttu-id="19656-135">Intressanta delarna är:</span><span class="sxs-lookup"><span data-stu-id="19656-135">The interesting pieces are:</span></span>

#### <a name="event-hub-components"></a><span data-ttu-id="19656-136">Event Hub komponenter</span><span class="sxs-lookup"><span data-stu-id="19656-136">Event Hub components</span></span>

<span data-ttu-id="19656-137">Den komponent som läser och skriver till Azure Event Hubs finns i den [HDInsight databasen](https://github.com/hdinsight/mvn-rep).</span><span class="sxs-lookup"><span data-stu-id="19656-137">The component that reads and writes to Azure Event Hubs is located in the [HDInsight repository](https://github.com/hdinsight/mvn-rep).</span></span> <span data-ttu-id="19656-138">I de följande avsnitten i den `POM.xml` fil att läsa in komponenterna från den här lagringsplatsen</span><span class="sxs-lookup"><span data-stu-id="19656-138">The following sections in the `POM.xml` file load the components from this repository</span></span>

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="the-eventhubs-storm-spout-dependency"></a><span data-ttu-id="19656-139">Beroendet EventHubs Storm-kanalen</span><span class="sxs-lookup"><span data-stu-id="19656-139">The EventHubs Storm Spout dependency</span></span>

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

<span data-ttu-id="19656-140">Den här xml definierar ett beroende för eventhubs paket, som innehåller både en kanal för att läsa från Event Hubs och en bult för att skriva till den.</span><span class="sxs-lookup"><span data-stu-id="19656-140">This xml defines a dependency for the eventhubs package, which contains both a spout for reading from Event Hubs, and a bolt for writing to it.</span></span>

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

<span data-ttu-id="19656-141">Den här xml konfigurerar projektet för att generera utdata för Java 8, som används av HDInsight 3.5 eller högre.</span><span class="sxs-lookup"><span data-stu-id="19656-141">This xml configures the project to generate output for Java 8, which is used by HDInsight 3.5 or higher.</span></span>

#### <a name="the-maven-shade-plugin"></a><span data-ttu-id="19656-142">Maven-skugga-plugin-programmet</span><span class="sxs-lookup"><span data-stu-id="19656-142">The maven-shade-plugin</span></span>

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

<span data-ttu-id="19656-143">Den här xml konfigurerar lösningen för att paketera utdata till en uber jar.</span><span class="sxs-lookup"><span data-stu-id="19656-143">This xml configures the solution to package the output into an uber jar.</span></span> <span data-ttu-id="19656-144">Jar innehåller både Projektkod och nödvändiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="19656-144">The jar contains both the project code and required dependencies.</span></span> <span data-ttu-id="19656-145">Det används också för att:</span><span class="sxs-lookup"><span data-stu-id="19656-145">It is also used to:</span></span>

* <span data-ttu-id="19656-146">Byt namn på licensfiler för beroenden.</span><span class="sxs-lookup"><span data-stu-id="19656-146">Rename license files for the dependencies.</span></span>
* <span data-ttu-id="19656-147">Exkludera security-signaturer.</span><span class="sxs-lookup"><span data-stu-id="19656-147">Exclude security/signatures.</span></span>
* <span data-ttu-id="19656-148">Se till att flera implementeringar av samma gränssnitt slås samman till en transaktion.</span><span class="sxs-lookup"><span data-stu-id="19656-148">Ensure that multiple implementations of the same interface are merged into one entry.</span></span>

<span data-ttu-id="19656-149">Dessa konfigurationsinställningar undvika fel vid körning.</span><span class="sxs-lookup"><span data-stu-id="19656-149">These configuration settings prevent errors at runtime.</span></span>

#### <a name="topology-definitions"></a><span data-ttu-id="19656-150">Topologi definitioner</span><span class="sxs-lookup"><span data-stu-id="19656-150">Topology definitions</span></span>

<span data-ttu-id="19656-151">Det här exemplet används den [som](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span><span class="sxs-lookup"><span data-stu-id="19656-151">This example uses the [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span></span> <span data-ttu-id="19656-152">Det här ramverket använder YAML för att definiera topologierna.</span><span class="sxs-lookup"><span data-stu-id="19656-152">This framework uses YAML to define the topologies.</span></span> <span data-ttu-id="19656-153">Den största fördelen är att du inte hårddisken kodning topologi i Java-kod.</span><span class="sxs-lookup"><span data-stu-id="19656-153">The primary benefit is that you aren't hard coding the topology in Java code.</span></span> <span data-ttu-id="19656-154">Du kan ändra den innan du skickar topologi, utan att kompilera om allt eftersom definitionen YAML.</span><span class="sxs-lookup"><span data-stu-id="19656-154">Since the definition is YAML, you can change it before submitting the topology, without having to recompile everything.</span></span>

<span data-ttu-id="19656-155">__Writer.yaml__:</span><span class="sxs-lookup"><span data-stu-id="19656-155">__writer.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure the Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
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
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through the components
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

<span data-ttu-id="19656-156">__Reader.yaml__:</span><span class="sxs-lookup"><span data-stu-id="19656-156">__reader.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure the Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
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
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
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

# How data flows through the components
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

#### <a name="tell-the-topology-about-event-hub"></a><span data-ttu-id="19656-157">Berätta Event Hub topologin</span><span class="sxs-lookup"><span data-stu-id="19656-157">Tell the topology about Event Hub</span></span>

<span data-ttu-id="19656-158">Vid körning av `dev.properties` används för att skicka Event Hub-konfigurationen till i topologin.</span><span class="sxs-lookup"><span data-stu-id="19656-158">At run time, the `dev.properties` file is used to pass the Event Hub configuration to the topology.</span></span> <span data-ttu-id="19656-159">Följande exempel är standardinnehållet i filen:</span><span class="sxs-lookup"><span data-stu-id="19656-159">The following example is the default contents of the file:</span></span>

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a><span data-ttu-id="19656-160">Konfigurera miljövariabler</span><span class="sxs-lookup"><span data-stu-id="19656-160">Configure environment variables</span></span>

<span data-ttu-id="19656-161">Följande miljövariabler kan anges när du installerar Java och JDK på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="19656-161">The following environment variables may be set when you install Java and the JDK on your development workstation.</span></span> <span data-ttu-id="19656-162">Dock bör du kontrollera att de finns och att de innehåller rätt värden för ditt system.</span><span class="sxs-lookup"><span data-stu-id="19656-162">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="19656-163">**JAVA_HOME** -måste peka på den katalog där med Java runtime environment (JRE) har installerats.</span><span class="sxs-lookup"><span data-stu-id="19656-163">**JAVA_HOME** - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="19656-164">Till exempel en Unix- eller Linux-distribution, den inte innehålla ett värde som liknar `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="19656-164">For example, in a Unix or Linux distribution, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="19656-165">I Windows, skulle det ha ett värde som liknar`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="19656-165">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>
* <span data-ttu-id="19656-166">**SÖKVÄGEN** -bör innehålla följande sökvägar:</span><span class="sxs-lookup"><span data-stu-id="19656-166">**PATH** - should contain the following paths:</span></span>

  * <span data-ttu-id="19656-167">**JAVA_HOME** (eller motsvarande sökväg)</span><span class="sxs-lookup"><span data-stu-id="19656-167">**JAVA_HOME** (or the equivalent path)</span></span>
  * <span data-ttu-id="19656-168">**JAVA_HOME\bin** (eller motsvarande sökväg)</span><span class="sxs-lookup"><span data-stu-id="19656-168">**JAVA_HOME\bin** (or the equivalent path)</span></span>
  * <span data-ttu-id="19656-169">Katalogen där Maven är installerat</span><span class="sxs-lookup"><span data-stu-id="19656-169">The directory where Maven is installed</span></span>

## <a name="configure-event-hub"></a><span data-ttu-id="19656-170">Konfigurera Event Hub</span><span class="sxs-lookup"><span data-stu-id="19656-170">Configure Event Hub</span></span>

<span data-ttu-id="19656-171">Händelsehubbar är datakällan för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="19656-171">Event Hubs is the data source for this example.</span></span> <span data-ttu-id="19656-172">Använd följande steg för att skapa en Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="19656-172">Use the following steps to create a Event Hub.</span></span>

1. <span data-ttu-id="19656-173">Från den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **ny** > **Service Bus** > **Händelsehubb** > **skapa anpassade**.</span><span class="sxs-lookup"><span data-stu-id="19656-173">From the [Azure Classic Portal](https://manage.windowsazure.com), select **NEW** > **Service Bus** > **Event Hub** > **Custom Create**.</span></span>

2. <span data-ttu-id="19656-174">På den **lägga till en ny Händelsehubb** anger en **Händelsehubbens namn**.</span><span class="sxs-lookup"><span data-stu-id="19656-174">On the **Add a new Event Hub** screen, enter an **Event Hub Name**.</span></span> <span data-ttu-id="19656-175">Välj den **Region** att skapa hubben i, och skapa ett namnområde eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="19656-175">Select the **Region** to create the hub in, and then create a namespace or select an existing one.</span></span> <span data-ttu-id="19656-176">Klicka slutligen på den **pilen** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="19656-176">Finally, click the **Arrow** to continue.</span></span>

    ![sida 1 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > <span data-ttu-id="19656-178">Välj samma **plats** som ditt Storm på HDInsight-servern för att minska latensen och kostnader.</span><span class="sxs-lookup"><span data-stu-id="19656-178">Select the same **Location** as your Storm on HDInsight server to reduce latency and costs.</span></span>

3. <span data-ttu-id="19656-179">På den **konfigurera Event Hub** anger den **partitions antal** och **meddelandet kvarhållning** värden.</span><span class="sxs-lookup"><span data-stu-id="19656-179">On the **Configure Event Hub** screen, enter the **Partition count** and **Message Retention** values.</span></span> <span data-ttu-id="19656-180">I det här exemplet använder du en partitionsantal 10 och ett meddelande kvarhållning av 1.</span><span class="sxs-lookup"><span data-stu-id="19656-180">For this example, use a partition count of 10 and a message retention of 1.</span></span> <span data-ttu-id="19656-181">Observera antalet partitioner eftersom du behöver det här värdet senare.</span><span class="sxs-lookup"><span data-stu-id="19656-181">Note the partition count because you need this value later.</span></span>

    ![sida 2 i guiden](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. <span data-ttu-id="19656-183">När händelsehubben har skapats, Välj namnområdet, Välj **Händelsehubbar**, och välj sedan händelsehubben som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="19656-183">After the event hub has been created, select the namespace, select **Event Hubs**, and then select the event hub that you created earlier.</span></span>
5. <span data-ttu-id="19656-184">Välj **konfigurera**, sedan skapa två nya åtkomstprinciper för med hjälp av följande information:</span><span class="sxs-lookup"><span data-stu-id="19656-184">Select **Configure**, then create two new access policies by using the following information:</span></span>

    <table>
    <tr><th><span data-ttu-id="19656-185">Namn</span><span class="sxs-lookup"><span data-stu-id="19656-185">Name</span></span></th><th><span data-ttu-id="19656-186">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="19656-186">Permissions</span></span></th></tr>
    <tr><td><span data-ttu-id="19656-187">Skrivare</span><span class="sxs-lookup"><span data-stu-id="19656-187">Writer</span></span></td><td><span data-ttu-id="19656-188">Skicka</span><span class="sxs-lookup"><span data-stu-id="19656-188">Send</span></span></td></tr>
    <tr><td><span data-ttu-id="19656-189">Läsare</span><span class="sxs-lookup"><span data-stu-id="19656-189">Reader</span></span></td><td><span data-ttu-id="19656-190">Lyssna</span><span class="sxs-lookup"><span data-stu-id="19656-190">Listen</span></span></td></tr>
    </table>

    <span data-ttu-id="19656-191">När du har skapat behörigheterna som du väljer den **spara** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="19656-191">After You create the permissions, select the **Save** icon at the bottom of the page.</span></span> <span data-ttu-id="19656-192">Dessa principer för delad åtkomst används för att läsa och skriva till Händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="19656-192">These shared access policies are used to read and write to Event Hub.</span></span>

    ![principer](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. <span data-ttu-id="19656-194">När du har sparat principerna använder den **nyckelgenerator för delad åtkomst** längst ned på sidan för att hämta nyckeln för den **writer** och **reader** principer.</span><span class="sxs-lookup"><span data-stu-id="19656-194">After you save the policies, use the **Shared access key generator** at the bottom of the page to retrieve the key for the **writer** and **reader** policies.</span></span> <span data-ttu-id="19656-195">Spara de här nycklarna.</span><span class="sxs-lookup"><span data-stu-id="19656-195">Save these keys.</span></span>

## <a name="download-and-build-the-project"></a><span data-ttu-id="19656-196">Hämta och skapa projektet</span><span class="sxs-lookup"><span data-stu-id="19656-196">Download and build the project</span></span>

1. <span data-ttu-id="19656-197">Hämta projektet från GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="19656-197">Download the project from GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span></span> <span data-ttu-id="19656-198">Du kan hämta paketet som ett zip-arkiv, eller använda [git](https://git-scm.com/) att klona projektet lokalt.</span><span class="sxs-lookup"><span data-stu-id="19656-198">You can either download the package as a zip archive, or use [git](https://git-scm.com/) to clone the project locally.</span></span>

2. <span data-ttu-id="19656-199">Ändra den `dev.properties` fil med konfigurationen för din Händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="19656-199">Modify the `dev.properties` file with the configuration for your Event Hub.</span></span>

3. <span data-ttu-id="19656-200">Använd följande för att bygga och paket i projektet:</span><span class="sxs-lookup"><span data-stu-id="19656-200">Use the following to build and package the project:</span></span>

        mvn package

    <span data-ttu-id="19656-201">Det här kommandot laddar ned nödvändiga beroendena, versioner, och paket i projektet.</span><span class="sxs-lookup"><span data-stu-id="19656-201">This command downloads required dependencies, builds, and then packages the project.</span></span> <span data-ttu-id="19656-202">Utdata lagras i den **/target** katalogen som **EventHubExample-1.0-SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="19656-202">The output is stored in the **/target** directory as **EventHubExample-1.0-SNAPSHOT.jar**.</span></span>

## <a name="test-locally"></a><span data-ttu-id="19656-203">Testa lokalt</span><span class="sxs-lookup"><span data-stu-id="19656-203">Test locally</span></span>

<span data-ttu-id="19656-204">Eftersom dessa topologier kan bara läsa och skriva till Event Hubs, kan du testa dem lokalt om du har en [Storm utvecklingsmiljö](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="19656-204">Since these topologies just read and write to Event Hubs, you can test them locally if you have a [Storm development environment](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span></span> <span data-ttu-id="19656-205">Använd följande steg för att köra lokalt i dev-miljö:</span><span class="sxs-lookup"><span data-stu-id="19656-205">Use the following steps to run locally in the dev environment:</span></span>

1. <span data-ttu-id="19656-206">Kör skrivaren:</span><span class="sxs-lookup"><span data-stu-id="19656-206">Run the writer:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. <span data-ttu-id="19656-207">Kör läsaren:</span><span class="sxs-lookup"><span data-stu-id="19656-207">Run the reader:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * <span data-ttu-id="19656-208">`--local`: Kör topologin i lokalt läge (icke-distribuerade).</span><span class="sxs-lookup"><span data-stu-id="19656-208">`--local`: Run the topology in local mode (non-distributed).</span></span>
> * <span data-ttu-id="19656-209">`-R /writer.yaml`: Läsa in definitionen topologi från den `resources` paketeras i jar.</span><span class="sxs-lookup"><span data-stu-id="19656-209">`-R /writer.yaml`: Load the topology definition from the `resources` packaged in the jar.</span></span> <span data-ttu-id="19656-210">Ange sökvägen till den som den sista parametern om topologin är en fil på det lokala filsystemet.</span><span class="sxs-lookup"><span data-stu-id="19656-210">If the topology is a file on the local file system, specify the path to it as the last parameter instead.</span></span>
> * <span data-ttu-id="19656-211">`--filter dev.properties`: Använd innehållet i `dev.properties` att fylla i värdena i topologin definitioner.</span><span class="sxs-lookup"><span data-stu-id="19656-211">`--filter dev.properties`: Use the contents of `dev.properties` to fill in the values in the topology definitions.</span></span> <span data-ttu-id="19656-212">Till exempel `${eventhub.read.policy.name}`.</span><span class="sxs-lookup"><span data-stu-id="19656-212">For example, `${eventhub.read.policy.name}`.</span></span>

<span data-ttu-id="19656-213">Utdata loggas i konsolen när du kör lokalt.</span><span class="sxs-lookup"><span data-stu-id="19656-213">Output is logged to the console when running locally.</span></span> <span data-ttu-id="19656-214">Använd __Ctrl + C__ att stoppa topologin.</span><span class="sxs-lookup"><span data-stu-id="19656-214">Use __Ctrl+C__ to stop the topology.</span></span>

## <a name="deploy-the-topologies"></a><span data-ttu-id="19656-215">Distribuera topologierna</span><span class="sxs-lookup"><span data-stu-id="19656-215">Deploy the topologies</span></span>

1. <span data-ttu-id="19656-216">Använd SCP för att kopiera jar-paket till ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="19656-216">Use SCP to copy the jar package to your HDInsight cluster.</span></span> <span data-ttu-id="19656-217">Ersätt användarnamn med SSH-användare för klustret.</span><span class="sxs-lookup"><span data-stu-id="19656-217">Replace USERNAME with the SSH user for your cluster.</span></span> <span data-ttu-id="19656-218">Ersätt KLUSTERNAMN med namnet på ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="19656-218">Replace CLUSTERNAME with the name of your HDInsight cluster:</span></span>

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    <span data-ttu-id="19656-219">Om du använder ett lösenord för SSH-konto uppmanas du att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="19656-219">If you used a password for your SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="19656-220">Om du använder en SSH-nyckel med kontot, kan du behöva använda de `-i` parametern för att ange sökvägen till nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="19656-220">If you used an SSH key with the account, you may need to use the `-i` parameter to specify the path to the key file.</span></span> <span data-ttu-id="19656-221">Till exempel, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span><span class="sxs-lookup"><span data-stu-id="19656-221">For example, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span></span>

    <span data-ttu-id="19656-222">Det här kommandot kopieras filen till arbetskatalogen för SSH-användare i klustret.</span><span class="sxs-lookup"><span data-stu-id="19656-222">This command copies the file to the home directory of your SSH user on the cluster.</span></span>

2. <span data-ttu-id="19656-223">När filen har överförts, kan du använda SSH för att ansluta till HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="19656-223">Once the file has finished uploading, use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="19656-224">Ersätt **användarnamn** namnet på SSH-inloggning.</span><span class="sxs-lookup"><span data-stu-id="19656-224">Replace **USERNAME** the name of your SSH login.</span></span> <span data-ttu-id="19656-225">Ersätt **KLUSTERNAMN** med ditt HDInsight-klustrets namn:</span><span class="sxs-lookup"><span data-stu-id="19656-225">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > <span data-ttu-id="19656-226">Om du använder ett lösenord för SSH-konto uppmanas du att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="19656-226">If you used a password for your SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="19656-227">Om du använder en SSH-nyckel med kontot, kan du behöva använda de `-i` parametern för att ange sökvägen till nyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="19656-227">If you used an SSH key with the account, you may need to use the `-i` parameter to specify the path to the key file.</span></span> <span data-ttu-id="19656-228">I följande exempel laddas den privata nyckeln från `~/.ssh/id_rsa`:</span><span class="sxs-lookup"><span data-stu-id="19656-228">The following example loads the private key from `~/.ssh/id_rsa`:</span></span>
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. <span data-ttu-id="19656-229">Använd följande kommando för att starta topologierna:</span><span class="sxs-lookup"><span data-stu-id="19656-229">Use the following command to start the topologies:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * <span data-ttu-id="19656-230">`--remote`: Skickar topologi till Nimbus-tjänsten som börjar på worker-noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="19656-230">`--remote`: Submits the topology to the Nimbus service, which starts it on the worker nodes in the cluster.</span></span>

4. <span data-ttu-id="19656-231">Du kan visa data som loggats i https://CLUSTERNAME.azurehdinsight.net/stormui, där __KLUSTERNAMN__ är namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="19656-231">To view the logged data, go to https://CLUSTERNAME.azurehdinsight.net/stormui, where __CLUSTERNAME__ is the name of your HDInsight cluster.</span></span> <span data-ttu-id="19656-232">Välj topologierna och öka detaljnivån till komponenterna.</span><span class="sxs-lookup"><span data-stu-id="19656-232">Select the topologies and drill down to the components.</span></span> <span data-ttu-id="19656-233">Välj den __port__ post för en instans av en komponent som ska visa loggade informationen.</span><span class="sxs-lookup"><span data-stu-id="19656-233">Select the __port__ entry for an instance of a component to view logged information.</span></span>

5. <span data-ttu-id="19656-234">Använd följande kommandon för att stoppa topologierna:</span><span class="sxs-lookup"><span data-stu-id="19656-234">Use the following commands to stop the topologies:</span></span>

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a><span data-ttu-id="19656-235">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="19656-235">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="19656-236">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="19656-236">Next steps</span></span>

* [<span data-ttu-id="19656-237">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="19656-237">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
