---
title: "aaaProcess händelser från Event Hubs med Storm - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur tooprocess data från Azure Event Hubs med en C# Storm-topologi skapar i Visual Studio med hello HDInsight tools för Visual Studio."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="17b19-103">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="17b19-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="17b19-104">Lär dig hur toowork med Händelsehubbar från Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17b19-104">Learn how toowork with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="17b19-105">Det här dokumentet använder en C# Storm-topologi tooread och skriva data från Evbent hubbar</span><span class="sxs-lookup"><span data-stu-id="17b19-105">This document uses a C# Storm topology tooread and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="17b19-106">En Java-version av det här projektet finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="17b19-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="17b19-107">SCP.NET</span></span>

<span data-ttu-id="17b19-108">hello stegen i det här dokumentet använder SCP.NET NuGet-paketet som gör det enkelt toocreate C#-topologier och komponenter för användning med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17b19-108">hello steps in this document use SCP.NET, a NuGet package that makes it easy toocreate C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17b19-109">Medan hello stegen i det här dokumentet förlitar sig på en Windows-utvecklingsmiljö med Visual Studio, hello kompilerade projekt kan vara skickade tooa Storm på HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="17b19-109">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooa Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="17b19-110">Linux-baserade kluster som skapas efter den 28 oktober 2016 stöder endast SCP.NET topologier.</span><span class="sxs-lookup"><span data-stu-id="17b19-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="17b19-111">HDInsight 3.4 och större användning monoljud toorun C#-topologier.</span><span class="sxs-lookup"><span data-stu-id="17b19-111">HDInsight 3.4 and greater use Mono toorun C# topologies.</span></span> <span data-ttu-id="17b19-112">hello-exempel som används i det här dokumentet fungerar med HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="17b19-112">hello example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="17b19-113">Om du planerar att skapa egna .NET-lösningar för HDInsight, kontrollera hello [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokument för potentiella inkompatibiliteter.</span><span class="sxs-lookup"><span data-stu-id="17b19-113">If you plan on creating your own .NET solutions for HDInsight, check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="17b19-114">Klustret versionshantering</span><span class="sxs-lookup"><span data-stu-id="17b19-114">Cluster versioning</span></span>

<span data-ttu-id="17b19-115">Hej Microsoft.SCP.Net.SDK NuGet-paketet som du använder för ditt projekt måste matcha hello huvudversion av Storm installerad på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="17b19-115">hello Microsoft.SCP.Net.SDK NuGet package you use for your project must match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="17b19-116">HDInsight-version 3.5 och 3,6 använda Storm 1.x, så du måste använda SCP.NET version 1.0.x.x med dessa kluster.</span><span class="sxs-lookup"><span data-stu-id="17b19-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17b19-117">hello exemplet i det här dokumentet förväntar sig ett HDInsight-3.5 eller 3,6 klustret.</span><span class="sxs-lookup"><span data-stu-id="17b19-117">hello example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="17b19-118">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="17b19-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="17b19-119">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="17b19-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="17b19-120">C#-topologier måste också ha .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="17b19-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-toowork-with-event-hubs"></a><span data-ttu-id="17b19-121">Hur toowork med Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="17b19-121">How toowork with Event Hubs</span></span>

