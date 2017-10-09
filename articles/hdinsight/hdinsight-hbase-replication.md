---
title: "aaaConfigure HBase-kluster-replikering i virtuella nätverk - Azure | Microsoft Docs"
description: "Lär dig hur tooconfigure HBase-replikering för belastningsutjämning, hög tillgänglighet, noll avbrottstid migrering/uppdatera från en HDInsight version tooanother och katastrofåterställning."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: ba1c44f26b7cbf4a7a88159b12b3e064ea9f9a20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hbase-cluster-replication-within-virtual-networks"></a><span data-ttu-id="3db78-103">Konfigurera HBase-kluster-replikering i virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="3db78-103">Configure HBase cluster replication within virtual networks</span></span>

<span data-ttu-id="3db78-104">Lär dig hur tooconfigure HBase-replikering i ett virtuellt nätverk (VNet) eller mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3db78-104">Learn how tooconfigure HBase replication within one virtual network (VNet) or between two virtual networks.</span></span>

<span data-ttu-id="3db78-105">Kluster-replikering använder en käll-push-metod.</span><span class="sxs-lookup"><span data-stu-id="3db78-105">Cluster replication uses a source-push methodology.</span></span> <span data-ttu-id="3db78-106">Ett HBase-kluster kan vara en käll- eller ett mål eller det kan uppfylla båda roller på samma gång.</span><span class="sxs-lookup"><span data-stu-id="3db78-106">An HBase cluster can be a source or a destination, or it can fulfill both roles at once.</span></span> <span data-ttu-id="3db78-107">Replikering är asynkron och hello målet för replikering är slutliga konsekvensen.</span><span class="sxs-lookup"><span data-stu-id="3db78-107">Replication is asynchronous, and hello goal of replication is eventual consistency.</span></span> <span data-ttu-id="3db78-108">När hello källa tar emot en redigera tooa kolumnfamilj med replikering aktiverad, är att redigera spridda tooall målkluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-108">When hello source receives an edit tooa column family with replication enabled, that edit is propagated tooall destination clusters.</span></span> <span data-ttu-id="3db78-109">När data har replikerats från ett kluster tooanother är hello källa kluster och alla kluster som redan förbrukats hello data spårade tooprevent replikering slingor.</span><span class="sxs-lookup"><span data-stu-id="3db78-109">When data is replicated from one cluster tooanother, hello source cluster and all clusters that have already consumed hello data are tracked tooprevent replication loops.</span></span>

