---
title: Spegling Apache Kafka - avsnitt i Azure HDInsight | Microsoft Docs
description: "Lär dig använda Apache Kafka spegling för att underhålla en replik av en Kafka på HDInsight-kluster genom att spegla avsnitt till ett sekundärt kluster."
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
ms.openlocfilehash: e418cb01e1a9168e3662e8d6242903e052b6047b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-mirrormaker-to-replicate-apache-kafka-topics-with-kafka-on-hdinsight-preview"></a><span data-ttu-id="bb209-103">Använd MirrorMaker för att replikera Apache Kafka avsnitt med Kafka på HDInsight (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="bb209-103">Use MirrorMaker to replicate Apache Kafka topics with Kafka on HDInsight (preview)</span></span>

<span data-ttu-id="bb209-104">Lär dig använda Apache Kafka databasspegling funktionen för att replikera avsnitt till sekundära kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-104">Learn how to use Apache Kafka's mirroring feature to replicate topics to a secondary cluster.</span></span> <span data-ttu-id="bb209-105">Spegling kan kördes som en kontinuerlig process eller används ibland som en metod för att migrera data från ett kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-105">Mirroring can be ran as a continuous process, or used intermittently as a method of migrating data from one cluster to another.</span></span>

<span data-ttu-id="bb209-106">I det här exemplet används spegling för att replikera information mellan två HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-106">In this example, mirroring is used to replicate topics between two HDInsight clusters.</span></span> <span data-ttu-id="bb209-107">Båda klustren finns i ett Azure Virtual Network i samma region.</span><span class="sxs-lookup"><span data-stu-id="bb209-107">Both clusters are in an Azure Virtual Network in the same region.</span></span>

> [!WARNING]
> <span data-ttu-id="bb209-108">Spegling ska inte ses som ett sätt att uppnå feltolerans.</span><span class="sxs-lookup"><span data-stu-id="bb209-108">Mirroring should not be considered as a means to achieve fault-tolerance.</span></span> <span data-ttu-id="bb209-109">Förskjutning av objekt inom ett ämne skiljer sig mellan käll- och kluster, så att klienter inte kan använda två synonymt.</span><span class="sxs-lookup"><span data-stu-id="bb209-109">The offset to items within a topic are different between the source and destination clusters, so clients cannot use the two interchangeably.</span></span>
>
> <span data-ttu-id="bb209-110">Om du är orolig feltolerans, anger du replikering för avsnitten i klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-110">If you are concerned about fault tolerance, you should set replication for the topics within your cluster.</span></span> <span data-ttu-id="bb209-111">Mer information finns i [Kom igång med Kafka på HDInsight](hdinsight-apache-kafka-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bb209-111">For more information, see [Get started with Kafka on HDInsight](hdinsight-apache-kafka-get-started.md).</span></span>

## <a name="how-kafka-mirroring-works"></a><span data-ttu-id="bb209-112">Så här fungerar Kafka spegling</span><span class="sxs-lookup"><span data-stu-id="bb209-112">How Kafka mirroring works</span></span>

<span data-ttu-id="bb209-113">Spegling fungerar med hjälp av verktyget MirrorMaker (del av Apache Kafka) att använda poster ämnen på källklustret och sedan skapa en lokal kopia på målklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-113">Mirroring works by using the MirrorMaker tool (part of Apache Kafka) to consume records from topics on the source cluster and then create a local copy on the destination cluster.</span></span> <span data-ttu-id="bb209-114">MirrorMaker använder (minst) *konsumenter* som läses från källklustret och en *producenten* som skriver till lokala (mål) klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-114">MirrorMaker uses one (or more) *consumers* that read from the source cluster, and a *producer* that writes to the local (destination) cluster.</span></span>

<span data-ttu-id="bb209-115">Följande diagram illustrerar processen spegling:</span><span class="sxs-lookup"><span data-stu-id="bb209-115">The following diagram illustrates the Mirroring process:</span></span>

![Diagram över databasspegling processen](./media/hdinsight-apache-kafka-mirroring/kafka-mirroring.png)

