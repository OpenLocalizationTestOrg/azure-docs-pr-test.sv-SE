---
title: aaaAnalyze sensordata med Apache Storm och HBase | Microsoft Docs
description: "Lär dig hur tooconnect tooApache Storm med ett virtuellt nätverk. Använda Storm med HBase tooprocess sensordata från en händelsehubb och visualisera med D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a><span data-ttu-id="a0f9f-104">Analysera sensordata med Apache Storm, Event Hub och HBase i HDInsight (Hadoop)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-104">Analyze sensor data with Apache Storm, Event Hub, and HBase in HDInsight (Hadoop)</span></span>

<span data-ttu-id="a0f9f-105">Lär dig hur toouse Apache Storm på HDInsight tooprocess sensordata från Azure Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-105">Learn how toouse Apache Storm on HDInsight tooprocess sensor data from Azure Event Hub.</span></span> <span data-ttu-id="a0f9f-106">hello data lagras i Apache HBase i HDInsight och visualiseras med D3.js.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-106">hello data is then stored into Apache HBase on HDInsight, and visualized using D3.js.</span></span>

<span data-ttu-id="a0f9f-107">hello Azure Resource Manager-mall som används i det här dokumentet visar hur toocreate flera Azure-resurser i en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-107">hello Azure Resource Manager template used in this document demonstrates how toocreate multiple Azure resources in a resource group.</span></span> <span data-ttu-id="a0f9f-108">hello mallen skapar ett Azure Virtual Network, två HDInsight-kluster (Storm och HBase) och en Webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-108">hello template creates an Azure Virtual Network, two HDInsight clusters (Storm and HBase) and an Azure Web App.</span></span> <span data-ttu-id="a0f9f-109">En node.js-implementering av en webbinstrumentpanel i realtid är automatiskt distribuerade toohello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-109">A node.js implementation of a real-time web dashboard is automatically deployed toohello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="a0f9f-110">hello informationen i det här dokumentet och exemplet i det här dokumentet kräver HDInsight version 3,6.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-110">hello information in this document and example in this document require HDInsight version 3.6.</span></span>
>
> <span data-ttu-id="a0f9f-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a0f9f-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0f9f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="a0f9f-113">Prerequisites</span></span>

