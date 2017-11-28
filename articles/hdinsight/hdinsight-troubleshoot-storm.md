---
title: "aaaTroubleshoot Storm med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du använder Apache Storm med Azure HDInsight."
keywords: "Azure HDInsight, Storm, vanliga frågor och svar, felsökningsguide för vanliga problem"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="31414-104">Felsöka Storm med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="31414-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="31414-105">Läs mer om hello de främsta problemen och sina lösningar för att arbeta med Apache Storm-nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="31414-105">Learn about hello top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a><span data-ttu-id="31414-106">Hur kommer jag åt hello Storm-Användargränssnittet på ett kluster</span><span class="sxs-lookup"><span data-stu-id="31414-106">How do I access hello Storm UI on a cluster</span></span>
<span data-ttu-id="31414-107">Har du två alternativ för att komma åt hello Storm-Användargränssnittet från en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="31414-107">You have two options for accessing hello Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="31414-108">Ambari UI</span><span class="sxs-lookup"><span data-stu-id="31414-108">Ambari UI</span></span>
1. <span data-ttu-id="31414-109">Gå toohello Ambari instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="31414-109">Go toohello Ambari dashboard.</span></span>
2. <span data-ttu-id="31414-110">I hello lista över tjänster, Välj **Storm**.</span><span class="sxs-lookup"><span data-stu-id="31414-110">In hello list of services, select **Storm**.</span></span>
3. <span data-ttu-id="31414-111">I hello **snabblänkar** väljer du **Storm-Användargränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="31414-111">In hello **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="31414-112">Direktlänk</span><span class="sxs-lookup"><span data-stu-id="31414-112">Direct link</span></span>
<span data-ttu-id="31414-113">Du kan komma åt hello Storm-Användargränssnittet på hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="31414-113">You can access hello Storm UI at hello following URL:</span></span>

<span data-ttu-id="31414-114">https://\<klustrat DNS-namn\>/stormui</span><span class="sxs-lookup"><span data-stu-id="31414-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="31414-115">Exempel:</span><span class="sxs-lookup"><span data-stu-id="31414-115">Example:</span></span>

 <span data-ttu-id="31414-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="31414-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a><span data-ttu-id="31414-117">Hur överföra Storm hubb kanal kontrollpunkt händelseinformation från en topologi tooanother</span><span class="sxs-lookup"><span data-stu-id="31414-117">How do I transfer Storm event hub spout checkpoint information from one topology tooanother</span></span>

