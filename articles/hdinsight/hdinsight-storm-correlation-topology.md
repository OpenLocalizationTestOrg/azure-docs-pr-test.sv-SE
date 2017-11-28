---
title: "aaaCorrelate händelser över tid med Storm och HBase i HDInsight"
description: "Lär dig hur toocorrelate händelser som visas vid olika tidpunkter med Storm och HBase på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 17d11479-2d02-4790-8d29-05fb38351479
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: f915eef77bbda5b02bfd02ad0b5a4923ff2f4f26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="correlate-events-that-arrive-at-different-times-using-storm-and-hbase"></a><span data-ttu-id="706ff-103">Samordna händelser anländer vid olika tidpunkter med Storm och HBase</span><span class="sxs-lookup"><span data-stu-id="706ff-103">Correlate events that arrive at different times using Storm and HBase</span></span>

<span data-ttu-id="706ff-104">Du kan jämföra poster som visas vid olika tidpunkter med hjälp av en beständig datalager med Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="706ff-104">By using a persistent data store with Apache Storm, you can correlate data entries that arrive at different times.</span></span> <span data-ttu-id="706ff-105">Till exempel varade länka logga in och logga ut händelser för en användare session toocalculate hur länge hello session.</span><span class="sxs-lookup"><span data-stu-id="706ff-105">For example, linking login and logout events for a user session toocalculate how long hello session lasted.</span></span>

<span data-ttu-id="706ff-106">I detta dokument kan du lära dig hur toocreate en grundläggande C# Storm-topologi som följer logga in och logga ut händelser för användarsessioner och beräknar hello varaktighet hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="706ff-106">In this document, you learn how toocreate a basic C# Storm topology that tracks login and logout events for user sessions, and calculates hello duration of hello session.</span></span> <span data-ttu-id="706ff-107">hello topologin använder HBase som en beständig datalager.</span><span class="sxs-lookup"><span data-stu-id="706ff-107">hello topology uses HBase as a persistent data store.</span></span> <span data-ttu-id="706ff-108">HBase kan du också tooperform batch frågor på hello historiska data tooproduce ytterligare insikter.</span><span class="sxs-lookup"><span data-stu-id="706ff-108">HBase also allows you tooperform batch queries on hello historical data tooproduce additional insights.</span></span> <span data-ttu-id="706ff-109">Hur många användarsessioner har startats eller avslutats under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="706ff-109">For example, how many user sessions were started or ended during a specific time period.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="706ff-110">Krav</span><span class="sxs-lookup"><span data-stu-id="706ff-110">Prerequisites</span></span>