* <span data-ttu-id="a0f9f-114">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-114">An Azure subscription.</span></span>
* <span data-ttu-id="a0f9f-115">[Node.js](http://nodejs.org/): använda toopreview hello web instrumentpanelen lokalt på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-115">[Node.js](http://nodejs.org/): Used toopreview hello web dashboard locally on your development environment.</span></span>
* <span data-ttu-id="a0f9f-116">[Java och hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): används toodevelop hello Storm-topologi.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-116">[Java and hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Used toodevelop hello Storm topology.</span></span>
* <span data-ttu-id="a0f9f-117">[Maven](http://maven.apache.org/what-is-maven.html): använda toobuild och kompilera hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-117">[Maven](http://maven.apache.org/what-is-maven.html): Used toobuild and compile hello project.</span></span>
* <span data-ttu-id="a0f9f-118">[Git](http://git-scm.com/): använda toodownload hello projektet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-118">[Git](http://git-scm.com/): Used toodownload hello project from GitHub.</span></span>
* <span data-ttu-id="a0f9f-119">En **SSH** klienten: används tooconnect toohello Linux-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-119">An **SSH** client: Used tooconnect toohello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="a0f9f-120">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="a0f9f-121">Du behöver inte ett befintligt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-121">You do not need an existing HDInsight cluster.</span></span> <span data-ttu-id="a0f9f-122">hello stegen i det här dokumentet skapa hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-122">hello steps in this document create hello following resources:</span></span>
> 
> * <span data-ttu-id="a0f9f-123">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="a0f9f-123">An Azure Virtual Network</span></span>
> * <span data-ttu-id="a0f9f-124">Ett Storm på HDInsight-kluster (Linux-baserade två arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-124">A Storm on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="a0f9f-125">En HBase på HDInsight-kluster (Linux-baserade två arbetarnoder)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-125">An HBase on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="a0f9f-126">En Azure-Webbapp som är värd för hello web instrumentpanelen</span><span class="sxs-lookup"><span data-stu-id="a0f9f-126">An Azure Web App that hosts hello web dashboard</span></span>

## <a name="architecture"></a><span data-ttu-id="a0f9f-127">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="a0f9f-127">Architecture</span></span>

![Arkitekturdiagram](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

<span data-ttu-id="a0f9f-129">Det här exemplet består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-129">This example consists of hello following components:</span></span>

* <span data-ttu-id="a0f9f-130">**Händelsehubbar i Azure**: innehåller data som samlas in från sensorer.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-130">**Azure Event Hubs**: Contains data that is collected from sensors.</span></span>
* <span data-ttu-id="a0f9f-131">**Storm på HDInsight**: ger realtidsbearbetning av data från Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-131">**Storm on HDInsight**: Provides real-time processing of data from Event Hub.</span></span>
* <span data-ttu-id="a0f9f-132">**HBase på HDInsight**: ger en beständig NoSQL-databas för data när den har bearbetats av Storm.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-132">**HBase on HDInsight**: Provides a persistent NoSQL data store for data after it has been processed by Storm.</span></span>
* <span data-ttu-id="a0f9f-133">**Azure Virtual Network service**: möjliggör säker kommunikation mellan hello Storm på HDInsight och HBase på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-133">**Azure Virtual Network service**: Enables secure communications between hello Storm on HDInsight and HBase on HDInsight clusters.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="a0f9f-134">Ett virtuellt nätverk krävs när du använder hello Java HBase klient-API.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-134">A virtual network is required when using hello Java HBase client API.</span></span> <span data-ttu-id="a0f9f-135">Exponeras inte via hello offentliga gateway för HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-135">It is not exposed over hello public gateway for HBase clusters.</span></span> <span data-ttu-id="a0f9f-136">Installera HBase och Storm-kluster i samma virtuella nätverk gör hello hello Storm-kluster (eller andra system i hello virtuella nätverk) toodirectly åtkomst till HBase med klient-API.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-136">Installing HBase and Storm clusters into hello same virtual network allows hello Storm cluster (or any other system on hello virtual network) toodirectly access HBase using client API.</span></span>

* <span data-ttu-id="a0f9f-137">**Instrumentpanelen webbplats**: ett exempel instrumentpanel diagram data i realtid.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-137">**Dashboard website**: An example dashboard that charts data in real time.</span></span>
  
  * <span data-ttu-id="a0f9f-138">hello webbplats är implementerat i Node.js.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-138">hello website is implemented in Node.js.</span></span>
  * <span data-ttu-id="a0f9f-139">[Socket.IO](http://socket.io/) används för kommunikation mellan hello Storm-topologi och hello webbplats i realtid.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-139">[Socket.io](http://socket.io/) is used for real-time communication between hello Storm topology and hello website.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="a0f9f-140">Med Socket.io för kommunikation är en implementering detaljer.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-140">Using Socket.io for communication is an implementation detail.</span></span> <span data-ttu-id="a0f9f-141">Du kan använda alla kommunikationssystem som oformaterade WebSockets eller SignalR.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-141">You can use any communications framework, such as raw WebSockets or SignalR.</span></span>

  * <span data-ttu-id="a0f9f-142">[D3.js](http://d3js.org/) används toograph hello data som skickas toohello webbplats.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-142">[D3.js](http://d3js.org/) is used toograph hello data that is sent toohello website.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0f9f-143">Två kluster krävs, eftersom det inte finns några stöds metoden toocreate ett HDInsight-kluster för både Storm och HBase.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-143">Two clusters are required, as there is no supported method toocreate one HDInsight cluster for both Storm and HBase.</span></span>

<span data-ttu-id="a0f9f-144">hello topologi läser data från Event Hub med hjälp av hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) klass och skriver data till HBase med hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) klass.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-144">hello topology reads data from Event Hub by using hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) class, and writes data into HBase using hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) class.</span></span> <span data-ttu-id="a0f9f-145">Kommunikation med webbplatser för hello åstadkoms med hjälp av [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-145">Communication with hello website is accomplished by using [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span></span>

<span data-ttu-id="a0f9f-146">hello följande diagram beskriver hello layout hello topologi:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-146">hello following diagram explains hello layout of hello topology:</span></span>

![diagram över topologi](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> <span data-ttu-id="a0f9f-148">Det här diagrammet är en förenklad vy av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-148">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="a0f9f-149">En instans av varje komponent skapas för varje partition i din Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-149">An instance of each component is created for each partition in your Event Hub.</span></span> <span data-ttu-id="a0f9f-150">Dessa instanser är fördelade på hello noder i klustret hello och data dirigeras mellan dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-150">These instances are distributed across hello nodes in hello cluster, and data is routed between them as follows:</span></span>
> 
> * <span data-ttu-id="a0f9f-151">Data från hello kanal toohello tolken är Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-151">Data from hello spout toohello parser is load balanced.</span></span>
> * <span data-ttu-id="a0f9f-152">Data från hello parsern toohello instrumentpanelen och HBase är grupperad efter enhets-ID, så att meddelanden från hello samma enhet alltid flöda toohello samma komponent.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-152">Data from hello parser toohello Dashboard and HBase is grouped by Device ID, so that messages from hello same device always flow toohello same component.</span></span>

### <a name="topology-components"></a><span data-ttu-id="a0f9f-153">Topologi komponenter</span><span class="sxs-lookup"><span data-stu-id="a0f9f-153">Topology components</span></span>

* <span data-ttu-id="a0f9f-154">**Event Hub-kanalen**: hello kanal har angetts som en del av Apache Storm-version 0.10.0 och högre.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-154">**Event Hub Spout**: hello spout is provided as part of Apache Storm version 0.10.0 and higher.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="a0f9f-155">hello Event Hub-kanal som används i det här exemplet kräver ett Storm på HDInsight-kluster av version 3.5 eller 3,6.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-155">hello Event Hub spout used in this example requires a Storm on HDInsight cluster version 3.5 or 3.6.</span></span>

* <span data-ttu-id="a0f9f-156">**ParserBolt.java**: hello data som genereras av hello kanal är raw JSON och ibland fler än en händelse genereras i taget.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-156">**ParserBolt.java**: hello data that is emitted by hello spout is raw JSON, and occasionally more than one event is emitted at a time.</span></span> <span data-ttu-id="a0f9f-157">Bulten läser hello data som sänds av hello prata och Parsar hello JSON-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-157">This bolt reads hello data emitted by hello spout and parses hello JSON message.</span></span> <span data-ttu-id="a0f9f-158">hello bult skickar sedan hello data som en tuppel som innehåller flera fält.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-158">hello bolt then emits hello data as a tuple that contains multiple fields.</span></span>
* <span data-ttu-id="a0f9f-159">**DashboardBolt.java**: den här komponenten visar hur toouse hello Socket.io klientbibliotek för Java toosend data i realtid toohello web instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-159">**DashboardBolt.java**: This component demonstrates how toouse hello Socket.io client library for Java toosend data in real time toohello web dashboard.</span></span>
* <span data-ttu-id="a0f9f-160">**Ingen hbase.yaml**: hello topologi definition som används vid körning i lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-160">**no-hbase.yaml**: hello topology definition used when running in local mode.</span></span> <span data-ttu-id="a0f9f-161">Den använder inte HBase-komponenter.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-161">It does not use HBase components.</span></span>
* <span data-ttu-id="a0f9f-162">**med hbase.yaml**: hello topologi definition används när du kör hello-topologi på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-162">**with-hbase.yaml**: hello topology definition used when running hello topology on hello cluster.</span></span> <span data-ttu-id="a0f9f-163">Den använder HBase-komponenter.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-163">It does use HBase components.</span></span>
* <span data-ttu-id="a0f9f-164">**dev.Properties**: hello konfigurationsinformation för hello Event Hub kanal, HBase bult och instrumentpanelskomponenter.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-164">**dev.properties**: hello configuration information for hello Event Hub spout, HBase bolt, and dashboard components.</span></span>

## <a name="prepare-your-environment"></a><span data-ttu-id="a0f9f-165">Förbered din miljö</span><span class="sxs-lookup"><span data-stu-id="a0f9f-165">Prepare your environment</span></span>

<span data-ttu-id="a0f9f-166">Innan du använder det här exemplet måste du skapa en Azure-Händelsehubb, vilka hello Storm-topologi läser från.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-166">Before you use this example, you must create an Azure Event Hub, which hello Storm topology reads from.</span></span>

### <a name="configure-event-hub"></a><span data-ttu-id="a0f9f-167">Konfigurera Event Hub</span><span class="sxs-lookup"><span data-stu-id="a0f9f-167">Configure Event Hub</span></span>

<span data-ttu-id="a0f9f-168">Event Hub är hello datakälla för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-168">Event Hub is hello data source for this example.</span></span> <span data-ttu-id="a0f9f-169">Använd följande steg toocreate en Händelsehubb hello.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-169">Use hello following steps toocreate an Event Hub.</span></span>

1. <span data-ttu-id="a0f9f-170">Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny** -> **Sakernas Internet** -> **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-170">From hello [Azure portal](https://portal.azure.com), select **+ New** -> **Internet of Things** -> **Event Hubs**.</span></span>
2. <span data-ttu-id="a0f9f-171">I hello **skapa Namespace** avsnittet, utföra hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-171">In hello **Create Namespace** section, perform hello following tasks:</span></span>
   
   1. <span data-ttu-id="a0f9f-172">Ange en **namn** för hello namnområde.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-172">Enter a **Name** for hello namespace.</span></span>
   2. <span data-ttu-id="a0f9f-173">Välj en prisnivå.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-173">Select a pricing tier.</span></span> <span data-ttu-id="a0f9f-174">**Grundläggande** är tillräcklig för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-174">**Basic** is sufficient for this example.</span></span>
   3. <span data-ttu-id="a0f9f-175">Välj hello Azure **prenumeration** toouse.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-175">Select hello Azure **Subscription** toouse.</span></span>
   4. <span data-ttu-id="a0f9f-176">Välj en befintlig resursgrupp eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-176">Either select an existing resource group or create a new one.</span></span>
   5. <span data-ttu-id="a0f9f-177">Välj hello **plats** för hello Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-177">Select hello **Location** for hello Event Hub.</span></span>
   6. <span data-ttu-id="a0f9f-178">Välj **PIN-kod toodashboard**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-178">Select **Pin toodashboard**, and then click **Create**.</span></span>

3. <span data-ttu-id="a0f9f-179">När hello-processen är klar visas hello Händelsehubbar information för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-179">When hello creation process completes, hello Event Hubs information for your namespace is displayed.</span></span> <span data-ttu-id="a0f9f-180">Här kan du välja **+ Lägg till Händelsehubben**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-180">From here, select **+ Add Event Hub**.</span></span> <span data-ttu-id="a0f9f-181">I hello **skapa Event Hub** ange ett namn på **sensordata**, och välj sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-181">In hello **Create Event Hub** section, enter a name of **sensordata**, and then select **Create**.</span></span> <span data-ttu-id="a0f9f-182">Lämna hello andra fält till hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-182">Leave hello other fields at hello default values.</span></span>
4. <span data-ttu-id="a0f9f-183">Hej Händelsehubbar Se för namnområdet, Välj **Händelsehubbar**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-183">From hello Event Hubs view for your namespace, select **Event Hubs**.</span></span> <span data-ttu-id="a0f9f-184">Välj hello **sensordata** transaktionen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-184">Select hello **sensordata** entry.</span></span>
5. <span data-ttu-id="a0f9f-185">Hej sensordata Händelsehubb, Välj **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-185">From hello sensordata Event Hub, select **Shared access policies**.</span></span> <span data-ttu-id="a0f9f-186">Använd hello **+ Lägg till** länk tooadd hello följande principer:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-186">Use hello **+ Add** link tooadd hello following policies:</span></span>

    | <span data-ttu-id="a0f9f-187">Principnamn</span><span class="sxs-lookup"><span data-stu-id="a0f9f-187">Policy name</span></span> | <span data-ttu-id="a0f9f-188">Anspråk</span><span class="sxs-lookup"><span data-stu-id="a0f9f-188">Claims</span></span> |
    | ----- | ----- |
    | <span data-ttu-id="a0f9f-189">enheter</span><span class="sxs-lookup"><span data-stu-id="a0f9f-189">devices</span></span> | <span data-ttu-id="a0f9f-190">Skicka</span><span class="sxs-lookup"><span data-stu-id="a0f9f-190">Send</span></span> |
    | <span data-ttu-id="a0f9f-191">Storm</span><span class="sxs-lookup"><span data-stu-id="a0f9f-191">storm</span></span> | <span data-ttu-id="a0f9f-192">Lyssna</span><span class="sxs-lookup"><span data-stu-id="a0f9f-192">Listen</span></span> |

1. <span data-ttu-id="a0f9f-193">Markera båda principer och anteckna hello **PRIMÄRNYCKEL** värde.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-193">Select both policies and make a note of hello **PRIMARY KEY** value.</span></span> <span data-ttu-id="a0f9f-194">Du behöver hello värde för båda principerna i kommande steg.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-194">You need hello value for both policies in future steps.</span></span>

## <a name="download-and-configure-hello-project"></a><span data-ttu-id="a0f9f-195">Hämta och konfigurera hello-projekt</span><span class="sxs-lookup"><span data-stu-id="a0f9f-195">Download and configure hello project</span></span>

<span data-ttu-id="a0f9f-196">Använd hello följande toodownload hello projektet från GitHub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-196">Use hello following toodownload hello project from GitHub.</span></span>

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

<span data-ttu-id="a0f9f-197">När hello-kommandot har slutförts har hello följande katalogstruktur:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-197">After hello command completes, you have hello following directory structure:</span></span>

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> <span data-ttu-id="a0f9f-198">Det går inte att det här dokumentet i toofull information om hello-kod som ingår i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-198">This document does not go in toofull details of hello code included in this example.</span></span> <span data-ttu-id="a0f9f-199">Hello koden är helt kommenterad.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-199">However, hello code is fully commented.</span></span>

<span data-ttu-id="a0f9f-200">tooconfigure hello projektet tooread från Event Hub, öppna hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` och Lägg till din Event Hub information toohello följande rader:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-200">tooconfigure hello project tooread from Event Hub, open hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` file and add your Event Hub information toohello following lines:</span></span>

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a><span data-ttu-id="a0f9f-201">Kompilera och testa lokalt</span><span class="sxs-lookup"><span data-stu-id="a0f9f-201">Compile and test locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0f9f-202">Med hello topologi lokalt kräver en fungerande Storm-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-202">Using hello topology locally requires a working Storm development environment.</span></span> <span data-ttu-id="a0f9f-203">Mer information finns i [ställa in en Storm-utvecklingsmiljö](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) på Apache.org.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-203">For more information, see [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) at Apache.org.</span></span>

> [!WARNING]
> <span data-ttu-id="a0f9f-204">Om du använder en Windows-utvecklingsmiljö, kan du få en `java.io.IOException` körs när hello topologi lokalt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-204">If you are using a Windows development environment, you may receive a `java.io.IOException` when running hello topology locally.</span></span> <span data-ttu-id="a0f9f-205">I så fall, flytta toorunning hello-topologi på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-205">If so, move on toorunning hello topology on HDInsight.</span></span>

<span data-ttu-id="a0f9f-206">Innan du testar, måste du starta hello instrumentpanelen tooview hello utdata från hello topologi och generera data toostore i Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-206">Before testing, you must start hello dashboard tooview hello output of hello topology and generate data toostore in Event Hub.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a0f9f-207">Hej HBase-komponenten i den här topologin är inte aktivt när du testar lokalt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-207">hello HBase component of this topology is not active when testing locally.</span></span> <span data-ttu-id="a0f9f-208">hello Java API för hello HBase-kluster kan inte nås från utanför hello Azure Virtual Network som innehåller hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-208">hello Java API for hello HBase cluster cannot be accessed from outside hello Azure Virtual Network that contains hello clusters.</span></span>

### <a name="start-hello-web-application"></a><span data-ttu-id="a0f9f-209">Starta hello-webbprogram</span><span class="sxs-lookup"><span data-stu-id="a0f9f-209">Start hello web application</span></span>

1. <span data-ttu-id="a0f9f-210">Öppna en kommandotolk och ändra sökvägen för`hdinsight-eventhub-example/dashboard`.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-210">Open a command prompt and change directories too`hdinsight-eventhub-example/dashboard`.</span></span> <span data-ttu-id="a0f9f-211">Använd hello följande kommando tooinstall hello beroenden som krävs av hello-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-211">Use hello following command tooinstall hello dependencies needed by hello web application:</span></span>
   
    ```bash
    npm install
    ```

2. <span data-ttu-id="a0f9f-212">Använd hello följande kommando toostart hello-webbprogram:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-212">Use hello following command toostart hello web application:</span></span>
   
    ```bash
    node server.js
    ```
   
    <span data-ttu-id="a0f9f-213">Du ser ett meddelande liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-213">You see a message similar toohello following text:</span></span>
   
        Server listening at port 3000

3. <span data-ttu-id="a0f9f-214">Öppna en webbläsare och ange `http://localhost:3000/` som hello-adress.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-214">Open a web browser and enter `http://localhost:3000/` as hello address.</span></span> <span data-ttu-id="a0f9f-215">En sida liknande toohello följande bild visas:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-215">A page similar toohello following image is displayed:</span></span>
   
    ![instrumentpanel för webbprogram](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    <span data-ttu-id="a0f9f-217">Lämna den här Kommandotolken öppen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-217">Leave this command prompt open.</span></span> <span data-ttu-id="a0f9f-218">Efter testning, använda Ctrl-C toostop hello-webbserver.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-218">After testing, use Ctrl-C toostop hello web server.</span></span>

### <a name="generate-data"></a><span data-ttu-id="a0f9f-219">Generera data</span><span class="sxs-lookup"><span data-stu-id="a0f9f-219">Generate data</span></span>

> [!NOTE]
> <span data-ttu-id="a0f9f-220">hello använder stegen i det här avsnittet Node.js så att de kan användas på alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-220">hello steps in this section use Node.js so that they can be used on any platform.</span></span> <span data-ttu-id="a0f9f-221">Andra språk-exempel finns hello `SendEvents` directory.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-221">For other language examples, see hello `SendEvents` directory.</span></span>

1. <span data-ttu-id="a0f9f-222">Öppna en ny fråga eller shell terminal och ändra kataloger för`hdinsight-eventhub-example/SendEvents/nodejs`.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-222">Open a new prompt, shell, or terminal, and change directories too`hdinsight-eventhub-example/SendEvents/nodejs`.</span></span> <span data-ttu-id="a0f9f-223">tooinstall hello beroenden som krävs av programmet hello, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-223">tooinstall hello dependencies needed by hello application, use hello following command:</span></span>

    ```bash
    npm install
    ```

2. <span data-ttu-id="a0f9f-224">Öppna hello `app.js` i en textredigerare och Lägg till hello Event Hub-information som du tidigare hämtade:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-224">Open hello `app.js` file in a text editor and add hello Event Hub information you obtained earlier:</span></span>
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="a0f9f-225">Det här exemplet förutsätter att du har använt `sensordata` som hello namnet på din Event Hub.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-225">This example assumes that you have used `sensordata` as hello name of your Event Hub.</span></span> <span data-ttu-id="a0f9f-226">Och att `devices` som hello namnet på hello-princip som har en `Send` anspråk.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-226">And that `devices` as hello name of hello policy that has a `Send` claim.</span></span>

3. <span data-ttu-id="a0f9f-227">Använd följande kommando tooinsert nya poster i Event Hub hello:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-227">Use hello following command tooinsert new entries in Event Hub:</span></span>
   
    ```bash
    node app.js
    ```
   
    <span data-ttu-id="a0f9f-228">Du ser flera rader på utdata som innehåller hello data skickas tooEvent hubb:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-228">You see several lines of output that contain hello data sent tooEvent Hub:</span></span>
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-hello-topology"></a><span data-ttu-id="a0f9f-229">Skapa och starta hello-topologi</span><span class="sxs-lookup"><span data-stu-id="a0f9f-229">Build and start hello topology</span></span>

1. <span data-ttu-id="a0f9f-230">Öppna en ny kommandotolk och ändra sökvägen för`hdinsight-eventhub-example/TemperatureMonitor`.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-230">Open a new command prompt and change directories too`hdinsight-eventhub-example/TemperatureMonitor`.</span></span> <span data-ttu-id="a0f9f-231">toobuild och paketet hello topologi använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-231">toobuild and package hello topology, use hello following command:</span></span> 

    ```bash
    mvn clean package
    ```

2. <span data-ttu-id="a0f9f-232">toostart Hej topologi i lokalt läge, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-232">toostart hello topology in local mode, use hello following command:</span></span>

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * <span data-ttu-id="a0f9f-233">`--local`startar hello topologi i lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-233">`--local` starts hello topology in local mode.</span></span>
    * <span data-ttu-id="a0f9f-234">`--filter`använder hello `dev.properties` filen toopopulate parametrar i hello topologi definition.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-234">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="a0f9f-235">`resources/no-hbase.yaml`använder hello `no-hbase.yaml` topologi definition.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-235">`resources/no-hbase.yaml` uses hello `no-hbase.yaml` topology definition.</span></span>
 
   <span data-ttu-id="a0f9f-236">När den har startat, hello topologi läser poster från Händelsehubb och skickar dem toohello instrumentpanelen som körs på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-236">Once started, hello topology reads entries from Event Hub, and sends them toohello dashboard running on your local machine.</span></span> <span data-ttu-id="a0f9f-237">Du bör se rader som visas i hello webbinstrumentpanel, liknande toohello följande bild:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-237">You should see lines appear in hello web dashboard, similar toohello following image:</span></span>
   
    ![instrumentpanel med data](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. <span data-ttu-id="a0f9f-239">När instrumentpanelen hello körs använder hello `node app.js` kommando från hello föregående steg toosend nya data tooEvent Hubs.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-239">While hello dashboard is running, use hello `node app.js` command from hello previous steps toosend new data tooEvent Hubs.</span></span> <span data-ttu-id="a0f9f-240">Eftersom hello temperatur värden genereras slumpmässigt, bör hello diagram uppdatera tooshow stora förändringar i temperatur.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-240">Because hello temperature values are randomly generated, hello graph should update tooshow large changes in temperature.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a0f9f-241">Du måste vara i hello **hdinsight-eventhub-exempel/SendEvents/Nodejs** directory när du använder hello `node app.js` kommando.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-241">You must be in hello **hdinsight-eventhub-example/SendEvents/Nodejs** directory when using hello `node app.js` command.</span></span>

3. <span data-ttu-id="a0f9f-242">När du har verifierat hello instrumentpanelen uppdaterar stoppa hello topologi med Ctrl + C.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-242">After verifying that hello dashboard updates, stop hello topology using Ctrl+C.</span></span> <span data-ttu-id="a0f9f-243">Du kan också använda Ctrl + C toostop hello lokal webbserver.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-243">You can use Ctrl+C toostop hello local web server also.</span></span>

## <a name="create-a-storm-and-hbase-cluster"></a><span data-ttu-id="a0f9f-244">Skapa ett Storm och HBase-kluster</span><span class="sxs-lookup"><span data-stu-id="a0f9f-244">Create a Storm and HBase cluster</span></span>

<span data-ttu-id="a0f9f-245">hello stegen i det här avsnittet används en [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md) toocreate ett Azure-nätverk och en Storm och HBase-kluster på hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-245">hello steps in this section use an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) toocreate an Azure Virtual Network and a Storm and HBase cluster on hello virtual network.</span></span> <span data-ttu-id="a0f9f-246">hello mallen även skapar en Azure Web App och distribuerar en kopia av hello instrumentpanelen till den.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-246">hello template also creates an Azure Web App and deploys a copy of hello dashboard into it.</span></span>

> [!NOTE]
> <span data-ttu-id="a0f9f-247">Ett virtuellt nätverk används så att hello-topologi som körs på hello Storm-kluster kan kommunicera direkt med hello HBase-kluster med hjälp av hello HBase Java API.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-247">A virtual network is used so that hello topology running on hello Storm cluster can directly communicate with hello HBase cluster using hello HBase Java API.</span></span>

<span data-ttu-id="a0f9f-248">hello Resource Manager-mall som används i det här dokumentet finns i en offentlig blob-behållare på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-248">hello Resource Manager template used in this document is located in a public blob container at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span></span>

1. <span data-ttu-id="a0f9f-249">Klicka på hello efter knappen toosign i tooAzure och öppna hello Resource Manager-mall i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-249">Click hello following button toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="a0f9f-250">Från hello **anpassad distribution** ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-250">From hello **Custom deployment** section, enter hello following values:</span></span>
   
    ![HDInsight-parametrar](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * <span data-ttu-id="a0f9f-252">**Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Storm och HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-252">**Base Cluster Name**: This value is used as hello base name for hello Storm and HBase clusters.</span></span> <span data-ttu-id="a0f9f-253">Ange till exempel **abc** skapar en Storm-kluster med namnet **storm abc** och ett HBase-kluster med namnet **hbase abc**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-253">For example, entering **abc** creates a Storm cluster named **storm-abc** and an HBase cluster named **hbase-abc**.</span></span>
   * <span data-ttu-id="a0f9f-254">**Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello Storm och HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-254">**Cluster Login User Name**: hello admin user name for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="a0f9f-255">**Klustret inloggningslösenordet**: hello administratörslösenord för hello Storm och HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-255">**Cluster Login Password**: hello admin user password for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="a0f9f-256">**SSH-användarnamn**: hello SSH användaren toocreate för hello Storm och HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-256">**SSH User Name**: hello SSH user toocreate for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="a0f9f-257">**SSH-lösenordet**: hello användarlösenord hello SSH för hello Storm och HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-257">**SSH Password**: hello password for hello SSH user for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="a0f9f-258">**Plats**: hello region som hello kluster skapas i.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-258">**Location**: hello region that hello clusters are created in.</span></span>
     
     <span data-ttu-id="a0f9f-259">Klicka på **OK** toosave hello parametrar.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-259">Click **OK** toosave hello parameters.</span></span>

3. <span data-ttu-id="a0f9f-260">Använd hello **grunderna** avsnittet toocreate en resursgrupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-260">Use hello **Basics** section toocreate a resource group or select an existing one.</span></span>
4. <span data-ttu-id="a0f9f-261">I hello **resursgruppsplats** väljer hello samma plats som du valt för hello **plats** parameter i hello **inställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-261">In hello **Resource group location** dropdown menu, select hello same location as you selected for hello **Location** parameter in hello **Settings** section.</span></span>
5. <span data-ttu-id="a0f9f-262">Läs hello villkor och välj sedan **acceptera toohello villkoren ovan**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-262">Read hello terms and conditions, and then select **I agree toohello terms and conditions stated above**.</span></span>
6. <span data-ttu-id="a0f9f-263">Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-263">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="a0f9f-264">Det tar cirka 20 minuter toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-264">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="a0f9f-265">När hello resurserna har skapats, visas information om hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-265">Once hello resources have been created, information about hello resource group is displayed.</span></span>

![Resursgruppen för hello vnet och kluster](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="a0f9f-267">Observera att hello namnen på hello HDInsight-kluster är **storm-BASENAME** och **hbase BASENAME**, där BASENAME är hello namn du angett toohello mall.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-267">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **hbase-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="a0f9f-268">Du kan använda dessa namn i ett senare steg när du ansluter toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-268">You use these names in a later step when connecting toohello clusters.</span></span> <span data-ttu-id="a0f9f-269">Observera att hello namnet på webbplatsen för hello instrumentpanelen är också **basename instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-269">Also note that hello name of hello dashboard site is **basename-dashboard**.</span></span> <span data-ttu-id="a0f9f-270">Det här värdet används senare i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-270">This value is used later in this document.</span></span>

## <a name="configure-hello-dashboard-bolt"></a><span data-ttu-id="a0f9f-271">Konfigurera hello instrumentpanelen bult</span><span class="sxs-lookup"><span data-stu-id="a0f9f-271">Configure hello Dashboard bolt</span></span>

<span data-ttu-id="a0f9f-272">toosend data toohello instrumentpanelen distribueras som en webbapp, måste du ändra följande rad i hello hello `dev.properties`fil:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-272">toosend data toohello dashboard deployed as a web app, you must modify hello following line in hello `dev.properties`file:</span></span>

```yaml
dashboard.uri: http://localhost:3000
```

<span data-ttu-id="a0f9f-273">Ändra `http://localhost:3000` för`http://BASENAME-dashboard.azurewebsites.net` och spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-273">Change `http://localhost:3000` too`http://BASENAME-dashboard.azurewebsites.net` and save hello file.</span></span> <span data-ttu-id="a0f9f-274">Ersätt **BASENAME** med hello grundläggande namnet du angav i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-274">Replace **BASENAME** with hello base name you provided in hello previous step.</span></span> <span data-ttu-id="a0f9f-275">Du kan också använda hello resursgruppen som skapats tidigare tooselect hello instrumentpanelen och visa hello-URL.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-275">You can also use hello resource group created previously tooselect hello dashboard and view hello URL.</span></span>

## <a name="create-hello-hbase-table"></a><span data-ttu-id="a0f9f-276">Skapa hello HBase-tabell</span><span class="sxs-lookup"><span data-stu-id="a0f9f-276">Create hello HBase table</span></span>

<span data-ttu-id="a0f9f-277">toostore data i HBase, vi måste först skapa en tabell.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-277">toostore data in HBase, we must first create a table.</span></span> <span data-ttu-id="a0f9f-278">Skapa resurser som Storm måste toowrite att i förväg, eftersom den försöker toocreate resurser från inuti en Storm-topologi kan resultera i flera instanser försök toocreate hello samma resurs.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-278">Pre-create resources that Storm needs toowrite to, as trying toocreate resources from inside a Storm topology can result in multiple instances trying toocreate hello same resource.</span></span> <span data-ttu-id="a0f9f-279">Skapa hello resurser utanför hello topologi och använda Storm för läsning/skrivning och analyser.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-279">Create hello resources outside hello topology and use Storm for reading/writing and analytics.</span></span>

1. <span data-ttu-id="a0f9f-280">Använda SSH tooconnect toohello HBase-kluster med hello SSH-användare och lösenord som du har angett toohello mall när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-280">Use SSH tooconnect toohello HBase cluster using hello SSH user and password you supplied toohello template during cluster creation.</span></span> <span data-ttu-id="a0f9f-281">Om exempelvis ansluter hello `ssh` kommandot om du vill använda hello följande syntax:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-281">For example, if connecting using hello `ssh` command, you would use hello following syntax:</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    <span data-ttu-id="a0f9f-282">Ersätt `sshuser` med hello SSH-användarnamnet du angav när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-282">Replace `sshuser` with hello SSH user name you provided when creating hello cluster.</span></span> <span data-ttu-id="a0f9f-283">Ersätt `clustername` med hello HBase klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-283">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="a0f9f-284">Starta hello HBase-gränssnittet från hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-284">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="a0f9f-285">När hello shell har lästs in, finns en `hbase(main):001:0>` prompt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-285">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="a0f9f-286">Ange hello efter kommandot toocreate en tabell toostore hello sensordata från hello HBase-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-286">From hello HBase shell, enter hello following command toocreate a table toostore hello sensor data:</span></span>
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. <span data-ttu-id="a0f9f-287">Kontrollera hello tabellen har skapats med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-287">Verify that hello table has been created by using hello following command:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="a0f9f-288">Detta returnerar information liknande toohello följande exempel, som anger att det finns 0 rader i tabellen hello.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-288">This returns information similar toohello following example, indicating that there are 0 rows in hello table.</span></span>
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. <span data-ttu-id="a0f9f-289">Ange `exit` tooexit hello HBase-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-289">Enter `exit` tooexit hello HBase shell:</span></span>

## <a name="configure-hello-hbase-bolt"></a><span data-ttu-id="a0f9f-290">Konfigurera hello HBase bult</span><span class="sxs-lookup"><span data-stu-id="a0f9f-290">Configure hello HBase bolt</span></span>

<span data-ttu-id="a0f9f-291">toowrite tooHBase från hello Storm-kluster, måste du ange hello HBase bulten med hello konfigurationsinformation för HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-291">toowrite tooHBase from hello Storm cluster, you must provide hello HBase bolt with hello configuration details of your HBase cluster.</span></span>

1. <span data-ttu-id="a0f9f-292">Använd någon av följande exempel tooretrieve hello Zookeeper kvorum för HBase-kluster hello:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-292">Use one of hello following examples tooretrieve hello Zookeeper quorum for your HBase cluster:</span></span>

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > <span data-ttu-id="a0f9f-293">Ersätt `your_HDInsight_cluster_name` med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-293">Replace `your_HDInsight_cluster_name` with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="a0f9f-294">Mer information om hur du installerar hello `jq` -verktyget, se [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-294">For more information on installing hello `jq` utility, see [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span></span>
    >
    > <span data-ttu-id="a0f9f-295">När du uppmanas du ange hello lösenord för inloggning för serveradministratör hello HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-295">When prompted, enter hello password for hello HDInsight admin login.</span></span>

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > <span data-ttu-id="a0f9f-296">Ersätt ”your_HDInsight_cluster_name med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-296">Replace \`your_HDInsight_cluster_name with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="a0f9f-297">När du uppmanas du ange hello lösenord för inloggning för serveradministratör hello HDInsight.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-297">When prompted, enter hello password for hello HDInsight admin login.</span></span>
    >
    > <span data-ttu-id="a0f9f-298">Det här exemplet kräver Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-298">This example requires Azure PowerShell.</span></span> <span data-ttu-id="a0f9f-299">Mer information om hur du använder Azure PowerShell finns [Kom igång med Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-299">For more information on using Azure PowerShell, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span></span>

    <span data-ttu-id="a0f9f-300">hello information som returnerades av de här exemplen är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-300">hello information returned by these examples is similar toohello following text:</span></span>

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    <span data-ttu-id="a0f9f-301">Den här informationen används av Storm toocommunicate med hello HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-301">This information is used by Storm toocommunicate with hello HBase cluster.</span></span>

2. <span data-ttu-id="a0f9f-302">Ändra hello `dev.properties` och Lägg till hello Zookeeper kvorum information toohello följande rad:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-302">Modify hello `dev.properties` file and add hello Zookeeper quorum information toohello following line:</span></span>

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a><span data-ttu-id="a0f9f-303">Skapa paket och distribuera hello lösning tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="a0f9f-303">Build, package, and deploy hello solution tooHDInsight</span></span>

<span data-ttu-id="a0f9f-304">Använd hello följande steg toodeploy hello Storm-topologi toohello storm-kluster i din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-304">In your development environment, use hello following steps toodeploy hello Storm topology toohello storm cluster.</span></span>

1. <span data-ttu-id="a0f9f-305">Från hello `TemperatureMonitor` katalogen Använd hello följande kommando tooperform en ny version och skapa ett JAR-paket från projektet:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-305">From hello `TemperatureMonitor` directory, use hello following command tooperform a new build and create a JAR package from your project:</span></span>
   
        mvn clean package
   
    <span data-ttu-id="a0f9f-306">Det här kommandot skapar en fil med namnet `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `mål-katalogen i ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-306">This command creates a file named `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `target\` directory of your project.</span></span>

2. <span data-ttu-id="a0f9f-307">Använd scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` och `dev.properties` filer tooyour Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-307">Use scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` and `dev.properties` files tooyour Storm cluster.</span></span> <span data-ttu-id="a0f9f-308">Följande exempel Ersätt i hello `sshuser` med hello SSH-användare som du angav när du skapar hello kluster och `clustername` med hello namnet Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-308">In hello following example, replace `sshuser` with hello SSH user you provided when creating hello cluster, and `clustername` with hello name of your Storm cluster.</span></span> <span data-ttu-id="a0f9f-309">När du uppmanas du ange hello lösenord för hello SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-309">When prompted, enter hello password for hello SSH user.</span></span>
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > <span data-ttu-id="a0f9f-310">Det kan ta flera minuter tooupload hello filer.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-310">It may take several minutes tooupload hello files.</span></span>

    <span data-ttu-id="a0f9f-311">Mer information om hur du använder hello `scp` och `ssh` kommandon med HDInsight finns [använda SSH med HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-311">For more information on using hello `scp` and `ssh` commands with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

3. <span data-ttu-id="a0f9f-312">När hello-filen har överförts, ansluta toohello Storm-kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-312">Once hello file has been uploaded, connect toohello Storm cluster using SSH.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="a0f9f-313">Ersätt `sshuser` med hello SSH-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-313">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="a0f9f-314">Ersätt `clustername` med hello Storm klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-314">Replace `clustername` with hello Storm cluster name.</span></span>

4. <span data-ttu-id="a0f9f-315">toostart hello topologi, använder du följande kommando från SSH-session för hello hello:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-315">toostart hello topology, use hello following command from hello SSH session:</span></span>
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * <span data-ttu-id="a0f9f-316">`--remote`skickar hello topologi toohello Nimbus-tjänsten, som distribuerar det toohello handledarens noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-316">`--remote` submits hello topology toohello Nimbus service, which distributes it toohello supervisor nodes in hello cluster.</span></span>
    * <span data-ttu-id="a0f9f-317">`--filter`använder hello `dev.properties` filen toopopulate parametrar i hello topologi definition.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-317">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="a0f9f-318">`-R /with-hbase.yaml`använder hello `with-hbase.yaml` topologi som ingår i hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-318">`-R /with-hbase.yaml` uses hello `with-hbase.yaml` topology included in hello package.</span></span>

5. <span data-ttu-id="a0f9f-319">Öppna en webbläsare toohello webbplats som du har publicerat i Azure, sedan används hello när hello-topologi har börjat `node app.js` kommandot toosend data tooEvent hubb.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-319">After hello topology has started, open a browser toohello website you published on Azure, then use hello `node app.js` command toosend data tooEvent Hub.</span></span> <span data-ttu-id="a0f9f-320">Du bör se hello web instrumentpanelen toodisplay hello uppdateringsinformation.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-320">You should see hello web dashboard update toodisplay hello information.</span></span>
   
    ![instrumentpanel](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a><span data-ttu-id="a0f9f-322">Visa data i HBase</span><span class="sxs-lookup"><span data-stu-id="a0f9f-322">View HBase data</span></span>

<span data-ttu-id="a0f9f-323">Använd följande steg tooconnect tooHBase hello och kontrollera att hello data har skrivits toohello tabell:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-323">Use hello following steps tooconnect tooHBase and verify that hello data has been written toohello table:</span></span>

1. <span data-ttu-id="a0f9f-324">Använda SSH tooconnect toohello HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-324">Use SSH tooconnect toohello HBase cluster.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="a0f9f-325">Ersätt `sshuser` med hello SSH-användarnamn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-325">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="a0f9f-326">Ersätt `clustername` med hello HBase klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-326">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="a0f9f-327">Starta hello HBase-gränssnittet från hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-327">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="a0f9f-328">När hello shell har lästs in, finns en `hbase(main):001:0>` prompt.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-328">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="a0f9f-329">Visa rader från hello tabellen:</span><span class="sxs-lookup"><span data-stu-id="a0f9f-329">View rows from hello table:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="a0f9f-330">Det här kommandot returnerar informationen liknande toohello följande text, som anger att det finns data i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-330">This command returns information similar toohello following text, indicating that there is data in hello table.</span></span>
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > <span data-ttu-id="a0f9f-331">Åtgärden genomsökning returnerar högst 10 raderna från hello tabell.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-331">This scan operation returns a maximum of 10 rows from hello table.</span></span>

## <a name="delete-your-clusters"></a><span data-ttu-id="a0f9f-332">Ta bort dina kluster</span><span class="sxs-lookup"><span data-stu-id="a0f9f-332">Delete your clusters</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="a0f9f-333">toodelete hello kluster, lagring och webb-app samtidigt, ta bort hello resursgruppen som innehåller dem..</span><span class="sxs-lookup"><span data-stu-id="a0f9f-333">toodelete hello clusters, storage, and web app at one time, delete hello resource group that contains them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a0f9f-334">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a0f9f-334">Next steps</span></span>

<span data-ttu-id="a0f9f-335">Fler exempel på Storm-topologier med HDInsight finns i [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)</span><span class="sxs-lookup"><span data-stu-id="a0f9f-335">For more examples of Storm topologies with HDInsight, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md)</span></span>

<span data-ttu-id="a0f9f-336">Mer information om Apache Storm finns hello [Apache Storm](https://storm.incubator.apache.org/) plats.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-336">For more information about Apache Storm, see hello [Apache Storm](https://storm.incubator.apache.org/) site.</span></span>

<span data-ttu-id="a0f9f-337">Mer information om HBase i HDInsight finns hello [HBase med HDInsight Overview](hdinsight-hbase-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-337">For more information about HBase on HDInsight, see hello [HBase with HDInsight Overview](hdinsight-hbase-overview.md).</span></span>

<span data-ttu-id="a0f9f-338">Mer information om Socket.io finns hello [socket.io](http://socket.io/) plats.</span><span class="sxs-lookup"><span data-stu-id="a0f9f-338">For more information about Socket.io, see hello [socket.io](http://socket.io/) site.</span></span>

<span data-ttu-id="a0f9f-339">Läs mer om D3.js [D3.js - Data drivs dokument](http://d3js.org/).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-339">For more information about D3.js, see [D3.js - Data Driven Documents](http://d3js.org/).</span></span>

<span data-ttu-id="a0f9f-340">Information om hur du skapar topologier i Java finns [utveckla Java-topologier för Apache Storm på HDInsight](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-340">For information about creating topologies in Java, see [Develop Java topologies for Apache Storm on HDInsight](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="a0f9f-341">Information om hur du skapar topologier i .NET finns [utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="a0f9f-341">For information about creating topologies in .NET, see [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

[azure-portal]: https://portal.azure.com
