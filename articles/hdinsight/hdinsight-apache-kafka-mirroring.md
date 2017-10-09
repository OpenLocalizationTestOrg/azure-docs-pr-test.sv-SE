---
title: avsnitt om aaaMirror Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Kafka spegling funktion toomaintain en replik av en Kafka på HDInsight-kluster genom att spegla avsnitt tooa sekundära kluster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 015d276e-f678-4f2b-9572-75553c56625b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/13/2017
ms.author: larryfr
ms.openlocfilehash: 5ace0251d7402d4d7d9b28726e253ce7091a87ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-mirrormaker-tooreplicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="4764c-103">Använd MirrorMaker tooreplicate Apache Kafka avsnitt med Kafka på HDInsight (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="4764c-103">Use MirrorMaker tooreplicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="4764c-104">Lär dig hur toouse Apache Kafka spegling funktionen tooreplicate avsnitt tooa sekundära kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-104">Learn how toouse Apache Kafka's mirroring feature tooreplicate topics tooa secondary cluster.</span></span> <span data-ttu-id="4764c-105">Spegling kan kördes som en kontinuerlig process eller används ibland som en metod för att migrera data från ett kluster tooanother.</span><span class="sxs-lookup"><span data-stu-id="4764c-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster tooanother.</span></span>

<span data-ttu-id="4764c-106">I det här exemplet är spegling används tooreplicate avsnitt mellan två HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-106">In this example, mirroring is used tooreplicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="4764c-107">Båda klustren finns i ett Azure Virtual Network i hello samma region.</span><span class="sxs-lookup"><span data-stu-id="4764c-107">Both clusters are in an Azure Virtual Network in hello same region.</span></span>

> [!WARNING]
> <span data-ttu-id="4764c-108">Spegling ska inte ses som ett sätt tooachieve feltolerans.</span><span class="sxs-lookup"><span data-stu-id="4764c-108">Mirroring should not be considered as a means tooachieve fault-tolerance.</span></span> <span data-ttu-id="4764c-109">hello offset tooitems inom ett ämne skiljer sig mellan hello käll- och kluster, så att klienter inte kan använda hello två synonymt.</span><span class="sxs-lookup"><span data-stu-id="4764c-109">hello offset tooitems within a topic are different between hello source and destination clusters, so clients cannot use hello two interchangeably.</span></span>
>
> <span data-ttu-id="4764c-110">Om du är orolig feltolerans, anger du replikering för hello avsnitt i klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-110">If you are concerned about fault tolerance, you should set replication for hello topics within your cluster.</span></span> <span data-ttu-id="4764c-111">Mer information finns i [Kom igång med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4764c-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="4764c-112">Så här fungerar Kafka spegling</span><span class="sxs-lookup"><span data-stu-id="4764c-112">How Kafka mirroring works</span></span>

<span data-ttu-id="4764c-113">Spegling fungerar genom att använda hello MirrorMaker verktyget (del av Apache Kafka) tooconsume poster från avsnitt på hello källklustret och sedan skapa en lokal kopia på hello målklustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-113">Mirroring works by using hello MirrorMaker tool (part of Apache Kafka) tooconsume records from topics on hello source cluster and then create a local copy on hello destination cluster.</span></span> <span data-ttu-id="4764c-114">MirrorMaker använder (minst) *konsumenter* som läses från hello källklustret och en *producenten* som skriver toohello lokala (mål) klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-114">MirrorMaker uses one (or more) *consumers* that read from hello source cluster, and a *producer* that writes toohello local (destination) cluster.</span></span>

<span data-ttu-id="4764c-115">hello följande diagram illustrerar processen för spegling av hello:</span><span class="sxs-lookup"><span data-stu-id="4764c-115">hello following diagram illustrates hello Mirroring process:</span></span>

![Diagram över hello spegling process](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="4764c-117">Apache Kafka på HDInsight ger inte tillgång toohello Kafka service över hello offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="4764c-117">Apache Kafka on HDInsight does not provide access toohello Kafka service over hello public internet.</span></span> <span data-ttu-id="4764c-118">Kafka producenter och konsumenter måste vara i hello samma virtuella Azure-nätverket som hello noder i hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-118">Kafka producers or consumers must be in hello same Azure virtual network as hello nodes in hello Kafka cluster.</span></span> <span data-ttu-id="4764c-119">I det här exemplet finns både hello Kafka källa och målkluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4764c-119">For this example, both hello Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="4764c-120">hello följande diagram visar hur kommunikation som flödar mellan hello kluster:</span><span class="sxs-lookup"><span data-stu-id="4764c-120">hello following diagram shows how communication flows between hello clusters:</span></span>

![Diagram över käll- och Kafka-kluster i Azure-nätverk](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="4764c-122">hello käll- och kluster kan vara olika hello antalet noder och partitioner och Offset inom hello avsnitt skiljer sig också.</span><span class="sxs-lookup"><span data-stu-id="4764c-122">hello source and destination clusters can be different in hello number of nodes and partitions, and offsets within hello topics are different also.</span></span> <span data-ttu-id="4764c-123">Spegling underhåller hello nyckelvärdet som används för partitionering, så poster ordning bevaras på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="4764c-123">Mirroring maintains hello key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="4764c-124">Spegling över nätverksgränser</span><span class="sxs-lookup"><span data-stu-id="4764c-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="4764c-125">Om du behöver toomirror mellan Kafka kluster i olika nätverk, finns följande ytterligare överväganden hello:</span><span class="sxs-lookup"><span data-stu-id="4764c-125">If you need toomirror between Kafka clusters in different networks, there are hello following additional considerations:</span></span>

* <span data-ttu-id="4764c-126">**Gateways**: hello nätverk måste vara kan toocommunicate på hello TCPIP-nivå.</span><span class="sxs-lookup"><span data-stu-id="4764c-126">**Gateways**: hello networks must be able toocommunicate at hello TCPIP level.</span></span>

* <span data-ttu-id="4764c-127">**Namnmatchning**: hello Kafka kluster i varje nätverk måste vara kan tooconnect tooeach andra med hjälp av värdnamn.</span><span class="sxs-lookup"><span data-stu-id="4764c-127">**Name resolution**: hello Kafka clusters in each network must be able tooconnect tooeach other by using hostnames.</span></span> <span data-ttu-id="4764c-128">Det här kan kräva en Domain Name System (DNS)-server i varje nätverk som är konfigurerad tooforward begäranden toohello andra nätverk.</span><span class="sxs-lookup"><span data-stu-id="4764c-128">This may require a Domain Name System (DNS) server in each network that is configured tooforward requests toohello other networks.</span></span>

    <span data-ttu-id="4764c-129">När du skapar ett virtuellt Azure-nätverk i stället för med hello nätverket hello automatisk DNS som måste du ange en anpassad DNS-server och hello IP-adress för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="4764c-129">When creating an Azure Virtual Network, instead of using hello automatic DNS provided with hello network, you must specify a custom DNS server and hello IP address for hello server.</span></span> <span data-ttu-id="4764c-130">Efter hello virtuella nätverket har skapats, måste du skapa en virtuell Azure-dator som använder den IP-adressen och sedan installera och konfigurera DNS-programvaran på den.</span><span class="sxs-lookup"><span data-stu-id="4764c-130">After hello Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="4764c-131">Skapa och konfigurera hello anpassad DNS-server innan du installerar HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4764c-131">Create and configure hello custom DNS server before installing HDInsight into hello Virtual Network.</span></span> <span data-ttu-id="4764c-132">Det finns ingen ytterligare konfiguration krävs för HDInsight toouse hello DNS-server som konfigurerats för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="4764c-132">There is no additional configuration required for HDInsight toouse hello DNS server configured for hello Virtual Network.</span></span>

<span data-ttu-id="4764c-133">Mer information om hur du ansluter två virtuella Azure-nätverk finns [konfigurera VNet-till-VNet-anslutningen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="4764c-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="4764c-134">Skapa Kafka kluster</span><span class="sxs-lookup"><span data-stu-id="4764c-134">Create Kafka clusters</span></span>

<span data-ttu-id="4764c-135">Du kan skapa ett virtuellt Azure-nätverk och Kafka kluster manuellt, är det enklare toouse en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="4764c-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier toouse an Azure Resource Manager template.</span></span> <span data-ttu-id="4764c-136">Använd följande steg toodeploy hello Azure-nätverk och två Kafka kluster tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4764c-136">Use hello following steps toodeploy an Azure virtual network and two Kafka clusters tooyour Azure subscription.</span></span>

1. <span data-ttu-id="4764c-137">Använd hello efter knappen toosign i tooAzure och öppna hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4764c-137">Use hello following button toosign in tooAzure and open hello template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   
    <span data-ttu-id="4764c-138">hello Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="4764c-138">hello Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="4764c-139">tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="4764c-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="4764c-140">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="4764c-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="4764c-141">Använd hello efter poster för information om toopopulate hello hello **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="4764c-141">Use hello following information toopopulate hello entries on hello **Custom deployment** blade:</span></span>
    
    ![HDInsight anpassad distribution](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="4764c-143">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="4764c-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="4764c-144">Den här gruppen innehåller hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-144">This group contains hello HDInsight cluster.</span></span>

    * <span data-ttu-id="4764c-145">**Plats**: Välj en plats geografiskt nära tooyou.</span><span class="sxs-lookup"><span data-stu-id="4764c-145">**Location**: Select a location geographically close tooyou.</span></span>
     
    * <span data-ttu-id="4764c-146">**Basera klusternamnet**: det här värdet används som hello huvudnamnet för hello Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-146">**Base Cluster Name**: This value is used as hello base name for hello Kafka clusters.</span></span> <span data-ttu-id="4764c-147">Ange till exempel **hdi** skapar kluster med namnet **källa hdi** och **dest hdi**.</span><span class="sxs-lookup"><span data-stu-id="4764c-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="4764c-148">**Klustrets inloggningsnamn**: hello administratörsanvändarnamnet för hello käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-148">**Cluster Login User Name**: hello admin user name for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="4764c-149">**Klustret inloggningslösenordet**: hello administratörslösenord för hello käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-149">**Cluster Login Password**: hello admin user password for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="4764c-150">**SSH-användarnamn**: hello SSH användaren toocreate för hello käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-150">**SSH User Name**: hello SSH user toocreate for hello source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="4764c-151">**SSH-lösenordet**: hello användarlösenord hello SSH för hello käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-151">**SSH Password**: hello password for hello SSH user for hello source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="4764c-152">Läs hello **villkor**, och välj sedan **acceptera toohello villkoren ovan**.</span><span class="sxs-lookup"><span data-stu-id="4764c-152">Read hello **Terms and Conditions**, and then select **I agree toohello terms and conditions stated above**.</span></span>

4. <span data-ttu-id="4764c-153">Kontrollera slutligen **PIN-kod toodashboard** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="4764c-153">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="4764c-154">Det tar cirka 20 minuter toocreate hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-154">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="4764c-155">När hello resurserna har skapats, är du omdirigerade tooa bladet för hello resursgruppen som innehåller hello kluster och web instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="4764c-155">Once hello resources have been created, you are redirected tooa blade for hello resource group that contains hello clusters and web dashboard.</span></span>

![Blad för resursgrupp för hello vnet och kluster](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="4764c-157">Observera att hello namnen på hello HDInsight-kluster är **källa BASENAME** och **dest BASENAME**, där BASENAME är hello namn du angett toohello mall.</span><span class="sxs-lookup"><span data-stu-id="4764c-157">Notice that hello names of hello HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="4764c-158">Du kan använda dessa namn i senare steg när du ansluter toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-158">You use these names in later steps when connecting toohello clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="4764c-159">Skapa avsnitt</span><span class="sxs-lookup"><span data-stu-id="4764c-159">Create topics</span></span>

1. <span data-ttu-id="4764c-160">Ansluta toohello **källa** kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="4764c-160">Connect toohello **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="4764c-161">Ersätt **sshuser** med hello SSH-användarnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-161">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="4764c-162">Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-162">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="4764c-163">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4764c-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4764c-164">Använd hello följande kommandon toofind hello Zookeeper-värdar för hello källklustret:</span><span class="sxs-lookup"><span data-stu-id="4764c-164">Use hello following commands toofind hello Zookeeper hosts for hello source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get hello zookeeper hosts for hello source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with hello password for hello cluster.

    Replace `$CLUSTERNAME` with hello name of hello source cluster.

3. toocreate a topic named `testtopic`, use hello following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="4764c-165">Använd hello efter kommandot tooverify som hello avsnittet skapades:</span><span class="sxs-lookup"><span data-stu-id="4764c-165">Use hello following command tooverify that hello topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="4764c-166">hello svaret innehåller `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="4764c-166">hello response contains `testtopic`.</span></span>

4. <span data-ttu-id="4764c-167">Använd hello följande tooview hello Zookeeper värdinformation för detta (hello **källa**) klustret:</span><span class="sxs-lookup"><span data-stu-id="4764c-167">Use hello following tooview hello Zookeeper host information for this (hello **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="4764c-168">Detta returnerar information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="4764c-168">This returns information similar toohello following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="4764c-169">Spara den här informationen.</span><span class="sxs-lookup"><span data-stu-id="4764c-169">Save this information.</span></span> <span data-ttu-id="4764c-170">Den används i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="4764c-170">It is used in hello next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="4764c-171">Konfigurera spegling</span><span class="sxs-lookup"><span data-stu-id="4764c-171">Configure mirroring</span></span>

1. <span data-ttu-id="4764c-172">Ansluta toohello **mål** kluster med hjälp av en annan SSH-session:</span><span class="sxs-lookup"><span data-stu-id="4764c-172">Connect toohello **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="4764c-173">Ersätt **sshuser** med hello SSH-användarnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-173">Replace **sshuser** with hello SSH user name used when creating hello cluster.</span></span> <span data-ttu-id="4764c-174">Ersätt **BASENAME** med hello basnamn används när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-174">Replace **BASENAME** with hello base name used when creating hello cluster.</span></span>

    <span data-ttu-id="4764c-175">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="4764c-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="4764c-176">Använd hello följande kommando toocreate en `consumer.properties` fil som beskriver hur toocommunicate med hello **källa** klustret:</span><span class="sxs-lookup"><span data-stu-id="4764c-176">Use hello following command toocreate a `consumer.properties` file that describes how toocommunicate with hello **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="4764c-177">Använd hello följande text som hello hello `consumer.properties` fil:</span><span class="sxs-lookup"><span data-stu-id="4764c-177">Use hello following text as hello contents of hello `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="4764c-178">Ersätt **SOURCE_ZKHOSTS** med hello Zookeeper värd information från hello **källa** klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-178">Replace **SOURCE_ZKHOSTS** with hello Zookeeper hosts information from hello **source** cluster.</span></span>

    <span data-ttu-id="4764c-179">Den här filen beskriver hello konsumenten information toouse vid läsning från hello källa Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-179">This file describes hello consumer information toouse when reading from hello source Kafka cluster.</span></span> <span data-ttu-id="4764c-180">Mer information konsumenten konfiguration finns i [konsumenten konfigurationerna](https://kafka.apache.org/documentation#consumerconfigs) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="4764c-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="4764c-181">toosave hello-fil, Använd **Ctrl + X**, **Y**, och sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="4764c-181">toosave hello file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="4764c-182">Innan du konfigurerar hello producenten som kommunicerar med hello målklustret du hitta hello broker värdar för hello **mål** klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-182">Before configuring hello producer that communicates with hello destination cluster, you must find hello broker hosts for hello **destination** cluster.</span></span> <span data-ttu-id="4764c-183">Använd följande kommandon tooretrieve hello informationen:</span><span class="sxs-lookup"><span data-stu-id="4764c-183">Use hello following commands tooretrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="4764c-184">Ersätt `$PASSWORD` med hello inloggning (admin) kontolösenord för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-184">Replace `$PASSWORD` with hello login account (admin) password for hello cluster.</span></span>

    <span data-ttu-id="4764c-185">Ersätt `$CLUSTERNAME` med hello namnet hello målklustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-185">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="4764c-186">Dessa kommandon returnera information liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="4764c-186">These commands return information similar toohello following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="4764c-187">Använd hello följande toocreate en `producer.properties` fil som beskriver hur toocommunicate med hello **mål** klustret:</span><span class="sxs-lookup"><span data-stu-id="4764c-187">Use hello following toocreate a `producer.properties` file that describes how toocommunicate with hello **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="4764c-188">Använd hello följande text som hello hello `producer.properties` fil:</span><span class="sxs-lookup"><span data-stu-id="4764c-188">Use hello following text as hello contents of hello `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="4764c-189">Ersätt **DEST_BROKERS** med hello broker information från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="4764c-189">Replace **DEST_BROKERS** with hello broker information from hello previous step.</span></span>

    <span data-ttu-id="4764c-190">Mer information producenten konfiguration finns i [producenten konfigurationerna](https://kafka.apache.org/documentation#producerconfigs) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="4764c-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="4764c-191">Starta MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="4764c-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="4764c-192">Från hello SSH-anslutning toohello **mål** bör du använda följande kommando toostart hello MirrorMaker processen hello:</span><span class="sxs-lookup"><span data-stu-id="4764c-192">From hello SSH connection toohello **destination** cluster, use hello following command toostart hello MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="4764c-193">hello-parametrar som används i det här exemplet är:</span><span class="sxs-lookup"><span data-stu-id="4764c-193">hello parameters used in this example are:</span></span>

    * <span data-ttu-id="4764c-194">**--consumer.config**: Anger hello-fil som innehåller konsumenten egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4764c-194">**--consumer.config**: Specifies hello file that contains consumer properties.</span></span> <span data-ttu-id="4764c-195">Dessa egenskaper är används toocreate konsumenter som läser från hello *källa* Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-195">These properties are used toocreate a consumer that reads from hello *source* Kafka cluster.</span></span>

    * <span data-ttu-id="4764c-196">**--producer.config**: Anger hello-fil som innehåller producenten egenskaper.</span><span class="sxs-lookup"><span data-stu-id="4764c-196">**--producer.config**: Specifies hello file that contains producer properties.</span></span> <span data-ttu-id="4764c-197">Dessa egenskaper är används toocreate producent som skriver toohello *mål* Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-197">These properties are used toocreate a producer that writes toohello *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="4764c-198">**--godkända**: en lista över ämnen som MirrorMaker replikerar från hello källa klustret toohello mål.</span><span class="sxs-lookup"><span data-stu-id="4764c-198">**--whitelist**: A list of topics that MirrorMaker replicates from hello source cluster toohello destination.</span></span>

    * <span data-ttu-id="4764c-199">**--num.streams**: hello antalet konsumenten trådar toocreate.</span><span class="sxs-lookup"><span data-stu-id="4764c-199">**--num.streams**: hello number of consumer threads toocreate.</span></span>

 <span data-ttu-id="4764c-200">Vid start returnerar MirrorMaker information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="4764c-200">On startup, MirrorMaker returns information similar toohello following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="4764c-201">Från hello SSH-anslutning toohello **källa** kluster kan använda följande kommando toostart producent hello och skicka meddelanden toohello avsnittet:</span><span class="sxs-lookup"><span data-stu-id="4764c-201">From hello SSH connection toohello **source** cluster, use hello following command toostart a producer and send messages toohello topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="4764c-202">Ersätt `$PASSWORD` med hello inloggning (admin) lösenord för hello källa kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-202">Replace `$PASSWORD` with hello login (admin) password for hello source cluster.</span></span>

    <span data-ttu-id="4764c-203">Ersätt `$CLUSTERNAME` med hello hello källa klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="4764c-203">Replace `$CLUSTERNAME` with hello name of hello source cluster.</span></span>

     <span data-ttu-id="4764c-204">När du kommer till en tom rad med en markör Skriv i några textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="4764c-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="4764c-205">Dessa skickas toohello avsnittet på hello **källa** klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-205">These are sent toohello topic on hello **source** cluster.</span></span> <span data-ttu-id="4764c-206">När du är klar kan du använda **Ctrl + C** tooend hello producenten processen.</span><span class="sxs-lookup"><span data-stu-id="4764c-206">When done, use **Ctrl + C** tooend hello producer process.</span></span>

3. <span data-ttu-id="4764c-207">Från hello SSH-anslutning toohello **mål** klustret använder **Ctrl + C** tooend hello MirrorMaker process.</span><span class="sxs-lookup"><span data-stu-id="4764c-207">From hello SSH connection toohello **destination** cluster, use **Ctrl + C** tooend hello MirrorMaker process.</span></span> <span data-ttu-id="4764c-208">Och sedan använda hello följande kommandon tooverify som hello `testtopic` artikeln skapades och informationen i avsnittet hello var replikerade toothis spegling:</span><span class="sxs-lookup"><span data-stu-id="4764c-208">Then use hello following commands tooverify that hello `testtopic` topic was created, and that data in hello topic was replicated toothis mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="4764c-209">Ersätt `$PASSWORD` med hello inloggning (admin) lösenord för hello målklustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-209">Replace `$PASSWORD` with hello login (admin) password for hello destination cluster.</span></span>

    <span data-ttu-id="4764c-210">Ersätt `$CLUSTERNAME` med hello namnet hello målklustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-210">Replace `$CLUSTERNAME` with hello name of hello destination cluster.</span></span>

    <span data-ttu-id="4764c-211">hello lista över ämnen innehåller nu `testtopic`, som skapas när MirrorMaster speglar hello avsnittet från hello källa klustret toohello mål.</span><span class="sxs-lookup"><span data-stu-id="4764c-211">hello list of topics now includes `testtopic`, which is created when MirrorMaster mirrors hello topic from hello source cluster toohello destination.</span></span> <span data-ttu-id="4764c-212">hälsningsmeddelande som hämtats från hello-avsnittet är hello samma som har angetts på hello källa klustret.</span><span class="sxs-lookup"><span data-stu-id="4764c-212">hello messages retrieved from hello topic are hello same as entered on hello source cluster.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="4764c-213">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="4764c-213">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="4764c-214">Eftersom hello stegen i det här dokumentet skapa båda klustren i hello samma Azure-resursgrupp, kan du ta bort hello resursgrupp i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4764c-214">Since hello steps in this document create both clusters in hello same Azure resource group, you can delete hello resource group in hello Azure portal.</span></span> <span data-ttu-id="4764c-215">Ta bort hello resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet, hello Azure Virtual Network och storage-konto som används av hello kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-215">Deleting hello resource group removes all resources created by following this document, hello Azure Virtual Network, and storage account used by hello clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4764c-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4764c-216">Next Steps</span></span>

<span data-ttu-id="4764c-217">I det här dokumentet du har lärt dig hur toouse MirrorMaker toocreate en replik av en Kafka kluster.</span><span class="sxs-lookup"><span data-stu-id="4764c-217">In this document, you learned how toouse MirrorMaker toocreate a replica of a Kafka cluster.</span></span> <span data-ttu-id="4764c-218">Använd följande länkar toodiscover hello andra sätt toowork med Kafka:</span><span class="sxs-lookup"><span data-stu-id="4764c-218">Use hello following links toodiscover other ways toowork with Kafka:</span></span>

* <span data-ttu-id="4764c-219">[Apache Kafka MirrorMaker dokumentationen](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) på cwiki.apache.org.</span><span class="sxs-lookup"><span data-stu-id="4764c-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="4764c-220">Kom igång med Apache Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4764c-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="4764c-221">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4764c-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="4764c-222">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="4764c-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="4764c-223">Ansluta tooKafka via ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="4764c-223">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