<span data-ttu-id="31414-118">När du utvecklar topologier som läses från Azure Event Hubs genom att använda hello HDInsight Storm event hub kanal .jar-fil, måste du distribuera en topologi som har samma namn på ett nytt kluster hello.</span><span class="sxs-lookup"><span data-stu-id="31414-118">When you develop topologies that read from Azure Event Hubs by using hello HDInsight Storm event hub spout .jar file, you must deploy a topology that has hello same name on a new cluster.</span></span> <span data-ttu-id="31414-119">Du måste dock behålla hello kontrollpunktsdata som har utförts tooApache ZooKeeper på hello gamla klustret.</span><span class="sxs-lookup"><span data-stu-id="31414-119">However, you must retain hello checkpoint data that was committed tooApache ZooKeeper on hello old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="31414-120">Kontrollpunktsdata ska lagras</span><span class="sxs-lookup"><span data-stu-id="31414-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="31414-121">Kontrollpunktsdata för förskjutningarna lagras av hello event hub kanal i ZooKeeper i två rot-sökvägar:</span><span class="sxs-lookup"><span data-stu-id="31414-121">Checkpoint data for offsets is stored by hello event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="31414-122">Icke-transaktionell kanal kontrollpunkter lagras i /eventhubspout.</span><span class="sxs-lookup"><span data-stu-id="31414-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="31414-123">Transaktionell kanal kontrollpunktsdata lagras i / transaktionella.</span><span class="sxs-lookup"><span data-stu-id="31414-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-toorestore"></a><span data-ttu-id="31414-124">Hur toorestore</span><span class="sxs-lookup"><span data-stu-id="31414-124">How toorestore</span></span>
<span data-ttu-id="31414-125">tooget hello skript och bibliotek om du använder tooexport data utanför ZooKeeper och sedan importera hello data tillbaka tooZooKeeper med ett nytt namn, se [HDInsight Storm exempel](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="31414-125">tooget hello scripts and libraries that you use tooexport data out of ZooKeeper and then import hello data back tooZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="31414-126">hello lib mappen innehåller .jar-filer som innehåller hello implementering för hello exporten/importen.</span><span class="sxs-lookup"><span data-stu-id="31414-126">hello lib folder has .jar files that contain hello implementation for hello export/import operation.</span></span> <span data-ttu-id="31414-127">hello bash har ett exempelskript som visar hur tooexport data från hello ZooKeeper-server på hello gamla klustret och sedan importera den bakre toohello ZooKeeper-server på hello nya klustret.</span><span class="sxs-lookup"><span data-stu-id="31414-127">hello bash folder has an example script that demonstrates how tooexport data from hello ZooKeeper server on hello old cluster, and then import it back toohello ZooKeeper server on hello new cluster.</span></span>

<span data-ttu-id="31414-128">Kör hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skript från hello ZooKeeper-noder tooexport och därefter importera data.</span><span class="sxs-lookup"><span data-stu-id="31414-128">Run hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from hello ZooKeeper nodes tooexport and then import data.</span></span> <span data-ttu-id="31414-129">Uppdatera hello skriptet toohello rätt Hortonworks Data Platform (HDP) version.</span><span class="sxs-lookup"><span data-stu-id="31414-129">Update hello script toohello correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="31414-130">(Vi arbetar på att skripten generiska i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="31414-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="31414-131">Allmänt skript kan köras från en nod på hello kluster utan ändringar av hello användare.)</span><span class="sxs-lookup"><span data-stu-id="31414-131">Generic scripts can run from any node on hello cluster without modifications by hello user.)</span></span>

<span data-ttu-id="31414-132">hello export-kommando skriver hello metadata tooan Apache Hadoop Distributed File System (HDFS) väg (i Azure Blob Storage eller Azure Data Lake Store store) på en plats som du anger.</span><span class="sxs-lookup"><span data-stu-id="31414-132">hello export command writes hello metadata tooan Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="31414-133">Exempel</span><span class="sxs-lookup"><span data-stu-id="31414-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="31414-134">Exportera offset metadata</span><span class="sxs-lookup"><span data-stu-id="31414-134">Export offset metadata</span></span>
1. <span data-ttu-id="31414-135">Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="31414-135">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="31414-136">Hello kör följande kommando (när du har uppdaterat hello HDP versionssträng) tooexport ZooKeeper förskjutning data toohello /stormmetadta/zkdata HDFS sökväg:</span><span class="sxs-lookup"><span data-stu-id="31414-136">Run hello following command (after you update hello HDP version string) tooexport ZooKeeper offset data toohello /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="31414-137">Importera offset metadata</span><span class="sxs-lookup"><span data-stu-id="31414-137">Import offset metadata</span></span>
1. <span data-ttu-id="31414-138">Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="31414-138">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="31414-139">Kör hello följande kommando (när du har uppdaterat hello HDP versionssträng) tooimport ZooKeeper förskjutning data från hello HDFS sökväg/stormmetadata/zkdata toohello ZooKeeper-server på hello målkluster:</span><span class="sxs-lookup"><span data-stu-id="31414-139">Run hello following command (after you update hello HDP version string) tooimport ZooKeeper offset data from hello HDFS path /stormmetadata/zkdata toohello ZooKeeper server on hello target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a><span data-ttu-id="31414-140">Ta bort offset metadata så att topologier kan börja bearbeta data från hello början eller från en tidsstämpel som hello-användaren väljer</span><span class="sxs-lookup"><span data-stu-id="31414-140">Delete offset metadata so that topologies can start processing data from hello beginning, or from a timestamp that hello user chooses</span></span>
1. <span data-ttu-id="31414-141">Använda SSH toogo toohello ZooKeeper kluster på hello klustret från vilka hello kontrollpunkten förskjutningen måste toobe exporteras.</span><span class="sxs-lookup"><span data-stu-id="31414-141">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="31414-142">Kör hello följande kommando (när du har uppdaterat hello HDP versionssträng) toodelete alla ZooKeeper förskjutning data i hello aktuella klustret:</span><span class="sxs-lookup"><span data-stu-id="31414-142">Run hello following command (after you update hello HDP version string) toodelete all ZooKeeper offset data in hello current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="31414-143">Hur jag för att hitta Storm binärfiler i ett kluster</span><span class="sxs-lookup"><span data-stu-id="31414-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="31414-144">Storm-binärfiler för hello aktuella HDP stack finns i /usr/hdp/current/storm-client.</span><span class="sxs-lookup"><span data-stu-id="31414-144">Storm binaries for hello current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="31414-145">hello plats är hello samma både för huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="31414-145">hello location is hello same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="31414-146">Det kan finnas flera binärfiler för specifika HDP versioner i /usr/hdp (till exempel /usr/hdp/2.5.0.1233/storm).</span><span class="sxs-lookup"><span data-stu-id="31414-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="31414-147">Hej /usr/hdp/current/storm-client mappen är symlinked toohello senaste versionen som körs på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="31414-147">hello /usr/hdp/current/storm-client folder is symlinked toohello latest version that is running on hello cluster.</span></span>

<span data-ttu-id="31414-148">Mer information finns i [Anslut tooan HDInsight-kluster med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) och [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="31414-148">For more information, see [Connect tooan HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="31414-149">Hur vet hello topologi för distribution av ett Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="31414-149">How do I determine hello deployment topology of a Storm cluster</span></span>
<span data-ttu-id="31414-150">Först identifiera alla komponenter som installeras med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="31414-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="31414-151">Ett Storm-kluster består av fyra nod:</span><span class="sxs-lookup"><span data-stu-id="31414-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="31414-152">Gateway-noder</span><span class="sxs-lookup"><span data-stu-id="31414-152">Gateway nodes</span></span>
* <span data-ttu-id="31414-153">HEAD-noder</span><span class="sxs-lookup"><span data-stu-id="31414-153">Head nodes</span></span>
* <span data-ttu-id="31414-154">ZooKeeper-noder</span><span class="sxs-lookup"><span data-stu-id="31414-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="31414-155">Arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="31414-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="31414-156">Gateway-noder</span><span class="sxs-lookup"><span data-stu-id="31414-156">Gateway nodes</span></span>
<span data-ttu-id="31414-157">En gateway-noden är en gateway och en omvänd proxy-tjänst som gör allmän åtkomst tooan active Ambari management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="31414-157">A gateway node is a gateway and reverse proxy service that enables public access tooan active Ambari management service.</span></span> <span data-ttu-id="31414-158">Den hanterar också Ambari ledande val.</span><span class="sxs-lookup"><span data-stu-id="31414-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="31414-159">HEAD-noder</span><span class="sxs-lookup"><span data-stu-id="31414-159">Head nodes</span></span>
<span data-ttu-id="31414-160">Storm huvudnoderna kör hello följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="31414-160">Storm head nodes run hello following services:</span></span>
* <span data-ttu-id="31414-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="31414-161">Nimbus</span></span>
* <span data-ttu-id="31414-162">Ambari-server</span><span class="sxs-lookup"><span data-stu-id="31414-162">Ambari server</span></span>
* <span data-ttu-id="31414-163">Ambari mått server</span><span class="sxs-lookup"><span data-stu-id="31414-163">Ambari Metrics server</span></span>
* <span data-ttu-id="31414-164">Ambari mått insamlaren</span><span class="sxs-lookup"><span data-stu-id="31414-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="31414-165">ZooKeeper-noder</span><span class="sxs-lookup"><span data-stu-id="31414-165">ZooKeeper nodes</span></span>
<span data-ttu-id="31414-166">HDInsight levereras med ett kvorum för ZooKeeper tre noder.</span><span class="sxs-lookup"><span data-stu-id="31414-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="31414-167">hello kvorum storlek är fast och kan inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="31414-167">hello quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="31414-168">Storm-tjänster i hello kluster är konfigurerade tooautomatically Använd hello ZooKeeper kvorum.</span><span class="sxs-lookup"><span data-stu-id="31414-168">Storm services in hello cluster are configured tooautomatically use hello ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="31414-169">Arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="31414-169">Worker nodes</span></span>
<span data-ttu-id="31414-170">Storm arbetarnoder kör hello följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="31414-170">Storm worker nodes run hello following services:</span></span>
* <span data-ttu-id="31414-171">Överordnad</span><span class="sxs-lookup"><span data-stu-id="31414-171">Supervisor</span></span>
* <span data-ttu-id="31414-172">Worker Java virtuella datorer (JVMs) för topologier som körs</span><span class="sxs-lookup"><span data-stu-id="31414-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="31414-173">Ambari-agent</span><span class="sxs-lookup"><span data-stu-id="31414-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="31414-174">Hur jag för att hitta Storm event hub kanal binärfiler för utveckling</span><span class="sxs-lookup"><span data-stu-id="31414-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="31414-175">Mer information om hur du använder Storm event hub kanal .jar filer med din topologi finns hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="31414-175">For more information about using Storm event hub spout .jar files with your topology, see hello following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="31414-176">Java-baserad topologi</span><span class="sxs-lookup"><span data-stu-id="31414-176">Java-based topology</span></span>
[<span data-ttu-id="31414-177">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="31414-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="31414-178">C#-baserade topologi (Mono på HDInsight 3.4 + Linux Storm-kluster)</span><span class="sxs-lookup"><span data-stu-id="31414-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
<span data-ttu-id="31414-179">[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology) (Bearbeta händelser från Azure Event Hubs med Storm i HDInsight (C#))</span><span class="sxs-lookup"><span data-stu-id="31414-179">[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)</span></span>
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="31414-180">Senaste Storm event hub-kanalen binärfiler för HDInsight 3.5 + Linux Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="31414-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="31414-181">toolearn hur toouse hello senaste Storm event hub kanal som fungerar med HDInsight 3.5 + Linux Storm-kluster finns i hello mvn-lagringsplatsen [Readme-filen](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="31414-181">toolearn how toouse hello latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see hello mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="31414-182">Käll-kodexempel</span><span class="sxs-lookup"><span data-stu-id="31414-182">Source code examples</span></span>
<span data-ttu-id="31414-183">Se [exempel](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) på hur tooread och skriva från Azure Event Hub med en Apache Storm-topologi (skriven i Java) på ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="31414-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how tooread and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="31414-184">Hur jag för att hitta Storm Log4J konfigurationsfiler på kluster</span><span class="sxs-lookup"><span data-stu-id="31414-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="31414-185">tooidentify Apache Log4J konfigurationsfiler för Storm-tjänster.</span><span class="sxs-lookup"><span data-stu-id="31414-185">tooidentify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="31414-186">På huvudnoderna</span><span class="sxs-lookup"><span data-stu-id="31414-186">On head nodes</span></span>
<span data-ttu-id="31414-187">Hej Nimbus Log4J konfiguration läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="31414-187">hello Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="31414-188">På arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="31414-188">On worker nodes</span></span>
<span data-ttu-id="31414-189">hello handledarens Log4J konfiguration läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="31414-189">hello supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="31414-190">hello worker Log4J-konfigurationsfilen har lästs från/usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="31414-190">hello worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="31414-191">Exempel: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="31414-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

