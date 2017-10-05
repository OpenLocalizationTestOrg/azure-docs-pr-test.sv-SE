---
title: "Felsöka Storm med Azure HDInsight | Microsoft Docs"
description: "Få svar på vanliga frågor om hur du använder Apache Storm med Azure HDInsight."
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
ms.openlocfilehash: 70a3d762431d90acdd6ed2a432a569f34d0ce447
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="90484-104">Felsöka Storm med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="90484-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="90484-105">Läs mer om de vanligaste problemen och sina lösningar för att arbeta med Apache Storm-nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="90484-105">Learn about the top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a><span data-ttu-id="90484-106">Hur kommer jag åt Storm-Användargränssnittet på ett kluster</span><span class="sxs-lookup"><span data-stu-id="90484-106">How do I access the Storm UI on a cluster</span></span>
<span data-ttu-id="90484-107">Har du två alternativ för att komma åt Storm-Användargränssnittet från en webbläsare:</span><span class="sxs-lookup"><span data-stu-id="90484-107">You have two options for accessing the Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="90484-108">Ambari UI</span><span class="sxs-lookup"><span data-stu-id="90484-108">Ambari UI</span></span>
1. <span data-ttu-id="90484-109">Gå till instrumentpanelen Ambari.</span><span class="sxs-lookup"><span data-stu-id="90484-109">Go to the Ambari dashboard.</span></span>
2. <span data-ttu-id="90484-110">Välj i listan över tjänster, **Storm**.</span><span class="sxs-lookup"><span data-stu-id="90484-110">In the list of services, select **Storm**.</span></span>
3. <span data-ttu-id="90484-111">I den **snabblänkar** väljer du **Storm-Användargränssnittet**.</span><span class="sxs-lookup"><span data-stu-id="90484-111">In the **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="90484-112">Direktlänk</span><span class="sxs-lookup"><span data-stu-id="90484-112">Direct link</span></span>
<span data-ttu-id="90484-113">Du kan komma åt Storm-Användargränssnittet på följande URL:</span><span class="sxs-lookup"><span data-stu-id="90484-113">You can access the Storm UI at the following URL:</span></span>

<span data-ttu-id="90484-114">https://\<klustrat DNS-namn\>/stormui</span><span class="sxs-lookup"><span data-stu-id="90484-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="90484-115">Exempel:</span><span class="sxs-lookup"><span data-stu-id="90484-115">Example:</span></span>

 <span data-ttu-id="90484-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="90484-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a><span data-ttu-id="90484-117">Hur överför Storm hubb kanal kontrollpunkt händelseinformation från en topologi till en annan</span><span class="sxs-lookup"><span data-stu-id="90484-117">How do I transfer Storm event hub spout checkpoint information from one topology to another</span></span>

