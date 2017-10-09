---
title: aaaStart med Apache Kafka - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toocreate en Apache Kafka kluster på Azure HDInsight. Lär dig hur toocreate ämnen, prenumeranter och konsumenter."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 43585abf-bec1-4322-adde-6db21de98d7f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/14/2017
ms.author: larryfr
ms.openlocfilehash: b93299d88dc2cf9a9764662509308ff75fd74474
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="11b96-104">Kom igång med Apache Kafka (förhandsversion) i HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-104">Start with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="11b96-105">Lär dig hur toocreate och använda en [Apache Kafka](https://kafka.apache.org) kluster på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="11b96-105">Learn how toocreate and use an [Apache Kafka](https://kafka.apache.org) cluster on Azure HDInsight.</span></span> <span data-ttu-id="11b96-106">Kafka är en distribuerad direktuppspelningsplattform med öppen källkod som är tillgänglig i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="11b96-106">Kafka is an open-source, distributed streaming platform that is available with HDInsight.</span></span> <span data-ttu-id="11b96-107">Det används ofta som förhandlare meddelandet eftersom det ger liknande funktioner tooa publicera och prenumerera meddelandekön.</span><span class="sxs-lookup"><span data-stu-id="11b96-107">It is often used as a message broker, as it provides functionality similar tooa publish-subscribe message queue.</span></span>

> [!NOTE]
> <span data-ttu-id="11b96-108">Det finns för närvarande två versioner av Kafka med HDInsight; 0.9.0 (HDInsight 3.4) och 0.10.0 (HDInsight 3.5 och 3.6).</span><span class="sxs-lookup"><span data-stu-id="11b96-108">There are currently two versions of Kafka available with HDInsight; 0.9.0 (HDInsight 3.4) and 0.10.0 (HDInsight 3.5 and 3.6).</span></span> <span data-ttu-id="11b96-109">hello stegen i det här dokumentet förutsätter att du använder Kafka på HDInsight 3,6.</span><span class="sxs-lookup"><span data-stu-id="11b96-109">hello steps in this document assume that you are using Kafka on HDInsight 3.6.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a><span data-ttu-id="11b96-110">Skapa ett Kafka-kluster</span><span class="sxs-lookup"><span data-stu-id="11b96-110">Create a Kafka cluster</span></span>

<span data-ttu-id="11b96-111">Använd hello följa steg toocreate en Kafka på HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="11b96-111">Use hello following steps toocreate a Kafka on HDInsight cluster:</span></span>