<span data-ttu-id="bb209-117">Apache Kafka på HDInsight ger inte tillgång till tjänsten Kafka via det offentliga internet.</span><span class="sxs-lookup"><span data-stu-id="bb209-117">Apache Kafka on HDInsight does not provide access to the Kafka service over the public internet.</span></span> <span data-ttu-id="bb209-118">Kafka producenter och konsumenter måste vara i samma virtuella Azure-nätverket som noder i klustret Kafka.</span><span class="sxs-lookup"><span data-stu-id="bb209-118">Kafka producers or consumers must be in the same Azure virtual network as the nodes in the Kafka cluster.</span></span> <span data-ttu-id="bb209-119">I det här exemplet finns både Kafka käll- och kluster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="bb209-119">For this example, both the Kafka source and destination clusters are located in an Azure virtual network.</span></span> <span data-ttu-id="bb209-120">Följande diagram visar hur kommunikation som flödar mellan kluster:</span><span class="sxs-lookup"><span data-stu-id="bb209-120">The following diagram shows how communication flows between the clusters:</span></span>

![Diagram över käll- och Kafka-kluster i Azure-nätverk](./media/hdinsight-apache-kafka-mirroring/spark-kafka-vnet.png)

<span data-ttu-id="bb209-122">Käll- och kluster kan skilja sig i antal noder och partitioner och förskjutningar i avsnittet skiljer sig också.</span><span class="sxs-lookup"><span data-stu-id="bb209-122">The source and destination clusters can be different in the number of nodes and partitions, and offsets within the topics are different also.</span></span> <span data-ttu-id="bb209-123">Spegling underhåller värdet för nyckeln som används för partitionering, så poster ordning bevaras på grundval av per nyckel.</span><span class="sxs-lookup"><span data-stu-id="bb209-123">Mirroring maintains the key value that is used for partitioning, so record order is preserved on a per-key basis.</span></span>

### <a name="mirroring-across-network-boundaries"></a><span data-ttu-id="bb209-124">Spegling över nätverksgränser</span><span class="sxs-lookup"><span data-stu-id="bb209-124">Mirroring across network boundaries</span></span>

<span data-ttu-id="bb209-125">Om du behöver spegling mellan Kafka kluster i olika nätverk finns följande ytterligare överväganden:</span><span class="sxs-lookup"><span data-stu-id="bb209-125">If you need to mirror between Kafka clusters in different networks, there are the following additional considerations:</span></span>

* <span data-ttu-id="bb209-126">**Gateways**: nätverk måste kunna kommunicera på TCPIP-nivå.</span><span class="sxs-lookup"><span data-stu-id="bb209-126">**Gateways**: The networks must be able to communicate at the TCPIP level.</span></span>

* <span data-ttu-id="bb209-127">**Namnmatchning**: den Kafka kluster i varje nätverk måste kunna ansluta till varandra med hjälp av värdnamn.</span><span class="sxs-lookup"><span data-stu-id="bb209-127">**Name resolution**: The Kafka clusters in each network must be able to connect to each other by using hostnames.</span></span> <span data-ttu-id="bb209-128">Det här kan kräva en Domain Name System (DNS)-server i varje nätverk som har konfigurerats som vidarebefordrar begäranden till andra nätverk.</span><span class="sxs-lookup"><span data-stu-id="bb209-128">This may require a Domain Name System (DNS) server in each network that is configured to forward requests to the other networks.</span></span>

    <span data-ttu-id="bb209-129">När du skapar ett virtuellt Azure-nätverk istället för att använda automatisk DNS som ingår i nätverket, måste du ange en anpassad DNS-server och IP-adressen för servern.</span><span class="sxs-lookup"><span data-stu-id="bb209-129">When creating an Azure Virtual Network, instead of using the automatic DNS provided with the network, you must specify a custom DNS server and the IP address for the server.</span></span> <span data-ttu-id="bb209-130">När det virtuella nätverket har skapats, måste du skapa en virtuell Azure-dator som använder den IP-adressen och sedan installera och konfigurera DNS-programvaran på den.</span><span class="sxs-lookup"><span data-stu-id="bb209-130">After the Virtual Network has been created, you must then create an Azure Virtual Machine that uses that IP address, then install and configure DNS software on it.</span></span>

    > [!WARNING]
    > <span data-ttu-id="bb209-131">Skapa och konfigurera anpassade DNS-servern innan du installerar HDInsight till det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="bb209-131">Create and configure the custom DNS server before installing HDInsight into the Virtual Network.</span></span> <span data-ttu-id="bb209-132">Det finns ingen ytterligare konfiguration krävs för HDInsight för att använda DNS-server som konfigurerats för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="bb209-132">There is no additional configuration required for HDInsight to use the DNS server configured for the Virtual Network.</span></span>

