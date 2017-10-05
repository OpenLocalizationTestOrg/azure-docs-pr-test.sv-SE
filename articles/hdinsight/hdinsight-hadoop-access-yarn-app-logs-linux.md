---
title: "Åtkomst Hadoop YARN programloggar på Linux-baserade HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur du kommer åt YARN programloggar på en Linux-baserat HDInsight (Hadoop)-kluster med hjälp av kommandoraden och en webbläsare."
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
ms.openlocfilehash: fbbbddc47f24a46eac9bc64d4420ee8429ed4ad1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="access-yarn-application-logs-on-linux-based-hdinsight"></a><span data-ttu-id="ab31c-103">Åtkomst programloggar YARN på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="ab31c-103">Access YARN application logs on Linux-based HDInsight</span></span>

<span data-ttu-id="ab31c-104">Lär dig hur du kommer åt loggarna för YARN (ännu en annan resurs förhandlare) program på ett Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ab31c-104">Learn how to access the logs for YARN (Yet Another Resource Negotiator) applications on a Hadoop cluster in Azure HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ab31c-105">Stegen i det här dokumentet kräver ett HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="ab31c-105">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="ab31c-106">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="ab31c-106">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ab31c-107">Mer information finns i [HDInsight component-versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ab31c-107">For more information, see [HDInsight component versioning](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="ab31c-108"><a name="YARNTimelineServer"></a>YARN tidslinjen Server</span><span class="sxs-lookup"><span data-stu-id="ab31c-108"><a name="YARNTimelineServer"></a>YARN Timeline Server</span></span>

<span data-ttu-id="ab31c-109">Den [YARN tidslinjen Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) ger allmän information om slutförda program och programinformation om framework-specifika via två olika gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ab31c-109">The [YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) provides generic information on completed applications and framework-specific application information through two different interfaces.</span></span> <span data-ttu-id="ab31c-110">Närmare bestämt:</span><span class="sxs-lookup"><span data-stu-id="ab31c-110">Specifically:</span></span>

* <span data-ttu-id="ab31c-111">Lagring och hämtning av information för Allmänt program på HDInsight-kluster har aktiverat med version 3.1.1.374 eller högre.</span><span class="sxs-lookup"><span data-stu-id="ab31c-111">Storage and retrieval of generic application information on HDInsight clusters has been enabled with version 3.1.1.374 or higher.</span></span>
* <span data-ttu-id="ab31c-112">Framework-specifik information PROGRAMKOMPONENTEN tidslinjen Server är inte tillgänglig på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab31c-112">The framework-specific application information component of the Timeline Server is not currently available on HDInsight clusters.</span></span>

<span data-ttu-id="ab31c-113">Allmän information om program innehåller följande typ av data:</span><span class="sxs-lookup"><span data-stu-id="ab31c-113">Generic information on applications includes the following type of data:</span></span>

* <span data-ttu-id="ab31c-114">Program-ID, en unik identifierare för ett program</span><span class="sxs-lookup"><span data-stu-id="ab31c-114">The application ID, a unique identifier of an application</span></span>
* <span data-ttu-id="ab31c-115">Den användare som startade programmet</span><span class="sxs-lookup"><span data-stu-id="ab31c-115">The user who started the application</span></span>
* <span data-ttu-id="ab31c-116">Information om försök görs att slutföra programmet</span><span class="sxs-lookup"><span data-stu-id="ab31c-116">Information on attempts made to complete the application</span></span>
* <span data-ttu-id="ab31c-117">De behållare som används av ett visst program försök</span><span class="sxs-lookup"><span data-stu-id="ab31c-117">The containers used by any given application attempt</span></span>

## <span data-ttu-id="ab31c-118"><a name="YARNAppsAndLogs"></a>YARN program och loggfiler</span><span class="sxs-lookup"><span data-stu-id="ab31c-118"><a name="YARNAppsAndLogs"></a>YARN applications and logs</span></span>

<span data-ttu-id="ab31c-119">YARN stöder flera programmeringsmodeller (MapReduce är en av dem) genom Frikoppling resurshantering från schemaläggning/programövervakning.</span><span class="sxs-lookup"><span data-stu-id="ab31c-119">YARN supports multiple programming models (MapReduce being one of them) by decoupling resource management from application scheduling/monitoring.</span></span> <span data-ttu-id="ab31c-120">YARN använder en global *ResourceManager* (RM) per arbetsnoden *NodeManagers* (NMs), och programspecifika *ApplicationMasters* (AMs).</span><span class="sxs-lookup"><span data-stu-id="ab31c-120">YARN uses a global *ResourceManager* (RM), per-worker-node *NodeManagers* (NMs), and per-application *ApplicationMasters* (AMs).</span></span> <span data-ttu-id="ab31c-121">Programspecifika AM förhandlar resurser (processor, minne, disk, nätverk) för att köra programmet med RM.</span><span class="sxs-lookup"><span data-stu-id="ab31c-121">The per-application AM negotiates resources (CPU, memory, disk, network) for running your application with the RM.</span></span> <span data-ttu-id="ab31c-122">RM fungerar med NMs att bevilja dessa resurser, som beviljas som *behållare*.</span><span class="sxs-lookup"><span data-stu-id="ab31c-122">The RM works with NMs to grant these resources, which are granted as *containers*.</span></span> <span data-ttu-id="ab31c-123">AM ansvarar för att spåra förloppet för de behållare som tilldelats av RM.</span><span class="sxs-lookup"><span data-stu-id="ab31c-123">The AM is responsible for tracking the progress of the containers assigned to it by the RM.</span></span> <span data-ttu-id="ab31c-124">Ett program kan kräva många behållare beroende på typ av programmet.</span><span class="sxs-lookup"><span data-stu-id="ab31c-124">An application may require many containers depending on the nature of the application.</span></span>

<span data-ttu-id="ab31c-125">Varje program kan bestå av flera *program försöker*.</span><span class="sxs-lookup"><span data-stu-id="ab31c-125">Each application may consist of multiple *application attempts*.</span></span> <span data-ttu-id="ab31c-126">Om ett program inte kan du göra det som ett nytt försök.</span><span class="sxs-lookup"><span data-stu-id="ab31c-126">If an application fails, it may be retried as a new attempt.</span></span> <span data-ttu-id="ab31c-127">Varje försök körs i en behållare.</span><span class="sxs-lookup"><span data-stu-id="ab31c-127">Each attempt runs in a container.</span></span> <span data-ttu-id="ab31c-128">I en mening ger en behållare kontext för grundläggande enheten för arbete som utförs av en YARN-programmet.</span><span class="sxs-lookup"><span data-stu-id="ab31c-128">In a sense, a container provides the context for basic unit of work performed by a YARN application.</span></span> <span data-ttu-id="ab31c-129">Allt arbete som utförs inom ramen för en behållare har utförts på enskild arbetsnoden som behållaren har tilldelats.</span><span class="sxs-lookup"><span data-stu-id="ab31c-129">All work that is done within the context of a container is performed on the single worker node on which the container was allocated.</span></span> <span data-ttu-id="ab31c-130">Se [YARN begrepp] [ YARN-concepts] ytterligare referens.</span><span class="sxs-lookup"><span data-stu-id="ab31c-130">See [YARN Concepts][YARN-concepts] for further reference.</span></span>

<span data-ttu-id="ab31c-131">Programloggarna (och associerade behållaren loggar) är kritiska felsöka problematiska Hadoop-program.</span><span class="sxs-lookup"><span data-stu-id="ab31c-131">Application logs (and the associated container logs) are critical in debugging problematic Hadoop applications.</span></span> <span data-ttu-id="ab31c-132">YARN tillhandahåller ett bra ramverk för att samla in, sammanställa och lagra programloggarna med den [loggen aggregering] [ log-aggregation] funktion.</span><span class="sxs-lookup"><span data-stu-id="ab31c-132">YARN provides a nice framework for collecting, aggregating, and storing application logs with the [Log Aggregation][log-aggregation] feature.</span></span> <span data-ttu-id="ab31c-133">Funktionen Log aggregering gör att nå programloggarna mer deterministisk.</span><span class="sxs-lookup"><span data-stu-id="ab31c-133">The Log Aggregation feature makes accessing application logs more deterministic.</span></span> <span data-ttu-id="ab31c-134">Den sammanställer loggar över alla behållare på en arbetsnod och lagrar dem som en sammansatt loggfil per arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="ab31c-134">It aggregates logs across all containers on a worker node and stores them as one aggregated log file per worker node.</span></span> <span data-ttu-id="ab31c-135">Loggen är lagrade på filsystemet när ett program har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ab31c-135">The log is stored on the default file system after an application finishes.</span></span> <span data-ttu-id="ab31c-136">Programmet kan använda hundratals eller tusentals behållare, men loggar för alla behållare som körs på en enda arbetsnod aggregeras alltid till en fil.</span><span class="sxs-lookup"><span data-stu-id="ab31c-136">Your application may use hundreds or thousands of containers, but logs for all containers run on a single worker node are always aggregated to a single file.</span></span> <span data-ttu-id="ab31c-137">Det är därför bara 1 loggen per arbetsnoden som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="ab31c-137">So there is only 1 log per worker node used by your application.</span></span> <span data-ttu-id="ab31c-138">Aggregering av loggen är aktiverad som standard på HDInsight-kluster av version 3.0 och senare.</span><span class="sxs-lookup"><span data-stu-id="ab31c-138">Log Aggregation is enabled by default on HDInsight clusters version 3.0 and above.</span></span> <span data-ttu-id="ab31c-139">Sammanställda loggarna finns i standardlagring för klustret.</span><span class="sxs-lookup"><span data-stu-id="ab31c-139">Aggregated logs are located in default storage for the cluster.</span></span> <span data-ttu-id="ab31c-140">Följande är HDFS-sökvägen till loggar:</span><span class="sxs-lookup"><span data-stu-id="ab31c-140">The following path is the HDFS path to the logs:</span></span>

    /app-logs/<user>/logs/<applicationId>

<span data-ttu-id="ab31c-141">I sökvägen `user` är namnet på den användare som startade programmet.</span><span class="sxs-lookup"><span data-stu-id="ab31c-141">In the path, `user` is the name of the user who started the application.</span></span> <span data-ttu-id="ab31c-142">Den `applicationId` är den unika identifieraren som tilldelats till ett program av YARN RM.</span><span class="sxs-lookup"><span data-stu-id="ab31c-142">The `applicationId` is the unique identifier assigned to an application by the YARN RM.</span></span>

<span data-ttu-id="ab31c-143">Sammanställda loggar är inte direkt läsbar, som de är skrivna en [TFile][T-file], [binärformat] [ binary-format] indexeras av behållare.</span><span class="sxs-lookup"><span data-stu-id="ab31c-143">The aggregated logs are not directly readable, as they are written in a [TFile][T-file], [binary format][binary-format] indexed by container.</span></span> <span data-ttu-id="ab31c-144">Använda YARN ResourceManager loggarna eller CLI-verktygen för att visa dessa loggar som oformaterad text för program eller behållare av intresse.</span><span class="sxs-lookup"><span data-stu-id="ab31c-144">Use the YARN ResourceManager logs or CLI tools to view these logs as plain text for applications or containers of interest.</span></span>

## <a name="yarn-cli-tools"></a><span data-ttu-id="ab31c-145">YARN CLI-verktygen</span><span class="sxs-lookup"><span data-stu-id="ab31c-145">YARN CLI tools</span></span>

<span data-ttu-id="ab31c-146">Om du vill använda YARN CLI-verktygen måste du först ansluta till HDInsight-klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="ab31c-146">To use the YARN CLI tools, you must first connect to the HDInsight cluster using SSH.</span></span> <span data-ttu-id="ab31c-147">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="ab31c-147">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

<span data-ttu-id="ab31c-148">Du kan visa dessa loggar som oformaterad text genom att köra något av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ab31c-148">You can view these logs as plain text by running one of the following commands:</span></span>

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

<span data-ttu-id="ab31c-149">Ange den &lt;applicationId >, &lt;användaren som-igång-program >, &lt;behållar-ID >, och &lt;worker-nod-adress > information när du kör kommandona.</span><span class="sxs-lookup"><span data-stu-id="ab31c-149">Specify the &lt;applicationId>, &lt;user-who-started-the-application>, &lt;containerId>, and &lt;worker-node-address> information when running these commands.</span></span>

## <a name="yarn-resourcemanager-ui"></a><span data-ttu-id="ab31c-150">YARN ResourceManager-Användargränssnittet</span><span class="sxs-lookup"><span data-stu-id="ab31c-150">YARN ResourceManager UI</span></span>

<span data-ttu-id="ab31c-151">YARN ResourceManager-Användargränssnittet körs på klustret headnode.</span><span class="sxs-lookup"><span data-stu-id="ab31c-151">The YARN ResourceManager UI runs on the cluster headnode.</span></span> <span data-ttu-id="ab31c-152">Den kan nås via Ambari-webbgränssnittet.</span><span class="sxs-lookup"><span data-stu-id="ab31c-152">It is accessed through the Ambari web UI.</span></span> <span data-ttu-id="ab31c-153">Använd följande steg om du vill visa YARN-loggar:</span><span class="sxs-lookup"><span data-stu-id="ab31c-153">Use the following steps to view the YARN logs:</span></span>

1. <span data-ttu-id="ab31c-154">Navigera till https://CLUSTERNAME.azurehdinsight.net i din webbläsare.</span><span class="sxs-lookup"><span data-stu-id="ab31c-154">In your web browser, navigate to https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="ab31c-155">Ersätt KLUSTERNAMN med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ab31c-155">Replace CLUSTERNAME with the name of your HDInsight cluster.</span></span>
2. <span data-ttu-id="ab31c-156">Listan över tjänster till vänster och välj **YARN**.</span><span class="sxs-lookup"><span data-stu-id="ab31c-156">From the list of services on the left, select **YARN**.</span></span>

    ![Yarn-tjänsten som markerats](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)
3. <span data-ttu-id="ab31c-158">Från den **snabblänkar** listrutan, Välj en av klusternoderna head och välj sedan **ResourceManager loggen**.</span><span class="sxs-lookup"><span data-stu-id="ab31c-158">From the **Quick Links** dropdown, select one of the cluster head nodes and then select **ResourceManager Log**.</span></span>

    ![Yarn snabblänkar](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    <span data-ttu-id="ab31c-160">Visas en lista med länkar till YARN-loggar.</span><span class="sxs-lookup"><span data-stu-id="ab31c-160">You are presented with a list of links to YARN logs.</span></span>

[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
