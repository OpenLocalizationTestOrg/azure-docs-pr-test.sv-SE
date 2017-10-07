---
title: "aaaAccess Hadoop YARN program loggar på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooaccess YARN program loggar på en Linux-baserat HDInsight (Hadoop)-kluster med hjälp av både hello kommandoradsverktyg och en webbläsare."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 3ec08d20-4f19-4a8e-ac86-639c04d2f12e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 0bab356e3b97114abbb05712c8e7b21a194f2508
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a><span data-ttu-id="f2f6f-103">Åtkomst programloggar YARN på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="f2f6f-103">Access YARN application logs on Linux-based HDInsight</span></span>

<span data-ttu-id="f2f6f-104">Lär dig hur tooaccess hello loggar för YARN (ännu en annan resurs förhandlare) program på ett Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-104">Learn how tooaccess hello logs for YARN (Yet Another Resource Negotiator) applications on a Hadoop cluster in Azure HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f2f6f-105">hello stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-105">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="f2f6f-106">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-106">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f2f6f-107">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f2f6f-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="f2f6f-108"><a name="YARNTimelineServer"></a>YARN tidslinjen Server</span><span class="sxs-lookup"><span data-stu-id="f2f6f-108"><a name="YARNTimelineServer"></a>YARN Timeline Server</span></span>

<span data-ttu-id="f2f6f-109">Hej [YARN tidslinjen Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) ger allmän information om slutförda program och programinformation om framework-specifika via två olika gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-109">hello [YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) provides generic information on completed applications and framework-specific application information through two different interfaces.</span></span> <span data-ttu-id="f2f6f-110">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="f2f6f-110">Specifically:</span></span>

* <span data-ttu-id="f2f6f-111">Lagring och hämtning av information för Allmänt program på HDInsight-kluster har aktiverat med version 3.1.1.374 eller högre.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-111">Storage and retrieval of generic application information on HDInsight clusters has been enabled with version 3.1.1.374 or higher.</span></span>
* <span data-ttu-id="f2f6f-112">hello framework-specifik information programkomponent av hello tidslinjen Server är inte tillgänglig på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-112">hello framework-specific application information component of hello Timeline Server is not currently available on HDInsight clusters.</span></span>

<span data-ttu-id="f2f6f-113">Allmän information om program innehåller hello efter typ av data:</span><span class="sxs-lookup"><span data-stu-id="f2f6f-113">Generic information on applications includes hello following type of data:</span></span>

* <span data-ttu-id="f2f6f-114">hello program-ID, en unik identifierare för ett program</span><span class="sxs-lookup"><span data-stu-id="f2f6f-114">hello application ID, a unique identifier of an application</span></span>
* <span data-ttu-id="f2f6f-115">hello-användare som startade hello program</span><span class="sxs-lookup"><span data-stu-id="f2f6f-115">hello user who started hello application</span></span>
* <span data-ttu-id="f2f6f-116">Information om försök görs toocomplete hello program</span><span class="sxs-lookup"><span data-stu-id="f2f6f-116">Information on attempts made toocomplete hello application</span></span>
* <span data-ttu-id="f2f6f-117">hello-behållare som används av ett visst program försök</span><span class="sxs-lookup"><span data-stu-id="f2f6f-117">hello containers used by any given application attempt</span></span>

## <span data-ttu-id="f2f6f-118"><a name="YARNAppsAndLogs"></a>YARN program och loggfiler</span><span class="sxs-lookup"><span data-stu-id="f2f6f-118"><a name="YARNAppsAndLogs"></a>YARN applications and logs</span></span>