<span data-ttu-id="90484-118">När du utvecklar topologier som läses från Azure Event Hubs genom att använda HDInsight Storm event hub-kanalen .jar-fil, måste du distribuera en topologi som har samma namn på ett nytt kluster.</span><span class="sxs-lookup"><span data-stu-id="90484-118">When you develop topologies that read from Azure Event Hubs by using the HDInsight Storm event hub spout .jar file, you must deploy a topology that has the same name on a new cluster.</span></span> <span data-ttu-id="90484-119">Du måste dock behålla kontrollpunktsdata som har utförts till Apache ZooKeeper på det gamla klustret.</span><span class="sxs-lookup"><span data-stu-id="90484-119">However, you must retain the checkpoint data that was committed to Apache ZooKeeper on the old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="90484-120">Kontrollpunktsdata ska lagras</span><span class="sxs-lookup"><span data-stu-id="90484-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="90484-121">Kontrollpunktsdata för förskjutningarna lagras av event hub kanal i ZooKeeper i två rot-sökvägar:</span><span class="sxs-lookup"><span data-stu-id="90484-121">Checkpoint data for offsets is stored by the event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="90484-122">Icke-transaktionell kanal kontrollpunkter lagras i /eventhubspout.</span><span class="sxs-lookup"><span data-stu-id="90484-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="90484-123">Transaktionell kanal kontrollpunktsdata lagras i / transaktionella.</span><span class="sxs-lookup"><span data-stu-id="90484-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-to-restore"></a><span data-ttu-id="90484-124">Så här återställer du</span><span class="sxs-lookup"><span data-stu-id="90484-124">How to restore</span></span>
<span data-ttu-id="90484-125">Skript och bibliotek som används för att exportera data från ZooKeeper och importera sedan data tillbaka till ZooKeeper med ett nytt namn finns [HDInsight Storm exempel](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="90484-125">To get the scripts and libraries that you use to export data out of ZooKeeper and then import the data back to ZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="90484-126">Mappen lib har .jar-filer som innehåller implementeringen för exporten/importen.</span><span class="sxs-lookup"><span data-stu-id="90484-126">The lib folder has .jar files that contain the implementation for the export/import operation.</span></span> <span data-ttu-id="90484-127">Bash-mappen innehåller ett exempelskript som visar hur du exporterar data från ZooKeeper-server på det gamla klustret och sedan importera den till ZooKeeper-server på det nya klustret.</span><span class="sxs-lookup"><span data-stu-id="90484-127">The bash folder has an example script that demonstrates how to export data from the ZooKeeper server on the old cluster, and then import it back to the ZooKeeper server on the new cluster.</span></span>

<span data-ttu-id="90484-128">Kör den [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) skriptet från ZooKeeper-noder för att exportera och importera data.</span><span class="sxs-lookup"><span data-stu-id="90484-128">Run the [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from the ZooKeeper nodes to export and then import data.</span></span> <span data-ttu-id="90484-129">Uppdatera skriptet till rätt Hortonworks Data Platform (HDP) version.</span><span class="sxs-lookup"><span data-stu-id="90484-129">Update the script to the correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="90484-130">(Vi arbetar på att skripten generiska i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90484-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="90484-131">Allmänt skript kan köras från en nod i klustret utan ändringar av användaren.)</span><span class="sxs-lookup"><span data-stu-id="90484-131">Generic scripts can run from any node on the cluster without modifications by the user.)</span></span>

<span data-ttu-id="90484-132">Kommandot Exportera skriver metadata till en Apache Hadoop Distributed File System (HDFS)-sökväg (i Azure Blob Storage eller Azure Data Lake Store store) på en plats som du anger.</span><span class="sxs-lookup"><span data-stu-id="90484-132">The export command writes the metadata to an Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="90484-133">Exempel</span><span class="sxs-lookup"><span data-stu-id="90484-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="90484-134">Exportera offset metadata</span><span class="sxs-lookup"><span data-stu-id="90484-134">Export offset metadata</span></span>
1. <span data-ttu-id="90484-135">Använda SSH för att gå till ZooKeeper-kluster på klustret där kontrollpunkten förskjutning måste exporteras.</span><span class="sxs-lookup"><span data-stu-id="90484-135">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="90484-136">Kör följande kommando (när du har uppdaterat Versionsträngen HDP) om du vill exportera ZooKeeper offset data till /stormmetadta/zkdata HDFS-sökväg:</span><span class="sxs-lookup"><span data-stu-id="90484-136">Run the following command (after you update the HDP version string) to export ZooKeeper offset data to the /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="90484-137">Importera offset metadata</span><span class="sxs-lookup"><span data-stu-id="90484-137">Import offset metadata</span></span>
1. <span data-ttu-id="90484-138">Använda SSH för att gå till ZooKeeper-kluster på klustret där kontrollpunkten förskjutning måste exporteras.</span><span class="sxs-lookup"><span data-stu-id="90484-138">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="90484-139">Kör följande kommando (när du har uppdaterat Versionsträngen HDP) för att importera ZooKeeper offset data från HDFS sökväg /stormmetadata/zkdata till målklustret ZooKeeper-servern:</span><span class="sxs-lookup"><span data-stu-id="90484-139">Run the following command (after you update the HDP version string) to import ZooKeeper offset data from the HDFS path /stormmetadata/zkdata to the ZooKeeper server on the target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a><span data-ttu-id="90484-140">Ta bort offset metadata så att topologier kan starta databearbetning från början, eller från en tidsstämpel som användaren väljer</span><span class="sxs-lookup"><span data-stu-id="90484-140">Delete offset metadata so that topologies can start processing data from the beginning, or from a timestamp that the user chooses</span></span>
1. <span data-ttu-id="90484-141">Använda SSH för att gå till ZooKeeper-kluster på klustret där kontrollpunkten förskjutning måste exporteras.</span><span class="sxs-lookup"><span data-stu-id="90484-141">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="90484-142">Kör följande kommando (när du har uppdaterat Versionsträngen HDP) för att ta bort alla ZooKeeper offset data i det aktuella klustret:</span><span class="sxs-lookup"><span data-stu-id="90484-142">Run the following command (after you update the HDP version string) to delete all ZooKeeper offset data in the current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="90484-143">Hur jag för att hitta Storm binärfiler i ett kluster</span><span class="sxs-lookup"><span data-stu-id="90484-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="90484-144">Storm-binärfiler för den aktuella HDP-stacken är i /usr/hdp/current/storm-client.</span><span class="sxs-lookup"><span data-stu-id="90484-144">Storm binaries for the current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="90484-145">Platsen är samma både för huvudnoderna och arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="90484-145">The location is the same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="90484-146">Det kan finnas flera binärfiler för specifika HDP versioner i /usr/hdp (till exempel /usr/hdp/2.5.0.1233/storm).</span><span class="sxs-lookup"><span data-stu-id="90484-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="90484-147">Mappen /usr/hdp/current/storm-client är symlinked till den senaste versionen som körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="90484-147">The /usr/hdp/current/storm-client folder is symlinked to the latest version that is running on the cluster.</span></span>

<span data-ttu-id="90484-148">Mer information finns i [Anslut till ett HDInsight-kluster med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) och [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="90484-148">For more information, see [Connect to an HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="90484-149">Hur vet topologi för distribution av ett Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="90484-149">How do I determine the deployment topology of a Storm cluster</span></span>
<span data-ttu-id="90484-150">Först identifiera alla komponenter som installeras med Storm på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="90484-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="90484-151">Ett Storm-kluster består av fyra nod:</span><span class="sxs-lookup"><span data-stu-id="90484-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="90484-152">Gateway-noder</span><span class="sxs-lookup"><span data-stu-id="90484-152">Gateway nodes</span></span>
* <span data-ttu-id="90484-153">HEAD-noder</span><span class="sxs-lookup"><span data-stu-id="90484-153">Head nodes</span></span>
* <span data-ttu-id="90484-154">ZooKeeper-noder</span><span class="sxs-lookup"><span data-stu-id="90484-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="90484-155">Arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="90484-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="90484-156">Gateway-noder</span><span class="sxs-lookup"><span data-stu-id="90484-156">Gateway nodes</span></span>
<span data-ttu-id="90484-157">En gateway-noden är en gateway och en omvänd proxy-tjänst som gör offentlig åtkomst till en aktiv Ambari management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="90484-157">A gateway node is a gateway and reverse proxy service that enables public access to an active Ambari management service.</span></span> <span data-ttu-id="90484-158">Den hanterar också Ambari ledande val.</span><span class="sxs-lookup"><span data-stu-id="90484-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="90484-159">HEAD-noder</span><span class="sxs-lookup"><span data-stu-id="90484-159">Head nodes</span></span>
<span data-ttu-id="90484-160">Storm huvudnoderna kör följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="90484-160">Storm head nodes run the following services:</span></span>
* <span data-ttu-id="90484-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="90484-161">Nimbus</span></span>
* <span data-ttu-id="90484-162">Ambari-server</span><span class="sxs-lookup"><span data-stu-id="90484-162">Ambari server</span></span>
* <span data-ttu-id="90484-163">Ambari mått server</span><span class="sxs-lookup"><span data-stu-id="90484-163">Ambari Metrics server</span></span>
* <span data-ttu-id="90484-164">Ambari mått insamlaren</span><span class="sxs-lookup"><span data-stu-id="90484-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="90484-165">ZooKeeper-noder</span><span class="sxs-lookup"><span data-stu-id="90484-165">ZooKeeper nodes</span></span>
<span data-ttu-id="90484-166">HDInsight levereras med ett kvorum för ZooKeeper tre noder.</span><span class="sxs-lookup"><span data-stu-id="90484-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="90484-167">Kvorum storleken är fast och kan inte konfigureras.</span><span class="sxs-lookup"><span data-stu-id="90484-167">The quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="90484-168">Storm-tjänster i klustret har konfigurerats för att automatiskt använda ZooKeeper kvorum.</span><span class="sxs-lookup"><span data-stu-id="90484-168">Storm services in the cluster are configured to automatically use the ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="90484-169">Arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="90484-169">Worker nodes</span></span>
<span data-ttu-id="90484-170">Storm arbetarnoder kör följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="90484-170">Storm worker nodes run the following services:</span></span>
* <span data-ttu-id="90484-171">Överordnad</span><span class="sxs-lookup"><span data-stu-id="90484-171">Supervisor</span></span>
* <span data-ttu-id="90484-172">Worker Java virtuella datorer (JVMs) för topologier som körs</span><span class="sxs-lookup"><span data-stu-id="90484-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="90484-173">Ambari-agent</span><span class="sxs-lookup"><span data-stu-id="90484-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="90484-174">Hur jag för att hitta Storm event hub kanal binärfiler för utveckling</span><span class="sxs-lookup"><span data-stu-id="90484-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="90484-175">Mer information om hur du använder Storm event hub kanal .jar filer med din topologi finns i följande resurser.</span><span class="sxs-lookup"><span data-stu-id="90484-175">For more information about using Storm event hub spout .jar files with your topology, see the following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="90484-176">Java-baserad topologi</span><span class="sxs-lookup"><span data-stu-id="90484-176">Java-based topology</span></span>
[<span data-ttu-id="90484-177">Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)</span><span class="sxs-lookup"><span data-stu-id="90484-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="90484-178">C#-baserade topologi (Mono på HDInsight 3.4 + Linux Storm-kluster)</span><span class="sxs-lookup"><span data-stu-id="90484-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
<span data-ttu-id="90484-179">[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology) (Bearbeta händelser från Azure Event Hubs med Storm i HDInsight (C#))</span><span class="sxs-lookup"><span data-stu-id="90484-179">[Process events from Azure Event Hubs with Storm on HDInsight (C#)](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)</span></span>
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="90484-180">Senaste Storm event hub-kanalen binärfiler för HDInsight 3.5 + Linux Storm-kluster</span><span class="sxs-lookup"><span data-stu-id="90484-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="90484-181">Information om hur du använder den senaste Storm event hub-kanal som fungerar med HDInsight 3.5 + Linux Storm-kluster finns mvn-lagringsplatsen [Readme-filen](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="90484-181">To learn how to use the latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see the mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="90484-182">Käll-kodexempel</span><span class="sxs-lookup"><span data-stu-id="90484-182">Source code examples</span></span>
<span data-ttu-id="90484-183">Se [exempel](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) på hur du kan läsa och skriva från Azure Event Hub med en Apache Storm-topologi (skriven i Java) på ett Azure HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="90484-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how to read and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="90484-184">Hur jag för att hitta Storm Log4J konfigurationsfiler på kluster</span><span class="sxs-lookup"><span data-stu-id="90484-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="90484-185">Identifiera Apache Log4J konfigurationsfiler för Storm-tjänster.</span><span class="sxs-lookup"><span data-stu-id="90484-185">To identify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="90484-186">På huvudnoderna</span><span class="sxs-lookup"><span data-stu-id="90484-186">On head nodes</span></span>
<span data-ttu-id="90484-187">Nimbus Log4J konfigurationen läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="90484-187">The Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="90484-188">På arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="90484-188">On worker nodes</span></span>
<span data-ttu-id="90484-189">Konfigurationen av handledarens Log4J läses från/usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="90484-189">The supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="90484-190">Worker Log4J-konfigurationsfilen har lästs från/usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="90484-190">The worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="90484-191">Exempel: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="90484-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