* <span data-ttu-id="706ff-111">Visual Studio och hello HDInsight tools för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="706ff-111">Visual Studio and hello HDInsight tools for Visual Studio.</span></span> <span data-ttu-id="706ff-112">Mer information finns i [komma igång med hello HDInsight tools för Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="706ff-112">For more information, see [Get started using hello HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="706ff-113">Apache Storm på HDInsight-kluster (Windows-baserade).</span><span class="sxs-lookup"><span data-stu-id="706ff-113">Apache Storm on HDInsight cluster (Windows-based).</span></span>

  > [!WARNING]
  > <span data-ttu-id="706ff-114">Medan SCP.NET topologier stöds på Linux-baserade Storm-kluster som skapas efter 2016/10/28 fungerar hello HBase SDK för .NET-paket som är tillgängliga från och med 10/28/2016 inte på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="706ff-114">While SCP.NET topologies are supported on Linux-based Storm clusters created after 10/28/2016, hello HBase SDK for .NET package available as of 10/28/2016 does not work correctly on Linux-based HDInsight</span></span>

* <span data-ttu-id="706ff-115">Apache HBase på HDInsight-kluster (Linux och Windows-baserade).</span><span class="sxs-lookup"><span data-stu-id="706ff-115">Apache HBase on HDInsight cluster (Linux or Windows-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="706ff-116">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="706ff-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="706ff-117">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="706ff-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="706ff-118">[Java](https://java.com) 1.7 eller mer på din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="706ff-118">[Java](https://java.com) 1.7 or greater on your development environment.</span></span> <span data-ttu-id="706ff-119">Java är används toopackage hello topologi när det inskickade toohello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="706ff-119">Java is used toopackage hello topology when it is submitted toohello HDInsight cluster.</span></span>

  * <span data-ttu-id="706ff-120">Hej **JAVA_HOME** miljö variabeln måste punkt toohello katalog som innehåller Java.</span><span class="sxs-lookup"><span data-stu-id="706ff-120">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="706ff-121">Hej **%JAVA_HOME%/bin** katalogen måste vara i hello sökväg</span><span class="sxs-lookup"><span data-stu-id="706ff-121">hello **%JAVA_HOME%/bin** directory must be in hello path</span></span>

## <a name="architecture"></a><span data-ttu-id="706ff-122">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="706ff-122">Architecture</span></span>

![Diagram över hello dataflöde via hello-topologi](./media/hdinsight-storm-correlation-topology/correlationtopology.png)

<span data-ttu-id="706ff-124">Korrelerar händelser kräver ett gemensamt ID för hello händelsekälla.</span><span class="sxs-lookup"><span data-stu-id="706ff-124">Correlating events requires a common identifier for hello event source.</span></span> <span data-ttu-id="706ff-125">Till exempel ett användar-ID, sessions-ID eller annan typ av data som är en) unikt och b) ingår i alla data som skickas tooStorm.</span><span class="sxs-lookup"><span data-stu-id="706ff-125">For example, a user ID, session ID, or other piece of data that is a) unique and b) included in all data sent tooStorm.</span></span> <span data-ttu-id="706ff-126">Det här exemplet används en GUID-värde toorepresent ett sessions-ID.</span><span class="sxs-lookup"><span data-stu-id="706ff-126">This example uses a GUID value toorepresent a session ID.</span></span>

<span data-ttu-id="706ff-127">Det här exemplet består av två HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="706ff-127">This example consists of two HDInsight clusters:</span></span>

* <span data-ttu-id="706ff-128">HBase: beständiga datalager för historiska data</span><span class="sxs-lookup"><span data-stu-id="706ff-128">HBase: persistent data store for historical data</span></span>
* <span data-ttu-id="706ff-129">Storm: används tooingest inkommande data</span><span class="sxs-lookup"><span data-stu-id="706ff-129">Storm: used tooingest incoming data</span></span>

<span data-ttu-id="706ff-130">hello data genereras slumpmässigt av hello Storm-topologi och består av hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="706ff-130">hello data is randomly generated by hello Storm topology, and consists of hello following items:</span></span>

* <span data-ttu-id="706ff-131">Sessions-ID: ett GUID som unikt identifierar varje session</span><span class="sxs-lookup"><span data-stu-id="706ff-131">Session ID: a GUID that uniquely identifies each session</span></span>
* <span data-ttu-id="706ff-132">Händelse: en START- eller händelse.</span><span class="sxs-lookup"><span data-stu-id="706ff-132">Event: a START or END event.</span></span> <span data-ttu-id="706ff-133">I det här exemplet inträffar START alltid före slut</span><span class="sxs-lookup"><span data-stu-id="706ff-133">For this example, START always occurs before END</span></span>
* <span data-ttu-id="706ff-134">Tid: hello tidpunkt hello händelsen.</span><span class="sxs-lookup"><span data-stu-id="706ff-134">Time: hello time of hello event.</span></span>

<span data-ttu-id="706ff-135">Dessa data bearbetas och lagras i HBase.</span><span class="sxs-lookup"><span data-stu-id="706ff-135">This data is processed and stored in HBase.</span></span>

### <a name="storm-topology"></a><span data-ttu-id="706ff-136">Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="706ff-136">Storm topology</span></span>