<span data-ttu-id="3db78-110">I den här självstudiekursen konfigurerar du en källa mål-replikeringen.</span><span class="sxs-lookup"><span data-stu-id="3db78-110">In this tutorial, you will configure a source-destination replication.</span></span> <span data-ttu-id="3db78-111">Andra klustertopologier finns [referenshandbok för Apache HBase](http://hbase.apache.org/book.html#_cluster_replication).</span><span class="sxs-lookup"><span data-stu-id="3db78-111">For other cluster topologies, see [Apache HBase Reference Guide](http://hbase.apache.org/book.html#_cluster_replication).</span></span>

<span data-ttu-id="3db78-112">HBase replikering användning ärenden för ett enda virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="3db78-112">HBase replication usage cases for a single virtual network:</span></span>

* <span data-ttu-id="3db78-113">Belastningsutjämning – till exempel kör skanningar eller MapReduce-jobb på hello målklustret och vill föra in data på hello källklustret</span><span class="sxs-lookup"><span data-stu-id="3db78-113">Load balancing--for example, running scans or MapReduce jobs on hello destination cluster and ingesting data on hello source cluster</span></span>
* <span data-ttu-id="3db78-114">Hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="3db78-114">High availability</span></span>
* <span data-ttu-id="3db78-115">Migrera data från en HBase-kluster tooanother</span><span class="sxs-lookup"><span data-stu-id="3db78-115">Migrating data from one HBase cluster tooanother</span></span>
* <span data-ttu-id="3db78-116">Uppgradera en Azure HDInsight-kluster från en version tooanother</span><span class="sxs-lookup"><span data-stu-id="3db78-116">Upgrading an Azure HDInsight cluster from one version tooanother</span></span>

<span data-ttu-id="3db78-117">HBase replikering användning fall för två virtuella nätverk:</span><span class="sxs-lookup"><span data-stu-id="3db78-117">HBase replication usage cases for two virtual networks:</span></span>

* <span data-ttu-id="3db78-118">Haveriberedskap</span><span class="sxs-lookup"><span data-stu-id="3db78-118">Disaster recovery</span></span>
* <span data-ttu-id="3db78-119">Belastningsutjämning och partitionering hello program</span><span class="sxs-lookup"><span data-stu-id="3db78-119">Load balancing and partitioning hello application</span></span>
* <span data-ttu-id="3db78-120">Hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="3db78-120">High availability</span></span>

<span data-ttu-id="3db78-121">Du replikerar kluster med [skript åtgärd](hdinsight-hadoop-customize-cluster-linux.md) skript som finns i [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span><span class="sxs-lookup"><span data-stu-id="3db78-121">You replicate clusters by using [script action](hdinsight-hadoop-customize-cluster-linux.md) scripts located at [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3db78-122">Krav</span><span class="sxs-lookup"><span data-stu-id="3db78-122">Prerequisites</span></span>
<span data-ttu-id="3db78-123">Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3db78-123">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="3db78-124">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="3db78-124">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

## <a name="configure-hello-environments"></a><span data-ttu-id="3db78-125">Konfigurera hello-miljöer</span><span class="sxs-lookup"><span data-stu-id="3db78-125">Configure hello environments</span></span>

<span data-ttu-id="3db78-126">Det finns tre möjliga konfigurationer:</span><span class="sxs-lookup"><span data-stu-id="3db78-126">There are three possible configurations:</span></span>

- <span data-ttu-id="3db78-127">Två HBase-kluster i ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="3db78-127">Two HBase clusters in one Azure virtual network</span></span>
- <span data-ttu-id="3db78-128">Två HBase-kluster i två olika virtuella nätverk i hello samma region</span><span class="sxs-lookup"><span data-stu-id="3db78-128">Two HBase clusters in two different virtual networks in hello same region</span></span>
- <span data-ttu-id="3db78-129">Två HBase-kluster i två olika virtuella nätverk i två olika regioner (geo-replikering)</span><span class="sxs-lookup"><span data-stu-id="3db78-129">Two HBase clusters in two different virtual networks in two different regions (geo-replication)</span></span>

<span data-ttu-id="3db78-130">toomake den enklare tooconfigure hello-miljöer, har vi skapat några [Azure Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3db78-130">toomake it easier tooconfigure hello environments, we have created some [Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="3db78-131">Om du föredrar tooconfigure hello miljöer genom att använda andra metoder, se:</span><span class="sxs-lookup"><span data-stu-id="3db78-131">If you prefer tooconfigure hello environments by using other methods, see:</span></span>

- [<span data-ttu-id="3db78-132">Skapa Linux-baserade Hadoop-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3db78-132">Create Linux-based Hadoop clusters in HDInsight</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
- [<span data-ttu-id="3db78-133">Skapa HBase-kluster i Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="3db78-133">Create HBase clusters in Azure Virtual Network</span></span>](hdinsight-hbase-provision-vnet.md)

### <a name="configure-one-virtual-network"></a><span data-ttu-id="3db78-134">Konfigurera ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="3db78-134">Configure one virtual network</span></span>

<span data-ttu-id="3db78-135">Klicka på följande bild toocreate två HBase-kluster i hello hello samma virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3db78-135">Click hello following image toocreate two HBase clusters in hello same virtual network.</span></span> <span data-ttu-id="3db78-136">hello mallen lagras i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span><span class="sxs-lookup"><span data-stu-id="3db78-136">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

### <a name="configure-two-virtual-networks-in-hello-same-region"></a><span data-ttu-id="3db78-137">Konfigurera två virtuella nätverk i hello samma region</span><span class="sxs-lookup"><span data-stu-id="3db78-137">Configure two virtual networks in hello same region</span></span>

<span data-ttu-id="3db78-138">Klicka på följande bild toocreate två virtuella nätverk med VNet-peering och två HBase-kluster i hello hello samma region.</span><span class="sxs-lookup"><span data-stu-id="3db78-138">Click hello following image toocreate two virtual networks with VNet peering and two HBase clusters in hello same region.</span></span> <span data-ttu-id="3db78-139">hello mallen lagras i [Azure QuickStart mallar](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span><span class="sxs-lookup"><span data-stu-id="3db78-139">hello template is stored in [Azure QuickStart Templates](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/).</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>



<span data-ttu-id="3db78-140">Det här scenariot kräver [VNet-peering](../virtual-network/virtual-network-peering-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3db78-140">This scenario requires [VNet peering](../virtual-network/virtual-network-peering-overview.md).</span></span> <span data-ttu-id="3db78-141">hello mallen kan VNet-peering.</span><span class="sxs-lookup"><span data-stu-id="3db78-141">hello template enables VNet peering.</span></span>   

<span data-ttu-id="3db78-142">HBase-replikering använder IP-adresser för hello ZooKeeper virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3db78-142">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="3db78-143">Du måste konfigurera statiska IP-adresser för hello mål HBase ZooKeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="3db78-143">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span>

<span data-ttu-id="3db78-144">**tooconfigure statiska IP-adresser**</span><span class="sxs-lookup"><span data-stu-id="3db78-144">**tooconfigure static IP addresses**</span></span>

1. <span data-ttu-id="3db78-145">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3db78-145">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3db78-146">Hello vänstra menyn klickar du på **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="3db78-146">From hello left menu, click **Resource Groups**.</span></span>
3. <span data-ttu-id="3db78-147">Klicka på resursgruppen som innehåller hello mål HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-147">Click your resource group that contains hello destination HBase cluster.</span></span> <span data-ttu-id="3db78-148">Detta är hello resursgrupp som du angav när du använde hello Resource Manager-mall toocreate hello miljö.</span><span class="sxs-lookup"><span data-stu-id="3db78-148">This is hello resource group that you specified when you used hello Resource Manager template toocreate hello environment.</span></span> <span data-ttu-id="3db78-149">Du kan använda hello filter toonarrow hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="3db78-149">You can use hello filter toonarrow down hello list.</span></span> <span data-ttu-id="3db78-150">Du kan se en lista över resurser som innehåller hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3db78-150">You can see a list of resources that contains hello two virtual networks.</span></span>
4. <span data-ttu-id="3db78-151">Klicka på hello virtuella nätverk som innehåller hello mål HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-151">Click hello virtual network that contains hello destination HBase cluster.</span></span> <span data-ttu-id="3db78-152">Klicka till exempel **xxxx vnet2**.</span><span class="sxs-lookup"><span data-stu-id="3db78-152">For example, click **xxxx-vnet2**.</span></span> <span data-ttu-id="3db78-153">Du kan se tre enheter med namn som börjar med **nic-zookeepermode -**.</span><span class="sxs-lookup"><span data-stu-id="3db78-153">You can see three devices with names that start with **nic-zookeepermode-**.</span></span> <span data-ttu-id="3db78-154">Dessa enheter är hello tre ZooKeeper virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3db78-154">Those devices are hello three ZooKeeper VMs.</span></span>
5. <span data-ttu-id="3db78-155">Klicka på en hello ZooKeeper virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3db78-155">Click one of hello ZooKeeper VMs.</span></span>
6. <span data-ttu-id="3db78-156">Klicka på **IP-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="3db78-156">Click **IP configurations**.</span></span>
7. <span data-ttu-id="3db78-157">Klicka på **ipConfig1** hello-listan.</span><span class="sxs-lookup"><span data-stu-id="3db78-157">Click **ipConfig1** from hello list.</span></span>
8. <span data-ttu-id="3db78-158">Klicka på **Statiska**, och Skriv ned hello IP-adress.</span><span class="sxs-lookup"><span data-stu-id="3db78-158">Click **Static**, and write down hello actual IP address.</span></span> <span data-ttu-id="3db78-159">Du behöver hello IP-adress när du kör hello åtgärd tooenable replikering.</span><span class="sxs-lookup"><span data-stu-id="3db78-159">You will need hello IP address when you run hello script action tooenable replication.</span></span>

  ![HDInsight HBase replikering ZooKeeper statisk IP-adress](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. <span data-ttu-id="3db78-161">Upprepa steg 6 tooset hello statisk IP-adress för hello andra två ZooKeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="3db78-161">Repeat step 6 tooset hello static IP address for hello other two ZooKeeper nodes.</span></span>

<span data-ttu-id="3db78-162">För hello mellan VNet-scenario, måste du använda hello **- IP-** växeln när du anropar hello **hdi_enable_replication.sh** skript åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3db78-162">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

### <a name="configure-two-virtual-networks-in-two-different-regions"></a><span data-ttu-id="3db78-163">Konfigurera två virtuella nätverk i två olika regioner</span><span class="sxs-lookup"><span data-stu-id="3db78-163">Configure two virtual networks in two different regions</span></span>

<span data-ttu-id="3db78-164">Klicka på hello följande bild toocreate två virtuella nätverk i två olika regioner.</span><span class="sxs-lookup"><span data-stu-id="3db78-164">Click hello following image toocreate two virtual networks in two different regions.</span></span> <span data-ttu-id="3db78-165">hello mallen lagras i en offentlig Azure Blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="3db78-165">hello template is stored in a public Azure Blob container.</span></span>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy tooAzure"></a>

<span data-ttu-id="3db78-166">Skapa en VPN-gateway mellan hello två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3db78-166">Create a VPN gateway between hello two virtual networks.</span></span> <span data-ttu-id="3db78-167">Instruktioner finns i [skapa ett VNet med en plats-till-plats-anslutning](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3db78-167">For instructions, see [Create a VNet with a site-to-site connection](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).</span></span>

<span data-ttu-id="3db78-168">HBase-replikering använder IP-adresser för hello ZooKeeper virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3db78-168">HBase replication uses IP addresses of hello ZooKeeper VMs.</span></span> <span data-ttu-id="3db78-169">Du måste konfigurera statiska IP-adresser för hello mål HBase ZooKeeper-noder.</span><span class="sxs-lookup"><span data-stu-id="3db78-169">You must configure static IP addresses for hello destination HBase ZooKeeper nodes.</span></span> <span data-ttu-id="3db78-170">tooconfigure statisk IP-adress, finns hello ”konfigurera två virtuella nätverk i hello samma region” i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="3db78-170">tooconfigure static IP, see hello "Configure two virtual networks in hello same region" section in this article.</span></span>

<span data-ttu-id="3db78-171">För hello mellan VNet-scenario, måste du använda hello **- IP-** växeln när du anropar hello **hdi_enable_replication.sh** skript åtgärd.</span><span class="sxs-lookup"><span data-stu-id="3db78-171">For hello cross-VNet scenario, you must use hello **-ip** switch when calling hello **hdi_enable_replication.sh** script action.</span></span>

## <a name="load-test-data"></a><span data-ttu-id="3db78-172">Läs in testdata</span><span class="sxs-lookup"><span data-stu-id="3db78-172">Load test data</span></span>

<span data-ttu-id="3db78-173">När du replikerar ett kluster måste du ange hello tabeller tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="3db78-173">When you replicate a cluster, you must specify hello tables tooreplicate.</span></span> <span data-ttu-id="3db78-174">I det här avsnittet ska du läsa in vissa data i hello källklustret.</span><span class="sxs-lookup"><span data-stu-id="3db78-174">In this section, you will load some data into hello source cluster.</span></span> <span data-ttu-id="3db78-175">I nästa avsnitt om hello aktiverar du replikering mellan två kluster med hello.</span><span class="sxs-lookup"><span data-stu-id="3db78-175">In hello next section, you will enable replication between hello two clusters.</span></span>

<span data-ttu-id="3db78-176">Följ anvisningarna hello i [självstudier för HBase: komma igång med Apache HBase med Linux-baserade Hadoop i HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate en **kontakter** table och infoga data i hello-tabellen.</span><span class="sxs-lookup"><span data-stu-id="3db78-176">Follow hello instructions in [HBase tutorial: Get started using Apache HBase with Linux-based Hadoop in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md) toocreate a **Contacts** table and insert some data into hello table.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="3db78-177">Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="3db78-177">Enable replication</span></span>

<span data-ttu-id="3db78-178">hello följande steg visar hur toocall hello skriptet åtgärdsskriptet från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3db78-178">hello following steps show how toocall hello script action script from hello Azure portal.</span></span> <span data-ttu-id="3db78-179">Kör en skriptåtgärd med hjälp av Azure PowerShell och hello Azure-kommandoradsgränssnittet (CLI), finns i [anpassa Linux-baserade HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="3db78-179">For running a script action by using Azure PowerShell and hello Azure command-line interface (CLI), see [Customize Linux-based HDInsight clusters using script action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

<span data-ttu-id="3db78-180">**tooenable HBase-replikering från hello Azure-portalen**</span><span class="sxs-lookup"><span data-stu-id="3db78-180">**tooenable HBase replication from hello Azure portal**</span></span>

1. <span data-ttu-id="3db78-181">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3db78-181">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3db78-182">Öppna hello källa HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-182">Open hello source HBase cluster.</span></span>
3. <span data-ttu-id="3db78-183">Hello kluster-menyn, klickar du på **skriptåtgärder**.</span><span class="sxs-lookup"><span data-stu-id="3db78-183">From hello cluster menu, click **Script Actions**.</span></span>
4. <span data-ttu-id="3db78-184">Klicka på **skicka nya** från hello överkant hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="3db78-184">Click **Submit New** from hello top of hello blade.</span></span>
5. <span data-ttu-id="3db78-185">Välj eller ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="3db78-185">Select or enter hello following information:</span></span>

  - <span data-ttu-id="3db78-186">**Namnet**: Ange **Aktivera replikering**.</span><span class="sxs-lookup"><span data-stu-id="3db78-186">**Name**: Enter **Enable replication**.</span></span>
  - <span data-ttu-id="3db78-187">**Bash Webbadress för skriptet**: Ange **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span><span class="sxs-lookup"><span data-stu-id="3db78-187">**Bash Script URL**: Enter **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh**.</span></span>
  - <span data-ttu-id="3db78-188">**HEAD**: markerad.</span><span class="sxs-lookup"><span data-stu-id="3db78-188">**Head**: Selected.</span></span> <span data-ttu-id="3db78-189">Rensa hello andra nodtyper.</span><span class="sxs-lookup"><span data-stu-id="3db78-189">Clear hello other node types.</span></span>
  - <span data-ttu-id="3db78-190">**Parametrarna**: hello följande exempel parametrar Aktivera replikering för alla befintliga hello-tabeller och kopiera alla hello data från hello källa klustret toohello målklustret:</span><span class="sxs-lookup"><span data-stu-id="3db78-190">**Parameters**: hello following sample parameters enable replication for all hello existing tables and copy all hello data from hello source cluster toohello destination cluster:</span></span>

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. <span data-ttu-id="3db78-191">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3db78-191">Click **Create**.</span></span> <span data-ttu-id="3db78-192">hello skript kan ta lite tid, särskilt när hello - copydata argument används.</span><span class="sxs-lookup"><span data-stu-id="3db78-192">hello script can take some time, especially when hello -copydata argument is used.</span></span>

<span data-ttu-id="3db78-193">Argument som krävs:</span><span class="sxs-lookup"><span data-stu-id="3db78-193">Required arguments:</span></span>

|<span data-ttu-id="3db78-194">Namn</span><span class="sxs-lookup"><span data-stu-id="3db78-194">Name</span></span>|<span data-ttu-id="3db78-195">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3db78-195">Description</span></span>|
|----|-----------|
|<span data-ttu-id="3db78-196">-s - src-kluster</span><span class="sxs-lookup"><span data-stu-id="3db78-196">-s, --src-cluster</span></span> | <span data-ttu-id="3db78-197">Ange hello DNS-namnet på hello källa HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-197">Specify hello DNS name of hello source HBase cluster.</span></span> <span data-ttu-id="3db78-198">Till exempel: -s hbsrccluster,--src-kluster = hbsrccluster</span><span class="sxs-lookup"><span data-stu-id="3db78-198">For example: -s hbsrccluster, --src-cluster=hbsrccluster</span></span> |
|<span data-ttu-id="3db78-199">-d,--dst-kluster</span><span class="sxs-lookup"><span data-stu-id="3db78-199">-d, --dst-cluster</span></span> | <span data-ttu-id="3db78-200">Ange hello DNS-namnet på hello mål (replik) HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-200">Specify hello DNS name of hello destination (replica) HBase cluster.</span></span> <span data-ttu-id="3db78-201">Till exempel: -s dsthbcluster,--src-kluster = dsthbcluster</span><span class="sxs-lookup"><span data-stu-id="3db78-201">For example: -s dsthbcluster, --src-cluster=dsthbcluster</span></span> |
|<span data-ttu-id="3db78-202">-sp,--src-ambari-lösenordet</span><span class="sxs-lookup"><span data-stu-id="3db78-202">-sp, --src-ambari-password</span></span> | <span data-ttu-id="3db78-203">Ange hello administratörslösenord för Ambari på hello källa HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-203">Specify hello admin password for Ambari on hello source HBase cluster.</span></span> |
|<span data-ttu-id="3db78-204">-dp,--dst-ambari-lösenordet</span><span class="sxs-lookup"><span data-stu-id="3db78-204">-dp, --dst-ambari-password</span></span> | <span data-ttu-id="3db78-205">Ange hello administratörslösenord för Ambari på hello mål HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-205">Specify hello admin password for Ambari on hello destination HBase cluster.</span></span>|

<span data-ttu-id="3db78-206">Valfria argument:</span><span class="sxs-lookup"><span data-stu-id="3db78-206">Optional arguments:</span></span>

|<span data-ttu-id="3db78-207">Namn</span><span class="sxs-lookup"><span data-stu-id="3db78-207">Name</span></span>|<span data-ttu-id="3db78-208">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="3db78-208">Description</span></span>|
|----|-----------|
|<span data-ttu-id="3db78-209">-su,--src-ambari-användare</span><span class="sxs-lookup"><span data-stu-id="3db78-209">-su, --src-ambari-user</span></span> | <span data-ttu-id="3db78-210">Ange hello administratörsanvändarnamnet för Ambari på hello källa HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-210">Specify hello admin username for Ambari on hello source HBase cluster.</span></span> <span data-ttu-id="3db78-211">hello standardvärdet är **admin**.</span><span class="sxs-lookup"><span data-stu-id="3db78-211">hello default value is **admin**.</span></span> |
|<span data-ttu-id="3db78-212">-du--dst-ambari-användare</span><span class="sxs-lookup"><span data-stu-id="3db78-212">-du, --dst-ambari-user</span></span> | <span data-ttu-id="3db78-213">Ange hello administratörsanvändarnamnet för Ambari på hello mål HBase-kluster.</span><span class="sxs-lookup"><span data-stu-id="3db78-213">Specify hello admin username for Ambari on hello destination HBase cluster.</span></span> <span data-ttu-id="3db78-214">hello standardvärdet är **admin**.</span><span class="sxs-lookup"><span data-stu-id="3db78-214">hello default value is **admin**.</span></span> |
|<span data-ttu-id="3db78-215">-t,--tabell-lista</span><span class="sxs-lookup"><span data-stu-id="3db78-215">-t, --table-list</span></span> | <span data-ttu-id="3db78-216">Ange hello tabeller toobe replikeras.</span><span class="sxs-lookup"><span data-stu-id="3db78-216">Specify hello tables toobe replicated.</span></span> <span data-ttu-id="3db78-217">Till exempel:--tabell lista = ”tabell1; tabell2; tabell 3”.</span><span class="sxs-lookup"><span data-stu-id="3db78-217">For example: --table-list="table1;table2;table3".</span></span> <span data-ttu-id="3db78-218">Om du inte anger tabeller, replikeras alla befintliga HBase-tabeller.</span><span class="sxs-lookup"><span data-stu-id="3db78-218">If you don't specify tables, all existing HBase tables are replicated.</span></span>|
|<span data-ttu-id="3db78-219">-m,--dator</span><span class="sxs-lookup"><span data-stu-id="3db78-219">-m, --machine</span></span> | <span data-ttu-id="3db78-220">Ange hello huvudnod där hello skriptåtgärd körs.</span><span class="sxs-lookup"><span data-stu-id="3db78-220">Specify hello head node where hello script action will run.</span></span> <span data-ttu-id="3db78-221">hello-värdet är antingen hn1 eller hn0.</span><span class="sxs-lookup"><span data-stu-id="3db78-221">hello value is either hn1 or hn0.</span></span> <span data-ttu-id="3db78-222">Eftersom hn0 är vanligtvis busier, bör du använda hn1.</span><span class="sxs-lookup"><span data-stu-id="3db78-222">Because hn0 is usually busier, we recommend using hn1.</span></span> <span data-ttu-id="3db78-223">Du kan använda det här alternativet när du kör hello $0 skriptet som en skriptåtgärd från hello HDInsight-portalen eller Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3db78-223">You use this option when you're running hello $0 script as a script action from hello HDInsight portal or Azure PowerShell.</span></span>|
|<span data-ttu-id="3db78-224">-ip</span><span class="sxs-lookup"><span data-stu-id="3db78-224">-ip</span></span> | <span data-ttu-id="3db78-225">Det här argumentet är obligatorisk när du aktivera replikering mellan två virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3db78-225">This argument is required when you're enabling replication between two virtual networks.</span></span> <span data-ttu-id="3db78-226">Det här argumentet fungerar som en växel toouse hello statiska IP-adresser för ZooKeeper-noder från repliken kluster i stället för FQDN-namn.</span><span class="sxs-lookup"><span data-stu-id="3db78-226">This argument acts as a switch toouse hello static IPs of ZooKeeper nodes from replica clusters instead of FQDN names.</span></span> <span data-ttu-id="3db78-227">hello statiska IP-adresser måste toobe förkonfigurerade innan du aktiverar replikering.</span><span class="sxs-lookup"><span data-stu-id="3db78-227">hello static IPs need toobe preconfigured before you enable replication.</span></span> |
|<span data-ttu-id="3db78-228">-cp, - copydata</span><span class="sxs-lookup"><span data-stu-id="3db78-228">-cp, -copydata</span></span> | <span data-ttu-id="3db78-229">Aktivera hello migrering av befintliga data på hello tabeller där replikering har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="3db78-229">Enable hello migration of existing data on hello tables where replication is enabled.</span></span> |
|<span data-ttu-id="3db78-230">-rpm, -replikera-phoenix-metadata</span><span class="sxs-lookup"><span data-stu-id="3db78-230">-rpm, -replicate-phoenix-meta</span></span> | <span data-ttu-id="3db78-231">Aktivera replikering på Phoenix systemtabeller.</span><span class="sxs-lookup"><span data-stu-id="3db78-231">Enable replication on Phoenix system tables.</span></span> <br><br><span data-ttu-id="3db78-232">*Använd det här alternativet med försiktighet.*</span><span class="sxs-lookup"><span data-stu-id="3db78-232">*Use this option with caution.*</span></span> <span data-ttu-id="3db78-233">Vi rekommenderar att du återskapar Phoenix tabeller på replik kluster innan du använder det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="3db78-233">We recommend that you re-create Phoenix tables on replica clusters before you use this script.</span></span> |
|<span data-ttu-id="3db78-234">-h,--hjälp</span><span class="sxs-lookup"><span data-stu-id="3db78-234">-h, --help</span></span> | <span data-ttu-id="3db78-235">Visa information om användning.</span><span class="sxs-lookup"><span data-stu-id="3db78-235">Display usage information.</span></span> |

<span data-ttu-id="3db78-236">Hej print_usage() avsnitt i hello [skriptet](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) ger en detaljerad förklaring av parametrarna.</span><span class="sxs-lookup"><span data-stu-id="3db78-236">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) provides a detailed explanation of parameters.</span></span>

<span data-ttu-id="3db78-237">När hello skriptåtgärder är har distribuerats, du använder SSH tooconnect toohello mål HBase-kluster och verifiera hello data har replikerats.</span><span class="sxs-lookup"><span data-stu-id="3db78-237">After hello script action is successfully deployed, you can use SSH tooconnect toohello destination HBase cluster, and verify hello data has been replicated.</span></span>

### <a name="replication-scenarios"></a><span data-ttu-id="3db78-238">Replikeringsscenarier</span><span class="sxs-lookup"><span data-stu-id="3db78-238">Replication scenarios</span></span>

<span data-ttu-id="3db78-239">hello visar följande lista du ibland allmän användning och deras parameterinställningarna:</span><span class="sxs-lookup"><span data-stu-id="3db78-239">hello following list shows you some general usage cases and their parameter settings:</span></span>

- <span data-ttu-id="3db78-240">**Aktivera replikering för alla tabeller mellan hello två kluster**.</span><span class="sxs-lookup"><span data-stu-id="3db78-240">**Enable replication on all tables between hello two clusters**.</span></span> <span data-ttu-id="3db78-241">Det här scenariot kräver inte hello kopia/migrering av befintliga data på hello tabeller och den använder inte Phoenix tabeller.</span><span class="sxs-lookup"><span data-stu-id="3db78-241">This scenario does not require hello copy/migration of existing data on hello tables, and it does not use Phoenix tables.</span></span> <span data-ttu-id="3db78-242">Använd hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="3db78-242">Use hello following parameters:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- <span data-ttu-id="3db78-243">**Aktivera replikering för specifika tabeller**.</span><span class="sxs-lookup"><span data-stu-id="3db78-243">**Enable replication on specific tables**.</span></span> <span data-ttu-id="3db78-244">Använd följande parametrar tooenable replikering på tabell1 och tabell2 tabell 3 hello:</span><span class="sxs-lookup"><span data-stu-id="3db78-244">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- <span data-ttu-id="3db78-245">**Aktivera replikering för specifika tabeller och kopiera hello befintliga data**.</span><span class="sxs-lookup"><span data-stu-id="3db78-245">**Enable replication on specific tables and copy hello existing data**.</span></span> <span data-ttu-id="3db78-246">Använd följande parametrar tooenable replikering på tabell1 och tabell2 tabell 3 hello:</span><span class="sxs-lookup"><span data-stu-id="3db78-246">Use hello following parameters tooenable replication on table1, table2, and table3:</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- <span data-ttu-id="3db78-247">**Aktivera replikering för alla tabeller med replikering phoenix metadata från källan toodestination**.</span><span class="sxs-lookup"><span data-stu-id="3db78-247">**Enable replication on all tables with replicating phoenix metadata from source toodestination**.</span></span> <span data-ttu-id="3db78-248">Phoenix metadata replikering är inte exakt och ska aktiveras med försiktighet.</span><span class="sxs-lookup"><span data-stu-id="3db78-248">Phoenix metadata replication is not perfect and should be enabled with caution.</span></span>

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a><span data-ttu-id="3db78-249">Kopiera och migrera data</span><span class="sxs-lookup"><span data-stu-id="3db78-249">Copy and migrate data</span></span>

<span data-ttu-id="3db78-250">Det finns två separata skript åtgärd skript för att kopiera/migrera data när replikeringen har aktiverats:</span><span class="sxs-lookup"><span data-stu-id="3db78-250">There are two separate script action scripts for copying/migrating data after replication is enabled:</span></span>

- <span data-ttu-id="3db78-251">[Skriptet för små tabeller](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (några få gigabyte i storlek och övergripande kopian är förväntade toofinish på mindre än en timme)</span><span class="sxs-lookup"><span data-stu-id="3db78-251">[Script for small tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (a few gigabytes in size, and overall copy is expected toofinish in less than one hour)</span></span>

- <span data-ttu-id="3db78-252">[Skriptet för stora tabeller](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (förväntade tootake som är längre än en timme toocopy)</span><span class="sxs-lookup"><span data-stu-id="3db78-252">[Script for large tables](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (expected tootake longer than one hour toocopy)</span></span>

<span data-ttu-id="3db78-253">Du kan följa samma procedur i hello [Aktivera replikering](#enable-replication) toocall hello skriptåtgärd med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="3db78-253">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

<span data-ttu-id="3db78-254">Hej print_usage() avsnitt i hello [skriptet](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) ger en detaljerad beskrivning av parametrar.</span><span class="sxs-lookup"><span data-stu-id="3db78-254">hello print_usage() section of hello [script](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) provides a detailed description of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="3db78-255">Scenarier</span><span class="sxs-lookup"><span data-stu-id="3db78-255">Scenarios</span></span>

- <span data-ttu-id="3db78-256">**Kopiera specifika tabeller (test1 test2 och test3) för alla rader som redigeras fram tills nu (aktuell tidsstämpel)**:</span><span class="sxs-lookup"><span data-stu-id="3db78-256">**Copy specific tables (test1, test2, and test3) for all rows edited till now (current time stamp)**:</span></span>

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  <span data-ttu-id="3db78-257">eller</span><span class="sxs-lookup"><span data-stu-id="3db78-257">or</span></span>

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- <span data-ttu-id="3db78-258">**Kopiera specifika tabeller med angivet tidsintervall**:</span><span class="sxs-lookup"><span data-stu-id="3db78-258">**Copy specific tables with specified time range**:</span></span>

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a><span data-ttu-id="3db78-259">Inaktivera replikering</span><span class="sxs-lookup"><span data-stu-id="3db78-259">Disable replication</span></span>

<span data-ttu-id="3db78-260">toodisable replikering, som du använder ett annat skript åtgärd skript som finns i [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span><span class="sxs-lookup"><span data-stu-id="3db78-260">toodisable replication, you use another script action script located at [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh).</span></span> <span data-ttu-id="3db78-261">Du kan följa samma procedur i hello [Aktivera replikering](#enable-replication) toocall hello skriptåtgärd med hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="3db78-261">You can follow hello same procedure in [Enable replication](#enable-replication) toocall hello script action with hello following parameters:</span></span>

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari Password> <-all|-t "table1;table2;...">  

<span data-ttu-id="3db78-262">Hej print_usage() avsnitt i hello [skriptet](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) ger en detaljerad förklaring av parametrarna.</span><span class="sxs-lookup"><span data-stu-id="3db78-262">hello print_usage() section of hello [script](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) provides a detailed explanation of parameters.</span></span>

### <a name="scenarios"></a><span data-ttu-id="3db78-263">Scenarier</span><span class="sxs-lookup"><span data-stu-id="3db78-263">Scenarios</span></span>

- <span data-ttu-id="3db78-264">**Inaktivera replikering för alla tabeller**:</span><span class="sxs-lookup"><span data-stu-id="3db78-264">**Disable replication on all tables**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  <span data-ttu-id="3db78-265">eller</span><span class="sxs-lookup"><span data-stu-id="3db78-265">or</span></span>

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari username> --src-ambari-password=<source cluster Ambari password>

- <span data-ttu-id="3db78-266">**Inaktivera replikering för angivna tabeller (tabell1 tabell2 och tabell 3)**:</span><span class="sxs-lookup"><span data-stu-id="3db78-266">**Disable replication on specified tables (table1, table2, and table3)**:</span></span>

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a><span data-ttu-id="3db78-267">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3db78-267">Next steps</span></span>

<span data-ttu-id="3db78-268">I kursen får du lära sig hur tooconfigure HBase-replikering mellan två datacenter.</span><span class="sxs-lookup"><span data-stu-id="3db78-268">In this tutorial, you learned how tooconfigure HBase replication across two datacenters.</span></span> <span data-ttu-id="3db78-269">toolearn mer information om HDInsight och HBase, se:</span><span class="sxs-lookup"><span data-stu-id="3db78-269">toolearn more about HDInsight and HBase, see:</span></span>

* <span data-ttu-id="3db78-270">[Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started]</span><span class="sxs-lookup"><span data-stu-id="3db78-270">[Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started]</span></span>
* <span data-ttu-id="3db78-271">[Översikt över HDInsight HBase][hdinsight-hbase-overview]</span><span class="sxs-lookup"><span data-stu-id="3db78-271">[HDInsight HBase overview][hdinsight-hbase-overview]</span></span>
* <span data-ttu-id="3db78-272">[Skapa HBase-kluster i Azure-nätverk][hdinsight-hbase-provision-vnet]</span><span class="sxs-lookup"><span data-stu-id="3db78-272">[Create HBase clusters in Azure Virtual Network][hdinsight-hbase-provision-vnet]</span></span>
* <span data-ttu-id="3db78-273">[Analysera realtid Twitter-åsikter med HBase][hdinsight-hbase-twitter-sentiment]</span><span class="sxs-lookup"><span data-stu-id="3db78-273">[Analyze real-time Twitter sentiment with HBase][hdinsight-hbase-twitter-sentiment]</span></span>
* <span data-ttu-id="3db78-274">[Analysera sensordata med Storm och HBase i HDInsight (Hadoop)][hdinsight-sensor-data]</span><span class="sxs-lookup"><span data-stu-id="3db78-274">[Analyzing sensor data with Storm and HBase in HDInsight (Hadoop)][hdinsight-sensor-data]</span></span>

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