<span data-ttu-id="f2f6f-119">YARN stöder flera programmeringsmodeller (MapReduce är en av dem) genom Frikoppling resurshantering från schemaläggning/programövervakning.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-119">YARN supports multiple programming models (MapReduce being one of them) by decoupling resource management from application scheduling/monitoring.</span></span> <span data-ttu-id="f2f6f-120">YARN använder en global *ResourceManager* (RM) per arbetsnoden *NodeManagers* (NMs), och programspecifika *ApplicationMasters* (AMs).</span><span class="sxs-lookup"><span data-stu-id="f2f6f-120">YARN uses a global *ResourceManager* (RM), per-worker-node *NodeManagers* (NMs), and per-application *ApplicationMasters* (AMs).</span></span> <span data-ttu-id="f2f6f-121">hello förhandlar programspecifika AM resurser (processor, minne, disk, nätverk) för att köra programmet med hello RM.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-121">hello per-application AM negotiates resources (CPU, memory, disk, network) for running your application with hello RM.</span></span> <span data-ttu-id="f2f6f-122">hello RM fungerar med NMs toogrant dessa resurser, som beviljas som *behållare*.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-122">hello RM works with NMs toogrant these resources, which are granted as *containers*.</span></span> <span data-ttu-id="f2f6f-123">hello AM ansvarar för att spåra hello fortskrider hello behållare som tilldelats tooit av hello RM.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-123">hello AM is responsible for tracking hello progress of hello containers assigned tooit by hello RM.</span></span> <span data-ttu-id="f2f6f-124">Ett program kan kräva många behållare beroende på hello uppbyggnad hello program.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-124">An application may require many containers depending on hello nature of hello application.</span></span>

<span data-ttu-id="f2f6f-125">Varje program kan bestå av flera *program försöker*.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-125">Each application may consist of multiple *application attempts*.</span></span> <span data-ttu-id="f2f6f-126">Om ett program inte kan du göra det som ett nytt försök.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-126">If an application fails, it may be retried as a new attempt.</span></span> <span data-ttu-id="f2f6f-127">Varje försök körs i en behållare.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-127">Each attempt runs in a container.</span></span> <span data-ttu-id="f2f6f-128">I en mening ger en behållare hello kontext för grundläggande enheten för arbete som utförs av en YARN-programmet.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-128">In a sense, a container provides hello context for basic unit of work performed by a YARN application.</span></span> <span data-ttu-id="f2f6f-129">Allt arbete som utförs inom hello kontext för en behållare har utförts på hello enskild worker nod på vilka hello behållaren har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-129">All work that is done within hello context of a container is performed on hello single worker node on which hello container was allocated.</span></span> <span data-ttu-id="f2f6f-130">Se [YARN begrepp] [ YARN-concepts] ytterligare referens.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-130">See [YARN Concepts][YARN-concepts] for further reference.</span></span>

<span data-ttu-id="f2f6f-131">Programmet loggar (och associerade hello behållaren loggar) är kritiska felsöka problematiska Hadoop-program.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-131">Application logs (and hello associated container logs) are critical in debugging problematic Hadoop applications.</span></span> <span data-ttu-id="f2f6f-132">YARN tillhandahåller ett bra ramverk för att samla in, sammanställa och lagra programloggarna med hello [loggen aggregering] [ log-aggregation] funktion.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-132">YARN provides a nice framework for collecting, aggregating, and storing application logs with hello [Log Aggregation][log-aggregation] feature.</span></span> <span data-ttu-id="f2f6f-133">hello loggen aggregering funktionen gör att nå programloggarna mer deterministisk.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-133">hello Log Aggregation feature makes accessing application logs more deterministic.</span></span> <span data-ttu-id="f2f6f-134">Den sammanställer loggar över alla behållare på en arbetsnod och lagrar dem som en sammansatt loggfil per arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-134">It aggregates logs across all containers on a worker node and stores them as one aggregated log file per worker node.</span></span> <span data-ttu-id="f2f6f-135">hello loggen är lagrade på hello standardfilsystem när ett program har slutförts.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-135">hello log is stored on hello default file system after an application finishes.</span></span> <span data-ttu-id="f2f6f-136">Programmet kan använda hundratals eller tusentals behållare, men loggar för alla behållare som körs på en enda arbetsnod är en fil alltid sammansatt tooa.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-136">Your application may use hundreds or thousands of containers, but logs for all containers run on a single worker node are always aggregated tooa single file.</span></span> <span data-ttu-id="f2f6f-137">Det är därför bara 1 loggen per arbetsnoden som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-137">So there is only 1 log per worker node used by your application.</span></span> <span data-ttu-id="f2f6f-138">Aggregering av loggen är aktiverad som standard på HDInsight-kluster av version 3.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-138">Log Aggregation is enabled by default on HDInsight clusters version 3.0 and above.</span></span> <span data-ttu-id="f2f6f-139">Sammanställda loggarna finns i standard lagringen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-139">Aggregated logs are located in default storage for hello cluster.</span></span> <span data-ttu-id="f2f6f-140">hello följande sökväg är hello HDFS sökväg toohello loggar:</span><span class="sxs-lookup"><span data-stu-id="f2f6f-140">hello following path is hello HDFS path toohello logs:</span></span>

    /app-logs/<user>/logs/<applicationId>