<span data-ttu-id="bb209-133">Mer information om hur du ansluter två virtuella Azure-nätverk finns [konfigurera VNet-till-VNet-anslutningen](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="bb209-133">For more information on connecting two Azure Virtual Networks, see [Configure a VNet-to-VNet connection](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).</span></span>

## <a name="create-kafka-clusters"></a><span data-ttu-id="bb209-134">Skapa Kafka kluster</span><span class="sxs-lookup"><span data-stu-id="bb209-134">Create Kafka clusters</span></span>

<span data-ttu-id="bb209-135">Du kan skapa ett virtuellt Azure-nätverk och Kafka kluster manuellt, men det är enklare att använda en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="bb209-135">While you can create an Azure virtual network and Kafka clusters manually, it's easier to use an Azure Resource Manager template.</span></span> <span data-ttu-id="bb209-136">Använd följande steg för att distribuera ett virtuellt Azure-nätverk och två kluster med Kafka till din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="bb209-136">Use the following steps to deploy an Azure virtual network and two Kafka clusters to your Azure subscription.</span></span>

1. <span data-ttu-id="bb209-137">Använd knappen följande för att logga in på Azure och öppna mallen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb209-137">Use the following button to sign in to Azure and open the template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json" target="_blank"><img src="./media/hdinsight-apache-kafka-mirroring/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
    <span data-ttu-id="bb209-138">Azure Resource Manager-mallen finns på **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span><span class="sxs-lookup"><span data-stu-id="bb209-138">The Azure Resource Manager template is located at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="bb209-139">Klustret måste innehålla minst tre arbetsnoder för att garantera tillgängligheten för Kafka i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="bb209-139">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span> <span data-ttu-id="bb209-140">Den här mallen skapar ett Kafka kluster som innehåller tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="bb209-140">This template creates a Kafka cluster that contains three worker nodes.</span></span>

2. <span data-ttu-id="bb209-141">Använd följande information för att fylla i posterna på den **anpassad distribution** bladet:</span><span class="sxs-lookup"><span data-stu-id="bb209-141">Use the following information to populate the entries on the **Custom deployment** blade:</span></span>
    
    ![HDInsight anpassad distribution](./media/hdinsight-apache-kafka-mirroring/parameters.png)
    
    * <span data-ttu-id="bb209-143">**Resursgruppen**: skapa en grupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="bb209-143">**Resource group**: Create a group or select an existing one.</span></span> <span data-ttu-id="bb209-144">Den här gruppen innehåller HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-144">This group contains the HDInsight cluster.</span></span>

    * <span data-ttu-id="bb209-145">**Plats**: Välj en plats geografiskt nära dig.</span><span class="sxs-lookup"><span data-stu-id="bb209-145">**Location**: Select a location geographically close to you.</span></span>
     
    * <span data-ttu-id="bb209-146">**Basera klusternamnet**: det här värdet används som det grundläggande namnet för Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-146">**Base Cluster Name**: This value is used as the base name for the Kafka clusters.</span></span> <span data-ttu-id="bb209-147">Ange till exempel **hdi** skapar kluster med namnet **källa hdi** och **dest hdi**.</span><span class="sxs-lookup"><span data-stu-id="bb209-147">For example, entering **hdi** creates clusters named **source-hdi** and **dest-hdi**.</span></span>

    * <span data-ttu-id="bb209-148">**Klustrets inloggningsnamn**: admin användarnamn för käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-148">**Cluster Login User Name**: The admin user name for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="bb209-149">**Klustret inloggningslösenordet**: administratörslösenord för käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-149">**Cluster Login Password**: The admin user password for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="bb209-150">**SSH-användarnamn**: SSH-användare för käll- och Kafka Skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-150">**SSH User Name**: The SSH user to create for the source and destination Kafka clusters.</span></span>

    * <span data-ttu-id="bb209-151">**SSH-lösenordet**: lösenord för SSH-användare för käll- och Kafka-kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-151">**SSH Password**: The password for the SSH user for the source and destination Kafka clusters.</span></span>

3. <span data-ttu-id="bb209-152">Läs den **villkor**, och välj sedan **jag samtycker till villkoren som anges ovan**.</span><span class="sxs-lookup"><span data-stu-id="bb209-152">Read the **Terms and Conditions**, and then select **I agree to the terms and conditions stated above**.</span></span>

4. <span data-ttu-id="bb209-153">Kontrollera slutligen **fäst på instrumentpanelen** och välj sedan **inköp**.</span><span class="sxs-lookup"><span data-stu-id="bb209-153">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="bb209-154">Det tar ungefär 20 minuter för att skapa kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-154">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="bb209-155">När resurserna som har skapats, omdirigeras till ett blad för resursgruppen som innehåller kluster och web instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="bb209-155">Once the resources have been created, you are redirected to a blade for the resource group that contains the clusters and web dashboard.</span></span>

![Blad för resursgrupp för virtuella nätverk och kluster](./media/hdinsight-apache-kafka-mirroring/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="bb209-157">Lägg märke till att namnen på HDInsight-kluster **källa BASENAME** och **dest BASENAME**, där BASENAME är det namn du angav i mallen.</span><span class="sxs-lookup"><span data-stu-id="bb209-157">Notice that the names of the HDInsight clusters are **source-BASENAME** and **dest-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="bb209-158">Du kan använda dessa namn i senare steg när du ansluter till kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-158">You use these names in later steps when connecting to the clusters.</span></span>

## <a name="create-topics"></a><span data-ttu-id="bb209-159">Skapa avsnitt</span><span class="sxs-lookup"><span data-stu-id="bb209-159">Create topics</span></span>

1. <span data-ttu-id="bb209-160">Ansluta till den **källa** kluster med SSH:</span><span class="sxs-lookup"><span data-stu-id="bb209-160">Connect to the **source** cluster using SSH:</span></span>

    ```bash
    ssh sshuser@source-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="bb209-161">Ersätt **sshuser** med SSH-användarnamn som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-161">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="bb209-162">Ersätt **BASENAME** med det grundläggande namnet som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-162">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="bb209-163">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="bb209-163">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="bb209-164">Använd följande kommandon för att hitta Zookeeper-värdar för källklustret:</span><span class="sxs-lookup"><span data-stu-id="bb209-164">Use the following commands to find the Zookeeper hosts for the source cluster:</span></span>

    ```bash
    # Install jq if it is not installed
    sudo apt -y install jq
    # get the zookeeper hosts for the source cluster
    export SOURCE_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    
    Replace `$PASSWORD` with the password for the cluster.

    Replace `$CLUSTERNAME` with the name of the source cluster.

3. To create a topic named `testtopic`, use the following command:

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 2 --partitions 8 --topic testtopic --zookeeper $SOURCE_ZKHOSTS
    ```

3. <span data-ttu-id="bb209-165">Använder du följande kommando för att verifiera att avsnittet skapades:</span><span class="sxs-lookup"><span data-stu-id="bb209-165">Use the following command to verify that the topic was created:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="bb209-166">Svaret innehåller `testtopic`.</span><span class="sxs-lookup"><span data-stu-id="bb209-166">The response contains `testtopic`.</span></span>

4. <span data-ttu-id="bb209-167">Använd följande för att visa information om Zookeeper värden för den här (den **källa**) klustret:</span><span class="sxs-lookup"><span data-stu-id="bb209-167">Use the following to view the Zookeeper host information for this (the **source**) cluster:</span></span>

    ```bash
    echo $SOURCE_ZKHOSTS
    ```

    <span data-ttu-id="bb209-168">Information returneras liknar följande:</span><span class="sxs-lookup"><span data-stu-id="bb209-168">This returns information similar to the following text:</span></span>

    `zk0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181,zk1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:2181`

    <span data-ttu-id="bb209-169">Spara den här informationen.</span><span class="sxs-lookup"><span data-stu-id="bb209-169">Save this information.</span></span> <span data-ttu-id="bb209-170">Den används i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bb209-170">It is used in the next section.</span></span>

## <a name="configure-mirroring"></a><span data-ttu-id="bb209-171">Konfigurera spegling</span><span class="sxs-lookup"><span data-stu-id="bb209-171">Configure mirroring</span></span>

1. <span data-ttu-id="bb209-172">Ansluta till den **mål** kluster med hjälp av en annan SSH-session:</span><span class="sxs-lookup"><span data-stu-id="bb209-172">Connect to the **destination** cluster using a different SSH session:</span></span>

    ```bash
    ssh sshuser@dest-BASENAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="bb209-173">Ersätt **sshuser** med SSH-användarnamn som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-173">Replace **sshuser** with the SSH user name used when creating the cluster.</span></span> <span data-ttu-id="bb209-174">Ersätt **BASENAME** med det grundläggande namnet som används när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-174">Replace **BASENAME** with the base name used when creating the cluster.</span></span>

    <span data-ttu-id="bb209-175">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="bb209-175">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="bb209-176">Använd följande kommando för att skapa en `consumer.properties` fil som beskriver hur du kan kommunicera med den **källa** klustret:</span><span class="sxs-lookup"><span data-stu-id="bb209-176">Use the following command to create a `consumer.properties` file that describes how to communicate with the **source** cluster:</span></span>

    ```bash
    nano consumer.properties
    ```

    <span data-ttu-id="bb209-177">Använd följande text som innehållet i den `consumer.properties` filen:</span><span class="sxs-lookup"><span data-stu-id="bb209-177">Use the following text as the contents of the `consumer.properties` file:</span></span>

    ```yaml
    zookeeper.connect=SOURCE_ZKHOSTS
    group.id=mirrorgroup
    ```

    <span data-ttu-id="bb209-178">Ersätt **SOURCE_ZKHOSTS** med Zookeeper värdar information från den **källa** klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-178">Replace **SOURCE_ZKHOSTS** with the Zookeeper hosts information from the **source** cluster.</span></span>

    <span data-ttu-id="bb209-179">Den här filen beskriver konsumenten ska användas vid läsning från källan Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-179">This file describes the consumer information to use when reading from the source Kafka cluster.</span></span> <span data-ttu-id="bb209-180">Mer information konsumenten konfiguration finns i [konsumenten konfigurationerna](https://kafka.apache.org/documentation#consumerconfigs) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="bb209-180">For more information consumer configuration, see [Consumer Configs](https://kafka.apache.org/documentation#consumerconfigs) at kafka.apache.org.</span></span>

    <span data-ttu-id="bb209-181">Om du vill spara filen, Använd **Ctrl + X**, **Y**, och sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="bb209-181">To save the file, use **Ctrl + X**, **Y**, and then **Enter**.</span></span>

3. <span data-ttu-id="bb209-182">Innan du konfigurerar producenten som kommunicerar med målklustret bör du hitta Service broker-värdar för den **mål** klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-182">Before configuring the producer that communicates with the destination cluster, you must find the broker hosts for the **destination** cluster.</span></span> <span data-ttu-id="bb209-183">Använd följande kommandon för att hämta den här informationen:</span><span class="sxs-lookup"><span data-stu-id="bb209-183">Use the following commands to retrieve this information:</span></span>

    ```bash
    sudo apt -y install jq
    DEST_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    echo $DEST_BROKERHOSTS
    ```

    <span data-ttu-id="bb209-184">Ersätt `$PASSWORD` med inloggningen (admin) kontolösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-184">Replace `$PASSWORD` with the login account (admin) password for the cluster.</span></span>

    <span data-ttu-id="bb209-185">Ersätt `$CLUSTERNAME` med namnet på målklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-185">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="bb209-186">Dessa kommandon returnera information som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="bb209-186">These commands return information similar to the following:</span></span>

        wn0-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn1-dest.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092

4. <span data-ttu-id="bb209-187">Använd följande för att skapa en `producer.properties` fil som beskriver hur du kan kommunicera med den **mål** klustret:</span><span class="sxs-lookup"><span data-stu-id="bb209-187">Use the following to create a `producer.properties` file that describes how to communicate with the **destination** cluster:</span></span>

    ```bash
    nano producer.properties
    ```

    <span data-ttu-id="bb209-188">Använd följande text som innehållet i den `producer.properties` filen:</span><span class="sxs-lookup"><span data-stu-id="bb209-188">Use the following text as the contents of the `producer.properties` file:</span></span>

    ```yaml
    bootstrap.servers=DEST_BROKERS
    compression.type=none
    ```

    <span data-ttu-id="bb209-189">Ersätt **DEST_BROKERS** med broker information från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="bb209-189">Replace **DEST_BROKERS** with the broker information from the previous step.</span></span>

    <span data-ttu-id="bb209-190">Mer information producenten konfiguration finns i [producenten konfigurationerna](https://kafka.apache.org/documentation#producerconfigs) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="bb209-190">For more information producer configuration, see [Producer Configs](https://kafka.apache.org/documentation#producerconfigs) at kafka.apache.org.</span></span>

## <a name="start-mirrormaker"></a><span data-ttu-id="bb209-191">Starta MirrorMaker</span><span class="sxs-lookup"><span data-stu-id="bb209-191">Start MirrorMaker</span></span>

1. <span data-ttu-id="bb209-192">Från SSH-anslutning till den **mål** bör du använda följande kommando för att starta processen MirrorMaker:</span><span class="sxs-lookup"><span data-stu-id="bb209-192">From the SSH connection to the **destination** cluster, use the following command to start the MirrorMaker process:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.MirrorMaker --consumer.config consumer.properties --producer.config producer.properties --whitelist testtopic --num.streams 4
    ```

    <span data-ttu-id="bb209-193">De parametrar som används i det här exemplet är:</span><span class="sxs-lookup"><span data-stu-id="bb209-193">The parameters used in this example are:</span></span>

    * <span data-ttu-id="bb209-194">**--consumer.config**: Anger den fil som innehåller konsumenten egenskaper.</span><span class="sxs-lookup"><span data-stu-id="bb209-194">**--consumer.config**: Specifies the file that contains consumer properties.</span></span> <span data-ttu-id="bb209-195">De här egenskaperna används för att skapa en konsument läser från den *källa* Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-195">These properties are used to create a consumer that reads from the *source* Kafka cluster.</span></span>

    * <span data-ttu-id="bb209-196">**--producer.config**: Anger den fil som innehåller producenten egenskaper.</span><span class="sxs-lookup"><span data-stu-id="bb209-196">**--producer.config**: Specifies the file that contains producer properties.</span></span> <span data-ttu-id="bb209-197">De här egenskaperna används för att skapa en producent som skriver till den *mål* Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-197">These properties are used to create a producer that writes to the *destination* Kafka cluster.</span></span>

    * <span data-ttu-id="bb209-198">**--godkända**: en lista över ämnen som MirrorMaker replikeras från källklustret till målet.</span><span class="sxs-lookup"><span data-stu-id="bb209-198">**--whitelist**: A list of topics that MirrorMaker replicates from the source cluster to the destination.</span></span>

    * <span data-ttu-id="bb209-199">**--num.streams**: antalet trådar som konsumenten att skapa.</span><span class="sxs-lookup"><span data-stu-id="bb209-199">**--num.streams**: The number of consumer threads to create.</span></span>

 <span data-ttu-id="bb209-200">Vid start returnerar MirrorMaker information som är liknar följande:</span><span class="sxs-lookup"><span data-stu-id="bb209-200">On startup, MirrorMaker returns information similar to the following text:</span></span>

    ```json
    {metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-3, security.protocol=PLAINTEXT}{metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-0, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-kafka.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-2, security.protocol=PLAINTEXT}
    metadata.broker.list=wn1-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092,wn0-source.aazwc2onlofevkbof0cuixrp5h.gx.internal.cloudapp.net:9092, request.timeout.ms=30000, client.id=mirror-group-1, security.protocol=PLAINTEXT}
    ```

2. <span data-ttu-id="bb209-201">Från SSH-anslutning till den **källa** bör du använda följande kommando för att starta en producent och skicka meddelanden till avsnittet:</span><span class="sxs-lookup"><span data-stu-id="bb209-201">From the SSH connection to the **source** cluster, use the following command to start a producer and send messages to the topic:</span></span>

    ```bash
    SOURCE_BROKERHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $SOURCE_BROKERHOSTS --topic testtopic
    ```

    <span data-ttu-id="bb209-202">Ersätt `$PASSWORD` med inloggningslösenordet (admin) för klustret som datakällan.</span><span class="sxs-lookup"><span data-stu-id="bb209-202">Replace `$PASSWORD` with the login (admin) password for the source cluster.</span></span>

    <span data-ttu-id="bb209-203">Ersätt `$CLUSTERNAME` med namnet på källklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-203">Replace `$CLUSTERNAME` with the name of the source cluster.</span></span>

     <span data-ttu-id="bb209-204">När du kommer till en tom rad med en markör Skriv i några textmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="bb209-204">When you arrive at a blank line with a cursor, type in a few text messages.</span></span> <span data-ttu-id="bb209-205">Dessa skickas till ämnet den **källa** klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-205">These are sent to the topic on the **source** cluster.</span></span> <span data-ttu-id="bb209-206">När du är klar kan du använda **Ctrl + C** att avsluta processen producenten.</span><span class="sxs-lookup"><span data-stu-id="bb209-206">When done, use **Ctrl + C** to end the producer process.</span></span>

3. <span data-ttu-id="bb209-207">Från SSH-anslutning till den **mål** klustret använder **Ctrl + C** att avsluta processen MirrorMaker.</span><span class="sxs-lookup"><span data-stu-id="bb209-207">From the SSH connection to the **destination** cluster, use **Ctrl + C** to end the MirrorMaker process.</span></span> <span data-ttu-id="bb209-208">Använd sedan följande kommandon för att kontrollera att den `testtopic` artikeln skapades och informationen i avsnittet har replikerats till den här spegling:</span><span class="sxs-lookup"><span data-stu-id="bb209-208">Then use the following commands to verify that the `testtopic` topic was created, and that data in the topic was replicated to this mirror:</span></span>

    ```bash
    DEST_ZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $DEST_ZKHOSTS
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --zookeeper $DEST_ZKHOSTS --topic testtopic --from-beginning
    ```

    <span data-ttu-id="bb209-209">Ersätt `$PASSWORD` med inloggningslösenordet (admin) för målklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-209">Replace `$PASSWORD` with the login (admin) password for the destination cluster.</span></span>

    <span data-ttu-id="bb209-210">Ersätt `$CLUSTERNAME` med namnet på målklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-210">Replace `$CLUSTERNAME` with the name of the destination cluster.</span></span>

    <span data-ttu-id="bb209-211">I listan med avsnitt innehåller nu `testtopic`, som skapas när MirrorMaster speglar avsnittet från källklustret till målet.</span><span class="sxs-lookup"><span data-stu-id="bb209-211">The list of topics now includes `testtopic`, which is created when MirrorMaster mirrors the topic from the source cluster to the destination.</span></span> <span data-ttu-id="bb209-212">Meddelanden som hämtas från avsnittet är samma som har angetts på källklustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-212">The messages retrieved from the topic are the same as entered on the source cluster.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="bb209-213">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="bb209-213">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="bb209-214">Eftersom stegen i det här dokumentet skapa båda klustren i samma Azure resursgrupp måste du ta bort resursgruppen i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="bb209-214">Since the steps in this document create both clusters in the same Azure resource group, you can delete the resource group in the Azure portal.</span></span> <span data-ttu-id="bb209-215">Ta bort resursgruppen tar bort alla resurser som har skapats genom att följa det här dokumentet, Azure Virtual Network och storage-konto som används av kluster.</span><span class="sxs-lookup"><span data-stu-id="bb209-215">Deleting the resource group removes all resources created by following this document, the Azure Virtual Network, and storage account used by the clusters.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb209-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb209-216">Next Steps</span></span>

<span data-ttu-id="bb209-217">I det här dokumentet du har lärt dig hur du använder MirrorMaker för att skapa en replik av en Kafka-klustret.</span><span class="sxs-lookup"><span data-stu-id="bb209-217">In this document, you learned how to use MirrorMaker to create a replica of a Kafka cluster.</span></span> <span data-ttu-id="bb209-218">Använd följande länkar för att identifiera andra sätt att arbeta med Kafka:</span><span class="sxs-lookup"><span data-stu-id="bb209-218">Use the following links to discover other ways to work with Kafka:</span></span>

* <span data-ttu-id="bb209-219">[Apache Kafka MirrorMaker dokumentationen](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) på cwiki.apache.org.</span><span class="sxs-lookup"><span data-stu-id="bb209-219">[Apache Kafka MirrorMaker documentation](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330) at cwiki.apache.org.</span></span>
* [<span data-ttu-id="bb209-220">Kom igång med Apache Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bb209-220">Get started with Apache Kafka on HDInsight</span></span>](hdinsight-apache-kafka-get-started.md)
* [<span data-ttu-id="bb209-221">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bb209-221">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="bb209-222">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="bb209-222">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="bb209-223">Ansluta till Kafka via ett trådlöst Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="bb209-223">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
