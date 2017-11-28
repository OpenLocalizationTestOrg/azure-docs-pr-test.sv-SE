---
title: "Kom igång med Apache Kafka – Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du skapar ett Apache Kafka-kluster i Azure HDInsight. Lär dig hur du skapar ämnen, prenumeranter och konsumenter."
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
ms.openlocfilehash: 03e6996f0f44e04978080b3bd267e924f342b7fc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="start-with-apache-kafka-preview-on-hdinsight"></a><span data-ttu-id="f6b71-104">Kom igång med Apache Kafka (förhandsversion) i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-104">Start with Apache Kafka (preview) on HDInsight</span></span>

<span data-ttu-id="f6b71-105">Lär dig hur du skapar och använder ett [Apache Kafka](https://kafka.apache.org)-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6b71-105">Learn how to create and use an [Apache Kafka](https://kafka.apache.org) cluster on Azure HDInsight.</span></span> <span data-ttu-id="f6b71-106">Kafka är en distribuerad direktuppspelningsplattform med öppen källkod som är tillgänglig i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6b71-106">Kafka is an open-source, distributed streaming platform that is available with HDInsight.</span></span> <span data-ttu-id="f6b71-107">Den används ofta som en asynkron meddelandekö eftersom den innehåller funktioner som påminner om en publicera-prenumerera-meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="f6b71-107">It is often used as a message broker, as it provides functionality similar to a publish-subscribe message queue.</span></span>

> [!NOTE]
> <span data-ttu-id="f6b71-108">Det finns för närvarande två versioner av Kafka med HDInsight; 0.9.0 (HDInsight 3.4) och 0.10.0 (HDInsight 3.5 och 3.6).</span><span class="sxs-lookup"><span data-stu-id="f6b71-108">There are currently two versions of Kafka available with HDInsight; 0.9.0 (HDInsight 3.4) and 0.10.0 (HDInsight 3.5 and 3.6).</span></span> <span data-ttu-id="f6b71-109">Stegen i det här dokumentet förutsätter att du använder Kafka på HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="f6b71-109">The steps in this document assume that you are using Kafka on HDInsight 3.6.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="create-a-kafka-cluster"></a><span data-ttu-id="f6b71-110">Skapa ett Kafka-kluster</span><span class="sxs-lookup"><span data-stu-id="f6b71-110">Create a Kafka cluster</span></span>

<span data-ttu-id="f6b71-111">Använd följande steg om du vill skapa en Kafka i HDInsight-klustret:</span><span class="sxs-lookup"><span data-stu-id="f6b71-111">Use the following steps to create a Kafka on HDInsight cluster:</span></span>

1. <span data-ttu-id="f6b71-112">Från [Azure Portal](https://portal.azure.com) väljer du **+ NY**, **Intelligence + Analytics** och väljer sedan **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="f6b71-112">From the [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>
   
    ![Skapa ett HDInsight-kluster](./media/hdinsight-apache-kafka-get-started/create-hdinsight.png)

2. <span data-ttu-id="f6b71-114">Från **Grundläggande**, ange följande information:</span><span class="sxs-lookup"><span data-stu-id="f6b71-114">From **Basics**, enter the following information:</span></span>

    * <span data-ttu-id="f6b71-115">**Klusternamn**: Namnet på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-115">**Cluster Name**: The name of the HDInsight cluster.</span></span>
    * <span data-ttu-id="f6b71-116">**Prenumeration**: Välj den prenumeration som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="f6b71-116">**Subscription**: Select the subscription to use.</span></span>
    * <span data-ttu-id="f6b71-117">**Användarnamn för klusterinloggning** och **Lösenord för klusterinloggning**: Inloggningen vid åtkomst till klustret via HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f6b71-117">**Cluster login username** and **Cluster login password**: The login when accessing the cluster over HTTPS.</span></span> <span data-ttu-id="f6b71-118">Du kan använda dessa autentiseringsuppgifter för att få åtkomst till tjänster som Ambari-webbgränssnittet eller REST API.</span><span class="sxs-lookup"><span data-stu-id="f6b71-118">You use these credentials to access services such as the Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="f6b71-119">**Secure Shell-användarnamn (SSH)**: Den inloggning som används vid åtkomst till klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="f6b71-119">**Secure Shell (SSH) username**: The login used when accessing the cluster over SSH.</span></span> <span data-ttu-id="f6b71-120">Som standard är lösenordet detsamma som lösenordet för klusterinloggning.</span><span class="sxs-lookup"><span data-stu-id="f6b71-120">By default the password is the same as the cluster login password.</span></span>
    * <span data-ttu-id="f6b71-121">**Resursgrupp**: Resursgruppen som klustret ska skapas i.</span><span class="sxs-lookup"><span data-stu-id="f6b71-121">**Resource Group**: The resource group to create the cluster in.</span></span>
    * <span data-ttu-id="f6b71-122">**Plats**: Azure-region som klustret ska skapas i.</span><span class="sxs-lookup"><span data-stu-id="f6b71-122">**Location**: The Azure region to create the cluster in.</span></span>
   
 ![Välj en prenumeration](./media/hdinsight-apache-kafka-get-started/hdinsight-basic-configuration.png)

3. <span data-ttu-id="f6b71-124">Välj **Klustertyp** och ange följande värden från **Klusterkonfiguration**:</span><span class="sxs-lookup"><span data-stu-id="f6b71-124">Select **Cluster type**, and then set the following values from **Cluster configuration**:</span></span>
   
    * <span data-ttu-id="f6b71-125">**Klustertyp**: Kafka</span><span class="sxs-lookup"><span data-stu-id="f6b71-125">**Cluster Type**: Kafka</span></span>

    * <span data-ttu-id="f6b71-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="f6b71-126">**Version**: Kafka 0.10.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="f6b71-127">**Klusternivå**: Standard</span><span class="sxs-lookup"><span data-stu-id="f6b71-127">**Cluster Tier**: Standard</span></span>
     
 <span data-ttu-id="f6b71-128">Slutligen kan spara inställningarna med kommandot **Välj**.</span><span class="sxs-lookup"><span data-stu-id="f6b71-128">Finally, use the **Select** button to save settings.</span></span>
     
 ![Välj klustertyp](./media/hdinsight-apache-kafka-get-started/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="f6b71-130">När du har valt klustertypen anger du klustertypen med hjälp av knappen __Välj__.</span><span class="sxs-lookup"><span data-stu-id="f6b71-130">After selecting the cluster type, use the __Select__ button to set the cluster type.</span></span> <span data-ttu-id="f6b71-131">Använd sedan knappen __Nästa__ och slutföra den grundläggande konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f6b71-131">Next, use the __Next__ button to finish basic configuration.</span></span>

5. <span data-ttu-id="f6b71-132">Från **Lagring** ska du välja eller skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f6b71-132">From **Storage**, select or create a Storage account.</span></span> <span data-ttu-id="f6b71-133">Lämna övriga fält på standardvärden för stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-133">For the steps in this document, leave the other fields at the default values.</span></span> <span data-ttu-id="f6b71-134">Spara lagringskonfigurationen genom att klicka på __Nästa__.</span><span class="sxs-lookup"><span data-stu-id="f6b71-134">Use the __Next__ button to save storage configuration.</span></span>

    ![Ange inställningarna för lagringskontot för HDInsight](./media/hdinsight-apache-kafka-get-started/set-hdinsight-storage-account.png)

6. <span data-ttu-id="f6b71-136">Från __Program (valfritt)__ väljer du __Nästa__ för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="f6b71-136">From __Applications (optional)__, select __Next__ to continue.</span></span> <span data-ttu-id="f6b71-137">Inga program krävs för det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-137">No applications are required for this example.</span></span>

7. <span data-ttu-id="f6b71-138">Från __Klusterstorlek__ väljer du __Nästa__ för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="f6b71-138">From __Cluster size__, select __Next__ to continue.</span></span>

    > [!WARNING]
    > <span data-ttu-id="f6b71-139">Klustret måste innehålla minst tre arbetsnoder för att garantera tillgängligheten för Kafka i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6b71-139">To guarantee availability of Kafka on HDInsight, your cluster must contain at least three worker nodes.</span></span>

    ![Ange klusterstorlek för Kafka](./media/hdinsight-apache-kafka-get-started/kafka-cluster-size.png)

    > [!NOTE]
    > <span data-ttu-id="f6b71-141">Antalet **diskar per arbetsnod** anger hur skalbart Kafka är i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6b71-141">The **disks per worker node** entry controls the scalability of Kafka on HDInsight.</span></span> <span data-ttu-id="f6b71-142">Mer information finns i [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md) (Konfigurera lagring och skalbarhet för Kafka i HDInsight).</span><span class="sxs-lookup"><span data-stu-id="f6b71-142">For more information, see [Configure storage and scalability of Kafka on HDInsight](hdinsight-apache-kafka-scalability.md).</span></span>

8. <span data-ttu-id="f6b71-143">Från __Avancerade inställningar__ väljer du __Nästa__ för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="f6b71-143">From __Advanced settings__, select __Next__ to continue.</span></span>

9. <span data-ttu-id="f6b71-144">Från **Sammanfattning** kan du granska konfigurationen för klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-144">From the **Summary**, review the configuration for the cluster.</span></span> <span data-ttu-id="f6b71-145">Använd länkarna __Redigera__ om du behöver ändra eventuella inställningar som är felaktiga.</span><span class="sxs-lookup"><span data-stu-id="f6b71-145">Use the __Edit__ links to change any settings that are incorrect.</span></span> <span data-ttu-id="f6b71-146">Till sist skapar du klustret genom att klicka på Skapa.</span><span class="sxs-lookup"><span data-stu-id="f6b71-146">Finally, use the__Create__ button to create the cluster.</span></span>
   
    ![Sammanfattning av klusterkonfiguration](./media/hdinsight-apache-kafka-get-started/hdinsight-configuration-summary.png)
   
    > [!NOTE]
    > <span data-ttu-id="f6b71-148">Det kan ta upp till 20 minuter att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-148">It can take up to 20 minutes to create the cluster.</span></span>

## <a name="connect-to-the-cluster"></a><span data-ttu-id="f6b71-149">Anslut till klustret</span><span class="sxs-lookup"><span data-stu-id="f6b71-149">Connect to the cluster</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6b71-150">När du utför följande steg måste du använda en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="f6b71-150">When performing the following steps, you must use an SSH client.</span></span> <span data-ttu-id="f6b71-151">Mer information finns i dokumentet [Använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f6b71-151">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

<span data-ttu-id="f6b71-152">Anslut till klustret via SSH från klienten:</span><span class="sxs-lookup"><span data-stu-id="f6b71-152">From your client, use SSH to connect to the cluster:</span></span>

```ssh SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net```

<span data-ttu-id="f6b71-153">Ersätt **SSHUSER** med det SSH-användarnamn som du angav när klustret skapades.</span><span class="sxs-lookup"><span data-stu-id="f6b71-153">Replace **SSHUSER** with the SSH username you provided during cluster creation.</span></span> <span data-ttu-id="f6b71-154">Ersätt **CLUSTERNAME** med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-154">Replace **CLUSTERNAME** with the name of the cluster.</span></span>

<span data-ttu-id="f6b71-155">När du uppmanas till det anger du det lösenord som du använde för SSH-kontot.</span><span class="sxs-lookup"><span data-stu-id="f6b71-155">When prompted, enter the password you used for the SSH account.</span></span>

<span data-ttu-id="f6b71-156">Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).</span><span class="sxs-lookup"><span data-stu-id="f6b71-156">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="f6b71-157"><a id="getkafkainfo"></a>Hämta information om värden i Zookeeper och Broker</span><span class="sxs-lookup"><span data-stu-id="f6b71-157"><a id="getkafkainfo"></a>Get the Zookeeper and Broker host information</span></span>

<span data-ttu-id="f6b71-158">När du arbetar med Kafka, måste du känna två värdvärden: *Zookeeper*-värdarna och *Broker*-värdarna.</span><span class="sxs-lookup"><span data-stu-id="f6b71-158">When working with Kafka, you must know two host values; the *Zookeeper* hosts and the *Broker* hosts.</span></span> <span data-ttu-id="f6b71-159">Dessa värdar används med Kafka-API och många av de verktyg som levereras med Kafka.</span><span class="sxs-lookup"><span data-stu-id="f6b71-159">These hosts are used with the Kafka API and many of the utilities that ship with Kafka.</span></span>

<span data-ttu-id="f6b71-160">Skapa miljövariabler som innehåller värdinformationen med hjälp av följande steg.</span><span class="sxs-lookup"><span data-stu-id="f6b71-160">Use the following steps to create environment variables that contain the host information.</span></span> <span data-ttu-id="f6b71-161">Dessa miljövariabler används i stegen i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-161">These environment variables are used in the steps in this document.</span></span>

1. <span data-ttu-id="f6b71-162">Använd följande kommando från en SSH-anslutning till klustret för att installera verktyget `jq`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-162">From an SSH connection to the cluster, use the following command to install the `jq` utility.</span></span> <span data-ttu-id="f6b71-163">Det här verktyget används för att parsa JSON-dokument och är användbart vid hämtning av värdinformation för asynkron meddelandekö:</span><span class="sxs-lookup"><span data-stu-id="f6b71-163">This utility is used to parse JSON documents, and is useful in retrieving the broker host information:</span></span>
   
    ```bash
    sudo apt -y install jq
    ```

2. <span data-ttu-id="f6b71-164">Ange miljövariabler med information som hämtas från Ambari med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="f6b71-164">To set the environment variables with information retrieved from Ambari, use the following commands:</span></span>

    ```bash
    CLUSTERNAME='your cluster name'
    PASSWORD='your cluster password'
    export KAFKAZKHOSTS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER | jq -r '["\(.host_components[].HostRoles.host_name):2181"] | join(",")' | cut -d',' -f1,2`

    export KAFKABROKERS=`curl -sS -u admin:$PASSWORD -G https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/KAFKA/components/KAFKA_BROKER | jq -r '["\(.host_components[].HostRoles.host_name):9092"] | join(",")' | cut -d',' -f1,2`

    echo '$KAFKAZKHOSTS='$KAFKAZKHOSTS
    echo '$KAFKABROKERS='$KAFKABROKERS
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="f6b71-165">Ange `CLUSTERNAME=` till namnet på Kafka-klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-165">Set `CLUSTERNAME=` to the name of the Kafka cluster.</span></span> <span data-ttu-id="f6b71-166">Ange `PASSWORD=` med inloggningslösenordet (för administratör) som du använde när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="f6b71-166">Set `PASSWORD=` to the login (admin) password you used when creating the cluster.</span></span>

    <span data-ttu-id="f6b71-167">Följande text är ett exempel på innehållet i `$KAFKAZKHOSTS`:</span><span class="sxs-lookup"><span data-stu-id="f6b71-167">The following text is an example of the contents of `$KAFKAZKHOSTS`:</span></span>
   
    `zk0-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181,zk2-kafka.eahjefxxp1netdbyklgqj5y1ud.ex.internal.cloudapp.net:2181`
   
    <span data-ttu-id="f6b71-168">Följande text är ett exempel på innehållet i `$KAFKABROKERS`:</span><span class="sxs-lookup"><span data-stu-id="f6b71-168">The following text is an example of the contents of `$KAFKABROKERS`:</span></span>
   
    `wn1-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092,wn0-kafka.eahjefxxp1netdbyklgqj5y1ud.cx.internal.cloudapp.net:9092`

    > [!NOTE]
    > <span data-ttu-id="f6b71-169">Kommandot `cut` används för att trimma listan över värdar till två värdposter.</span><span class="sxs-lookup"><span data-stu-id="f6b71-169">The `cut` command is used to trim the list of hosts to two host entries.</span></span> <span data-ttu-id="f6b71-170">Du behöver inte ange den fullständiga listan över värdar när du skapar en Kafka-konsument eller -producent.</span><span class="sxs-lookup"><span data-stu-id="f6b71-170">You do not need to provide the full list of hosts when creating a Kafka consumer or producer.</span></span>
   
    > [!WARNING]
    > <span data-ttu-id="f6b71-171">Förlita dig inte på att den information som returneras från den här sessionen alltid är korrekt.</span><span class="sxs-lookup"><span data-stu-id="f6b71-171">Do not rely on the information returned from this session to always be accurate.</span></span> <span data-ttu-id="f6b71-172">Om du skalar klustret läggs nya asynkrona meddelandeköer till respektive tas bort.</span><span class="sxs-lookup"><span data-stu-id="f6b71-172">If you scale the cluster, new brokers are added or removed.</span></span> <span data-ttu-id="f6b71-173">Om ett fel uppstår och en nod ersätts kan värdnamnet för noden ändras.</span><span class="sxs-lookup"><span data-stu-id="f6b71-173">If a failure occurs and a node is replaced, the host name for the node may change.</span></span>
    >
    > <span data-ttu-id="f6b71-174">Du bör hämta värdinformation om Zookeeper och asynkron meddelandekö strax innan du använder den för att kontrollera att du har giltig information.</span><span class="sxs-lookup"><span data-stu-id="f6b71-174">You should retrieve the Zookeeper and broker hosts information shortly before you use it to ensure you have valid information.</span></span>

## <a name="create-a-topic"></a><span data-ttu-id="f6b71-175">Skapa ett ämne</span><span class="sxs-lookup"><span data-stu-id="f6b71-175">Create a topic</span></span>

<span data-ttu-id="f6b71-176">Kafka lagrar dataströmmar i kategorier som kallas *ämnen*.</span><span class="sxs-lookup"><span data-stu-id="f6b71-176">Kafka stores streams of data in categories called *topics*.</span></span> <span data-ttu-id="f6b71-177">Från en SSH-anslutning till en klusterhuvudnod använder du ett skript som medföljer Kafka för att skapa ett ämne:</span><span class="sxs-lookup"><span data-stu-id="f6b71-177">From An SSH connection to a cluster headnode, use a script provided with Kafka to create a topic:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic test --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="f6b71-178">Det här kommandot ansluter till Zookeeper med hjälp av värdinformationen som lagras i `$KAFKAZKHOSTS`, och skapar sedan ett Kafka-ämne med namnet **test**.</span><span class="sxs-lookup"><span data-stu-id="f6b71-178">This command connects to Zookeeper using the host information stored in `$KAFKAZKHOSTS`, and then create Kafka topic named **test**.</span></span> <span data-ttu-id="f6b71-179">Du kan verifiera att ämnet har skapats med hjälp av följande skript för att lista ämnen:</span><span class="sxs-lookup"><span data-stu-id="f6b71-179">You can verify that the topic was created by using the following script to list topics:</span></span>

```bash
/usr/hdp/current/kafka-broker/bin/kafka-topics.sh --list --zookeeper $KAFKAZKHOSTS
```

<span data-ttu-id="f6b71-180">Utdata från det här kommandot visar Kafka-avsnitt som innehåller ämnet **test**.</span><span class="sxs-lookup"><span data-stu-id="f6b71-180">The output of this command lists Kafka topics, which contains the **test** topic.</span></span>

## <a name="produce-and-consume-records"></a><span data-ttu-id="f6b71-181">Skapa och använda poster</span><span class="sxs-lookup"><span data-stu-id="f6b71-181">Produce and consume records</span></span>

<span data-ttu-id="f6b71-182">Kafka lagrar *poster* i ämnen.</span><span class="sxs-lookup"><span data-stu-id="f6b71-182">Kafka stores *records* in topics.</span></span> <span data-ttu-id="f6b71-183">Poster produceras av *producenter*, och används av *konsumenter*.</span><span class="sxs-lookup"><span data-stu-id="f6b71-183">Records are produced by *producers*, and consumed by *consumers*.</span></span> <span data-ttu-id="f6b71-184">Producenter hämtar poster från asynkrona Kafka-*meddelandeköer*.</span><span class="sxs-lookup"><span data-stu-id="f6b71-184">Producers retrieve records from Kafka *brokers*.</span></span> <span data-ttu-id="f6b71-185">Varje arbetsnod i HDInsight-klustret är en asynkron Kafka-meddelandekö.</span><span class="sxs-lookup"><span data-stu-id="f6b71-185">Each worker node in your HDInsight cluster is a Kafka broker.</span></span>

<span data-ttu-id="f6b71-186">Använd följande steg för att lagra poster i det testämne som du skapade tidigare och läs dem sedan med en konsument:</span><span class="sxs-lookup"><span data-stu-id="f6b71-186">Use the following steps to store records into the test topic you created earlier, and then read them using a consumer:</span></span>

1. <span data-ttu-id="f6b71-187">Använd ett skript som medföljer Kafka för att skriva poster till ämnet från SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="f6b71-187">From the SSH session, use a script provided with Kafka to write records to the topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-producer.sh --broker-list $KAFKABROKERS --topic test
    ```
   
    <span data-ttu-id="f6b71-188">Du kommer inte tillbaka till uppmaningen efter det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="f6b71-188">You are not returned to the prompt after this command.</span></span> <span data-ttu-id="f6b71-189">Skriv i stället ange några textmeddelanden och använd sedan **Ctrl + C** för att sluta skicka till ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-189">Instead, type a few text messages and then use **Ctrl + C** to stop sending to the topic.</span></span> <span data-ttu-id="f6b71-190">Varje rad skickas som en separat post.</span><span class="sxs-lookup"><span data-stu-id="f6b71-190">Each line is sent as a separate record.</span></span>

2. <span data-ttu-id="f6b71-191">Använd ett skript som medföljer Kafka för att läsa poster från ämnet:</span><span class="sxs-lookup"><span data-stu-id="f6b71-191">Use a script provided with Kafka to read records from the topic:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic test --from-beginning
    ```
   
    <span data-ttu-id="f6b71-192">Med det här kommandot hämtar du posterna från ämnet och visar dem.</span><span class="sxs-lookup"><span data-stu-id="f6b71-192">This command retrieves the records from the topic and displays them.</span></span> <span data-ttu-id="f6b71-193">Med hjälp av `--from-beginning` anges att konsumenten ska starta från början av direktuppspelningen så att alla poster hämtas.</span><span class="sxs-lookup"><span data-stu-id="f6b71-193">Using `--from-beginning` tells the consumer to start from the beginning of the stream, so all records are retrieved.</span></span>

3. <span data-ttu-id="f6b71-194">Använd __Ctrl + C__ om du vill stoppa konsumenten.</span><span class="sxs-lookup"><span data-stu-id="f6b71-194">Use __Ctrl + C__ to stop the consumer.</span></span>

## <a name="producer-and-consumer-api"></a><span data-ttu-id="f6b71-195">Producent- och konsument-API</span><span class="sxs-lookup"><span data-stu-id="f6b71-195">Producer and consumer API</span></span>

<span data-ttu-id="f6b71-196">Du kan även programmässigt producera och använda poster med hjälp av [Kafka-API: er](http://kafka.apache.org/documentation#api).</span><span class="sxs-lookup"><span data-stu-id="f6b71-196">You can also programmatically produce and consume records using the [Kafka APIs](http://kafka.apache.org/documentation#api).</span></span> <span data-ttu-id="f6b71-197">Använd följande steg från din utvecklingsmiljö för att skapa en Java-producent och -konsument.</span><span class="sxs-lookup"><span data-stu-id="f6b71-197">To build a Java producer and consumer, use the following steps from your development environment.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6b71-198">Du måste ha följande komponenter installerade i utvecklingsmiljön:</span><span class="sxs-lookup"><span data-stu-id="f6b71-198">You must have the following components installed in your development environment:</span></span>
>
> * <span data-ttu-id="f6b71-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) eller motsvarande, till exempel OpenJDK.</span><span class="sxs-lookup"><span data-stu-id="f6b71-199">[Java JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or an equivalent, such as OpenJDK.</span></span>
>
> * [<span data-ttu-id="f6b71-200">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="f6b71-200">Apache Maven</span></span>](http://maven.apache.org/)
>
> * <span data-ttu-id="f6b71-201">En SSH-klient och kommandot `scp` .</span><span class="sxs-lookup"><span data-stu-id="f6b71-201">An SSH client and the `scp` command.</span></span> <span data-ttu-id="f6b71-202">Mer information finns i dokumentet [Använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f6b71-202">For more information, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) document.</span></span>

1. <span data-ttu-id="f6b71-203">Ladda ned exempel från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span><span class="sxs-lookup"><span data-stu-id="f6b71-203">Download the examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started).</span></span> <span data-ttu-id="f6b71-204">Använd projektet i katalogen `Producer-Consumer` för exemplet med producent/konsument.</span><span class="sxs-lookup"><span data-stu-id="f6b71-204">For the producer/consumer example, use the project in the `Producer-Consumer` directory.</span></span> <span data-ttu-id="f6b71-205">Det här exemplet innehåller följande klasser:</span><span class="sxs-lookup"><span data-stu-id="f6b71-205">This example contains the following classes:</span></span>
   
    * <span data-ttu-id="f6b71-206">**Kör** – Startar antingen konsumenten eller producenten.</span><span class="sxs-lookup"><span data-stu-id="f6b71-206">**Run** - starts either the consumer or producer.</span></span>

    * <span data-ttu-id="f6b71-207">**Producent** – Sparar 1 000 000 poster i ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-207">**Producer** - stores 1,000,000 records to the topic.</span></span>

    * <span data-ttu-id="f6b71-208">**Konsument** – Läser poster från ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-208">**Consumer** - reads records from the topic.</span></span>

2. <span data-ttu-id="f6b71-209">Skapa ett jar-paket genom att navigera till sökvägen för katalogen `Producer-Consumer` och sedan använda följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f6b71-209">To create a jar package, change directories to the location of the `Producer-Consumer` directory and use the following command:</span></span>

    ```
    mvn clean package
    ```

    <span data-ttu-id="f6b71-210">Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-210">This command creates a directory named `target`, that contains a file named `kafka-producer-consumer-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="f6b71-211">Använd följande kommandon för att kopiera filen `kafka-producer-consumer-1.0-SNAPSHOT.jar` till ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="f6b71-211">Use the following commands to copy the `kafka-producer-consumer-1.0-SNAPSHOT.jar` file to your HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-producer-consumer-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-producer-consumer.jar
    ```
   
    <span data-ttu-id="f6b71-212">Ersätt **SSHUSER** med SSH-användare för klustret och ersätt **CLUSTERNAME** med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-212">Replace **SSHUSER** with the SSH user for your cluster, and replace **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="f6b71-213">Ange lösenordet för SSH-användaren när du uppmanas till det.</span><span class="sxs-lookup"><span data-stu-id="f6b71-213">When prompted enter the password for the SSH user.</span></span>

4. <span data-ttu-id="f6b71-214">När filen kopierats med kommandot `scp` ansluter du till klustret via SSH.</span><span class="sxs-lookup"><span data-stu-id="f6b71-214">Once the `scp` command finishes copying the file, connect to the cluster using SSH.</span></span> <span data-ttu-id="f6b71-215">Skriv posterna till testämnet med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f6b71-215">Use the following command to write records to the test topic:</span></span>

    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS
    ```

5. <span data-ttu-id="f6b71-216">När processen är klar kan du använda följande kommando för att läsa från ämnet:</span><span class="sxs-lookup"><span data-stu-id="f6b71-216">Once the process has finished, use the following command to read from the topic:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS
    ```
   
    <span data-ttu-id="f6b71-217">De lästa posterna, tillsammans med antalet poster, visas.</span><span class="sxs-lookup"><span data-stu-id="f6b71-217">The records read, along with a count of records, is displayed.</span></span> <span data-ttu-id="f6b71-218">Du kan se några fler än 1 000 000 loggade om du har skickat flera poster till ämnet med hjälp av ett skript i ett tidigare steg.</span><span class="sxs-lookup"><span data-stu-id="f6b71-218">You may see a few more than 1,000,000 logged as you sent several records to the topic using a script in an earlier step.</span></span>

6. <span data-ttu-id="f6b71-219">Använd __Ctrl + C__ om du vill avsluta konsumenten.</span><span class="sxs-lookup"><span data-stu-id="f6b71-219">Use __Ctrl + C__ to exit the consumer.</span></span>

### <a name="multiple-consumers"></a><span data-ttu-id="f6b71-220">Flera konsumenter</span><span class="sxs-lookup"><span data-stu-id="f6b71-220">Multiple consumers</span></span>

<span data-ttu-id="f6b71-221">Kafka-konsumenter använder en konsumentgrupp vid läsning av poster.</span><span class="sxs-lookup"><span data-stu-id="f6b71-221">Kafka consumers use a consumer group when reading records.</span></span> <span data-ttu-id="f6b71-222">Att använda samma grupp med flera konsumenter resulterar i belastningsutjämnaravläsningar från ett ämne.</span><span class="sxs-lookup"><span data-stu-id="f6b71-222">Using the same group with multiple consumers results in load balanced reads from a topic.</span></span> <span data-ttu-id="f6b71-223">Varje konsument i gruppen tar emot en del av posterna.</span><span class="sxs-lookup"><span data-stu-id="f6b71-223">Each consumer in the group receives a portion of the records.</span></span> <span data-ttu-id="f6b71-224">Använd följande steg om du vill se hur det fungerar:</span><span class="sxs-lookup"><span data-stu-id="f6b71-224">To see this process in action, use the following steps:</span></span>

1. <span data-ttu-id="f6b71-225">Öppna en ny SSH-session i klustret, så att du har två av dessa.</span><span class="sxs-lookup"><span data-stu-id="f6b71-225">Open a new SSH session to the cluster, so that you have two of them.</span></span> <span data-ttu-id="f6b71-226">Använd följande för att starta en konsument med samma konsumentgrupps-ID i varje session:</span><span class="sxs-lookup"><span data-stu-id="f6b71-226">In each session, use the following to start a consumer with the same consumer group ID:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar consumer $KAFKABROKERS mygroup
    ```

    <span data-ttu-id="f6b71-227">Detta kommando startar en konsument med grupp-ID `mygroup`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-227">This command starts a consumer using the group ID `mygroup`.</span></span>

    > [!NOTE]
    > <span data-ttu-id="f6b71-228">Använd kommandona i avsnittet [Hämta värdinformation för Zookeeper och Broker](#getkafkainfo) till att ange `$KAFKABROKERS` för den här SSH-sessionen.</span><span class="sxs-lookup"><span data-stu-id="f6b71-228">Use the commands in the [Get the Zookeeper and Broker host information](#getkafkainfo) section to set `$KAFKABROKERS` for this SSH session.</span></span>

2. <span data-ttu-id="f6b71-229">Titta på när varje session räknar posterna som tas emot från ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-229">Watch as each session counts the records it receives from the topic.</span></span> <span data-ttu-id="f6b71-230">Summan av båda sessionerna ska vara identisk med den som du tidigare har tagit emot från en konsument.</span><span class="sxs-lookup"><span data-stu-id="f6b71-230">The total of both sessions should be the same as you received previously from one consumer.</span></span>

<span data-ttu-id="f6b71-231">Förbrukning av klienter i samma grupp hanteras via partitionerna för ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-231">Consumption by clients within the same group is handled through the partitions for the topic.</span></span> <span data-ttu-id="f6b71-232">För det `test`-ämne som skapades tidigare har den åtta partitioner.</span><span class="sxs-lookup"><span data-stu-id="f6b71-232">For the `test` topic created earlier, it has eight partitions.</span></span> <span data-ttu-id="f6b71-233">Om du öppnar åtta SSH-sessioner och startar en konsument i alla sessioner, läser varje konsument poster från en partition i ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-233">If you open eight SSH sessions and launch a consumer in all sessions, each consumer reads records from a single partition for the topic.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6b71-234">Det får inte finnas flera instanser av konsumenten i en konsumentgrupp än partitioner.</span><span class="sxs-lookup"><span data-stu-id="f6b71-234">There cannot be more consumer instances in a consumer group than partitions.</span></span> <span data-ttu-id="f6b71-235">I det här exemplet kan en konsumentgrupp innehålla upp till åtta konsumenter, eftersom det är antalet partitioner i ämnet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-235">In this example, one consumer group can contain up to eight consumers since that is the number of partitions in the topic.</span></span> <span data-ttu-id="f6b71-236">Du kan även ha flera konsumentgrupper med högst åtta konsumenter vardera.</span><span class="sxs-lookup"><span data-stu-id="f6b71-236">Or you can have multiple consumer groups, each with no more than eight consumers.</span></span>

<span data-ttu-id="f6b71-237">Poster som lagras i Kafka lagras i den ordning som de tas emot inom en partition.</span><span class="sxs-lookup"><span data-stu-id="f6b71-237">Records stored in Kafka are stored in the order they are received within a partition.</span></span> <span data-ttu-id="f6b71-238">För att uppnå sorterad leverans av poster *inom en partition* skapar du en konsumentgrupp där antalet konsumentinstanser matchar antalet partitioner.</span><span class="sxs-lookup"><span data-stu-id="f6b71-238">To achieve in-ordered delivery for records *within a partition*, create a consumer group where the number of consumer instances matches the number of partitions.</span></span> <span data-ttu-id="f6b71-239">För att uppnå sorterad leverans av poster *i ämnet* skapar du en konsumentgrupp med bara en konsumentinstans.</span><span class="sxs-lookup"><span data-stu-id="f6b71-239">To achieve in-ordered delivery for records *within the topic*, create a consumer group with only one consumer instance.</span></span>

## <a name="streaming-api"></a><span data-ttu-id="f6b71-240">Strömmande API</span><span class="sxs-lookup"><span data-stu-id="f6b71-240">Streaming API</span></span>

<span data-ttu-id="f6b71-241">Strömmande API lades till Kafka i version 0.10.0; tidigare versioner är beroende av Apache Spark eller Storm för bearbetning av dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="f6b71-241">The streaming API was added to Kafka in version 0.10.0; earlier versions rely on Apache Spark or Storm for stream processing.</span></span>

1. <span data-ttu-id="f6b71-242">Om du inte redan gjort det kan du hämta exemplen från [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) till din utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="f6b71-242">If you haven't already done so, download the examples from [https://github.com/Azure-Samples/hdinsight-kafka-java-get-started](https://github.com/Azure-Samples/hdinsight-kafka-java-get-started) to your development environment.</span></span> <span data-ttu-id="f6b71-243">För direktuppspelningsexemplet använder du projektet i katalogen `streaming`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-243">For the streaming example, use the project in the `streaming` directory.</span></span>
   
    <span data-ttu-id="f6b71-244">Det här projektet innehåller endast en klass `Stream`, som läser poster från ämnet `test` som har skapats tidigare.</span><span class="sxs-lookup"><span data-stu-id="f6b71-244">This project contains only one class, `Stream`, which reads records from the `test` topic created previously.</span></span> <span data-ttu-id="f6b71-245">Den räknar antalet lästa ord och genererar varje ord och antalet till ett ämne som heter `wordcounts`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-245">It counts the words read, and emits each word and count to a topic named `wordcounts`.</span></span> <span data-ttu-id="f6b71-246">Ämnet `wordcounts` skapas i ett senare steg i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-246">The `wordcounts` topic is created in a later step in this section.</span></span>

2. <span data-ttu-id="f6b71-247">Från kommandoraden i din utvecklingsmiljö ändrar du katalogerna till platsen för katalogen `Streaming` och använder sedan följande kommando för att skapa ett jar-paket:</span><span class="sxs-lookup"><span data-stu-id="f6b71-247">From the command line in your development environment, change directories to the location of the `Streaming` directory, and then use the following command to create a jar package:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="f6b71-248">Det här kommandot skapar en katalog med namnet `target`, som innehåller en fil med namnet `kafka-streaming-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-248">This command creates a directory named `target`, which contains a file named `kafka-streaming-1.0-SNAPSHOT.jar`.</span></span>

3. <span data-ttu-id="f6b71-249">Använd följande kommandon för att kopiera filen `kafka-streaming-1.0-SNAPSHOT.jar` till ditt HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="f6b71-249">Use the following commands to copy the `kafka-streaming-1.0-SNAPSHOT.jar` file to your HDInsight cluster:</span></span>
   
    ```bash
    scp ./target/kafka-streaming-1.0-SNAPSHOT.jar SSHUSER@CLUSTERNAME-ssh.azurehdinsight.net:kafka-streaming.jar
    ```
   
    <span data-ttu-id="f6b71-250">Ersätt **SSHUSER** med SSH-användare för klustret och ersätt **CLUSTERNAME** med namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="f6b71-250">Replace **SSHUSER** with the SSH user for your cluster, and replace **CLUSTERNAME** with the name of your cluster.</span></span> <span data-ttu-id="f6b71-251">Ange lösenordet för SSH-användaren när du uppmanas till det.</span><span class="sxs-lookup"><span data-stu-id="f6b71-251">When prompted enter the password for the SSH user.</span></span>

4. <span data-ttu-id="f6b71-252">När kommandot `scp` har slutfört kopieringen av filen ansluter du till klustret med SSH och använder sedan följande kommando för att skapa ämnet `wordcounts`:</span><span class="sxs-lookup"><span data-stu-id="f6b71-252">Once the `scp` command finishes copying the file, connect to the cluster using SSH, and then use the following command to create the `wordcounts` topic:</span></span>

    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic wordcounts --zookeeper $KAFKAZKHOSTS
    ```

5. <span data-ttu-id="f6b71-253">Starta sedan den direktuppspelade processen med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f6b71-253">Next, start the streaming process by using the following command:</span></span>
   
    ```bash
    java -jar kafka-streaming.jar $KAFKABROKERS $KAFKAZKHOSTS 2>/dev/null &
    ```
   
    <span data-ttu-id="f6b71-254">Detta kommando startar den direktuppspelade processen i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="f6b71-254">This command starts the streaming process in the background.</span></span>

6. <span data-ttu-id="f6b71-255">Använd följande kommando för att skicka meddelanden till ämnet `test`.</span><span class="sxs-lookup"><span data-stu-id="f6b71-255">Use the following command to send messages to the `test` topic.</span></span> <span data-ttu-id="f6b71-256">Meddelandena behandlas med direktuppspelade exempel:</span><span class="sxs-lookup"><span data-stu-id="f6b71-256">These messages are processed by the streaming example:</span></span>
   
    ```bash
    java -jar kafka-producer-consumer.jar producer $KAFKABROKERS &>/dev/null &
    ```

7. <span data-ttu-id="f6b71-257">Använd följande kommando för att visa utdata som skrivs till ämnet `wordcounts` genom den direktuppspelade processen:</span><span class="sxs-lookup"><span data-stu-id="f6b71-257">Use the following command to view the output that is written to the `wordcounts` topic by the streaming process:</span></span>
   
    ```bash
    /usr/hdp/current/kafka-broker/bin/kafka-console-consumer.sh --bootstrap-server $KAFKABROKERS --topic wordcounts --from-beginning --formatter kafka.tools.DefaultMessageFormatter --property print.key=true --property key.deserializer=org.apache.kafka.common.serialization.StringDeserializer --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer
    ```
   
    > [!NOTE]
    > <span data-ttu-id="f6b71-258">Om du vill visa data måste du beordra konsumenten att skriva ut nyckeln och ange vilken funktion för avserialisering som ska användas för nyckeln och värdet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-258">To view the data, you must tell the consumer to print the key and the deserializer to use for the key and value.</span></span> <span data-ttu-id="f6b71-259">Nyckelnamnet är ordet, och nyckelvärdet innehåller antalet.</span><span class="sxs-lookup"><span data-stu-id="f6b71-259">The key name is the word, and the key value contains the count.</span></span>
   
    <span data-ttu-id="f6b71-260">De utdata som genereras liknar följande text:</span><span class="sxs-lookup"><span data-stu-id="f6b71-260">The output is similar to the following text:</span></span>
   
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
    > <span data-ttu-id="f6b71-261">Antalet ökar varje gång ett ord påträffas.</span><span class="sxs-lookup"><span data-stu-id="f6b71-261">The count increments each time a word is encountered.</span></span>

7. <span data-ttu-id="f6b71-262">Använd __Ctrl + C__ om du vill avsluta konsumenten och använd sedan kommandot `fg` för att återställa den direktuppspelade bakgrundsaktiviteten till förgrunden igen.</span><span class="sxs-lookup"><span data-stu-id="f6b71-262">Use the __Ctrl + C__ to exit the consumer, then use the `fg` command to bring the streaming background task back to the foreground.</span></span> <span data-ttu-id="f6b71-263">Använd __Ctrl + C__ för att avsluta den också.</span><span class="sxs-lookup"><span data-stu-id="f6b71-263">Use __Ctrl + C__ to exit it also.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="f6b71-264">Ta bort klustret</span><span class="sxs-lookup"><span data-stu-id="f6b71-264">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a><span data-ttu-id="f6b71-265">Felsöka</span><span class="sxs-lookup"><span data-stu-id="f6b71-265">Troubleshoot</span></span>

<span data-ttu-id="f6b71-266">Om du får problem med att skapa HDInsight-kluster läser du [åtkomstkontrollkrav](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="f6b71-266">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6b71-267">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f6b71-267">Next steps</span></span>

<span data-ttu-id="f6b71-268">I detta dokument har du lärt dig grunderna för att arbeta med Apache Kafka i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="f6b71-268">In this document, you have learned the basics of working with Apache Kafka on HDInsight.</span></span> <span data-ttu-id="f6b71-269">Använd följande för att lära dig mer om att arbeta med Kafka:</span><span class="sxs-lookup"><span data-stu-id="f6b71-269">Use the following to learn more about working with Kafka:</span></span>

* [<span data-ttu-id="f6b71-270">Säkerställ en hög tillgänglighet för dina data med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-270">Ensure high availability of your data with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-high-availability.md)
* [<span data-ttu-id="f6b71-271">Öka skalbarheten genom att konfigurera hanterade diskar med Kafka i HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-271">Increase scalability by configuring managed disks with Kafka on HDInsight</span></span>](hdinsight-apache-kafka-scalability.md)
* <span data-ttu-id="f6b71-272">[Apache Kafka-dokumentation](http://kafka.apache.org/documentation.html) på kafka.apache.org.</span><span class="sxs-lookup"><span data-stu-id="f6b71-272">[Apache Kafka documentation](http://kafka.apache.org/documentation.html) at kafka.apache.org.</span></span>
* [<span data-ttu-id="f6b71-273">Använd MirrorMaker för att skapa en replik av Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-273">Use MirrorMaker to create a replica of Kafka on HDInsight</span></span>](hdinsight-apache-kafka-mirroring.md)
* [<span data-ttu-id="f6b71-274">Använda Apache Storm med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-274">Use Apache Storm with Kafka on HDInsight</span></span>](hdinsight-apache-storm-with-kafka.md)
* [<span data-ttu-id="f6b71-275">Använda Apache Spark med Kafka på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f6b71-275">Use Apache Spark with Kafka on HDInsight</span></span>](hdinsight-apache-spark-with-kafka.md)
* [<span data-ttu-id="f6b71-276">Ansluta till Kafka via ett trådlöst Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="f6b71-276">Connect to Kafka through an Azure Virtual Network</span></span>](hdinsight-apache-kafka-connect-vpn-gateway.md)
