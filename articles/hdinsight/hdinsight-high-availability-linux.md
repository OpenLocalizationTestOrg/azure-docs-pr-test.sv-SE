---
title: "aaaHigh tillgänglighet för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig mer om hur HDInsight-kluster bättre tillförlitlighet och tillgänglighet med hjälp av en ytterligare huvudnod. Lär dig hur detta påverkar Hadoop-tjänster, till exempel Ambari och Hive, samt hur tooindividually ansluta tooeach huvudnod via SSH."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
keywords: "hög tillgänglighet för hadoop"
ms.assetid: 99c9f59c-cf6b-4529-99d1-bf060435e8d4
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 07/28/2017
ms.author: larryfr
ms.openlocfilehash: 9ff62afe6b63b241cb984225233157219f8d7411
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-reliability-of-hadoop-clusters-in-hdinsight"></a><span data-ttu-id="f319a-105">Tillgänglighet och pålitlighet för Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f319a-105">Availability and reliability of Hadoop clusters in HDInsight</span></span>

<span data-ttu-id="f319a-106">HDInsight-kluster tillhandahåller två huvudnoderna tooincrease hello tillgänglighet och tillförlitlighet för Hadoop-tjänster och jobb som körs.</span><span class="sxs-lookup"><span data-stu-id="f319a-106">HDInsight clusters provide two head nodes tooincrease hello availability and reliability of Hadoop services and jobs running.</span></span>

<span data-ttu-id="f319a-107">Hadoop ger hög tillgänglighet och tillförlitlighet genom att replikera data och tjänster över flera noder i ett kluster.</span><span class="sxs-lookup"><span data-stu-id="f319a-107">Hadoop achieves high availability and reliability by replicating services and data across multiple nodes in a cluster.</span></span> <span data-ttu-id="f319a-108">Men standard Hadoop-distributioner har vanligtvis bara en huvudnod.</span><span class="sxs-lookup"><span data-stu-id="f319a-108">However standard distributions of Hadoop typically have only a single head node.</span></span> <span data-ttu-id="f319a-109">Eventuella avbrott i hello huvudnod kan orsaka hello klustret toostop fungerar.</span><span class="sxs-lookup"><span data-stu-id="f319a-109">Any outage of hello single head node can cause hello cluster toostop working.</span></span> <span data-ttu-id="f319a-110">HDInsight tillhandahåller två headnodes tooimprove Hadoop tillgänglighet och tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="f319a-110">HDInsight provides two headnodes tooimprove Hadoop's availability and reliability.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f319a-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f319a-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f319a-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f319a-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="availability-and-reliability-of-nodes"></a><span data-ttu-id="f319a-113">Tillgänglighet och tillförlitlighet för noder</span><span class="sxs-lookup"><span data-stu-id="f319a-113">Availability and reliability of nodes</span></span>

<span data-ttu-id="f319a-114">Noder i ett HDInsight-kluster implementeras med hjälp av Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="f319a-114">Nodes in an HDInsight cluster are implemented using Azure Virtual Machines.</span></span> <span data-ttu-id="f319a-115">hello beskrivs följande avsnitt enskilda hello nodtyper som används med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f319a-115">hello following sections discuss hello individual node types used with HDInsight.</span></span> 