<span data-ttu-id="706ff-137">När en session startar en **starta** händelse tas emot av hello topologi och loggas tooHBase.</span><span class="sxs-lookup"><span data-stu-id="706ff-137">When a session starts, a **START** event is received by hello topology and logged tooHBase.</span></span> <span data-ttu-id="706ff-138">När en **END** händelsen har tagits emot, hello topologi hämtar hello **starta** händelse och beräknar hello tiden mellan hello två händelser.</span><span class="sxs-lookup"><span data-stu-id="706ff-138">When an **END** event is received, hello topology retrieves hello **START** event and calculates hello time between hello two events.</span></span> <span data-ttu-id="706ff-139">Detta **varaktighet** värdet lagras sedan i HBase tillsammans med hello **END** händelseinformation.</span><span class="sxs-lookup"><span data-stu-id="706ff-139">This **Duration** value is then stored in HBase along with hello **END** event information.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="706ff-140">När den här topologin visar hello grundläggande mönster, måste en lösning för produktion tootake design för hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="706ff-140">While this topology demonstrates hello basic pattern, a production solution would need tootake design for hello following scenarios:</span></span>
>
> * <span data-ttu-id="706ff-141">Händelser anländer utanför ordning</span><span class="sxs-lookup"><span data-stu-id="706ff-141">Events arriving out of order</span></span>
> * <span data-ttu-id="706ff-142">Duplicera händelser</span><span class="sxs-lookup"><span data-stu-id="706ff-142">Duplicate events</span></span>
> * <span data-ttu-id="706ff-143">Ignorerade händelser</span><span class="sxs-lookup"><span data-stu-id="706ff-143">Dropped events</span></span>

<span data-ttu-id="706ff-144">Hej exempeltopologi består av hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="706ff-144">hello sample topology is composed of hello following components:</span></span>

* <span data-ttu-id="706ff-145">Session.CS: simulerar en användarsession genom att skapa ett slumpmässigt sessions-ID, start tiden och hur länge hello session varar.</span><span class="sxs-lookup"><span data-stu-id="706ff-145">Session.cs: simulates a user session by creating a random session ID, start time, and how long hello session lasts.</span></span>

* <span data-ttu-id="706ff-146">Spout.CS: skapar 100 sessioner, genererar en Starthändelse, väntar hello slumpmässig tidsgräns för varje session och skickar en händelse från SLUTPUNKT.</span><span class="sxs-lookup"><span data-stu-id="706ff-146">Spout.cs: creates 100 sessions, emits a START event, waits hello random timeout for each session, and then emits an END event.</span></span> <span data-ttu-id="706ff-147">Återanvänder avslutats sedan sessioner toogenerate nya.</span><span class="sxs-lookup"><span data-stu-id="706ff-147">Then recycles ended sessions toogenerate new ones.</span></span>

* <span data-ttu-id="706ff-148">HBaseLookupBolt.cs: använder hello sessions-ID toolook upp sessionsinformation från HBase.</span><span class="sxs-lookup"><span data-stu-id="706ff-148">HBaseLookupBolt.cs: uses hello session ID toolook up session information from HBase.</span></span> <span data-ttu-id="706ff-149">När en händelse från SLUTPUNKT bearbetas hittar hello motsvarande Starthändelse och beräknar hello varaktighet hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="706ff-149">When an END event is processed, it finds hello corresponding START event and calculates hello duration of hello session.</span></span>

* <span data-ttu-id="706ff-150">HBaseBolt.cs: Lagrar informationen i HBase.</span><span class="sxs-lookup"><span data-stu-id="706ff-150">HBaseBolt.cs: Stores information into HBase.</span></span>

* <span data-ttu-id="706ff-151">TypeHelper.cs: Hjälper dig med typkonvertering när från läsning / skrivning tooHBase.</span><span class="sxs-lookup"><span data-stu-id="706ff-151">TypeHelper.cs: Helps with type conversion when reading from/writing tooHBase.</span></span>

