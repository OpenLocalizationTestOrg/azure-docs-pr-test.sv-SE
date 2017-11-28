---
title: "Bearbeta händelser från Event Hubs med Storm - Azure HDInsight | Microsoft Docs"
description: "Lär dig mer om att bearbeta data från Azure Event Hubs med en C# Storm-topologi som skapats i Visual Studio med HDInsight tools för Visual Studio."
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
ms.openlocfilehash: 4b6fd87b057d93175d3ef284238d77be3bdde61d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="a7ad1-103">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (C#)</span><span class="sxs-lookup"><span data-stu-id="a7ad1-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="a7ad1-104">Lär dig hur du arbetar med Azure Event Hubs från Apache Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-104">Learn how to work with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="a7ad1-105">Det här dokumentet använder en C# Storm-topologi för att läsa och skriva data från Evbent Hubs</span><span class="sxs-lookup"><span data-stu-id="a7ad1-105">This document uses a C# Storm topology to read and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="a7ad1-106">En Java-version av det här projektet finns [bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="a7ad1-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="a7ad1-107">SCP.NET</span></span>

<span data-ttu-id="a7ad1-108">Stegen i det här dokumentet använder SCP.NET NuGet-paketet som gör det enkelt att skapa C#-topologier och komponenter för användning med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-108">The steps in this document use SCP.NET, a NuGet package that makes it easy to create C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7ad1-109">När stegen i det här dokumentet är beroende av en Windows-utvecklingsmiljö med Visual Studio, skickas kompilerade projektet till ett Storm på HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-109">While the steps in this document rely on a Windows development environment with Visual Studio, the compiled project can be submitted to a Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="a7ad1-110">Linux-baserade kluster som skapas efter den 28 oktober 2016 stöder endast SCP.NET topologier.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="a7ad1-111">HDInsight 3.4 och större användning Mono för att köra C#-topologier.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-111">HDInsight 3.4 and greater use Mono to run C# topologies.</span></span> <span data-ttu-id="a7ad1-112">Exemplet i det här dokumentet fungerar med HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-112">The example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="a7ad1-113">Om du planerar att skapa egna .NET-lösningar för HDInsight, kontrollerar den [monoljud kompatibilitet](http://www.mono-project.com/docs/about-mono/compatibility/) dokument för potentiella inkompatibiliteter.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-113">If you plan on creating your own .NET solutions for HDInsight, check the [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="a7ad1-114">Klustret versionshantering</span><span class="sxs-lookup"><span data-stu-id="a7ad1-114">Cluster versioning</span></span>

<span data-ttu-id="a7ad1-115">Microsoft.SCP.Net.SDK NuGet-paketet som du använder för ditt projekt måste matcha den huvudsakliga versionen av Storm installerad på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-115">The Microsoft.SCP.Net.SDK NuGet package you use for your project must match the major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="a7ad1-116">HDInsight-version 3.5 och 3,6 använda Storm 1.x, så du måste använda SCP.NET version 1.0.x.x med dessa kluster.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7ad1-117">Exemplet i det här dokumentet förväntar sig ett HDInsight-3.5 eller 3,6 klustret.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-117">The example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="a7ad1-118">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-118">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a7ad1-119">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="a7ad1-120">C#-topologier måste också ha .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-to-work-with-event-hubs"></a><span data-ttu-id="a7ad1-121">Hur du arbetar med Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="a7ad1-121">How to work with Event Hubs</span></span>

<span data-ttu-id="a7ad1-122">Microsoft tillhandahåller en uppsättning Java-komponenter som kan användas för att kommunicera med Händelsehubbar från en Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-122">Microsoft provides a set of Java components that can be used to communicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="a7ad1-123">Du hittar den Java-arkivfil (JAR) som innehåller ett HDInsight-3,6 kompatibel version av dessa komponenter på [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-123">You can find the Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a7ad1-124">Medan komponenterna som skrivits i Java, kan du enkelt använda dem från en C#-topologi.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-124">While the components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="a7ad1-125">I det här exemplet används följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-125">The following components are used in this example:</span></span>

* <span data-ttu-id="a7ad1-126">__EventHubSpout__: läser data från Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="a7ad1-127">__EventHubBolt__: skriver data till Händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-127">__EventHubBolt__: Writes data to Event Hubs.</span></span>
* <span data-ttu-id="a7ad1-128">__EventHubSpoutConfig__: används för att konfigurera EventHubSpout.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-128">__EventHubSpoutConfig__: Used to configure EventHubSpout.</span></span>
* <span data-ttu-id="a7ad1-129">__EventHubBoltConfig__: används för att konfigurera EventHubBolt.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-129">__EventHubBoltConfig__: Used to configure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="a7ad1-130">Exempel på användning kanal</span><span class="sxs-lookup"><span data-stu-id="a7ad1-130">Example spout usage</span></span>

<span data-ttu-id="a7ad1-131">SCP.NET ger metoder för att lägga till en EventHubSpout i topologin.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-131">SCP.NET provides methods for adding an EventHubSpout to your topology.</span></span> <span data-ttu-id="a7ad1-132">Metoderna gör det enklare att lägga till en kanal än att använda allmänna metoder för att lägga till en Java-komponent.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-132">These methods make it easier to add a spout than using the generic methods for adding a Java component.</span></span> <span data-ttu-id="a7ad1-133">I följande exempel visar hur du skapar en kanal med hjälp av den __SetEventHubSpout__ och **EventHubSpoutConfig** metoder som tillhandahålls av SCP.NET:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-133">The following example demonstrates how to create a spout by using the __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

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

<span data-ttu-id="a7ad1-134">I föregående exempel skapas en ny kanal komponent med namnet __EventHubSpout__, samt konfigurerar den att kommunicera med en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-134">The previous example creates a new spout component named __EventHubSpout__, and configures it to communicate with an event hub.</span></span> <span data-ttu-id="a7ad1-135">Parallellitet tips för komponenten har angetts till antalet partitioner i hubben.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-135">The parallelism hint for the component is set to the number of partitions in the event hub.</span></span> <span data-ttu-id="a7ad1-136">Den här inställningen Storm att skapa en instans av komponenten för varje partition.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-136">This setting allows Storm to create an instance of the component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="a7ad1-137">Exempel på bult användning</span><span class="sxs-lookup"><span data-stu-id="a7ad1-137">Example bolt usage</span></span>

<span data-ttu-id="a7ad1-138">Använd den **JavaComponmentConstructor** metod för att skapa en instans av bulten.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-138">Use the **JavaComponmentConstructor** method to create an instance of the bolt.</span></span> <span data-ttu-id="a7ad1-139">I följande exempel visar hur du skapar och konfigurerar en ny instans av den **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-139">The following example demonstrates how to create and configure a new instance of the **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="a7ad1-140">Det här exemplet används en Clojure uttryck som skickas som en sträng, i stället för **JavaComponentConstructor** att skapa en **EventHubBoltConfig**, som exemplet kanal.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** to create an **EventHubBoltConfig**, as the spout example did.</span></span> <span data-ttu-id="a7ad1-141">Antingen metoden fungerar.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-141">Either method works.</span></span> <span data-ttu-id="a7ad1-142">Använd den metod som känns bäst.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-142">Use the method that feels best to you.</span></span>

## <a name="download-the-completed-project"></a><span data-ttu-id="a7ad1-143">Hämta projektet slutförd</span><span class="sxs-lookup"><span data-stu-id="a7ad1-143">Download the completed project</span></span>

<span data-ttu-id="a7ad1-144">Du kan hämta en fullständig version av projektet har skapats i den här kursen från [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-144">You can download a complete version of the project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="a7ad1-145">Du behöver dock fortfarande ange konfigurationsinställningarna genom att följa stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-145">However, you still need to provide configuration settings by following the steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="a7ad1-146">Krav</span><span class="sxs-lookup"><span data-stu-id="a7ad1-146">Prerequisites</span></span>

* <span data-ttu-id="a7ad1-147">En [Apache Storm på HDInsight-kluster av version 3.5 eller 3,6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="a7ad1-148">Exemplet i det här dokumentet kräver Storm på HDInsight version 3.5 eller 3,6.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-148">The example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="a7ad1-149">Detta fungerar inte med äldre versioner av HDInsight, på grund av att bryta ändringar för klassen namn.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-149">This does not work with older versions of HDInsight, due to breaking class name changes.</span></span> <span data-ttu-id="a7ad1-150">En version av det här exemplet fungerar med äldre kluster, se [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="a7ad1-151">En [Azure händelsehubb](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="a7ad1-152">Den [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-152">The [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="a7ad1-153">Den [HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-153">The [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="a7ad1-154">Java JDK 1.8 eller senare på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="a7ad1-155">JDK hämtas från [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="a7ad1-156">Den **JAVA_HOME** miljövariabeln måste peka på den katalog som innehåller Java.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-156">The **JAVA_HOME** environment variable must point to the directory that contains Java.</span></span>
  * <span data-ttu-id="a7ad1-157">Den **%JAVA_HOME%/bin** katalogen måste vara i sökvägen.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-157">The **%JAVA_HOME%/bin** directory must be in the path.</span></span>

## <a name="download-the-event-hubs-components"></a><span data-ttu-id="a7ad1-158">Hämta Händelsehubbar-komponenter</span><span class="sxs-lookup"><span data-stu-id="a7ad1-158">Download the Event Hubs components</span></span>

<span data-ttu-id="a7ad1-159">Hämta Händelsehubbar kanal och bultar komponenten från [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-159">Download the Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="a7ad1-160">Skapa en katalog med namnet `eventhubspout`, och spara filen i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-160">Create a directory named `eventhubspout`, and save the file into the directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="a7ad1-161">Konfigurera Händelsehubbar</span><span class="sxs-lookup"><span data-stu-id="a7ad1-161">Configure Event Hubs</span></span>

<span data-ttu-id="a7ad1-162">Händelsehubbar är datakällan för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-162">Event Hubs is the data source for this example.</span></span> <span data-ttu-id="a7ad1-163">Använd informationen i avsnittet ”Skapa en händelsehubb” i [Kom igång med Händelsehubbar](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-163">Use the information in the "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="a7ad1-164">När händelsehubben har skapats kan visa den **EventHub** bladet i Azure portal och välj **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-164">After the event hub has been created, view the **EventHub** blade in the Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="a7ad1-165">Välj **+ Lägg till** att lägga till följande principer:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-165">Select **+ Add** to add the following policies:</span></span>

   | <span data-ttu-id="a7ad1-166">Namn</span><span class="sxs-lookup"><span data-stu-id="a7ad1-166">Name</span></span> | <span data-ttu-id="a7ad1-167">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="a7ad1-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="a7ad1-168">Skrivare</span><span class="sxs-lookup"><span data-stu-id="a7ad1-168">writer</span></span> |<span data-ttu-id="a7ad1-169">Skicka</span><span class="sxs-lookup"><span data-stu-id="a7ad1-169">Send</span></span> |
   | <span data-ttu-id="a7ad1-170">läsare</span><span class="sxs-lookup"><span data-stu-id="a7ad1-170">reader</span></span> |<span data-ttu-id="a7ad1-171">Lyssna</span><span class="sxs-lookup"><span data-stu-id="a7ad1-171">Listen</span></span> |

    ![Skärmbild av dela access principer fönster](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="a7ad1-173">Välj den **reader** och **writer** principer.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-173">Select the **reader** and **writer** policies.</span></span> <span data-ttu-id="a7ad1-174">Kopiera och spara primärnyckelvärdet för båda principerna enligt dessa värden används senare.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-174">Copy and save the primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-the-eventhubwriter"></a><span data-ttu-id="a7ad1-175">Konfigurera EventHubWriter</span><span class="sxs-lookup"><span data-stu-id="a7ad1-175">Configure the EventHubWriter</span></span>

1. <span data-ttu-id="a7ad1-176">Om du inte redan har installerat den senaste versionen av HDInsight tools för Visual Studio finns [komma igång med HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-176">If you have not already installed the latest version of the HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="a7ad1-177">Hämta lösningen från [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="a7ad1-177">Download the solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="a7ad1-178">I den **EventHubWriter** projektet öppnar den **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-178">In the **EventHubWriter** project, open the **App.config** file.</span></span> <span data-ttu-id="a7ad1-179">Använd informationen från händelsehubben som du tidigare har konfigurerat för att fylla i värdet för följande nycklar:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-179">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="a7ad1-180">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a7ad1-180">Key</span></span> | <span data-ttu-id="a7ad1-181">Värde</span><span class="sxs-lookup"><span data-stu-id="a7ad1-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="a7ad1-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="a7ad1-182">EventHubPolicyName</span></span> |<span data-ttu-id="a7ad1-183">skrivare (om du har använt ett annat namn för principen med *skicka* behörighet, Använd i stället.)</span><span class="sxs-lookup"><span data-stu-id="a7ad1-183">writer (If you used a different name for the policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="a7ad1-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="a7ad1-184">EventHubPolicyKey</span></span> |<span data-ttu-id="a7ad1-185">Nyckeln för principen writer.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-185">The key for the writer policy.</span></span> |
   | <span data-ttu-id="a7ad1-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="a7ad1-186">EventHubNamespace</span></span> |<span data-ttu-id="a7ad1-187">Det namnområde som innehåller din event hub.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-187">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="a7ad1-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="a7ad1-188">EventHubName</span></span> |<span data-ttu-id="a7ad1-189">Din händelsehubbens namn.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-189">Your event hub name.</span></span> |
   | <span data-ttu-id="a7ad1-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="a7ad1-190">EventHubPartitionCount</span></span> |<span data-ttu-id="a7ad1-191">Antalet partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-191">The number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="a7ad1-192">Spara och Stäng den **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-192">Save and close the **App.config** file.</span></span>

## <a name="configure-the-eventhubreader"></a><span data-ttu-id="a7ad1-193">Konfigurera EventHubReader</span><span class="sxs-lookup"><span data-stu-id="a7ad1-193">Configure the EventHubReader</span></span>

1. <span data-ttu-id="a7ad1-194">Öppna den **EventHubReader** projekt.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-194">Open the **EventHubReader** project.</span></span>

2. <span data-ttu-id="a7ad1-195">Öppna den **App.config** för den **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-195">Open the **App.config** file for the **EventHubReader**.</span></span> <span data-ttu-id="a7ad1-196">Använd informationen från händelsehubben som du tidigare har konfigurerat för att fylla i värdet för följande nycklar:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-196">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="a7ad1-197">Nyckel</span><span class="sxs-lookup"><span data-stu-id="a7ad1-197">Key</span></span> | <span data-ttu-id="a7ad1-198">Värde</span><span class="sxs-lookup"><span data-stu-id="a7ad1-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="a7ad1-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="a7ad1-199">EventHubPolicyName</span></span> |<span data-ttu-id="a7ad1-200">läsare (om du har använt ett annat namn för principen med *lyssna* behörighet, Använd i stället.)</span><span class="sxs-lookup"><span data-stu-id="a7ad1-200">reader (If you used a different name for the policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="a7ad1-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="a7ad1-201">EventHubPolicyKey</span></span> |<span data-ttu-id="a7ad1-202">Nyckeln för principen för läsare.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-202">The key for the reader policy.</span></span> |
   | <span data-ttu-id="a7ad1-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="a7ad1-203">EventHubNamespace</span></span> |<span data-ttu-id="a7ad1-204">Det namnområde som innehåller din event hub.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-204">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="a7ad1-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="a7ad1-205">EventHubName</span></span> |<span data-ttu-id="a7ad1-206">Din händelsehubbens namn.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-206">Your event hub name.</span></span> |
   | <span data-ttu-id="a7ad1-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="a7ad1-207">EventHubPartitionCount</span></span> |<span data-ttu-id="a7ad1-208">Antalet partitioner i din event hub.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-208">The number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="a7ad1-209">Spara och Stäng den **App.config** fil.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-209">Save and close the **App.config** file.</span></span>

## <a name="deploy-the-topologies"></a><span data-ttu-id="a7ad1-210">Distribuera topologierna</span><span class="sxs-lookup"><span data-stu-id="a7ad1-210">Deploy the topologies</span></span>

1. <span data-ttu-id="a7ad1-211">Från **Solution Explorer**, högerklicka på den **EventHubReader** projektet och välj **skicka till Storm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-211">From **Solution Explorer**, right-click the **EventHubReader** project, and select **Submit to Storm on HDInsight**.</span></span>

    ![Skärmbild av Solution Explorer med Skicka till Storm på HDInsight markerat](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="a7ad1-213">På den **skicka topologi** väljer din **Storm-kluster**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-213">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="a7ad1-214">Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj den katalog som innehåller JAR-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file that you downloaded earlier.</span></span> <span data-ttu-id="a7ad1-215">Klicka slutligen på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-215">Finally, click **Submit**.</span></span>

    ![Dialogrutan Skicka topologi skärmbild](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="a7ad1-217">När topologin har skickats av **Storm-topologier Viewer** visas.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-217">When the topology has been submitted, the **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="a7ad1-218">Om du vill visa information om topologin, Välj den **EventHubReader** topologi i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-218">To view information about the topology, select the **EventHubReader** topology in the left pane.</span></span>

    ![Skärmbild av Storm-topologier Viewer](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="a7ad1-220">Från **Solution Explorer**, högerklicka på den **EventHubWriter** projektet och välj **skicka till Storm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-220">From **Solution Explorer**, right-click the **EventHubWriter** project, and select **Submit to Storm on HDInsight**.</span></span>

5. <span data-ttu-id="a7ad1-221">På den **skicka topologi** väljer din **Storm-kluster**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-221">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="a7ad1-222">Expandera **ytterligare konfigurationer**väljer **Java sökvägar**väljer **...** , och välj den katalog som innehåller JAR-filen som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file you downloaded earlier.</span></span> <span data-ttu-id="a7ad1-223">Klicka slutligen på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="a7ad1-224">När topologin har skickats, uppdatera listan med topologi i den **Storm-topologier Viewer** att kontrollera att båda topologierna körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-224">When the topology has been submitted, refresh the topology list in the **Storm Topologies Viewer** to verify that both topologies are running on the cluster.</span></span>

7. <span data-ttu-id="a7ad1-225">I **Storm-topologier Viewer**, Välj den **EventHubReader** topologi.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-225">In **Storm Topologies Viewer**, select the **EventHubReader** topology.</span></span>

8. <span data-ttu-id="a7ad1-226">Dubbelklicka för att öppna komponenten i sammanfattningen bulten den **LogBolt** komponenten i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-226">To open the component summary for the bolt, double-click the **LogBolt** component in the diagram.</span></span>

9. <span data-ttu-id="a7ad1-227">I den **Executors** väljer du en av länkarna i den **Port** kolumn.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-227">In the **Executors** section, select one of the links in the **Port** column.</span></span> <span data-ttu-id="a7ad1-228">Visar information som loggas av komponenten.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-228">This displays information logged by the component.</span></span> <span data-ttu-id="a7ad1-229">Loggade informationen liknar följande:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-229">The logged information is similar to the following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a><span data-ttu-id="a7ad1-230">Stoppa topologierna</span><span class="sxs-lookup"><span data-stu-id="a7ad1-230">Stop the topologies</span></span>

<span data-ttu-id="a7ad1-231">Stoppa topologierna för att välja varje topologi i den **Storm-topologi Viewer**, klicka på **Kill**.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-231">To stop the topologies, select each topology in the **Storm Topology Viewer**, then click **Kill**.</span></span>

![Skärmbild av Storm-topologi Viewer, med Kill-knappen markerad](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="a7ad1-233">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="a7ad1-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="a7ad1-234">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a7ad1-234">Next steps</span></span>

<span data-ttu-id="a7ad1-235">I det här dokumentet har du lärt dig hur du använder Java Händelsehubbar kanal och bultar från en C#-topologi att arbeta med data i Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="a7ad1-235">In this document, you have learned how to use the Java Event Hubs spout and bolt from a C# topology to work with data in Azure Event Hubs.</span></span> <span data-ttu-id="a7ad1-236">Mer information om hur du skapar C#-topologier finns följande:</span><span class="sxs-lookup"><span data-stu-id="a7ad1-236">To learn more about creating C# topologies, see the following:</span></span>

* [<span data-ttu-id="a7ad1-237">Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a7ad1-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="a7ad1-238">Programmeringsguide för SCP</span><span class="sxs-lookup"><span data-stu-id="a7ad1-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="a7ad1-239">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="a7ad1-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