1. <span data-ttu-id="11b96-112">Från hello [Azure-portalen](https://portal.azure.com)väljer **+ ny**, **Intelligence + analys**, och välj sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="11b96-112">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>
   
    ![Skapa ett HDInsight-kluster](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. <span data-ttu-id="11b96-114">Från **grunderna**, ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="11b96-114">From **Basics**, enter hello following information:</span></span>

    * <span data-ttu-id="11b96-115">**Klusternamn**: hello namnet på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="11b96-115">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="11b96-116">**Prenumerationen**: Välj hello prenumeration toouse.</span><span class="sxs-lookup"><span data-stu-id="11b96-116">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="11b96-117">**Klustret inloggning användarnamn** och **klustret inloggningslösenordet**: hello inloggning vid åtkomst till hello klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="11b96-117">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="11b96-118">Du använder dessa autentiseringsuppgifter tooaccess tjänster, till exempel hello Ambari-Webbgränssnittet eller REST API.</span><span class="sxs-lookup"><span data-stu-id="11b96-118">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="11b96-119">**Secure Shell (SSH) användarnamn**: hello-inloggning som används vid åtkomst till hello klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="11b96-119">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="11b96-120">Som standard är hello lösenord hello samma som hello inloggningslösenordet för klustret.</span><span class="sxs-lookup"><span data-stu-id="11b96-120">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="11b96-121">**Resursgruppen**: hello resurs grupp toocreate hello klustret i.</span><span class="sxs-lookup"><span data-stu-id="11b96-121">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="11b96-122">**Plats**: hello Azure-region toocreate hello klustret i.</span><span class="sxs-lookup"><span data-stu-id="11b96-122">**Location**: hello Azure region toocreate hello cluster in.</span></span>
   
 ![Välj en prenumeration](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. <span data-ttu-id="11b96-124">Välj **kluster typen**, och sedan ange hello följande värden från **klusterkonfigurationen**:</span><span class="sxs-lookup"><span data-stu-id="11b96-124">Select **Cluster type**, and then set hello following values from **Cluster configuration**:</span></span>
   
    * <span data-ttu-id="11b96-125">**Klustertyp**: Kafka</span><span class="sxs-lookup"><span data-stu-id="11b96-125">**Cluster Type**: Kafka</span></span>

    * <span data-ttu-id="11b96-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="11b96-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="11b96-127">**Klusternivå**: Standard</span><span class="sxs-lookup"><span data-stu-id="11b96-127">**Cluster Tier**: Standard</span></span>
     
 <span data-ttu-id="11b96-128">Använd slutligen hello **Välj** knappen toosave inställningar.</span><span class="sxs-lookup"><span data-stu-id="11b96-128">Finally, use hello **Select** button toosave settings.</span></span>
     
 ![Välj klustertyp](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="11b96-130">När du har valt hello typ av kluster, använda hello __Välj__ knappen tooset hello typ av kluster.</span><span class="sxs-lookup"><span data-stu-id="11b96-130">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="11b96-131">Använd sedan hello __nästa__ toofinish grundläggande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="11b96-131">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="11b96-132">Från **Lagring** ska du välja eller skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="11b96-132">From **Storage**, select or create a Storage account.</span></span> <span data-ttu-id="11b96-133">Hello stegen i det här dokumentet, lämna hello andra fält till hello standardvärden.</span><span class="sxs-lookup"><span data-stu-id="11b96-133">For hello steps in this document, leave hello other fields at hello default values.</span></span> <span data-ttu-id="11b96-134">Använd hello __nästa__ toosave konfiguration för lagring.</span><span class="sxs-lookup"><span data-stu-id="11b96-134">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Ange hello konto lagringsinställningarna för HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. <span data-ttu-id="11b96-136">Från __program (valfritt)__väljer __nästa__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="11b96-136">From __Applications (optional)__, select __Next__ toocontinue.</span></span> <span data-ttu-id="11b96-137">Inga program krävs för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="11b96-137">No applications are required for this example.</span></span>

7. <span data-ttu-id="11b96-138">Från __klusterstorleken__väljer __nästa__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="11b96-138">From __Cluster size__, select __Next__ toocontinue.</span></span>

    > [!WARNING]
    > <span data-ttu-id="11b96-139">tooguarantee tillgängligheten för Kafka på HDInsight, klustret måste innehålla minst tre arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="11b96-139">tooguarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span>

    ![Ange hello Kafka klusterstorleken](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > <span data-ttu-id="11b96-141">Hej **diskar per arbetsnoden** post kontroller hello skalbarheten för Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="11b96-141">hello **disks per worker node** entry controls hello scalability of Kafka on HDInsight.</span></span> <span data-ttu-id="11b96-142">Mer information finns i [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (Konfigurera lagring och skalbarhet för Kafka i HDInsight).</span><span class="sxs-lookup"><span data-stu-id="11b96-142">For more information, see [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

8. <span data-ttu-id="11b96-143">Från __avancerade inställningar__väljer __nästa__ toocontinue.</span><span class="sxs-lookup"><span data-stu-id="11b96-143">From __Advanced settings__, select __Next__ toocontinue.</span></span>

9. <span data-ttu-id="11b96-144">Från hello **sammanfattning**, granska hello konfigurationen för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="11b96-144">From hello **Summary**, review hello configuration for hello cluster.</span></span> <span data-ttu-id="11b96-145">Använd hello __redigera__ länkar toochange inställningar som är felaktiga.</span><span class="sxs-lookup"><span data-stu-id="11b96-145">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="11b96-146">Slutligen Använd the__Create__ knappen toocreate hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="11b96-146">Finally, use the__Create__ button toocreate hello cluster.</span></span>
   
    ![Sammanfattning av klusterkonfiguration](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > <span data-ttu-id="11b96-148">Det kan ta upp too20 minuter toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="11b96-148">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="connect-toohello-cluster"></a><span data-ttu-id="11b96-149">Ansluta toohello kluster</span><span class="sxs-lookup"><span data-stu-id="11b96-149">Connect toohello cluster</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11b96-150">När du utför följande steg hello, måste du använda en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="11b96-150">When performing hello following steps, you must use an SSH client.</span></span> <span data-ttu-id="11b96-151">Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="11b96-151">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

<span data-ttu-id="11b96-152">Använd SSH tooconnect toohello kluster från din klient:</span><span class="sxs-lookup"><span data-stu-id="11b96-152">From your client, use SSH tooconnect toohello cluster:</span></span>

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

<span data-ttu-id="11b96-153">Ersätt **SSHUSER** med hello SSH-användarnamn som du angav när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="11b96-153">Replace **SSHUSER** with hello SSH username you provided during cluster creation.</span></span> <span data-ttu-id="11b96-154">Ersätt **KLUSTERNAMN** med hello hello klustrets namn.</span><span class="sxs-lookup"><span data-stu-id="11b96-154">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>

<span data-ttu-id="11b96-155">När du uppmanas ange hello-lösenord som du använde för hello SSH-konto.</span><span class="sxs-lookup"><span data-stu-id="11b96-155">When prompted, enter hello password you used for hello SSH account.</span></span>

<span data-ttu-id="11b96-156">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="11b96-156">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="11b96-157"><a id="getkafkainfo"></a>Hämta information om hello Zookeeper och Broker värden</span><span class="sxs-lookup"><span data-stu-id="11b96-157"><a id="getkafkainfo"></a>Get hello Zookeeper and Broker host information</span></span>

<span data-ttu-id="11b96-158">När du arbetar med Kafka, måste du känna två värden värden. Hej *Zookeeper* värdar och hello *Broker* värdar.</span><span class="sxs-lookup"><span data-stu-id="11b96-158">When working with Kafka, you must know two host values; hello *Zookeeper* hosts and hello *Broker* hosts.</span></span> <span data-ttu-id="11b96-159">Dessa värdar används med hello Kafka API och många hello verktyg som levereras med Kafka.</span><span class="sxs-lookup"><span data-stu-id="11b96-159">These hosts are used with hello Kafka API and many of hello utilities that ship with Kafka.</span></span>

<span data-ttu-id="11b96-160">Använd hello följande steg toocreate miljövariabler som innehåller information om hello värden.</span><span class="sxs-lookup"><span data-stu-id="11b96-160">Use hello following steps toocreate environment variables that contain hello host information.</span></span> <span data-ttu-id="11b96-161">De här miljövariablerna används i hello stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="11b96-161">These environment variables are used in hello steps in this document.</span></span>

1. <span data-ttu-id="11b96-162">Från ett SSH-anslutning toohello kluster, Använd hello följande kommando tooinstall hello `jq` verktyget.</span><span class="sxs-lookup"><span data-stu-id="11b96-162">From an SSH connection toohello cluster, use hello following command tooinstall hello `jq` utility.</span></span> <span data-ttu-id="11b96-163">Det här verktyget är används tooparse JSON-dokument och är användbart vid hämtning av information om hello broker värden:</span><span class="sxs-lookup"><span data-stu-id="11b96-163">This utility is used tooparse JSON documents, and is useful in retrieving hello broker host information:</span></span>
   
    ```bash
    sudo apt -y install jq
    ```

2. <span data-ttu-id="11b96-164">tooset hello miljövariablerna med information som hämtas från Ambari, Använd hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="11b96-164">tooset hello environment variables with information retrieved from Ambari, use hello following commands:</span></span>

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="11b96-165">Ange `CLUSTERNAME=` toohello namnet på hello Kafka klustret.</span><span class="sxs-lookup"><span data-stu-id="11b96-165">Set `CLUSTERNAME=` toohello name of hello Kafka cluster.</span></span> <span data-ttu-id="11b96-166">Ange `PASSWORD=` toohello inloggning (admin) lösenord som du använde när du skapar hello kluster.</span><span class="sxs-lookup"><span data-stu-id="11b96-166">Set `PASSWORD=` toohello login (admin) password you used when creating hello cluster.</span></span>

    <span data-ttu-id="11b96-167">hello följande är ett exempel på hello innehållet i `$KAFKAZKHOSTS`:</span><span class="sxs-lookup"><span data-stu-id="11b96-167">hello following text is an example of hello contents of `$KAFKAZKHOSTS`:</span></span>
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    <span data-ttu-id="11b96-168">hello följande är ett exempel på hello innehållet i `$KAFKABROKERS`:</span><span class="sxs-lookup"><span data-stu-id="11b96-168">hello following text is an example of hello contents of `$KAFKABROKERS`:</span></span>
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > <span data-ttu-id="11b96-169">Hej `cut` kommandot är används tootrim hello listan över värdar tootwo värdposter.</span><span class="sxs-lookup"><span data-stu-id="11b96-169">hello `cut` command is used tootrim hello list of hosts tootwo host entries.</span></span> <span data-ttu-id="11b96-170">Du behöver inte tooprovide hello fullständig lista över värdar när du skapar en Kafka konsument eller tillverkare.</span><span class="sxs-lookup"><span data-stu-id="11b96-170">You do not need tooprovide hello full list of hosts when creating a Kafka consumer or producer.</span></span>
   
    > [!WARNING]
    > <span data-ttu-id="11b96-171">Förlita dig inte på hello informationen som returneras från den här sessionen tooalways är korrekt.</span><span class="sxs-lookup"><span data-stu-id="11b96-171">Do not rely on hello information returned from this session tooalways be accurate.</span></span> <span data-ttu-id="11b96-172">Om du skalar hello kluster, nya mäklare läggs till eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="11b96-172">If you scale hello cluster, new brokers are added or removed.</span></span> <span data-ttu-id="11b96-173">Om ett fel inträffar och har ersatts av en nod, kan hello värdnamn för hello nod ändras.</span><span class="sxs-lookup"><span data-stu-id="11b96-173">If a failure occurs and a node is replaced, hello host name for hello node may change.</span></span>
    >
    > <span data-ttu-id="11b96-174">Du bör hämta information om hello Zookeeper och broker värdar strax innan du använder tooensure som du har en giltig information.</span><span class="sxs-lookup"><span data-stu-id="11b96-174">You should retrieve hello Zookeeper and broker hosts information shortly before you use it tooensure you have valid information.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="11b96-175">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="11b96-175">Create a topic</span></span>

<span data-ttu-id="11b96-176">Kafka lagrar dataströmmar i kategorier som kallas *ämnen*.</span><span class="sxs-lookup"><span data-stu-id="11b96-176">Kafka stores streams of data in categories called *topics*.</span></span> <span data-ttu-id="11b96-177">Använda ett skript som medföljer Kafka toocreate ett ämne från en SSH-anslutning tooa klustret headnode:</span><span class="sxs-lookup"><span data-stu-id="11b96-177">From An SSH connection tooa cluster headnode, use a script provided with Kafka toocreate a topic:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="11b96-178">Det här kommandot ansluter tooZookeeper med information om hello värden som lagras i `$KAFKAZKHOSTS`, och sedan skapa Kafka ämne med namnet **testa**.</span><span class="sxs-lookup"><span data-stu-id="11b96-178">This command connects tooZookeeper using hello host information stored in `$KAFKAZKHOSTS`, and then create Kafka topic named **test**.</span></span> <span data-ttu-id="11b96-179">Du kan verifiera hello avsnittet skapades med hjälp av hello följande skript toolist avsnitt:</span><span class="sxs-lookup"><span data-stu-id="11b96-179">You can verify that hello topic was created by using hello following script toolist topics:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="11b96-180">hello kommandots utdata visar Kafka avsnitt som innehåller hello **testa** avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-180">hello output of this command lists Kafka topics, which contains hello **test** topic.</span></span>

## <a name="produce-and-consume-records"></a><span data-ttu-id="11b96-181">Skapa och använda poster</span><span class="sxs-lookup"><span data-stu-id="11b96-181">Produce and consume records</span></span>

<span data-ttu-id="11b96-182">Kafka lagrar *poster* i ämnen.</span><span class="sxs-lookup"><span data-stu-id="11b96-182">Kafka stores *records* in topics.</span></span> <span data-ttu-id="11b96-183">Poster produceras av *producenter*, och används av *konsumenter*.</span><span class="sxs-lookup"><span data-stu-id="11b96-183">Records are produced by *producers*, and consumed by *consumers*.</span></span> <span data-ttu-id="11b96-184">Producenter hämtar poster från asynkrona Kafka-*meddelandeköer*.</span><span class="sxs-lookup"><span data-stu-id="11b96-184">Producers retrieve records from Kafka *brokers*.</span></span> <span data-ttu-id="11b96-185">Varje arbetsnod i HDInsight-klustret är en asynkron Kafka-meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="11b96-185">Each worker node in your HDInsight cluster is a Kafka broker.</span></span>

<span data-ttu-id="11b96-186">Använd följande steg toostore poster i hello test avsnittet du skapade tidigare och läsa dem med hjälp av en konsument hello:</span><span class="sxs-lookup"><span data-stu-id="11b96-186">Use hello following steps toostore records into hello test topic you created earlier, and then read them using a consumer:</span></span>

1. <span data-ttu-id="11b96-187">Använda ett skript som medföljer Kafka toowrite poster toohello avsnittet från hello SSH-session:</span><span class="sxs-lookup"><span data-stu-id="11b96-187">From hello SSH session, use a script provided with Kafka toowrite records toohello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    <span data-ttu-id="11b96-188">Du returneras inte toohello fråga efter det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="11b96-188">You are not returned toohello prompt after this command.</span></span> <span data-ttu-id="11b96-189">I stället skriver några textmeddelanden och använder sedan **Ctrl + C** toostop skicka toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-189">Instead, type a few text messages and then use **Ctrl + C** toostop sending toohello topic.</span></span> <span data-ttu-id="11b96-190">Varje rad skickas som en separat post.</span><span class="sxs-lookup"><span data-stu-id="11b96-190">Each line is sent as a separate record.</span></span>

2. <span data-ttu-id="11b96-191">Använda ett skript som medföljer Kafka tooread poster från hello avsnittet:</span><span class="sxs-lookup"><span data-stu-id="11b96-191">Use a script provided with Kafka tooread records from hello topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    <span data-ttu-id="11b96-192">Detta kommando hämtar hello poster hello artikeln och visar dem.</span><span class="sxs-lookup"><span data-stu-id="11b96-192">This command retrieves hello records from hello topic and displays them.</span></span> <span data-ttu-id="11b96-193">Med hjälp av `--from-beginning` anger hello konsumenten toostart från hello början av hello stream, så hämtas alla poster.</span><span class="sxs-lookup"><span data-stu-id="11b96-193">Using `--from-beginning` tells hello consumer toostart from hello beginning of hello stream, so all records are retrieved.</span></span>

3. <span data-ttu-id="11b96-194">Använd __Ctrl + C__ toostop hello konsumenten.</span><span class="sxs-lookup"><span data-stu-id="11b96-194">Use __Ctrl + C__ toostop hello consumer.</span></span>

## <a name="producer-and-consumer-api"></a><span data-ttu-id="11b96-195">Producent- och konsument-API</span><span class="sxs-lookup"><span data-stu-id="11b96-195">Producer and consumer API</span></span>

<span data-ttu-id="11b96-196">Du kan också programmässigt skapa och använda poster med hello [Kafka API: er](http://kafka.apache.org/documentation#api).</span><span class="sxs-lookup"><span data-stu-id="11b96-196">You can also programmatically produce and consume records using hello [Kafka APIs](http://kafka.apache.org/documentation#api).</span></span> <span data-ttu-id="11b96-197">toobuild Java producent och konsumenten, använder du hello följande steg från din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="11b96-197">toobuild a Java producer and consumer, use hello following steps from your development environment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11b96-198">Du måste ha följande komponenter installerade i din utvecklingsmiljö hello:</span><span class="sxs-lookup"><span data-stu-id="11b96-198">You must have hello following components installed in your development environment:</span></span>
>
> * <span data-ttu-id="11b96-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, till exempel OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="11b96-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or an equivalent, such as OpenJDK.</span></span>
>
> * [<span data-ttu-id="11b96-200">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="11b96-200">Apache Maven</span></span>](http://maven.apache.org/)
>
> * <span data-ttu-id="11b96-201">En SSH-klienten och hello `scp` kommando.</span><span class="sxs-lookup"><span data-stu-id="11b96-201">An SSH client and hello `scp` command.</span></span> <span data-ttu-id="11b96-202">Mer information finns i hello [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="11b96-202">For more information, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

1. <span data-ttu-id="11b96-203">Hämta hello exempel från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="11b96-203">Download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span></span> <span data-ttu-id="11b96-204">Använd hello-projekt i hello exempelvis hello producenten/konsumenten `Producer-Consumer` directory.</span><span class="sxs-lookup"><span data-stu-id="11b96-204">For hello producer/consumer example, use hello project in hello `Producer-Consumer` directory.</span></span> <span data-ttu-id="11b96-205">Det här exemplet innehåller hello följande klasser:</span><span class="sxs-lookup"><span data-stu-id="11b96-205">This example contains hello following classes:</span></span>
   
    * <span data-ttu-id="11b96-206">**Kör** -startar hello konsumenten eller tillverkare.</span><span class="sxs-lookup"><span data-stu-id="11b96-206">**Run** - starts either hello consumer or producer.</span></span>

    * <span data-ttu-id="11b96-207">**Producenten** -butiker 1 000 000 poster toohello avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-207">**Producer** - stores 1,000,000 records toohello topic.</span></span>

    * <span data-ttu-id="11b96-208">**Konsumenten** -läser poster från hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-208">**Consumer** - reads records from hello topic.</span></span>

2. <span data-ttu-id="11b96-209">toocreate ett jar-paket, ändra kataloger toohello placeringen hello `Producer-Consumer` katalogen och Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="11b96-209">toocreate a jar package, change directories toohello location of hello `Producer-Consumer` directory and use hello following command:</span></span>

    ```
    mvn clean package
    ```

    <span data-ttu-id="11b96-210">Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="11b96-210">This command creates a directory named `target`, that contains a file named `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="11b96-211">Använd hello följande kommandon toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` filen tooyour HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="11b96-211">Use hello following commands toocopy hello `kafka-producer-consumer-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    <span data-ttu-id="11b96-212">Ersätt **SSHUSER** med hello SSH-användare för klustret och Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="11b96-212">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="11b96-213">När du uppmanas ange hello lösenord för hello SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="11b96-213">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="11b96-214">En gång hello `scp` kommandot slutförs kopiera hello-fil, ansluta toohello kluster med SSH.</span><span class="sxs-lookup"><span data-stu-id="11b96-214">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH.</span></span> <span data-ttu-id="11b96-215">Använd följande kommando toowrite poster toohello testämne hello:</span><span class="sxs-lookup"><span data-stu-id="11b96-215">Use hello following command toowrite records toohello test topic:</span></span>

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. <span data-ttu-id="11b96-216">När hello processen är klar använder du hello efter kommandot tooread från hello avsnittet:</span><span class="sxs-lookup"><span data-stu-id="11b96-216">Once hello process has finished, use hello following command tooread from hello topic:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    <span data-ttu-id="11b96-217">hello poster läsas tillsammans med antalet poster visas.</span><span class="sxs-lookup"><span data-stu-id="11b96-217">hello records read, along with a count of records, is displayed.</span></span> <span data-ttu-id="11b96-218">Du kan se några fler än 1 000 000 loggas som du har skickat flera poster toohello avsnitt med ett skript i ett tidigare steg.</span><span class="sxs-lookup"><span data-stu-id="11b96-218">You may see a few more than 1,000,000 logged as you sent several records toohello topic using a script in an earlier step.</span></span>

6. <span data-ttu-id="11b96-219">Använd __Ctrl + C__ tooexit hello konsumenten.</span><span class="sxs-lookup"><span data-stu-id="11b96-219">Use __Ctrl + C__ tooexit hello consumer.</span></span>

### <a name="multiple-consumers"></a><span data-ttu-id="11b96-220">Flera konsumenter</span><span class="sxs-lookup"><span data-stu-id="11b96-220">Multiple consumers</span></span>

<span data-ttu-id="11b96-221">Kafka-konsumenter använder en konsumentgrupp vid läsning av poster.</span><span class="sxs-lookup"><span data-stu-id="11b96-221">Kafka consumers use a consumer group when reading records.</span></span> <span data-ttu-id="11b96-222">Hello samma grupp med flera konsumenter balanserade leder till att läsa in läsningar av ett ämne.</span><span class="sxs-lookup"><span data-stu-id="11b96-222">Using hello same group with multiple consumers results in load balanced reads from a topic.</span></span> <span data-ttu-id="11b96-223">Varje konsument i hello-gruppen tar emot en del av hello poster.</span><span class="sxs-lookup"><span data-stu-id="11b96-223">Each consumer in hello group receives a portion of hello records.</span></span> <span data-ttu-id="11b96-224">toosee hur det fungerar, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="11b96-224">toosee this process in action, use hello following steps:</span></span>

1. <span data-ttu-id="11b96-225">Öppna ett nytt SSH-session toohello kluster, så att du har två av dem.</span><span class="sxs-lookup"><span data-stu-id="11b96-225">Open a new SSH session toohello cluster, so that you have two of them.</span></span> <span data-ttu-id="11b96-226">I varje session använder hello följande toostart hello konsumenter med samma konsumenten grupp-ID:</span><span class="sxs-lookup"><span data-stu-id="11b96-226">In each session, use hello following toostart a consumer with hello same consumer group ID:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    <span data-ttu-id="11b96-227">Detta kommando startar konsumenter med hjälp av grupp-ID för hello `mygroup`.</span><span class="sxs-lookup"><span data-stu-id="11b96-227">This command starts a consumer using hello group ID `mygroup`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="11b96-228">Använd hello kommandon i hello [hämta hello Zookeeper och Broker värdinformation](#getkafkainfo) avsnittet tooset `$KAFKABROKERS` för den här SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="11b96-228">Use hello commands in hello [Get hello Zookeeper and Broker host information](#getkafkainfo) section tooset `$KAFKABROKERS` for this SSH session.</span></span>

2. <span data-ttu-id="11b96-229">Titta på som varje session antal hello-poster som tas emot från hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-229">Watch as each session counts hello records it receives from hello topic.</span></span> <span data-ttu-id="11b96-230">hello totalt båda sessioner bör vara hello samma som du tidigare fått från en klient.</span><span class="sxs-lookup"><span data-stu-id="11b96-230">hello total of both sessions should be hello same as you received previously from one consumer.</span></span>

<span data-ttu-id="11b96-231">Användning av klienter i samma grupp hanteras via hello partitioner för hello avsnittet hello.</span><span class="sxs-lookup"><span data-stu-id="11b96-231">Consumption by clients within hello same group is handled through hello partitions for hello topic.</span></span> <span data-ttu-id="11b96-232">För hello `test` avsnittet skapade tidigare, den har åtta partitioner.</span><span class="sxs-lookup"><span data-stu-id="11b96-232">For hello `test` topic created earlier, it has eight partitions.</span></span> <span data-ttu-id="11b96-233">Om du öppnar åtta SSH-sessioner och starta en konsument i alla sessioner läser varje konsument poster från en enda partition för hello ämnet.</span><span class="sxs-lookup"><span data-stu-id="11b96-233">If you open eight SSH sessions and launch a consumer in all sessions, each consumer reads records from a single partition for hello topic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11b96-234">Det får inte finnas flera instanser av konsumenten i en konsumentgrupp än partitioner.</span><span class="sxs-lookup"><span data-stu-id="11b96-234">There cannot be more consumer instances in a consumer group than partitions.</span></span> <span data-ttu-id="11b96-235">I det här exemplet kan en konsumentgrupp innehålla upp tooeight konsumenter eftersom hello antalet partitioner i hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-235">In this example, one consumer group can contain up tooeight consumers since that is hello number of partitions in hello topic.</span></span> <span data-ttu-id="11b96-236">Du kan även ha flera konsumentgrupper med högst åtta konsumenter vardera.</span><span class="sxs-lookup"><span data-stu-id="11b96-236">Or you can have multiple consumer groups, each with no more than eight consumers.</span></span>

<span data-ttu-id="11b96-237">Poster som lagras i Kafka lagras i hello ordning de tas emot inom en partition.</span><span class="sxs-lookup"><span data-stu-id="11b96-237">Records stored in Kafka are stored in hello order they are received within a partition.</span></span> <span data-ttu-id="11b96-238">tooachieve i sorterade levereras poster *inom en partition*, skapa en konsumentgrupp där hello antalet instanser av konsumenten matchar hello antalet partitioner.</span><span class="sxs-lookup"><span data-stu-id="11b96-238">tooachieve in-ordered delivery for records *within a partition*, create a consumer group where hello number of consumer instances matches hello number of partitions.</span></span> <span data-ttu-id="11b96-239">tooachieve i sorterade levereras poster *i hello avsnittet*, skapa en konsumentgrupp med endast en konsument-instans.</span><span class="sxs-lookup"><span data-stu-id="11b96-239">tooachieve in-ordered delivery for records *within hello topic*, create a consumer group with only one consumer instance.</span></span>

## <a name="streaming-api"></a><span data-ttu-id="11b96-240">Strömmande API</span><span class="sxs-lookup"><span data-stu-id="11b96-240">Streaming API</span></span>

<span data-ttu-id="11b96-241">hello streaming API har lagts till tooKafka i version 0.10.0; tidigare versioner förlitar sig på Apache Spark eller Storm för bearbetning av dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="11b96-241">hello streaming API was added tooKafka in version 0.10.0; earlier versions rely on Apache Spark or Storm for stream processing.</span></span>

1. <span data-ttu-id="11b96-242">Om du inte redan har gjort hämta hello exempel från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="11b96-242">If you haven't already done so, download hello examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) tooyour development environment.</span></span> <span data-ttu-id="11b96-243">För hello strömning exempel, använda hello projektet på hello `streaming` directory.</span><span class="sxs-lookup"><span data-stu-id="11b96-243">For hello streaming example, use hello project in hello `streaming` directory.</span></span>
   
    <span data-ttu-id="11b96-244">Det här projektet innehåller endast en klass `Stream`, som läser poster från hello `test` ämne som skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="11b96-244">This project contains only one class, `Stream`, which reads records from hello `test` topic created previously.</span></span> <span data-ttu-id="11b96-245">Det antal hello ord läsa och genererar varje ord och räknas tooa avsnitt med namnet `wordcounts`.</span><span class="sxs-lookup"><span data-stu-id="11b96-245">It counts hello words read, and emits each word and count tooa topic named `wordcounts`.</span></span> <span data-ttu-id="11b96-246">Hej `wordcounts` avsnittet skapas i ett senare steg i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-246">hello `wordcounts` topic is created in a later step in this section.</span></span>

2. <span data-ttu-id="11b96-247">Ändra kataloger toohello placeringen hello från kommandoraden i din utvecklingsmiljö hello `Streaming` katalogen och Använd hello efter kommandot toocreate ett jar-paket:</span><span class="sxs-lookup"><span data-stu-id="11b96-247">From hello command line in your development environment, change directories toohello location of hello `Streaming` directory, and then use hello following command toocreate a jar package:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="11b96-248">Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-streaming-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="11b96-248">This command creates a directory named `target`, which contains a file named `kafka-streaming-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="11b96-249">Använd hello följande kommandon toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` filen tooyour HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="11b96-249">Use hello following commands toocopy hello `kafka-streaming-1.0-SNAPSHOT.jar` file tooyour HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    <span data-ttu-id="11b96-250">Ersätt **SSHUSER** med hello SSH-användare för klustret och Ersätt **KLUSTERNAMN** med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="11b96-250">Replace **SSHUSER** with hello SSH user for your cluster, and replace **CLUSTERNAME** with hello name of your cluster.</span></span> <span data-ttu-id="11b96-251">När du uppmanas ange hello lösenord för hello SSH-användare.</span><span class="sxs-lookup"><span data-stu-id="11b96-251">When prompted enter hello password for hello SSH user.</span></span>

4. <span data-ttu-id="11b96-252">En gång hello `scp` kommandot slutförs kopiera hello-fil, ansluta toohello kluster med SSH och sedan använda följande kommando toocreate hello hello `wordcounts` avsnittet:</span><span class="sxs-lookup"><span data-stu-id="11b96-252">Once hello `scp` command finishes copying hello file, connect toohello cluster using SSH, and then use hello following command toocreate hello `wordcounts` topic:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. <span data-ttu-id="11b96-253">Därefter starta hello strömning processen med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="11b96-253">Next, start hello streaming process by using hello following command:</span></span>
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    <span data-ttu-id="11b96-254">Detta kommando startar hello strömning processen i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="11b96-254">This command starts hello streaming process in hello background.</span></span>

6. <span data-ttu-id="11b96-255">Använd hello följande kommando toosend meddelanden toohello `test` avsnittet.</span><span class="sxs-lookup"><span data-stu-id="11b96-255">Use hello following command toosend messages toohello `test` topic.</span></span> <span data-ttu-id="11b96-256">Dessa meddelanden bearbetas av hello strömning exempel:</span><span class="sxs-lookup"><span data-stu-id="11b96-256">These messages are processed by hello streaming example:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. <span data-ttu-id="11b96-257">Använd hello följande tooview hello kommandoutdata som skrivs toohello `wordcounts` ämne av hello strömning processen:</span><span class="sxs-lookup"><span data-stu-id="11b96-257">Use hello following command tooview hello output that is written toohello `wordcounts` topic by hello streaming process:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > <span data-ttu-id="11b96-258">tooview hello data, måste du se hello konsumenten tooprint hello nyckel och hello funktionen för avserialisering toouse för hello nyckel och värde.</span><span class="sxs-lookup"><span data-stu-id="11b96-258">tooview hello data, you must tell hello consumer tooprint hello key and hello deserializer toouse for hello key and value.</span></span> <span data-ttu-id="11b96-259">hello nyckelnamn är hello word och hello nyckelvärdet innehåller hello count.</span><span class="sxs-lookup"><span data-stu-id="11b96-259">hello key name is hello word, and hello key value contains hello count.</span></span>
   
    <span data-ttu-id="11b96-260">hello utdata är liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="11b96-260">hello output is similar toohello following text:</span></span>
   
        dwarfs  13635
        ago     13664
        snow    13636
        dwarfs  13636
        ago     13665
        a       13803
        ago     13666
        a       13804
        ago     13667
        ago     13668
        jumped  13640
        jumped  13641
        a       13805
        snow    13637
   
    > [!NOTE]
    > <span data-ttu-id="11b96-261">hello antal ökar varje gång ett ord har påträffats.</span><span class="sxs-lookup"><span data-stu-id="11b96-261">hello count increments each time a word is encountered.</span></span>

7. <span data-ttu-id="11b96-262">Använd hello __Ctrl + C__ tooexit hello konsumenten och Använd sedan hello `fg` kommandot toobring hello strömmande bakgrund uppgiften tillbaka toohello förgrunden.</span><span class="sxs-lookup"><span data-stu-id="11b96-262">Use hello __Ctrl + C__ tooexit hello consumer, then use hello `fg` command toobring hello streaming background task back toohello foreground.</span></span> <span data-ttu-id="11b96-263">Använd __Ctrl + C__ tooexit den också.</span><span class="sxs-lookup"><span data-stu-id="11b96-263">Use __Ctrl + C__ tooexit it also.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="11b96-264">Ta bort hello kluster</span><span class="sxs-lookup"><span data-stu-id="11b96-264">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="11b96-265">Felsöka</span><span class="sxs-lookup"><span data-stu-id="11b96-265">Troubleshoot</span></span>

<span data-ttu-id="11b96-266">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="11b96-266">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="11b96-267">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="11b96-267">Next steps</span></span>

<span data-ttu-id="11b96-268">I det här dokumentet har du lärt dig hello grunderna i att arbeta med Apache Kafka på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="11b96-268">In this document, you have learned hello basics of working with Apache Kafka on HDInsight.</span></span> <span data-ttu-id="11b96-269">Använd följande toolearn mer information om hur du arbetar med Kafka hello:</span><span class="sxs-lookup"><span data-stu-id="11b96-269">Use hello following toolearn more about working with Kafka:</span></span>

* [<span data-ttu-id="11b96-270">Säkerställ en hög tillgänglighet för dina data med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-270">Ensure high availability of your data with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-high-availability.md)
* [<span data-ttu-id="11b96-271">Öka skalbarheten genom att konfigurera hanterade diskar med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-271">Increase scalability by configuring managed disks with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* <span data-ttu-id="11b96-272">[Apache Kafka-dokumentation](http://kafka.apache.org/documentation.html) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="11b96-272">[Apache Kafka documentation](http://kafka.apache.org/documentation.html) at kafka.apache.org.</span></span>
* [<span data-ttu-id="11b96-273">Använda MirrorMaker toocreate en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-273">Use MirrorMaker toocreate a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="11b96-274">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-274">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="11b96-275">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="11b96-275">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="11b96-276">Ansluta tooKafka via ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="11b96-276">Connect tooKafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