### <a name="hbase-schema"></a><span data-ttu-id="706ff-152">HBase-schema</span><span class="sxs-lookup"><span data-stu-id="706ff-152">HBase schema</span></span>

<span data-ttu-id="706ff-153">I HBase lagras hello data i en tabell med följande inställningar för schema/hello:</span><span class="sxs-lookup"><span data-stu-id="706ff-153">In HBase, hello data is stored in a table with hello following schema/settings:</span></span>

* <span data-ttu-id="706ff-154">Radnyckel: hello sessions-ID används som hello nyckel för rader i den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="706ff-154">Row key: hello session ID is used as hello key for rows in this table.</span></span>

* <span data-ttu-id="706ff-155">Kolumnfamilj: hello namn är 'cf'.</span><span class="sxs-lookup"><span data-stu-id="706ff-155">Column family: hello family name is 'cf'.</span></span> <span data-ttu-id="706ff-156">Kolumner som lagras i den här serien är:</span><span class="sxs-lookup"><span data-stu-id="706ff-156">Columns stored in this family are:</span></span>

  * <span data-ttu-id="706ff-157">händelse: början eller slutet.</span><span class="sxs-lookup"><span data-stu-id="706ff-157">event: START or END.</span></span>

  * <span data-ttu-id="706ff-158">tid: hello tid i millisekunder som hello händelsen inträffade.</span><span class="sxs-lookup"><span data-stu-id="706ff-158">time: hello time in milliseconds that hello event occurred.</span></span>

  * <span data-ttu-id="706ff-159">varaktighet: hello är längden mellan START- och SLUTDATUM händelse.</span><span class="sxs-lookup"><span data-stu-id="706ff-159">duration: hello length between START and END event.</span></span>

* <span data-ttu-id="706ff-160">VERSIONER: hello 'cf-familjen anges tooretain 5 versioner av varje rad.</span><span class="sxs-lookup"><span data-stu-id="706ff-160">VERSIONS: hello 'cf' family is set tooretain 5 versions of each row.</span></span>

  > [!NOTE]
  > <span data-ttu-id="706ff-161">Versioner är en logg över tidigare värden som lagras för en specifik rad-nyckel.</span><span class="sxs-lookup"><span data-stu-id="706ff-161">Versions are a log of previous values stored for a specific row key.</span></span> <span data-ttu-id="706ff-162">Som standard returnerar HBase endast hello värdet för hello den senaste versionen av en rad.</span><span class="sxs-lookup"><span data-stu-id="706ff-162">By default, HBase only returns hello value for hello most recent version of a row.</span></span> <span data-ttu-id="706ff-163">I det här fallet används hello samma rad för alla händelser (START, avslutas.) varje version av en rad har identifierats av hello tidsstämpelvärde.</span><span class="sxs-lookup"><span data-stu-id="706ff-163">In this case, hello same row is used for all events (START, END.) each version of a row is identified by hello timestamp value.</span></span> <span data-ttu-id="706ff-164">Versioner ger en historisk bild av händelser som loggats för ett visst-ID.</span><span class="sxs-lookup"><span data-stu-id="706ff-164">Versions provide a historical view of events logged for a specific ID.</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="706ff-165">Ladda ned hello-projekt</span><span class="sxs-lookup"><span data-stu-id="706ff-165">Download hello project</span></span>