<span data-ttu-id="17b19-122">Microsoft tillhandahåller en uppsättning Java-komponenter som kan använda toocommunicate med Händelsehubbar från en Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="17b19-122">Microsoft provides a set of Java components that can be used toocommunicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="17b19-123">Du kan hitta hello Java Arkiv (JAR)-fil som innehåller ett HDInsight-3,6 kompatibel version av dessa komponenter på [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="17b19-123">You can find hello Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="17b19-124">Medan hello komponenter är skriven i Java, kan du enkelt använda dem från en C#-topologi.</span><span class="sxs-lookup"><span data-stu-id="17b19-124">While hello components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="17b19-125">i det här exemplet används hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="17b19-125">hello following components are used in this example:</span></span>

* <span data-ttu-id="17b19-126">__EventHubSpout__: läser data från Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="17b19-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="17b19-127">__EventHubBolt__: skriver data tooEvent Hubs.</span><span class="sxs-lookup"><span data-stu-id="17b19-127">__EventHubBolt__: Writes data tooEvent Hubs.</span></span>
* <span data-ttu-id="17b19-128">__EventHubSpoutConfig__: tooconfigure EventHubSpout används.</span><span class="sxs-lookup"><span data-stu-id="17b19-128">__EventHubSpoutConfig__: Used tooconfigure EventHubSpout.</span></span>
* <span data-ttu-id="17b19-129">__EventHubBoltConfig__: tooconfigure EventHubBolt används.</span><span class="sxs-lookup"><span data-stu-id="17b19-129">__EventHubBoltConfig__: Used tooconfigure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="17b19-130">Exempel på användning kanal</span><span class="sxs-lookup"><span data-stu-id="17b19-130">Example spout usage</span></span>

<span data-ttu-id="17b19-131">SCP.NET ger metoder för att lägga till en EventHubSpout tooyour topologi.</span><span class="sxs-lookup"><span data-stu-id="17b19-131">SCP.NET provides methods for adding an EventHubSpout tooyour topology.</span></span> <span data-ttu-id="17b19-132">Metoderna gör det enklare tooadd en kanal än att använda hello generiska metoder för att lägga till en Java-komponent.</span><span class="sxs-lookup"><span data-stu-id="17b19-132">These methods make it easier tooadd a spout than using hello generic methods for adding a Java component.</span></span> <span data-ttu-id="17b19-133">hello exemplet nedan visar hur toocreate en kanal med hjälp av hello __SetEventHubSpout__ och **EventHubSpoutConfig** metoder som tillhandahålls av SCP.NET:</span><span class="sxs-lookup"><span data-stu-id="17b19-133">hello following example demonstrates how toocreate a spout by using hello __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

<span data-ttu-id="17b19-134">hello det föregående exemplet skapas en ny kanal komponent med namnet __EventHubSpout__, och konfigurerar toocommunicate med en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="17b19-134">hello previous example creates a new spout component named __EventHubSpout__, and configures it toocommunicate with an event hub.</span></span> <span data-ttu-id="17b19-135">hello parallellitet tips för hello komponent anges toohello antalet partitioner i hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="17b19-135">hello parallelism hint for hello component is set toohello number of partitions in hello event hub.</span></span> <span data-ttu-id="17b19-136">Den här inställningen kan Storm toocreate en instans av hello-komponenten för varje partition.</span><span class="sxs-lookup"><span data-stu-id="17b19-136">This setting allows Storm toocreate an instance of hello component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="17b19-137">Exempel på bult användning</span><span class="sxs-lookup"><span data-stu-id="17b19-137">Example bolt usage</span></span>

<span data-ttu-id="17b19-138">Använd hello **JavaComponmentConstructor** metoden toocreate en instans av hello bulten.</span><span class="sxs-lookup"><span data-stu-id="17b19-138">Use hello **JavaComponmentConstructor** method toocreate an instance of hello bolt.</span></span> <span data-ttu-id="17b19-139">hello exemplet nedan visar hur toocreate och konfigurera en ny instans av hello **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="17b19-139">hello following example demonstrates how toocreate and configure a new instance of hello **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="17b19-140">Det här exemplet används en Clojure uttryck som skickas som en sträng, i stället för **JavaComponentConstructor** toocreate en **EventHubBoltConfig**, som gjorde hello kanal exempel.</span><span class="sxs-lookup"><span data-stu-id="17b19-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** toocreate an **EventHubBoltConfig**, as hello spout example did.</span></span> <span data-ttu-id="17b19-141">Antingen metoden fungerar.</span><span class="sxs-lookup"><span data-stu-id="17b19-141">Either method works.</span></span> <span data-ttu-id="17b19-142">Använd hello-metod som känns bästa tooyou.</span><span class="sxs-lookup"><span data-stu-id="17b19-142">Use hello method that feels best tooyou.</span></span>

## <a name="download-hello-completed-project"></a><span data-ttu-id="17b19-143">Ladda ned hello slutförts projekt</span><span class="sxs-lookup"><span data-stu-id="17b19-143">Download hello completed project</span></span>

<span data-ttu-id="17b19-144">Du kan hämta en fullständig version av hello projektet har skapats i den här kursen från [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="17b19-144">You can download a complete version of hello project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="17b19-145">Behöver du dock fortfarande tooprovide konfigurationsinställningarna genom att följa hello stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="17b19-145">However, you still need tooprovide configuration settings by following hello steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="17b19-146">Krav</span><span class="sxs-lookup"><span data-stu-id="17b19-146">Prerequisites</span></span>

* <span data-ttu-id="17b19-147">En [Apache Storm på HDInsight-kluster av version 3.5 eller 3,6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="17b19-148">hello-exempel som används i det här dokumentet kräver Storm på HDInsight version 3.5 eller 3,6.</span><span class="sxs-lookup"><span data-stu-id="17b19-148">hello example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="17b19-149">Detta fungerar inte med äldre versioner av HDInsight, på grund av toobreaking klassen namn ändras.</span><span class="sxs-lookup"><span data-stu-id="17b19-149">This does not work with older versions of HDInsight, due toobreaking class name changes.</span></span> <span data-ttu-id="17b19-150">En version av det här exemplet fungerar med äldre kluster, se [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="17b19-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="17b19-151">En [Azure händelsehubb](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="17b19-152">Hej [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="17b19-152">hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="17b19-153">Hej [HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-153">hello [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="17b19-154">Java JDK 1.8 eller senare på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="17b19-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="17b19-155">JDK hämtas från [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="17b19-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="17b19-156">Hej **JAVA_HOME** miljö variabeln måste punkt toohello katalog som innehåller Java.</span><span class="sxs-lookup"><span data-stu-id="17b19-156">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="17b19-157">Hej **%JAVA_HOME%/bin** katalogen måste vara i hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="17b19-157">hello **%JAVA_HOME%/bin** directory must be in hello path.</span></span>

## <a name="download-hello-event-hubs-components"></a><span data-ttu-id="17b19-158">Hämta hello Händelsehubbar komponenter</span><span class="sxs-lookup"><span data-stu-id="17b19-158">Download hello Event Hubs components</span></span>

<span data-ttu-id="17b19-159">Hämta hello Händelsehubbar prata och bultar komponenten från [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="17b19-159">Download hello Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="17b19-160">Skapa en katalog med namnet `eventhubspout`, och spara hello-filen till hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="17b19-160">Create a directory named `eventhubspout`, and save hello file into hello directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="17b19-161">Konfigurera Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="17b19-161">Configure Event Hubs</span></span>

<span data-ttu-id="17b19-162">Händelsehubbar är hello datakälla för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="17b19-162">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="17b19-163">Använd hello information i avsnittet ”Skapa en händelsehubb” Hej för i [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-163">Use hello information in hello "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="17b19-164">När hello händelsehubb har skapats kan du visa hello **EventHub** bladet i hello Azure portal och välj **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="17b19-164">After hello event hub has been created, view hello **EventHub** blade in hello Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="17b19-165">Välj **+ Lägg till** tooadd hello följande principer:</span><span class="sxs-lookup"><span data-stu-id="17b19-165">Select **+ Add** tooadd hello following policies:</span></span>

   | <span data-ttu-id="17b19-166">Namn</span><span class="sxs-lookup"><span data-stu-id="17b19-166">Name</span></span> | <span data-ttu-id="17b19-167">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="17b19-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="17b19-168">Skrivare</span><span class="sxs-lookup"><span data-stu-id="17b19-168">writer</span></span> |<span data-ttu-id="17b19-169">Skicka</span><span class="sxs-lookup"><span data-stu-id="17b19-169">Send</span></span> |
   | <span data-ttu-id="17b19-170">läsare</span><span class="sxs-lookup"><span data-stu-id="17b19-170">reader</span></span> |<span data-ttu-id="17b19-171">Lyssna</span><span class="sxs-lookup"><span data-stu-id="17b19-171">Listen</span></span> |

    ![Skärmbild av dela access principer fönster](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="17b19-173">Välj hello **reader** och **writer** principer.</span><span class="sxs-lookup"><span data-stu-id="17b19-173">Select hello **reader** and **writer** policies.</span></span> <span data-ttu-id="17b19-174">Kopiera och spara hello primärnyckelvärde för båda principerna enligt dessa värden används senare.</span><span class="sxs-lookup"><span data-stu-id="17b19-174">Copy and save hello primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-hello-eventhubwriter"></a><span data-ttu-id="17b19-175">Konfigurera hello EventHubWriter</span><span class="sxs-lookup"><span data-stu-id="17b19-175">Configure hello EventHubWriter</span></span>

1. <span data-ttu-id="17b19-176">Om du inte redan har installerat hello senaste versionen av hello HDInsight tools för Visual Studio finns [komma igång med HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="17b19-176">If you have not already installed hello latest version of hello HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="17b19-177">Hämta hello lösningen från [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="17b19-177">Download hello solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="17b19-178">I hello **EventHubWriter** projekt, öppna hello **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="17b19-178">In hello **EventHubWriter** project, open hello **App.config** file.</span></span> <span data-ttu-id="17b19-179">Använd hello information från hello händelsehubb att du konfigurerat tidigare toofill i hello-värdet för hello följande nycklar:</span><span class="sxs-lookup"><span data-stu-id="17b19-179">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="17b19-180">Nyckel</span><span class="sxs-lookup"><span data-stu-id="17b19-180">Key</span></span> | <span data-ttu-id="17b19-181">Värde</span><span class="sxs-lookup"><span data-stu-id="17b19-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="17b19-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="17b19-182">EventHubPolicyName</span></span> |<span data-ttu-id="17b19-183">skrivare (om du har använt ett annat namn för hello-princip med *skicka* behörighet, Använd i stället.)</span><span class="sxs-lookup"><span data-stu-id="17b19-183">writer (If you used a different name for hello policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="17b19-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="17b19-184">EventHubPolicyKey</span></span> |<span data-ttu-id="17b19-185">hello nyckel för hello writer princip.</span><span class="sxs-lookup"><span data-stu-id="17b19-185">hello key for hello writer policy.</span></span> |
   | <span data-ttu-id="17b19-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="17b19-186">EventHubNamespace</span></span> |<span data-ttu-id="17b19-187">hello-namnområde som innehåller din event hub.</span><span class="sxs-lookup"><span data-stu-id="17b19-187">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="17b19-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="17b19-188">EventHubName</span></span> |<span data-ttu-id="17b19-189">Din händelsehubbens namn.</span><span class="sxs-lookup"><span data-stu-id="17b19-189">Your event hub name.</span></span> |
   | <span data-ttu-id="17b19-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="17b19-190">EventHubPartitionCount</span></span> |<span data-ttu-id="17b19-191">hello antalet partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="17b19-191">hello number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="17b19-192">Spara och Stäng hello **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="17b19-192">Save and close hello **App.config** file.</span></span>

## <a name="configure-hello-eventhubreader"></a><span data-ttu-id="17b19-193">Konfigurera hello EventHubReader</span><span class="sxs-lookup"><span data-stu-id="17b19-193">Configure hello EventHubReader</span></span>

1. <span data-ttu-id="17b19-194">Öppna hello **EventHubReader** projekt.</span><span class="sxs-lookup"><span data-stu-id="17b19-194">Open hello **EventHubReader** project.</span></span>

2. <span data-ttu-id="17b19-195">Öppna hello **App.config** -filen för hello **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="17b19-195">Open hello **App.config** file for hello **EventHubReader**.</span></span> <span data-ttu-id="17b19-196">Använd hello information från hello händelsehubb att du konfigurerat tidigare toofill i hello-värdet för hello följande nycklar:</span><span class="sxs-lookup"><span data-stu-id="17b19-196">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="17b19-197">Nyckel</span><span class="sxs-lookup"><span data-stu-id="17b19-197">Key</span></span> | <span data-ttu-id="17b19-198">Värde</span><span class="sxs-lookup"><span data-stu-id="17b19-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="17b19-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="17b19-199">EventHubPolicyName</span></span> |<span data-ttu-id="17b19-200">läsare (om du har använt ett annat namn för hello-princip med *lyssna* behörighet, Använd i stället.)</span><span class="sxs-lookup"><span data-stu-id="17b19-200">reader (If you used a different name for hello policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="17b19-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="17b19-201">EventHubPolicyKey</span></span> |<span data-ttu-id="17b19-202">hello nyckel för hello reader princip.</span><span class="sxs-lookup"><span data-stu-id="17b19-202">hello key for hello reader policy.</span></span> |
   | <span data-ttu-id="17b19-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="17b19-203">EventHubNamespace</span></span> |<span data-ttu-id="17b19-204">hello-namnområde som innehåller din event hub.</span><span class="sxs-lookup"><span data-stu-id="17b19-204">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="17b19-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="17b19-205">EventHubName</span></span> |<span data-ttu-id="17b19-206">Din händelsehubbens namn.</span><span class="sxs-lookup"><span data-stu-id="17b19-206">Your event hub name.</span></span> |
   | <span data-ttu-id="17b19-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="17b19-207">EventHubPartitionCount</span></span> |<span data-ttu-id="17b19-208">hello antalet partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="17b19-208">hello number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="17b19-209">Spara och Stäng hello **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="17b19-209">Save and close hello **App.config** file.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="17b19-210">Distribuera hello topologier</span><span class="sxs-lookup"><span data-stu-id="17b19-210">Deploy hello topologies</span></span>

1. <span data-ttu-id="17b19-211">Från **Solution Explorer**, högerklicka på hello **EventHubReader** projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="17b19-211">From **Solution Explorer**, right-click hello **EventHubReader** project, and select **Submit tooStorm on HDInsight**.</span></span>

    ![Skärmbild av Solution Explorer med skicka tooStorm på HDInsight markerat](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="17b19-213">På hello **skicka topologi** väljer din **Storm-kluster**.</span><span class="sxs-lookup"><span data-stu-id="17b19-213">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="17b19-214">Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj hello-katalog som innehåller hello JAR-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="17b19-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file that you downloaded earlier.</span></span> <span data-ttu-id="17b19-215">Klicka slutligen på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="17b19-215">Finally, click **Submit**.</span></span>

    ![Dialogrutan Skicka topologi skärmbild](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="17b19-217">När hello-topologi har skickats, hello **Storm-topologier Viewer** visas.</span><span class="sxs-lookup"><span data-stu-id="17b19-217">When hello topology has been submitted, hello **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="17b19-218">tooview information om hello topologi, Välj hello **EventHubReader** topologi i hello till vänster.</span><span class="sxs-lookup"><span data-stu-id="17b19-218">tooview information about hello topology, select hello **EventHubReader** topology in hello left pane.</span></span>

    ![Skärmbild av Storm-topologier Viewer](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="17b19-220">Från **Solution Explorer**, högerklicka på hello **EventHubWriter** projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="17b19-220">From **Solution Explorer**, right-click hello **EventHubWriter** project, and select **Submit tooStorm on HDInsight**.</span></span>

5. <span data-ttu-id="17b19-221">På hello **skicka topologi** väljer din **Storm-kluster**.</span><span class="sxs-lookup"><span data-stu-id="17b19-221">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="17b19-222">Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj hello-katalog som innehåller hello JAR-filen du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="17b19-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file you downloaded earlier.</span></span> <span data-ttu-id="17b19-223">Klicka slutligen på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="17b19-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="17b19-224">När hello-topologi har skickats, uppdatera hello topologi listan i hello **Storm-topologier Viewer** tooverify båda topologierna körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="17b19-224">When hello topology has been submitted, refresh hello topology list in hello **Storm Topologies Viewer** tooverify that both topologies are running on hello cluster.</span></span>

7. <span data-ttu-id="17b19-225">I **Storm-topologier Viewer**väljer hello **EventHubReader** topologi.</span><span class="sxs-lookup"><span data-stu-id="17b19-225">In **Storm Topologies Viewer**, select hello **EventHubReader** topology.</span></span>

8. <span data-ttu-id="17b19-226">tooopen hello komponenten sammanfattning för hello bult dubbelklicka hello **LogBolt** komponenten i hello diagram.</span><span class="sxs-lookup"><span data-stu-id="17b19-226">tooopen hello component summary for hello bolt, double-click hello **LogBolt** component in hello diagram.</span></span>

9. <span data-ttu-id="17b19-227">I hello **Executors** väljer du någon av hello länkar i hello **Port** kolumn.</span><span class="sxs-lookup"><span data-stu-id="17b19-227">In hello **Executors** section, select one of hello links in hello **Port** column.</span></span> <span data-ttu-id="17b19-228">Visar information som loggas av hello-komponenten.</span><span class="sxs-lookup"><span data-stu-id="17b19-228">This displays information logged by hello component.</span></span> <span data-ttu-id="17b19-229">hello loggas information är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="17b19-229">hello logged information is similar toohello following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a><span data-ttu-id="17b19-230">Stoppa hello topologier</span><span class="sxs-lookup"><span data-stu-id="17b19-230">Stop hello topologies</span></span>

<span data-ttu-id="17b19-231">toostop hello topologier väljer varje topologi i hello **Storm-topologi Viewer**, klicka på **Kill**.</span><span class="sxs-lookup"><span data-stu-id="17b19-231">toostop hello topologies, select each topology in hello **Storm Topology Viewer**, then click **Kill**.</span></span>

![Skärmbild av Storm-topologi Viewer, med Kill-knappen markerad](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="17b19-233">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="17b19-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="17b19-234">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17b19-234">Next steps</span></span>

<span data-ttu-id="17b19-235">I det här dokumentet har du lärt dig hur toouse hello Java Händelsehubbar prata och bultar från en C#-topologi toowork med data i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="17b19-235">In this document, you have learned how toouse hello Java Event Hubs spout and bolt from a C# topology toowork with data in Azure Event Hubs.</span></span> <span data-ttu-id="17b19-236">toolearn mer om hur du skapar C#-topologier, finns följande hello:</span><span class="sxs-lookup"><span data-stu-id="17b19-236">toolearn more about creating C# topologies, see hello following:</span></span>

* [<span data-ttu-id="17b19-237">Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17b19-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="17b19-238">Programmeringsguide för SCP</span><span class="sxs-lookup"><span data-stu-id="17b19-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="17b19-239">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="17b19-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