<span data-ttu-id="f2f6f-141">Hello sökvägen `user` är hello användare som startade hello programmet hello namn.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-141">In hello path, `user` is hello name of hello user who started hello application.</span></span> <span data-ttu-id="f2f6f-142">Hej `applicationId` är hello Unik identifierare som tilldelas tooan program av hello YARN RM.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-142">hello `applicationId` is hello unique identifier assigned tooan application by hello YARN RM.</span></span>

<span data-ttu-id="f2f6f-143">hello aggregerade loggar är inte direkt läsbar, som de är skrivna en [TFile][T-file], [binärformat] [ binary-format] indexeras av behållare.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-143">hello aggregated logs are not directly readable, as they are written in a [TFile][T-file], [binary format][binary-format] indexed by container.</span></span> <span data-ttu-id="f2f6f-144">Använda hello YARN ResourceManager loggar eller CLI verktyg tooview loggarna som oformaterad text för program eller behållare av intresse.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-144">Use hello YARN ResourceManager logs or CLI tools tooview these logs as plain text for applications or containers of interest.</span></span>

## <a name="yarn-cli-tools"></a><span data-ttu-id="f2f6f-145">YARN CLI-verktygen</span><span class="sxs-lookup"><span data-stu-id="f2f6f-145">YARN CLI tools</span></span>

<span data-ttu-id="f2f6f-146">toouse hello YARN CLI-verktygen, måste du först ansluta toohello HDInsight-kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-146">toouse hello YARN CLI tools, you must first connect toohello HDInsight cluster using SSH.</span></span> <span data-ttu-id="f2f6f-147">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="f2f6f-147">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="f2f6f-148">Du kan visa dessa loggar som oformaterad text genom att köra ett hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f2f6f-148">You can view these logs as plain text by running one of hello following commands:</span></span>

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

<span data-ttu-id="f2f6f-149">Ange hello &lt;applicationId >, &lt;användaren som-igång-program >, &lt;behållar-ID >, och &lt;worker-nod-adress > information när du kör kommandona.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-149">Specify hello &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId>, and &lt;worker-node-address> information when running these commands.</span></span>

## <a name="yarn-resourcemanager-ui"></a><span data-ttu-id="f2f6f-150">YARN ResourceManager-Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="f2f6f-150">YARN ResourceManager UI</span></span>

<span data-ttu-id="f2f6f-151">Hej YARN ResourceManager-Användargränssnittet körs på hello klustret headnode.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-151">hello YARN ResourceManager UI runs on hello cluster headnode.</span></span> <span data-ttu-id="f2f6f-152">Den kan nås via hello Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-152">It is accessed through hello Ambari web UI.</span></span> <span data-ttu-id="f2f6f-153">Använd hello följande steg tooview hello YARN-loggar:</span><span class="sxs-lookup"><span data-stu-id="f2f6f-153">Use hello following steps tooview hello YARN logs:</span></span>

1. <span data-ttu-id="f2f6f-154">Navigera toohttps://CLUSTERNAME.azurehdinsight.net i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-154">In your web browser, navigate toohttps://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="f2f6f-155">Ersätt KLUSTERNAMN med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-155">Replace CLUSTERNAME with hello name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="f2f6f-156">Välj hello listan över tjänster hello vänster **YARN**.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-156">From hello list of services on hello left, select **YARN**.</span></span>

    ![Yarn-tjänsten som markerats](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. <span data-ttu-id="f2f6f-158">Från hello **snabblänkar** listrutan väljer du något av hello-klustrets huvudnoder och välj sedan **ResourceManager loggen**.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-158">From hello **Quick Links** dropdown, select one of hello cluster head nodes and then select **ResourceManager Log**.</span></span>

    ![Yarn snabblänkar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    <span data-ttu-id="f2f6f-160">Visas en lista med länkar tooYARN loggar.</span><span class="sxs-lookup"><span data-stu-id="f2f6f-160">You are presented with a list of links tooYARN logs.</span></span>

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