<span data-ttu-id="706ff-166">hello exempelprojektet kan hämtas från [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span><span class="sxs-lookup"><span data-stu-id="706ff-166">hello sample project can be downloaded from [https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation](https://github.com/Azure-Samples/hdinsight-storm-dotnet-event-correlation).</span></span>

<span data-ttu-id="706ff-167">Den här filen innehåller hello följande C#-projekt:</span><span class="sxs-lookup"><span data-stu-id="706ff-167">This download contains hello following C# projects:</span></span>

* <span data-ttu-id="706ff-168">CorrelationTopology: C# Storm-topologi som slumpmässigt genererar händelser som start- och slutdatum för användarsessioner.</span><span class="sxs-lookup"><span data-stu-id="706ff-168">CorrelationTopology: C# Storm topology that randomly emits start and end events for user sessions.</span></span> <span data-ttu-id="706ff-169">Varje session varar mellan 1 och 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="706ff-169">Each session lasts between 1 and 5 minutes.</span></span>

* <span data-ttu-id="706ff-170">SessionInfo: C#-konsolprogram som skapar hello HBase-tabell och innehåller exempelfrågor tooreturn information om lagrade sessionsdata.</span><span class="sxs-lookup"><span data-stu-id="706ff-170">SessionInfo: C# console application that creates hello HBase table, and provides example queries tooreturn information about stored session data.</span></span>

## <a name="create-hello-table"></a><span data-ttu-id="706ff-171">Skapa hello tabell</span><span class="sxs-lookup"><span data-stu-id="706ff-171">Create hello table</span></span>

1. <span data-ttu-id="706ff-172">Öppna hello **SessionInfo** projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="706ff-172">Open hello **SessionInfo** project in Visual Studio.</span></span>

2. <span data-ttu-id="706ff-173">I **Solution Explorer**, högerklicka på hello **SessionInfo** projektet och välj **egenskaper**.</span><span class="sxs-lookup"><span data-stu-id="706ff-173">In **Solution Explorer**, right-click hello **SessionInfo** project and select **Properties**.</span></span>

    ![Skärmbild av menyn med egenskaper valt](./media/hdinsight-storm-correlation-topology/selectproperties.png)

3. <span data-ttu-id="706ff-175">Välj **inställningar**, och sedan ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="706ff-175">Select **Settings**, then set hello following values:</span></span>

   * <span data-ttu-id="706ff-176">HBaseClusterURL: hello URL tooyour HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="706ff-176">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="706ff-177">Till exempel https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="706ff-177">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="706ff-178">HBaseClusterUserName: Hej admin/http-användarkonto för klustret</span><span class="sxs-lookup"><span data-stu-id="706ff-178">HBaseClusterUserName: hello admin/HTTP user account for your cluster</span></span>

   * <span data-ttu-id="706ff-179">HBaseClusterPassword: hello lösenordet för användarkontot för hello admin/HTTP</span><span class="sxs-lookup"><span data-stu-id="706ff-179">HBaseClusterPassword: hello password for hello admin/HTTP user account</span></span>

   * <span data-ttu-id="706ff-180">HBaseTableName: hello namnet på hello tabellen toouse med det här exemplet</span><span class="sxs-lookup"><span data-stu-id="706ff-180">HBaseTableName: hello name of hello table toouse with this example</span></span>

   * <span data-ttu-id="706ff-181">HBaseTableColumnFamily: hello kolumnnamn</span><span class="sxs-lookup"><span data-stu-id="706ff-181">HBaseTableColumnFamily: hello column family name</span></span>

     ![Bild av dialogrutan Inställningar](./media/hdinsight-storm-correlation-topology/settings.png)

4. <span data-ttu-id="706ff-183">Kör hello lösning.</span><span class="sxs-lookup"><span data-stu-id="706ff-183">Run hello solution.</span></span> <span data-ttu-id="706ff-184">När du uppmanas, Välj hello ”c” viktiga toocreate hello tabell på din HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="706ff-184">When prompted, select hello 'c' key toocreate hello table on your HBase cluster.</span></span>

## <a name="build-and-deploy-hello-storm-topology"></a><span data-ttu-id="706ff-185">Skapa och distribuera hello Storm-topologi</span><span class="sxs-lookup"><span data-stu-id="706ff-185">Build and deploy hello Storm topology</span></span>

1. <span data-ttu-id="706ff-186">Öppna hello **CorrelationTopology** lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="706ff-186">Open hello **CorrelationTopology** solution in Visual Studio.</span></span>

2. <span data-ttu-id="706ff-187">I **Solution Explorer**, högerklicka på hello **CorrelationTopology** projektet och välj Egenskaper.</span><span class="sxs-lookup"><span data-stu-id="706ff-187">In **Solution Explorer**, right-click hello **CorrelationTopology** project and select properties.</span></span>

3. <span data-ttu-id="706ff-188">I egenskapsfönstret för hello Välj **inställningar** och ange hello konfigurationsvärden för det här projektet.</span><span class="sxs-lookup"><span data-stu-id="706ff-188">In hello properties window, select **Settings** and enter hello configuration values for this project.</span></span> <span data-ttu-id="706ff-189">hello första 5 är hello samma värden som används av hello **SessionInfo** projektet:</span><span class="sxs-lookup"><span data-stu-id="706ff-189">hello first 5 are hello same values used by hello **SessionInfo** project:</span></span>

   * <span data-ttu-id="706ff-190">HBaseClusterURL: hello URL tooyour HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="706ff-190">HBaseClusterURL: hello URL tooyour HBase cluster.</span></span> <span data-ttu-id="706ff-191">Till exempel https://myhbasecluster.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="706ff-191">For example, https://myhbasecluster.azurehdinsight.net.</span></span>

   * <span data-ttu-id="706ff-192">HBaseClusterUserName: hello admin/http-användarkonto för klustret.</span><span class="sxs-lookup"><span data-stu-id="706ff-192">HBaseClusterUserName: hello admin/HTTP user account for your cluster.</span></span>

   * <span data-ttu-id="706ff-193">HBaseClusterPassword: hello lösenordet för användarkontot för hello admin/HTTP.</span><span class="sxs-lookup"><span data-stu-id="706ff-193">HBaseClusterPassword: hello password for hello admin/HTTP user account.</span></span>

   * <span data-ttu-id="706ff-194">HBaseTableName: hello namnet på hello tabellen toouse med det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="706ff-194">HBaseTableName: hello name of hello table toouse with this example.</span></span> <span data-ttu-id="706ff-195">Det här värdet är hello samma tabellnamn som används i hello SessionInfo projekt.</span><span class="sxs-lookup"><span data-stu-id="706ff-195">This value is hello same table name as used in hello SessionInfo project.</span></span>

   * <span data-ttu-id="706ff-196">HBaseTableColumnFamily: hello familj kolumnnamnet.</span><span class="sxs-lookup"><span data-stu-id="706ff-196">HBaseTableColumnFamily: hello column family name.</span></span> <span data-ttu-id="706ff-197">Det här värdet är hello samma familj kolumnnamn som används i hello SessionInfo projekt.</span><span class="sxs-lookup"><span data-stu-id="706ff-197">This value is hello same column family name as used in hello SessionInfo project.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="706ff-198">Ändra inte hello HBaseTableColumnNames, som hello standardvärdena är hello-namn som används av **SessionInfo** tooretrieve data.</span><span class="sxs-lookup"><span data-stu-id="706ff-198">Do not change hello HBaseTableColumnNames, as hello defaults are hello names used by **SessionInfo** tooretrieve data.</span></span>

4. <span data-ttu-id="706ff-199">Spara hello egenskaper och sedan skapa hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="706ff-199">Save hello properties, then build hello project.</span></span>

5. <span data-ttu-id="706ff-200">I **Solution Explorer**, högerklicka på hello-projektet och välj **skicka tooStorm på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="706ff-200">In **Solution Explorer**, right-click hello project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="706ff-201">Om du uppmanas du ange hello autentiseringsuppgifter för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="706ff-201">If prompted, enter hello credentials for your Azure subscription.</span></span>

   ![Bild av hello skicka toostorm menyobjekt](./media/hdinsight-storm-correlation-topology/submittostorm.png)

6. <span data-ttu-id="706ff-203">I hello **skicka topologi** dialogrutan, Välj hello Storm-kluster som du vill toodeploy den här topologin till.</span><span class="sxs-lookup"><span data-stu-id="706ff-203">In hello **Submit Topology** dialog, select hello Storm cluster you want toodeploy this topology to.</span></span>

   > [!NOTE]
   > <span data-ttu-id="706ff-204">hello kan första gången som du skickar en topologi, det ta några sekunder tooretrieve hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="706ff-204">hello first time you submit a topology, it may take a few seconds tooretrieve hello name of your HDInsight clusters.</span></span>

7. <span data-ttu-id="706ff-205">När hello-topologi har överföras och skickade toohello kluster, hello **Storm topologiska vyn** öppnas och visar hello kör topologi.</span><span class="sxs-lookup"><span data-stu-id="706ff-205">Once hello topology has been uploaded and submitted toohello cluster, hello **Storm Topology View** opens and displays hello running topology.</span></span> <span data-ttu-id="706ff-206">toorefresh hello data, Välj hello **CorrelationTopology** och använder hello uppdateringsknappen på hello uppifrån hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="706ff-206">toorefresh hello data, select hello **CorrelationTopology** and use hello refresh button at hello top right of hello page.</span></span>

   ![Bild av hello topologiska vyn](./media/hdinsight-storm-correlation-topology/topologyview.png)

   <span data-ttu-id="706ff-208">När hello topologi börjar generera data hello värdet i hello **avgivna** kolumnen steg.</span><span class="sxs-lookup"><span data-stu-id="706ff-208">When hello topology begins generating data, hello value in hello **Emitted** column increments.</span></span>

   > [!NOTE]
   > <span data-ttu-id="706ff-209">Om hello **Storm topologiska vyn** inte öppnas automatiskt, använder du följande steg tooopen hello den:</span><span class="sxs-lookup"><span data-stu-id="706ff-209">If hello **Storm Topology View** does not open automatically, use hello following steps tooopen it:</span></span>
   >
   > 1. <span data-ttu-id="706ff-210">I **Solution Explorer**, expandera **Azure**, och expandera sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="706ff-210">In **Solution Explorer**, expand **Azure**, and then expand **HDInsight**.</span></span>
   > 2. <span data-ttu-id="706ff-211">Högerklicka på hello Storm-kluster som hello topologi körs på och välj sedan **visa Storm-topologier**</span><span class="sxs-lookup"><span data-stu-id="706ff-211">Right-click hello Storm cluster that hello topology is running on, and then select **View Storm Topologies**</span></span>

## <a name="query-hello-data"></a><span data-ttu-id="706ff-212">Fråga hello data</span><span class="sxs-lookup"><span data-stu-id="706ff-212">Query hello data</span></span>

<span data-ttu-id="706ff-213">Använd hello följande steg tooquery hello data när data har släppts.</span><span class="sxs-lookup"><span data-stu-id="706ff-213">Once data has been emitted, use hello following steps tooquery hello data.</span></span>

1. <span data-ttu-id="706ff-214">Returnera toohello **SessionInfo** projekt.</span><span class="sxs-lookup"><span data-stu-id="706ff-214">Return toohello **SessionInfo** project.</span></span> <span data-ttu-id="706ff-215">Om inte körs startar du en ny instans av den.</span><span class="sxs-lookup"><span data-stu-id="706ff-215">If not running, start a new instance of it.</span></span>

2. <span data-ttu-id="706ff-216">När du uppmanas, Välj **s** toosearch för START-händelse.</span><span class="sxs-lookup"><span data-stu-id="706ff-216">When prompted, select **s** toosearch for START event.</span></span> <span data-ttu-id="706ff-217">Du tillfrågas tooenter start och avsluta tid toodefine tidsintervall - returneras endast händelser mellan dessa två gånger.</span><span class="sxs-lookup"><span data-stu-id="706ff-217">You are prompted tooenter a start and end time toodefine a time range - only events between these two times are returned.</span></span>

    <span data-ttu-id="706ff-218">Använd hello följande format när du lägger till hello start- och sluttider: hh: mm och ''M' eller 'pm'.</span><span class="sxs-lookup"><span data-stu-id="706ff-218">Use hello following format when entering hello start and end times: HH:MM and 'am' or 'pm'.</span></span> <span data-ttu-id="706ff-219">Till exempel 23:20:00.</span><span class="sxs-lookup"><span data-stu-id="706ff-219">For example, 11:20pm.</span></span>

    <span data-ttu-id="706ff-220">tooreturn loggas händelser, använda en starttid innan hello Storm-topologi har distribuerats och en sluttid för nu.</span><span class="sxs-lookup"><span data-stu-id="706ff-220">tooreturn logged events, use a start time from before hello Storm topology was deployed, and an end time of now.</span></span> <span data-ttu-id="706ff-221">hello returnera data innehåller poster liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="706ff-221">hello return data contains entries similar toohello following text:</span></span>

        Session e6992b3e-79be-4991-afcf-5cb47dd1c81c started at 6/5/2015 6:10:15 PM. Timestamp = 1433527820737

<span data-ttu-id="706ff-222">Söker efter slutet händelser fungerar hello samma som START-händelser.</span><span class="sxs-lookup"><span data-stu-id="706ff-222">Searching for END events works hello same as START events.</span></span> <span data-ttu-id="706ff-223">Dock genereras Sluthändelser slumpmässigt mellan 1 och 5 minuter efter hello START händelsen.</span><span class="sxs-lookup"><span data-stu-id="706ff-223">However, END events are generated randomly between 1 and 5 minutes after hello START event.</span></span> <span data-ttu-id="706ff-224">Du kan ha tootry några tid adressintervall toofind hello Sluthändelser.</span><span class="sxs-lookup"><span data-stu-id="706ff-224">You may have tootry a few time ranges toofind hello END events.</span></span> <span data-ttu-id="706ff-225">END-händelser innehåller också hello varaktighet hello session - hello skillnaden mellan hello händelse starttid och sluttid för händelsen.</span><span class="sxs-lookup"><span data-stu-id="706ff-225">END events also contain hello duration of hello session - hello difference between hello START event time and END event time.</span></span> <span data-ttu-id="706ff-226">Här är ett exempel på data för END händelser:</span><span class="sxs-lookup"><span data-stu-id="706ff-226">Here is an example of data for END events:</span></span>

    Session fc9fa8e6-6892-4073-93b3-a587040d892e lasted 2 minutes, and ended at 6/5/2015 6:12:15 PM

> [!NOTE]
> <span data-ttu-id="706ff-227">Hello tid-värden som du anger är i lokal tid, är hello tid som har returnerats från hello fråga i UTC.</span><span class="sxs-lookup"><span data-stu-id="706ff-227">While hello time values you enter are in local time, hello time returned from hello query is in UTC.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="706ff-228">Stoppa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="706ff-228">Stop hello topology</span></span>

<span data-ttu-id="706ff-229">När du är klar toostop hello topologi, returnera toohello **CorrelationTopology** projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="706ff-229">When you are ready toostop hello topology, return toohello **CorrelationTopology** project in Visual Studio.</span></span> <span data-ttu-id="706ff-230">I hello **Storm topologiska vyn**hello topologi och välj sedan använda hello **Kill** knappen hello överst i hello topologiska vyn.</span><span class="sxs-lookup"><span data-stu-id="706ff-230">In hello **Storm Topology View**, select hello topology and then use hello **Kill** button at hello top of hello topology view.</span></span>

## <a name="delete-your-cluster"></a><span data-ttu-id="706ff-231">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="706ff-231">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="706ff-232">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="706ff-232">Next steps</span></span>

<span data-ttu-id="706ff-233">Fler Storm-exempel finns [exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="706ff-233">For more Storm examples, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