> [!NOTE]
> <span data-ttu-id="f319a-116">Inte alla nodtyper används för en typ av kluster.</span><span class="sxs-lookup"><span data-stu-id="f319a-116">Not all node types are used for a cluster type.</span></span> <span data-ttu-id="f319a-117">Till exempel har en typ av Hadoop-kluster inte några Nimbus-noder.</span><span class="sxs-lookup"><span data-stu-id="f319a-117">For example, a Hadoop cluster type does not have any Nimbus nodes.</span></span> <span data-ttu-id="f319a-118">Mer information om noder som används av HDInsight-klustertyper i avsnittet hello klustret typer av hello [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-118">For more information on nodes used by HDInsight cluster types, see hello Cluster types section of hello [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) document.</span></span>

### <a name="head-nodes"></a><span data-ttu-id="f319a-119">HEAD-noder</span><span class="sxs-lookup"><span data-stu-id="f319a-119">Head nodes</span></span>

<span data-ttu-id="f319a-120">tooensure hög tillgänglighet för Hadoop-tjänster, HDInsight tillhandahåller två huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-120">tooensure high availability of Hadoop services, HDInsight provides two head nodes.</span></span> <span data-ttu-id="f319a-121">Båda huvudnoderna är aktiv och körs i hello HDInsight-kluster samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f319a-121">Both head nodes are active and running within hello HDInsight cluster simultaneously.</span></span> <span data-ttu-id="f319a-122">Vissa tjänster, till exempel HDFS eller YARN, är bara aktiva i en huvudnod vid en given tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="f319a-122">Some services, such as HDFS or YARN, are only 'active' on one head node at any given time.</span></span> <span data-ttu-id="f319a-123">Andra tjänster som HiveServer2 eller Hive MetaStore är aktiva på båda huvudnoderna på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="f319a-123">Other services such as HiveServer2 or Hive MetaStore are active on both head nodes at hello same time.</span></span>

<span data-ttu-id="f319a-124">HEAD-noder (och andra noder i HDInsight) har ett numeriskt värde som en del av hello värdnamnet för hello-nod.</span><span class="sxs-lookup"><span data-stu-id="f319a-124">Head nodes (and other nodes in HDInsight) have a numeric value as part of hello hostname of hello node.</span></span> <span data-ttu-id="f319a-125">Till exempel `hn0-CLUSTERNAME` eller `hn4-CLUSTERNAME`.</span><span class="sxs-lookup"><span data-stu-id="f319a-125">For example, `hn0-CLUSTERNAME` or `hn4-CLUSTERNAME`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f319a-126">Koppla inte hello numeriskt värde till om en nod är primär eller sekundär.</span><span class="sxs-lookup"><span data-stu-id="f319a-126">Do not associate hello numeric value with whether a node is primary or secondary.</span></span> <span data-ttu-id="f319a-127">hello numeriskt värde är endast tillgänglig tooprovide ett unikt namn för varje nod.</span><span class="sxs-lookup"><span data-stu-id="f319a-127">hello numeric value is only present tooprovide a unique name for each node.</span></span>

### <a name="nimbus-nodes"></a><span data-ttu-id="f319a-128">Nimbus-noder</span><span class="sxs-lookup"><span data-stu-id="f319a-128">Nimbus Nodes</span></span>

<span data-ttu-id="f319a-129">Nimbus-noder är tillgängliga med Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="f319a-129">Nimbus nodes are available with Storm clusters.</span></span> <span data-ttu-id="f319a-130">Hej Nimbus-noder ger liknande funktionalitet toohello Hadoop JobTracker genom att distribuera och övervaka bearbetning över arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="f319a-130">hello Nimbus nodes provide similar functionality toohello Hadoop JobTracker by distributing and monitoring processing across worker nodes.</span></span> <span data-ttu-id="f319a-131">HDInsight tillhandahåller två Nimbus-noder för Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="f319a-131">HDInsight provides two Nimbus nodes for Storm clusters</span></span>

### <a name="zookeeper-nodes"></a><span data-ttu-id="f319a-132">Zookeeper-noder</span><span class="sxs-lookup"><span data-stu-id="f319a-132">Zookeeper nodes</span></span>

<span data-ttu-id="f319a-133">[ZooKeeper](http://zookeeper.apache.org/) noder används för ledande val av master-tjänster på huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-133">[ZooKeeper](http://zookeeper.apache.org/) nodes are used for leader election of master services on head nodes.</span></span> <span data-ttu-id="f319a-134">De är också använda tooinsure tjänster, (worker) datanoder och gateways vet vilken huvudnod en master tjänst är aktiv på.</span><span class="sxs-lookup"><span data-stu-id="f319a-134">They are also used tooinsure that services, data (worker) nodes, and gateways know which head node a master service is active on.</span></span> <span data-ttu-id="f319a-135">Standard tillhandahåller HDInsight tre ZooKeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="f319a-135">By default, HDInsight provides three ZooKeeper nodes.</span></span>

### <a name="worker-nodes"></a><span data-ttu-id="f319a-136">Arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="f319a-136">Worker nodes</span></span>

<span data-ttu-id="f319a-137">Arbetsnoderna analysera hello faktiska data när ett jobb är skickade toohello klustret.</span><span class="sxs-lookup"><span data-stu-id="f319a-137">Worker nodes perform hello actual data analysis when a job is submitted toohello cluster.</span></span> <span data-ttu-id="f319a-138">Om en arbetsnod misslyckas är hello-aktiviteten som det fungerar skickade tooanother arbetsnoden.</span><span class="sxs-lookup"><span data-stu-id="f319a-138">If a worker node fails, hello task that it was performing is submitted tooanother worker node.</span></span> <span data-ttu-id="f319a-139">Som standard skapar HDInsight fyra arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-139">By default, HDInsight creates four worker nodes.</span></span> <span data-ttu-id="f319a-140">Du kan ändra det här antalet toosuit dina behov både under och efter klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="f319a-140">You can change this number toosuit your needs both during and after cluster creation.</span></span>

### <a name="edge-node"></a><span data-ttu-id="f319a-141">Kantnod</span><span class="sxs-lookup"><span data-stu-id="f319a-141">Edge node</span></span>

<span data-ttu-id="f319a-142">En kantnod deltar inte aktivt i dataanalys i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f319a-142">An edge node does not actively participate in data analysis within hello cluster.</span></span> <span data-ttu-id="f319a-143">Den används av utvecklare eller datavetare när du arbetar med Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f319a-143">It is used by developers or data scientists when working with Hadoop.</span></span> <span data-ttu-id="f319a-144">hello edge nod liv i hello samma virtuella Azure-nätverk som hello andra noder i klustret hello och direkt åtkomst till alla andra noder.</span><span class="sxs-lookup"><span data-stu-id="f319a-144">hello edge node lives in hello same Azure Virtual Network as hello other nodes in hello cluster, and can directly access all other nodes.</span></span> <span data-ttu-id="f319a-145">hello kantnod kan användas utan att göra resurser från kritiska Hadoop-tjänster eller analys jobb.</span><span class="sxs-lookup"><span data-stu-id="f319a-145">hello edge node can be used without taking resources away from critical Hadoop services or analysis jobs.</span></span>

<span data-ttu-id="f319a-146">R Server på HDInsight är för närvarande hello enda typ av kluster som innehåller en kantnod som standard.</span><span class="sxs-lookup"><span data-stu-id="f319a-146">Currently, R Server on HDInsight is hello only cluster type that provides an edge node by default.</span></span> <span data-ttu-id="f319a-147">R Server på HDInsight hello kantnod används testkod R lokalt på hello nod innan den skickas toohello kluster för distribuerad databehandling.</span><span class="sxs-lookup"><span data-stu-id="f319a-147">For R Server on HDInsight, hello edge node is used test R code locally on hello node before submitting it toohello cluster for distributed processing.</span></span>

<span data-ttu-id="f319a-148">Information om hur du använder en kantnod med klustertyper än R Server finns hello [använder edge-noder i HDInsight](hdinsight-apps-use-edge-node.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-148">For information on using an edge node with cluster types other than R Server, see hello [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) document.</span></span>

## <a name="accessing-hello-nodes"></a><span data-ttu-id="f319a-149">Åtkomst till hello noder</span><span class="sxs-lookup"><span data-stu-id="f319a-149">Accessing hello nodes</span></span>

<span data-ttu-id="f319a-150">Åtkomst toohello klustret via hello internet tillhandahålls via en gemensam gateway.</span><span class="sxs-lookup"><span data-stu-id="f319a-150">Access toohello cluster over hello internet is provided through a public gateway.</span></span> <span data-ttu-id="f319a-151">Hej kantnod åtkomst är begränsad tooconnecting toohello huvudnoderna och (om sådan finns).</span><span class="sxs-lookup"><span data-stu-id="f319a-151">Access is limited tooconnecting toohello head nodes and (if one exists) hello edge node.</span></span> <span data-ttu-id="f319a-152">Åtkomst tooservices körs på hello huvudnoderna genomförs inte genom att låta flera huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-152">Access tooservices running on hello head nodes is not effected by having multiple head nodes.</span></span> <span data-ttu-id="f319a-153">hello offentliga gateway vägar begäranden toohello huvudnod som är värd för hello den begärda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f319a-153">hello public gateway routes requests toohello head node that hosts hello requested service.</span></span> <span data-ttu-id="f319a-154">Om Ambari finns på hello andra huvudnod, dirigerar hello-gateway inkommande förfrågningar för Ambari toothat nod.</span><span class="sxs-lookup"><span data-stu-id="f319a-154">For example, if Ambari is currently hosted on hello secondary head node, hello gateway routes incoming requests for Ambari toothat node.</span></span>

<span data-ttu-id="f319a-155">Åtkomst via hello offentliga gateway är begränsad tooport 443 (HTTPS), 22 och 23.</span><span class="sxs-lookup"><span data-stu-id="f319a-155">Access over hello public gateway is limited tooport 443 (HTTPS), 22, and 23.</span></span>

* <span data-ttu-id="f319a-156">Port __443__ är används tooaccess Ambari och andra web Användargränssnittet eller REST API: er finns i hello head noderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-156">Port __443__ is used tooaccess Ambari and other web UI or REST APIs hosted on hello head nodes.</span></span>

* <span data-ttu-id="f319a-157">Port __22__ används tooaccess hello primära huvudnod eller kantnod med SSH.</span><span class="sxs-lookup"><span data-stu-id="f319a-157">Port __22__ is used tooaccess hello primary head node or edge node with SSH.</span></span>

* <span data-ttu-id="f319a-158">Port __23__ är används tooaccess hello andra huvudnod med SSH.</span><span class="sxs-lookup"><span data-stu-id="f319a-158">Port __23__ is used tooaccess hello secondary head node with SSH.</span></span> <span data-ttu-id="f319a-159">Till exempel `ssh username@mycluster-ssh.azurehdinsight.net` ansluter toohello primära huvudnod i hello kluster med namnet **mycluster**.</span><span class="sxs-lookup"><span data-stu-id="f319a-159">For example, `ssh username@mycluster-ssh.azurehdinsight.net` connects toohello primary head node of hello cluster named **mycluster**.</span></span>

<span data-ttu-id="f319a-160">Mer information om hur du använder SSH finns hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-160">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

### <a name="internal-fully-qualified-domain-names-fqdn"></a><span data-ttu-id="f319a-161">Internt fullständigt kvalificerade domännamn (FQDN)</span><span class="sxs-lookup"><span data-stu-id="f319a-161">Internal fully qualified domain names (FQDN)</span></span>

<span data-ttu-id="f319a-162">Noder i ett HDInsight-kluster har interna IP-adress och FQDN som endast kan nås från hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f319a-162">Nodes in an HDInsight cluster have an internal IP address and FQDN that can only be accessed from hello cluster.</span></span> <span data-ttu-id="f319a-163">Vid åtkomst till tjänster på hello-kluster med hjälp av hello interna FQDN eller IP-adress, bör du använda Ambari tooverify hello IP eller FQDN toouse vid åtkomst till hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f319a-163">When accessing services on hello cluster using hello internal FQDN or IP address, you should use Ambari tooverify hello IP or FQDN toouse when accessing hello service.</span></span>

<span data-ttu-id="f319a-164">Till exempel hello Oozie kan endast köras på en huvudnod och använder hello `oozie` hello URL toohello tjänsten krävs för kommando från en SSH-session.</span><span class="sxs-lookup"><span data-stu-id="f319a-164">For example, hello Oozie service can only run on one head node, and using hello `oozie` command from an SSH session requires hello URL toohello service.</span></span> <span data-ttu-id="f319a-165">URL: en kan hämtas från Ambari med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f319a-165">This URL can be retrieved from Ambari by using hello following command:</span></span>

    curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

<span data-ttu-id="f319a-166">Det här kommandot returnerar ett värde liknande toohello följande kommando, som innehåller hello Intern URL toouse med hello `oozie` kommando:</span><span class="sxs-lookup"><span data-stu-id="f319a-166">This command returns a value similar toohello following command, which contains hello internal URL toouse with hello `oozie` command:</span></span>

    "oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

<span data-ttu-id="f319a-167">Mer information om hur du arbetar med hello Ambari REST API finns [övervaka och hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="f319a-167">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

### <a name="accessing-other-node-types"></a><span data-ttu-id="f319a-168">Åtkomst till andra nodtyper</span><span class="sxs-lookup"><span data-stu-id="f319a-168">Accessing other node types</span></span>

<span data-ttu-id="f319a-169">Du kan ansluta toonodes som är inte direkt tillgänglig över hello internet med hjälp av hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="f319a-169">You can connect toonodes that are not directly accessible over hello internet by using hello following methods:</span></span>

* <span data-ttu-id="f319a-170">**SSH**: när du är ansluten tooa huvudnod via SSH, du kan sedan använda SSH från hello huvudnod tooconnect tooother noderna i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f319a-170">**SSH**: Once connected tooa head node using SSH, you can then use SSH from hello head node tooconnect tooother nodes in hello cluster.</span></span> <span data-ttu-id="f319a-171">Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-171">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

* <span data-ttu-id="f319a-172">**SSH-Tunnel**: Om du behöver tooaccess som en webbtjänst som finns på en av hello-noder som inte är exponerad toohello internet, måste du använda en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="f319a-172">**SSH Tunnel**: If you need tooaccess a web service hosted on one of hello nodes that is not exposed toohello internet, you must use an SSH tunnel.</span></span> <span data-ttu-id="f319a-173">Mer information finns i hello [använder en SSH-tunnel med HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-173">For more information, see hello [Use an SSH tunnel with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

* <span data-ttu-id="f319a-174">**Virtuella Azure-nätverket**: om din HDInsight klustret är en del av ett virtuellt Azure-nätverk, en resurs på hello samma virtuella nätverk direkt åtkomst till alla noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="f319a-174">**Azure Virtual Network**: If your HDInsight cluster is part of an Azure Virtual Network, any resource on hello same Virtual Network can directly access all nodes in hello cluster.</span></span> <span data-ttu-id="f319a-175">Mer information finns i hello [utöka HDInsight med hjälp av Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-175">For more information, see hello [Extend HDInsight using Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md) document.</span></span>

## <a name="how-toocheck-on-a-service-status"></a><span data-ttu-id="f319a-176">Hur toocheck på en Tjänststatus</span><span class="sxs-lookup"><span data-stu-id="f319a-176">How toocheck on a service status</span></span>

<span data-ttu-id="f319a-177">toocheck hello status för tjänster som körs på hello huvudnoderna kan använda hello Ambari-Webbgränssnittet eller hello Ambari REST API.</span><span class="sxs-lookup"><span data-stu-id="f319a-177">toocheck hello status of services that run on hello head nodes, use hello Ambari Web UI or hello Ambari REST API.</span></span>

### <a name="ambari-web-ui"></a><span data-ttu-id="f319a-178">Ambari-webbgränssnittet</span><span class="sxs-lookup"><span data-stu-id="f319a-178">Ambari Web UI</span></span>

<span data-ttu-id="f319a-179">Hej Ambari-Webbgränssnittet kan visas på https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="f319a-179">hello Ambari Web UI is viewable at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="f319a-180">Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f319a-180">Replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="f319a-181">Om du uppmanas ange hello HTTP-autentiseringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="f319a-181">If prompted, enter hello HTTP user credentials for your cluster.</span></span> <span data-ttu-id="f319a-182">hello Standardanvändarnamnet för HTTP är **admin** och hello lösenordet är hello lösenordet du angav när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="f319a-182">hello default HTTP user name is **admin** and hello password is hello password you entered when creating hello cluster.</span></span>

<span data-ttu-id="f319a-183">När du kommer på hello Ambari sidan tjänsterna hello installerade hello till vänster i hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="f319a-183">When you arrive on hello Ambari page, hello installed services are listed on hello left of hello page.</span></span>

![Installerade tjänster](./media/hdinsight-high-availability-linux/services.png)

<span data-ttu-id="f319a-185">Det finns ett antal ikoner som kan visas nästa tooa tooindicate Tjänststatus.</span><span class="sxs-lookup"><span data-stu-id="f319a-185">There are a series of icons that may appear next tooa service tooindicate status.</span></span> <span data-ttu-id="f319a-186">Alla aviseringar relaterade tooa service kan granskas med hello **aviseringar** länk hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="f319a-186">Any alerts related tooa service can be viewed using hello **Alerts** link at hello top of hello page.</span></span> <span data-ttu-id="f319a-187">Du kan markera varje tjänst tooview mer information om den.</span><span class="sxs-lookup"><span data-stu-id="f319a-187">You can select each service tooview more information on it.</span></span>

<span data-ttu-id="f319a-188">Hello sida innehåller information om hello status och konfiguration för varje tjänst, innehåller den inte information som huvudnod hello-tjänsten körs på.</span><span class="sxs-lookup"><span data-stu-id="f319a-188">While hello service page provides information on hello status and configuration of each service, it does not provide information on which head node hello service is running on.</span></span> <span data-ttu-id="f319a-189">tooview denna information, Använd hello **värdar** länk hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="f319a-189">tooview this information, use hello **Hosts** link at hello top of hello page.</span></span> <span data-ttu-id="f319a-190">Den här sidan visar värdar inom hello klustret, inklusive hello huvudnoderna.</span><span class="sxs-lookup"><span data-stu-id="f319a-190">This page displays hosts within hello cluster, including hello head nodes.</span></span>

![lista över värdar](./media/hdinsight-high-availability-linux/hosts.png)

<span data-ttu-id="f319a-192">Du väljer hello länk för en hello huvudnoderna visar hello tjänster och komponenter som körs på noden.</span><span class="sxs-lookup"><span data-stu-id="f319a-192">Selecting hello link for one of hello head nodes displays hello services and components running on that node.</span></span>

![Komponentstatus](./media/hdinsight-high-availability-linux/nodeservices.png)

<span data-ttu-id="f319a-194">Läs mer om hur du använder Ambari [övervaka och hantera HDInsight med Ambari-Webbgränssnittet för hello](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="f319a-194">For more information on using Ambari, see [Monitor and manage HDInsight using hello Ambari Web UI](hdinsight-hadoop-manage-ambari.md).</span></span>

### <a name="ambari-rest-api"></a><span data-ttu-id="f319a-195">Ambari REST API</span><span class="sxs-lookup"><span data-stu-id="f319a-195">Ambari REST API</span></span>

<span data-ttu-id="f319a-196">Hej Ambari REST API är tillgänglig över hello internet.</span><span class="sxs-lookup"><span data-stu-id="f319a-196">hello Ambari REST API is available over hello internet.</span></span> <span data-ttu-id="f319a-197">Hej HDInsight offentliga gateway hanterar routning begäranden toohello huvudnod som för närvarande är värd hello REST API.</span><span class="sxs-lookup"><span data-stu-id="f319a-197">hello HDInsight public gateway handles routing requests toohello head node that is currently hosting hello REST API.</span></span>

<span data-ttu-id="f319a-198">Du kan använda följande kommando toocheck hello tillstånd för en tjänst via hello Ambari REST API hello:</span><span class="sxs-lookup"><span data-stu-id="f319a-198">You can use hello following command toocheck hello state of a service through hello Ambari REST API:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* <span data-ttu-id="f319a-199">Ersätt **lösenord** med hello HTTP (admin) lösenord.</span><span class="sxs-lookup"><span data-stu-id="f319a-199">Replace **PASSWORD** with hello HTTP user (admin) account password.</span></span>
* <span data-ttu-id="f319a-200">Ersätt **KLUSTERNAMN** med hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="f319a-200">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
* <span data-ttu-id="f319a-201">Ersätt **SERVICENAME** hello namnet på Hej tjänst du vill toocheck hello status.</span><span class="sxs-lookup"><span data-stu-id="f319a-201">Replace **SERVICENAME** with hello name of hello service you want toocheck hello status of.</span></span>

<span data-ttu-id="f319a-202">Till exempel toocheck hello status för hello **HDFS** på ett kluster med namnet **mycluster**, med ett lösenord för **lösenord**, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="f319a-202">For example, toocheck hello status of hello **HDFS** service on a cluster named **mycluster**, with a password of **password**, you would use hello following command:</span></span>

    curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

<span data-ttu-id="f319a-203">hello svaret är liknande toohello följande JSON:</span><span class="sxs-lookup"><span data-stu-id="f319a-203">hello response is similar toohello following JSON:</span></span>

    {
      "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
      "ServiceInfo" : {
        "cluster_name" : "mycluster",
        "service_name" : "HDFS",
        "state" : "STARTED"
      }
    }

<span data-ttu-id="f319a-204">hello URL talar om för oss att hello-tjänsten körs på en huvudnod med namnet **hn0 CLUSTERNAME**.</span><span class="sxs-lookup"><span data-stu-id="f319a-204">hello URL tells us that hello service is currently running on a head node named **hn0-CLUSTERNAME**.</span></span>

<span data-ttu-id="f319a-205">hello tillstånd talar om för oss att hello-tjänsten körs för närvarande, eller **igång**.</span><span class="sxs-lookup"><span data-stu-id="f319a-205">hello state tells us that hello service is currently running, or **STARTED**.</span></span>

<span data-ttu-id="f319a-206">Om du inte vet vilka tjänster som är installerade på hello klustret använder du hello efter kommandot tooretrieve en lista:</span><span class="sxs-lookup"><span data-stu-id="f319a-206">If you do not know what services are installed on hello cluster, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

<span data-ttu-id="f319a-207">Mer information om hur du arbetar med hello Ambari REST API finns [övervaka och hantera HDInsight med hjälp av hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span><span class="sxs-lookup"><span data-stu-id="f319a-207">For more information on working with hello Ambari REST API, see [Monitor and Manage HDInsight using hello Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md).</span></span>

#### <a name="service-components"></a><span data-ttu-id="f319a-208">Tjänstens komponenter</span><span class="sxs-lookup"><span data-stu-id="f319a-208">Service components</span></span>

<span data-ttu-id="f319a-209">Tjänsterna kan innehålla komponenter som du vill toocheck hello status för individuellt.</span><span class="sxs-lookup"><span data-stu-id="f319a-209">Services may contain components that you wish toocheck hello status of individually.</span></span> <span data-ttu-id="f319a-210">HDFS innehåller till exempel hello NameNode komponent.</span><span class="sxs-lookup"><span data-stu-id="f319a-210">For example, HDFS contains hello NameNode component.</span></span> <span data-ttu-id="f319a-211">tooview information om en komponent, hello skulle kommandot se ut:</span><span class="sxs-lookup"><span data-stu-id="f319a-211">tooview information on a component, hello command would be:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

<span data-ttu-id="f319a-212">Om du inte vet vilka komponenter som tillhandahålls av en tjänst kan använda du hello efter kommandot tooretrieve en lista:</span><span class="sxs-lookup"><span data-stu-id="f319a-212">If you do not know what components are provided by a service, you can use hello following command tooretrieve a list:</span></span>

    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

## <a name="how-tooaccess-log-files-on-hello-head-nodes"></a><span data-ttu-id="f319a-213">Hur tooaccess loggfiler på hello huvudnoderna</span><span class="sxs-lookup"><span data-stu-id="f319a-213">How tooaccess log files on hello head nodes</span></span>

### <a name="ssh"></a><span data-ttu-id="f319a-214">SSH</span><span class="sxs-lookup"><span data-stu-id="f319a-214">SSH</span></span>

<span data-ttu-id="f319a-215">När anslutna tooa huvudnod via SSH loggfiler finns under **/var/log**.</span><span class="sxs-lookup"><span data-stu-id="f319a-215">While connected tooa head node through SSH, log files can be found under **/var/log**.</span></span> <span data-ttu-id="f319a-216">Till exempel **/var/log/hadoop-yarn/yarn** innehåller loggarna för YARN.</span><span class="sxs-lookup"><span data-stu-id="f319a-216">For example, **/var/log/hadoop-yarn/yarn** contain logs for YARN.</span></span>

<span data-ttu-id="f319a-217">Varje huvudnod kan ha unika loggposter, så du bör kontrollera hello loggar på båda.</span><span class="sxs-lookup"><span data-stu-id="f319a-217">Each head node can have unique log entries, so you should check hello logs on both.</span></span>

### <a name="sftp"></a><span data-ttu-id="f319a-218">SFTP</span><span class="sxs-lookup"><span data-stu-id="f319a-218">SFTP</span></span>

<span data-ttu-id="f319a-219">Du kan också ansluta toohello huvudnod använder hello SSH File Transfer Protocol eller SFTP Secure File Transfer Protocol () och hämta hello loggfiler direkt.</span><span class="sxs-lookup"><span data-stu-id="f319a-219">You can also connect toohello head node using hello SSH File Transfer Protocol or Secure File Transfer Protocol (SFTP), and download hello log files directly.</span></span>

<span data-ttu-id="f319a-220">Liknande toousing en SSH-klienten, när du ansluter toohello klustret måste du ange hello SSH användarens konto namn och hello SSH-adressen för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="f319a-220">Similar toousing an SSH client, when connecting toohello cluster you must provide hello SSH user account name and hello SSH address of hello cluster.</span></span> <span data-ttu-id="f319a-221">Till exempel `sftp username@mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="f319a-221">For example, `sftp username@mycluster-ssh.azurehdinsight.net`.</span></span> <span data-ttu-id="f319a-222">Ange hello lösenord för hello kontot när du uppmanas, eller ange en offentlig nyckel med hjälp av hello `-i` parameter.</span><span class="sxs-lookup"><span data-stu-id="f319a-222">Provide hello password for hello account when prompted, or provide a public key using hello `-i` parameter.</span></span>

<span data-ttu-id="f319a-223">När du är ansluten, visas en `sftp>` prompt.</span><span class="sxs-lookup"><span data-stu-id="f319a-223">Once connected, you are presented with a `sftp>` prompt.</span></span> <span data-ttu-id="f319a-224">I det här meddelandet kan du ändra kataloger, ladda upp och hämta filer.</span><span class="sxs-lookup"><span data-stu-id="f319a-224">From this prompt, you can change directories, upload, and download files.</span></span> <span data-ttu-id="f319a-225">Till exempel följande kommandon hello ändra kataloger toohello **/var/log/hadoop/hdfs** directory och sedan ladda ned alla filer i hello directory.</span><span class="sxs-lookup"><span data-stu-id="f319a-225">For example, hello following commands change directories toohello **/var/log/hadoop/hdfs** directory and then download all files in hello directory.</span></span>

    cd /var/log/hadoop/hdfs
    get *

<span data-ttu-id="f319a-226">En lista över tillgängliga kommandon, ange `help` på hello `sftp>` prompt.</span><span class="sxs-lookup"><span data-stu-id="f319a-226">For a list of available commands, enter `help` at hello `sftp>` prompt.</span></span>

> [!NOTE]
> <span data-ttu-id="f319a-227">Det finns också grafiska gränssnitt som gör att du toovisualize hello-filsystem när ansluten via SFTP.</span><span class="sxs-lookup"><span data-stu-id="f319a-227">There are also graphical interfaces that allow you toovisualize hello file system when connected using SFTP.</span></span> <span data-ttu-id="f319a-228">Till exempel [MobaXTerm](http://mobaxterm.mobatek.net/) kan du toobrowse hello file system med en liknande gränssnittet tooWindows Explorer.</span><span class="sxs-lookup"><span data-stu-id="f319a-228">For example, [MobaXTerm](http://mobaxterm.mobatek.net/) allows you toobrowse hello file system using an interface similar tooWindows Explorer.</span></span>

### <a name="ambari"></a><span data-ttu-id="f319a-229">Ambari</span><span class="sxs-lookup"><span data-stu-id="f319a-229">Ambari</span></span>

> [!NOTE]
> <span data-ttu-id="f319a-230">tooaccess loggfiler med Ambari måste du använda en SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="f319a-230">tooaccess log files using Ambari, you must use an SSH tunnel.</span></span> <span data-ttu-id="f319a-231">hello Webbgränssnitt för enskilda hello-tjänster visas inte offentligt på hello Internet.</span><span class="sxs-lookup"><span data-stu-id="f319a-231">hello web interfaces for hello individual services are not exposed publicly on hello Internet.</span></span> <span data-ttu-id="f319a-232">Information om hur du använder en SSH-tunnel finns hello [använda SSH-tunnlar](hdinsight-linux-ambari-ssh-tunnel.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-232">For information on using an SSH tunnel, see hello [Use SSH Tunneling](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="f319a-233">Hello Ambari-Webbgränssnittet, Välj hello-tjänsten som du vill tooview loggar för (till exempel YARN).</span><span class="sxs-lookup"><span data-stu-id="f319a-233">From hello Ambari Web UI, select hello service you wish tooview logs for (for example, YARN).</span></span> <span data-ttu-id="f319a-234">Använd sedan **snabblänkar** tooselect vilka huvudnod tooview hello-loggarna för.</span><span class="sxs-lookup"><span data-stu-id="f319a-234">Then use **Quick Links** tooselect which head node tooview hello logs for.</span></span>

![Med hjälp av snabb länkar tooview loggar](./media/hdinsight-high-availability-linux/viewlogs.png)

## <a name="how-tooconfigure-hello-node-size"></a><span data-ttu-id="f319a-236">Hur tooconfigure hello nodstorlek</span><span class="sxs-lookup"><span data-stu-id="f319a-236">How tooconfigure hello node size</span></span>

<span data-ttu-id="f319a-237">hello storleken på en nod kan endast väljas när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="f319a-237">hello size of a node can only be selected during cluster creation.</span></span> <span data-ttu-id="f319a-238">Du hittar en lista över hello annan VM-storlekar som finns tillgängliga för HDInsight på hello [HDInsight sida med priser](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="f319a-238">You can find a list of hello different VM sizes available for HDInsight on hello [HDInsight pricing page](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

<span data-ttu-id="f319a-239">När du skapar ett kluster måste ange du hello storleken på hello noder.</span><span class="sxs-lookup"><span data-stu-id="f319a-239">When creating a cluster, you can specify hello size of hello nodes.</span></span> <span data-ttu-id="f319a-240">hello följande information innehåller information om hur toospecify hello storlek med hello [Azure-portalen][preview-portal], [Azure PowerShell][azure-powershell], och hello [Azure CLI][azure-cli]:</span><span class="sxs-lookup"><span data-stu-id="f319a-240">hello following information provides guidance on how toospecify hello size using hello [Azure portal][preview-portal], [Azure PowerShell][azure-powershell], and hello [Azure CLI][azure-cli]:</span></span>

* <span data-ttu-id="f319a-241">**Azure-portalen**: när du skapar ett kluster måste du ange hello storleken på hello-noder som används av hello klustret:</span><span class="sxs-lookup"><span data-stu-id="f319a-241">**Azure portal**: When creating a cluster, you can set hello size of hello nodes used by hello cluster:</span></span>

    ![Bild av guiden för kluster skapas med val av storlek](./media/hdinsight-high-availability-linux/headnodesize.png)

* <span data-ttu-id="f319a-243">**Azure CLI**: när du använder hello `azure hdinsight cluster create` kommandot kan du ange hello storleken på hello head, arbetare och ZooKeeper-noder med hello `--headNodeSize`, `--workerNodeSize`, och `--zookeeperNodeSize` parametrar.</span><span class="sxs-lookup"><span data-stu-id="f319a-243">**Azure CLI**: When using hello `azure hdinsight cluster create` command, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `--headNodeSize`, `--workerNodeSize`, and `--zookeeperNodeSize` parameters.</span></span>

* <span data-ttu-id="f319a-244">**Azure PowerShell**: när du använder hello `New-AzureRmHDInsightCluster` cmdlet, kan du ange hello storleken på hello head, arbetare och ZooKeeper-noder med hello `-HeadNodeVMSize`, `-WorkerNodeSize`, och `-ZookeeperNodeSize` parametrar.</span><span class="sxs-lookup"><span data-stu-id="f319a-244">**Azure PowerShell**: When using hello `New-AzureRmHDInsightCluster` cmdlet, you can set hello size of hello head, worker, and ZooKeeper nodes by using hello `-HeadNodeVMSize`, `-WorkerNodeSize`, and `-ZookeeperNodeSize` parameters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f319a-245">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f319a-245">Next steps</span></span>

<span data-ttu-id="f319a-246">Använd hello följande länkar toolearn mer om vad som nämns i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f319a-246">Use hello following links toolearn more about things mentioned in this document.</span></span>

* [<span data-ttu-id="f319a-247">Ambari REST-referens</span><span class="sxs-lookup"><span data-stu-id="f319a-247">Ambari REST Reference</span></span>](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)
* [<span data-ttu-id="f319a-248">Installera och konfigurera hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f319a-248">Install and configure hello Azure CLI</span></span>](../cli-install-nodejs.md)
* <span data-ttu-id="f319a-249">Installera och konfigurera [Azure PowerShell](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="f319a-249">[Install and configure Azure PowerShell](/powershell/azure/overview)</span></span>
* [<span data-ttu-id="f319a-250">Hantera HDInsight med Ambari</span><span class="sxs-lookup"><span data-stu-id="f319a-250">Manage HDInsight using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)
* [<span data-ttu-id="f319a-251">Etablera Linux-baserade HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="f319a-251">Provision Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: /powershell/azureps-cmdlets-docs
[azure-cli]: ../cli-install-nodejs.md
