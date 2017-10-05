---
title: "Felsöka HBase med Azure HDInsight | Microsoft Docs"
description: "Få svar på vanliga frågor om hur du arbetar med HBase och Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinver
manager: ashitg
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 7/7/2017
ms.author: nitinver
ms.openlocfilehash: 15412c3853a2b8436c5e96034c9a92a2a1094662
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a><span data-ttu-id="d6081-103">Felsöka HBase med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d6081-103">Troubleshoot HBase by using Azure HDInsight</span></span>

<span data-ttu-id="d6081-104">Läs mer om de vanligaste problemen och sina lösningar när du arbetar med Apache HBase nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="d6081-104">Learn about the top issues and their resolutions when working with Apache HBase payloads in Apache Ambari.</span></span>

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a><span data-ttu-id="d6081-105">Hur kör hbck kommandot rapporter med flera otilldelade regioner</span><span class="sxs-lookup"><span data-stu-id="d6081-105">How do I run hbck command reports with multiple unassigned regions</span></span>

<span data-ttu-id="d6081-106">Ett vanligt felmeddelande som kan visas när du kör den `hbase hbck` kommandot är ”flera regioner som otilldelade eller tomrum i kedjan av regioner”.</span><span class="sxs-lookup"><span data-stu-id="d6081-106">A common error message that you might see when you run the `hbase hbck` command is "multiple regions being unassigned or holes in the chain of regions."</span></span>

<span data-ttu-id="d6081-107">I HBase Master-UI visas antalet regioner obalanserade över alla region-servrar.</span><span class="sxs-lookup"><span data-stu-id="d6081-107">In the HBase Master UI, you can see the number of regions that are unbalanced across all region servers.</span></span> <span data-ttu-id="d6081-108">Kör sedan `hbase hbck` kommandot för att se tomrum i kedjan region.</span><span class="sxs-lookup"><span data-stu-id="d6081-108">Then, you can run `hbase hbck` command to see holes in the region chain.</span></span>

<span data-ttu-id="d6081-109">Hål kanske på grund av regionerna som är offline, så åtgärda tilldelningarna först.</span><span class="sxs-lookup"><span data-stu-id="d6081-109">Holes might be caused by the offline regions, so fix the assignments first.</span></span> 

<span data-ttu-id="d6081-110">Utför följande steg för att göra otilldelade regioner till normalt läge:</span><span class="sxs-lookup"><span data-stu-id="d6081-110">To bring the unassigned regions back to a normal state, complete the following steps:</span></span>

1. <span data-ttu-id="d6081-111">Logga in på HDInsight-HBase-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="d6081-111">Sign in to the HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="d6081-112">Om du vill ansluta till ZooKeeper-shell, kör den `hbase zkcli` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-112">To connect with the ZooKeeper shell, run the `hbase zkcli` command.</span></span>
3. <span data-ttu-id="d6081-113">Kör den `rmr /hbase/regions-in-transition` kommando eller `rmr /hbase-unsecure/regions-in-transition` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-113">Run the `rmr /hbase/regions-in-transition` command or the `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="d6081-114">Avsluta från den `hbase zkcli` shell använder den `exit` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-114">To exit from the `hbase zkcli` shell, use the `exit` command.</span></span>
5. <span data-ttu-id="d6081-115">Öppna Gränssnittet Apache Ambari och starta sedan om tjänsten Active HBase Master.</span><span class="sxs-lookup"><span data-stu-id="d6081-115">Open the Apache Ambari UI, and then restart the Active HBase Master service.</span></span>
6. <span data-ttu-id="d6081-116">Kör den `hbase hbck` kommandot igen (utan alternativ).</span><span class="sxs-lookup"><span data-stu-id="d6081-116">Run the `hbase hbck` command again (without any options).</span></span> <span data-ttu-id="d6081-117">Kontrollera utdata från kommandot ska se till att alla regioner som tilldelas.</span><span class="sxs-lookup"><span data-stu-id="d6081-117">Check the output of this command to ensure that all regions are being assigned.</span></span>


## <span data-ttu-id="d6081-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hur kan jag åtgärda timeout problem när du använder hbck kommandon för region tilldelningar</span><span class="sxs-lookup"><span data-stu-id="d6081-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>How do I fix timeout issues when using hbck commands for region assignments</span></span>

### <a name="issue"></a><span data-ttu-id="d6081-119">Problem</span><span class="sxs-lookup"><span data-stu-id="d6081-119">Issue</span></span>

<span data-ttu-id="d6081-120">En möjlig orsak för timeout problem när du använder den `hbck` kommandot kan vara att flera regioner är i tillståndet ”i ett övergångsstadium” under lång tid.</span><span class="sxs-lookup"><span data-stu-id="d6081-120">A potential cause for timeout issues when you use the `hbck` command might be that several regions are in the "in transition" state for a long time.</span></span> <span data-ttu-id="d6081-121">Du kan se dessa regioner som offline i HBase Master UI.</span><span class="sxs-lookup"><span data-stu-id="d6081-121">You can see those regions as offline in the HBase Master UI.</span></span> <span data-ttu-id="d6081-122">Eftersom ett stort antal regioner försöker övergång, HBase Master kan timeout och går inte att sätta dessa regioner online igen.</span><span class="sxs-lookup"><span data-stu-id="d6081-122">Because a high number of regions are attempting to transition, HBase Master might timeout and be unable to bring those regions back online.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d6081-123">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-123">Resolution steps</span></span>

1. <span data-ttu-id="d6081-124">Logga in på HDInsight-HBase-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="d6081-124">Sign in to the HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="d6081-125">Om du vill ansluta till ZooKeeper-shell, kör den `hbase zkcli` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-125">To connect with the ZooKeeper shell, run the `hbase zkcli` command.</span></span>
3. <span data-ttu-id="d6081-126">Kör den `rmr /hbase/regions-in-transition` eller `rmr /hbase-unsecure/regions-in-transition` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-126">Run the `rmr /hbase/regions-in-transition` or the `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="d6081-127">Avsluta den `hbase zkcli` shell använder den `exit` kommando.</span><span class="sxs-lookup"><span data-stu-id="d6081-127">To exit the `hbase zkcli` shell, use the `exit` command.</span></span>
5. <span data-ttu-id="d6081-128">Starta om tjänsten Active HBase Master i Ambari-UI.</span><span class="sxs-lookup"><span data-stu-id="d6081-128">In the Ambari UI, restart the Active HBase Master service.</span></span>
6. <span data-ttu-id="d6081-129">Kör den `hbase hbck -fixAssignments` kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="d6081-129">Run the `hbase hbck -fixAssignments` command again.</span></span>

## <span data-ttu-id="d6081-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hur jag force-inaktivera HDFS felsäkert läge i ett kluster</span><span class="sxs-lookup"><span data-stu-id="d6081-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>How do I force-disable HDFS safe mode in a cluster</span></span>

### <a name="issue"></a><span data-ttu-id="d6081-131">Problem</span><span class="sxs-lookup"><span data-stu-id="d6081-131">Issue</span></span>

<span data-ttu-id="d6081-132">Den lokala Hadoop Distributed File System (HDFS) har fastnat i felsäkert läge i HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="d6081-132">The local Hadoop Distributed File System (HDFS) is stuck in safe mode on the HDInsight cluster.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d6081-133">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="d6081-133">Detailed description</span></span>

<span data-ttu-id="d6081-134">Det här felet kan orsakas av ett fel när du kör följande kommando för HDFS:</span><span class="sxs-lookup"><span data-stu-id="d6081-134">This error might be caused by a failure when you run the following HDFS command:</span></span>

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

<span data-ttu-id="d6081-135">Felet kan uppstå när du försöker köra kommandot ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="d6081-135">The error you might see when you try to run the command looks like this:</span></span>

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" to turn safe mode off.
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkNameNodeSafeMode(FSNamesystem.java:1359)
        at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.mkdirs(FSNamesystem.java:4010)
        at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.mkdirs(NameNodeRpcServer.java:1102)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.mkdirs(ClientNamenodeProtocolServerSideTranslatorPB.java:630)
        at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:640)
        at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:982)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2313)
        at org.apache.hadoop.ipc.Server$Handler$1.run(Server.java:2309)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:422)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1724)
        at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2307)
        at org.apache.hadoop.ipc.Client.getRpcResponse(Client.java:1552)
        at org.apache.hadoop.ipc.Client.call(Client.java:1496)
        at org.apache.hadoop.ipc.Client.call(Client.java:1396)
        at org.apache.hadoop.ipc.ProtobufRpcEngine$Invoker.invoke(ProtobufRpcEngine.java:233)
        at com.sun.proxy.$Proxy10.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolTranslatorPB.mkdirs(ClientNamenodeProtocolTranslatorPB.java:603)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:498)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invokeMethod(RetryInvocationHandler.java:278)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:194)
        at org.apache.hadoop.io.retry.RetryInvocationHandler.invoke(RetryInvocationHandler.java:176)
        at com.sun.proxy.$Proxy11.mkdirs(Unknown Source)
        at org.apache.hadoop.hdfs.DFSClient.primitiveMkdir(DFSClient.java:3061)
        at org.apache.hadoop.hdfs.DFSClient.mkdirs(DFSClient.java:3031)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1162)
        at org.apache.hadoop.hdfs.DistributedFileSystem$24.doCall(DistributedFileSystem.java:1158)
        at org.apache.hadoop.fs.FileSystemLinkResolver.resolve(FileSystemLinkResolver.java:81)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirsInternal(DistributedFileSystem.java:1158)
        at org.apache.hadoop.hdfs.DistributedFileSystem.mkdirs(DistributedFileSystem.java:1150)
        at org.apache.hadoop.fs.FileSystem.mkdirs(FileSystem.java:1898)
        at org.apache.hadoop.fs.shell.Mkdir.processNonexistentPath(Mkdir.java:76)
        at org.apache.hadoop.fs.shell.Command.processArgument(Command.java:273)
        at org.apache.hadoop.fs.shell.Command.processArguments(Command.java:255)
        at org.apache.hadoop.fs.shell.FsCommand.processRawArguments(FsCommand.java:119)
        at org.apache.hadoop.fs.shell.Command.run(Command.java:165)
        at org.apache.hadoop.fs.FsShell.run(FsShell.java:297)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
        at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:90)
        at org.apache.hadoop.fs.FsShell.main(FsShell.java:350)
mkdir: Cannot create directory /temp. Name node is in safe mode.
```

### <a name="probable-cause"></a><span data-ttu-id="d6081-136">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="d6081-136">Probable cause</span></span>

<span data-ttu-id="d6081-137">HDInsight-klustret har minskats till ett mycket få noder.</span><span class="sxs-lookup"><span data-stu-id="d6081-137">The HDInsight cluster has been scaled down to a very few nodes.</span></span> <span data-ttu-id="d6081-138">Antalet noder som är mindre än eller nära HDFS replikering faktorn.</span><span class="sxs-lookup"><span data-stu-id="d6081-138">The number of nodes is below or close to the HDFS replication factor.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d6081-139">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-139">Resolution steps</span></span> 

1. <span data-ttu-id="d6081-140">Hämta status för i HDFS på HDInsight-kluster genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d6081-140">Get the status of the HDFS on the HDInsight cluster by running the following commands:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   ```

   ```apache
   hdiuser@hn0-spark2:~$ hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -report
   Safe mode is ON
   Configured Capacity: 3372381241344 (3.07 TB)
   Present Capacity: 3138625077248 (2.85 TB)
   DFS Remaining: 3102710317056 (2.82 TB)
   DFS Used: 35914760192 (33.45 GB)
   DFS Used%: 1.14%
   Under replicated blocks: 0
   Blocks with corrupt replicas: 0
   Missing blocks: 0
   Missing blocks (with replication factor 1): 0

   -------------------------------------------------
   Live datanodes (8):

   Name: 10.0.0.17:30010 (10.0.0.17)
   Hostname: 10.0.0.17
   Decommission Status : Normal
   Configured Capacity: 421547655168 (392.60 GB)
   DFS Used: 5288128512 (4.92 GB)
   Non DFS Used: 29087272960 (27.09 GB)
   DFS Remaining: 387172253696 (360.58 GB)
   DFS Used%: 1.25%
   DFS Remaining%: 91.85%
   Configured Cache Capacity: 0 (0 B)
   Cache Used: 0 (0 B)
   Cache Remaining: 0 (0 B)
   Cache Used%: 100.00%
   Cache Remaining%: 0.00%
   Xceivers: 2
   Last contact: Wed Apr 05 16:22:00 UTC 2017
   ...

   ```
2. <span data-ttu-id="d6081-141">Du kan också kontrollera integriteten för HDFS på HDInsight-kluster med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d6081-141">You also can check the integrity of the HDFS on the HDInsight cluster by using the following commands:</span></span>

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting to namenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
   FSCK started by hdiuser (auth:SIMPLE) from /10.0.0.22 for path / at Wed Apr 05 16:40:28 UTC 2017
   ....................................................................................................

   ....................................................................................................
   ..................Status: HEALTHY
   Total size:    9330539472 B
   Total dirs:    37
   Total files:   2618
   Total symlinks:                0 (Files currently being written: 2)
   Total blocks (validated):      2535 (avg. block size 3680686 B)
   Minimally replicated blocks:   2535 (100.0 %)
   Over-replicated blocks:        0 (0.0 %)
   Under-replicated blocks:       0 (0.0 %)
   Mis-replicated blocks:         0 (0.0 %)
   Default replication factor:    3
   Average block replication:     3.0
   Corrupt blocks:                0
   Missing replicas:              0 (0.0 %)
   Number of data-nodes:          8
   Number of racks:               1
   FSCK ended at Wed Apr 05 16:40:28 UTC 2017 in 187 milliseconds

   The filesystem under path '/' is HEALTHY
   ```

3. <span data-ttu-id="d6081-142">Om du anser att det finns ingen saknas, skadad, eller under-replikerade block eller att dessa block kan ignoreras, kör följande kommando för att ta namn ur noden ur felsäkert läge:</span><span class="sxs-lookup"><span data-stu-id="d6081-142">If you determine that there are no missing, corrupt, or under-replicated blocks, or that those blocks can be ignored, run the following command to take the name node out of safe mode:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a><span data-ttu-id="d6081-143">Hur kan jag åtgärda JDBC eller SQLLine anslutningen problem med Apache Phoenix</span><span class="sxs-lookup"><span data-stu-id="d6081-143">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d6081-144">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-144">Resolution steps</span></span>

<span data-ttu-id="d6081-145">För att ansluta med Phoenix, måste du ange IP-adressen för en aktiv ZooKeeper-nod.</span><span class="sxs-lookup"><span data-stu-id="d6081-145">To connect with Phoenix, you must provide the IP address of an active ZooKeeper node.</span></span> <span data-ttu-id="d6081-146">Se till att tjänsten ZooKeeper till vilka sqlline.py försöker ansluta är igång.</span><span class="sxs-lookup"><span data-stu-id="d6081-146">Ensure that the ZooKeeper service to which sqlline.py is trying to connect is up and running.</span></span>
1. <span data-ttu-id="d6081-147">Logga in till HDInsight-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="d6081-147">Sign in to the HDInsight cluster by using SSH.</span></span>
2. <span data-ttu-id="d6081-148">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d6081-148">Enter the following command:</span></span>
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > <span data-ttu-id="d6081-149">Du kan hämta IP-adressen för den aktiva noden för ZooKeeper från Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="d6081-149">You can get the IP address of the active ZooKeeper node from the Ambari UI.</span></span> <span data-ttu-id="d6081-150">Gå till **HBase** > **snabblänkar** > **ZK\* (aktiv)** > **Zookeeper Info**.</span><span class="sxs-lookup"><span data-stu-id="d6081-150">Go to **HBase** > **Quick Links** > **ZK\* (Active)** > **Zookeeper Info**.</span></span> 

3. <span data-ttu-id="d6081-151">Om sqlline.py ansluter till Phoenix och inte inte timeout, kör du följande kommando för att kontrollera tillgängligheten och hälsotillståndet för Phoenix:</span><span class="sxs-lookup"><span data-stu-id="d6081-151">If the sqlline.py connects to Phoenix and does not timeout, run the following command to validate the availability and health of Phoenix:</span></span>

   ```apache
           !tables
           !quit
   ```      
4. <span data-ttu-id="d6081-152">Om det här kommandot fungerar finns det inga problem.</span><span class="sxs-lookup"><span data-stu-id="d6081-152">If this command works, there is no issue.</span></span> <span data-ttu-id="d6081-153">IP-adressen som användaren kan vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="d6081-153">The IP address provided by the user might be incorrect.</span></span> <span data-ttu-id="d6081-154">Om kommandot pausar en längre tid och visar sedan följande fel, Fortsätt till steg 5.</span><span class="sxs-lookup"><span data-stu-id="d6081-154">However, if the command pauses for an extended time and then displays the following error, continue to step 5.</span></span>

   ```apache
           Error while connecting to sqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting to jdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. <span data-ttu-id="d6081-155">Kör följande kommandon från huvudnod (hn0) för att diagnostisera villkoret Phoenix-SYSTEM. KATALOGEN tabell:</span><span class="sxs-lookup"><span data-stu-id="d6081-155">Run the following commands from the head node (hn0) to diagnose the condition of the Phoenix SYSTEM.CATALOG table:</span></span>

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   <span data-ttu-id="d6081-156">Kommandot ska returnera ett fel som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="d6081-156">The command should return an error similar to the following:</span></span> 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. <span data-ttu-id="d6081-157">Utför följande steg om du vill starta om tjänsten HMaster på alla ZooKeeper-noder i Ambari-UI:</span><span class="sxs-lookup"><span data-stu-id="d6081-157">In the Ambari UI, complete the following steps to restart the HMaster service on all ZooKeeper nodes:</span></span>

    1. <span data-ttu-id="d6081-158">I den **sammanfattning** avsnitt i HBase, gå till **HBase** > **Active HBase Master**.</span><span class="sxs-lookup"><span data-stu-id="d6081-158">In the **Summary** section of HBase, go to **HBase** > **Active HBase Master**.</span></span> 
    2. <span data-ttu-id="d6081-159">I den **komponenter** och starta om tjänsten HBase Master.</span><span class="sxs-lookup"><span data-stu-id="d6081-159">In the **Components** section, restart the HBase Master service.</span></span>
    3. <span data-ttu-id="d6081-160">Upprepa dessa steg för alla återstående **vänteläge HBase Master** tjänster.</span><span class="sxs-lookup"><span data-stu-id="d6081-160">Repeat these steps for all remaining **Standby HBase Master** services.</span></span> 

<span data-ttu-id="d6081-161">Det kan ta upp till fem minuter för tjänsten HBase Master att hålla och slutföra återställningen.</span><span class="sxs-lookup"><span data-stu-id="d6081-161">It can take up to five minutes for the HBase Master service to stabilize and finish the recovery process.</span></span> <span data-ttu-id="d6081-162">Upprepa sqlline.py-kommandon för att bekräfta att systemet efter några minuter. KATALOGEN tabell är igång och att den kan efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="d6081-162">After a few minutes, repeat the sqlline.py commands to confirm that the SYSTEM.CATALOG table is up, and that it can be queried.</span></span> 

<span data-ttu-id="d6081-163">När systemet. KATALOGEN tabellen är tillbaka till normal, problemet med anslutningen till Phoenix bör lösas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d6081-163">When the SYSTEM.CATALOG table is back to normal, the connectivity issue to Phoenix should be automatically resolved.</span></span>


## <a name="what-causes-a-master-server-to-fail-to-start"></a><span data-ttu-id="d6081-164">Vad som orsakar en huvudserver inte startas</span><span class="sxs-lookup"><span data-stu-id="d6081-164">What causes a master server to fail to start</span></span>

### <a name="error"></a><span data-ttu-id="d6081-165">Fel</span><span class="sxs-lookup"><span data-stu-id="d6081-165">Error</span></span> 

<span data-ttu-id="d6081-166">En atomisk namnbyte felet inträffar.</span><span class="sxs-lookup"><span data-stu-id="d6081-166">An atomic renaming failure occurs.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d6081-167">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="d6081-167">Detailed description</span></span>

<span data-ttu-id="d6081-168">Under startprocessen Slutför HMaster många initieringssstegen.</span><span class="sxs-lookup"><span data-stu-id="d6081-168">During the startup process, HMaster completes many initialization steps.</span></span> <span data-ttu-id="d6081-169">Dessa inkluderar flytta data från mappen grunden (tmp) till datamappen.</span><span class="sxs-lookup"><span data-stu-id="d6081-169">These include moving data from the scratch (.tmp) folder to the data folder.</span></span> <span data-ttu-id="d6081-170">HMaster kontrollerar också mappen write-ahead-loggar (WALs) om det finns några svarar region-servrar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="d6081-170">HMaster also looks at the write-ahead logs (WALs) folder to see if there are any unresponsive region servers, and so on.</span></span> 

<span data-ttu-id="d6081-171">Under start HMaster har en grundläggande `list` på dessa mappar.</span><span class="sxs-lookup"><span data-stu-id="d6081-171">During startup, HMaster does a basic `list` command on these folders.</span></span> <span data-ttu-id="d6081-172">Om HMaster ser en oväntad fil i någon av dessa mappar när som helst, genererar ett undantag och startar inte.</span><span class="sxs-lookup"><span data-stu-id="d6081-172">If at any time, HMaster sees an unexpected file in any of these folders, it throws an exception and doesn't start.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="d6081-173">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="d6081-173">Probable cause</span></span>

<span data-ttu-id="d6081-174">Försök att identifiera tidslinjen då filen skapades och se om det fanns en process krasch runt den tid då filen skapades i serverloggen region.</span><span class="sxs-lookup"><span data-stu-id="d6081-174">In the region server logs, try to identify the timeline of the file creation, and then see if there was a process crash around the time the file was created.</span></span> <span data-ttu-id="d6081-175">(Kontakta HBase support att hjälpa dig att göra detta). Det här hjälper oss tillhandahålla mer robusta mekanismer så att du kan undvika träffa programfelet och se till att rätt processen avstängningar.</span><span class="sxs-lookup"><span data-stu-id="d6081-175">(Contact HBase support to assist you in doing this.) This helps us provide more robust mechanisms, so that you can avoid hitting this bug, and ensure graceful process shutdowns.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="d6081-176">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-176">Resolution steps</span></span>

<span data-ttu-id="d6081-177">Kontrollera anropsstacken och försök att avgöra vilken mapp den orsakar problemet (till exempel kanske inte WALs mappen eller tmp).</span><span class="sxs-lookup"><span data-stu-id="d6081-177">Check the call stack and try to determine which folder might be causing the problem (for instance, it might be the WALs folder or the .tmp folder).</span></span> <span data-ttu-id="d6081-178">I Cloud Explorer eller med hjälp av HDFS-kommandon, försök sedan att leta reda på problemfilen.</span><span class="sxs-lookup"><span data-stu-id="d6081-178">Then, in Cloud Explorer or by using HDFS commands, try to locate the problem file.</span></span> <span data-ttu-id="d6081-179">Detta är normalt en \*-renamePending.json-filen.</span><span class="sxs-lookup"><span data-stu-id="d6081-179">Usually, this is a \*-renamePending.json file.</span></span> <span data-ttu-id="d6081-180">(Den \*-renamePending.json-filen är en journal som används för att genomföra åtgärden namnbyte i WASB-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="d6081-180">(The \*-renamePending.json file is a journal file that's used to implement the atomic rename operation in the WASB driver.</span></span> <span data-ttu-id="d6081-181">På grund av programfel i den här implementeringen kan dessa filer lämnas efter egna processer som kraschat osv.) Force-ta bort den här filen i Cloud Explorer eller med hjälp av HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="d6081-181">Due to bugs in this implementation, these files can be left over after process crashes, and so on.) Force-delete this file either in Cloud Explorer or by using HDFS commands.</span></span> 

<span data-ttu-id="d6081-182">Ibland kanske en temporär fil som heter ungefär *$$$. $$$* på den här platsen.</span><span class="sxs-lookup"><span data-stu-id="d6081-182">Sometimes, there might also be a temporary file named something like *$$$.$$$* at this location.</span></span> <span data-ttu-id="d6081-183">Du måste använda HDFS `ls` kommandot för att se filen, du kan inte se filen i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="d6081-183">You have to use HDFS `ls` command to see this file; you cannot see the file in Cloud Explorer.</span></span> <span data-ttu-id="d6081-184">Om du vill ta bort den här filen använder du kommandot HDFS `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span><span class="sxs-lookup"><span data-stu-id="d6081-184">To delete this file, use the HDFS command `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span></span>  

<span data-ttu-id="d6081-185">När du har kört dessa kommandon ska HMaster starta direkt.</span><span class="sxs-lookup"><span data-stu-id="d6081-185">After you've run these commands, HMaster should start immediately.</span></span> 

### <a name="error"></a><span data-ttu-id="d6081-186">Fel</span><span class="sxs-lookup"><span data-stu-id="d6081-186">Error</span></span>

<span data-ttu-id="d6081-187">Ingen serveradressen listas i *hbase: meta* för region xxx.</span><span class="sxs-lookup"><span data-stu-id="d6081-187">No server address is listed in *hbase: meta* for region xxx.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d6081-188">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="d6081-188">Detailed description</span></span>

<span data-ttu-id="d6081-189">Du kan se ett meddelande på ditt Linux-kluster som indikerar att den *hbase: meta* tabellen är inte online.</span><span class="sxs-lookup"><span data-stu-id="d6081-189">You might see a message on your Linux cluster that indicates that the *hbase: meta* table is not online.</span></span> <span data-ttu-id="d6081-190">Kör `hbck` kan rapportera som ”hbase: meta tabell replicaId 0 hittades inte på en region”.</span><span class="sxs-lookup"><span data-stu-id="d6081-190">Running `hbck` might report that "hbase: meta table replicaId 0 is not found on any region."</span></span> <span data-ttu-id="d6081-191">Problemet kan bero på att HMaster inte kunde initiera när du har startat om HBase.</span><span class="sxs-lookup"><span data-stu-id="d6081-191">The problem might be that HMaster could not initialize after you restarted HBase.</span></span> <span data-ttu-id="d6081-192">I loggarna HMaster, kan du se meddelandet ”: ingen serveradress som anges i hbase: metadata för region hbase: säkerhetskopiering \<regionnamn\>”.</span><span class="sxs-lookup"><span data-stu-id="d6081-192">In the HMaster logs, you might see the message: "No server address listed in hbase: meta for region hbase: backup \<region name\>".</span></span>  

### <a name="resolution-steps"></a><span data-ttu-id="d6081-193">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-193">Resolution steps</span></span>

1. <span data-ttu-id="d6081-194">Ange följande kommandon (ändra faktiska värden i tillämpliga fall) i HBase-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="d6081-194">In the HBase shell, enter the following commands (change actual values as applicable):</span></span>  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. <span data-ttu-id="d6081-195">Ta bort den *hbase: namnområdet* post.</span><span class="sxs-lookup"><span data-stu-id="d6081-195">Delete the *hbase: namespace* entry.</span></span> <span data-ttu-id="d6081-196">Den här posten kan vara samma fel som rapporteras när den *hbase: namnområdet* tabell genomsöks.</span><span class="sxs-lookup"><span data-stu-id="d6081-196">This entry might be the same error that's being reported when the *hbase: namespace* table is scanned.</span></span>

3. <span data-ttu-id="d6081-197">Starta om tjänsten Active HMaster för att få upp HBase i ett körtillstånd i Ambari-UI.</span><span class="sxs-lookup"><span data-stu-id="d6081-197">To bring up HBase in a running state, in the Ambari UI, restart the Active HMaster service.</span></span>  

4. <span data-ttu-id="d6081-198">I HBase-gränssnittet för att visa alla offline tabeller, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d6081-198">In the HBase shell, to bring up all offline tables, run the following command:</span></span>

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a><span data-ttu-id="d6081-199">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d6081-199">Additional reading</span></span>

[<span data-ttu-id="d6081-200">Det gick inte att bearbeta HBase-tabellen</span><span class="sxs-lookup"><span data-stu-id="d6081-200">Unable to process the HBase table</span></span>](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a><span data-ttu-id="d6081-201">Fel</span><span class="sxs-lookup"><span data-stu-id="d6081-201">Error</span></span>

<span data-ttu-id="d6081-202">HMaster tidsgränsen uppnås med ett allvarligt undantagsfel av typen ”java.io.IOException: för lång tid 300000ms väntar på att namnområdet tabellen som ska tilldelas”.</span><span class="sxs-lookup"><span data-stu-id="d6081-202">HMaster times out with a fatal exception similar to "java.io.IOException: Timedout 300000ms waiting for namespace table to be assigned."</span></span>

### <a name="detailed-description"></a><span data-ttu-id="d6081-203">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="d6081-203">Detailed description</span></span>

<span data-ttu-id="d6081-204">Det här problemet kan uppstå om du har många tabeller och regioner som inte har tömts när du startar om HMaster-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d6081-204">You might experience this issue if you have many tables and regions that have not been flushed when you restart your HMaster services.</span></span> <span data-ttu-id="d6081-205">Starta om misslyckas och föregående felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="d6081-205">Restart might fail, and you'll see the preceding error message.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="d6081-206">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="d6081-206">Probable cause</span></span>

<span data-ttu-id="d6081-207">Detta är ett känt problem med tjänsten HMaster.</span><span class="sxs-lookup"><span data-stu-id="d6081-207">This is a known issue with the HMaster service.</span></span> <span data-ttu-id="d6081-208">Allmän klustret startades uppgifter kan ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="d6081-208">General cluster startup tasks can take a long time.</span></span> <span data-ttu-id="d6081-209">HMaster stängs av eftersom tabellen namnområde ännu inte tilldelats.</span><span class="sxs-lookup"><span data-stu-id="d6081-209">HMaster shuts down because the namespace table isn’t yet assigned.</span></span> <span data-ttu-id="d6081-210">Detta inträffar endast i situationer där stora mängden unflushed data finns och en tidsgräns på fem minuter är inte tillräcklig.</span><span class="sxs-lookup"><span data-stu-id="d6081-210">This occurs only in scenarios in which large amount of unflushed data exists, and a timeout of five minutes is not sufficient.</span></span>
  
### <a name="resolution-steps"></a><span data-ttu-id="d6081-211">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-211">Resolution steps</span></span>

1. <span data-ttu-id="d6081-212">Ambari UI, gå till **HBase** > **konfigurationerna**.</span><span class="sxs-lookup"><span data-stu-id="d6081-212">In the Ambari UI, go to **HBase** > **Configs**.</span></span> <span data-ttu-id="d6081-213">Lägg till följande inställning i filen anpassade hbase-site.XML:</span><span class="sxs-lookup"><span data-stu-id="d6081-213">In the custom hbase-site.xml file, add the following setting:</span></span> 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. <span data-ttu-id="d6081-214">Starta om nödvändiga tjänster (HMaster och eventuellt andra HBase-tjänster).</span><span class="sxs-lookup"><span data-stu-id="d6081-214">Restart the required services (HMaster, and possibly other HBase services).</span></span>  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a><span data-ttu-id="d6081-215">Vad som orsakar en omstart av fel på en region-server</span><span class="sxs-lookup"><span data-stu-id="d6081-215">What causes a restart failure on a region server</span></span>

### <a name="issue"></a><span data-ttu-id="d6081-216">Problem</span><span class="sxs-lookup"><span data-stu-id="d6081-216">Issue</span></span>

<span data-ttu-id="d6081-217">Starta om fel inträffar på en region-server kan förhindras genom följande bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="d6081-217">A restart failure on a region server might be prevented by following best practices.</span></span> <span data-ttu-id="d6081-218">Vi rekommenderar att du pausar arbetsbelastning aktivitet när du planerar att starta om HBase region servrar.</span><span class="sxs-lookup"><span data-stu-id="d6081-218">We recommend that you pause heavy workload activity when you are planning to restart HBase region servers.</span></span> <span data-ttu-id="d6081-219">Om ett program fortsätter att ansluta med region servrar när shutdown pågår, blir omstarten region server långsammare med flera minuter.</span><span class="sxs-lookup"><span data-stu-id="d6081-219">If an application continues to connect with region servers when shutdown is in progress, the region server restart operation will be slower by several minutes.</span></span> <span data-ttu-id="d6081-220">Det är också bra att först tömma alla tabeller.</span><span class="sxs-lookup"><span data-stu-id="d6081-220">Also, it's a good idea to first flush all the tables.</span></span> <span data-ttu-id="d6081-221">En referens för hur du tömma tabeller finns [HDInsight HBase: hur du förbättrar tid för HBase-kluster-omstart av lokaliseraren tabeller](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span><span class="sxs-lookup"><span data-stu-id="d6081-221">For a reference on how to flush tables, see [HDInsight HBase: How to improve the HBase cluster restart time by flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span></span>

<span data-ttu-id="d6081-222">Om du har initierat omstarten på HBase region servrar från Ambari UI kan se du direkt region servrar fungerar korrekt, men de inte startas om direkt.</span><span class="sxs-lookup"><span data-stu-id="d6081-222">If you initiate the restart operation on HBase region servers from the Ambari UI, you immediately see that the region servers went down, but they don't restart right away.</span></span> 

<span data-ttu-id="d6081-223">Här är vad som händer i bakgrunden:</span><span class="sxs-lookup"><span data-stu-id="d6081-223">Here's what's happening behind the scenes:</span></span> 

1. <span data-ttu-id="d6081-224">Ambari-agenten skickar en stoppbegäran till region-servern.</span><span class="sxs-lookup"><span data-stu-id="d6081-224">The Ambari agent sends a stop request to the region server.</span></span>
2. <span data-ttu-id="d6081-225">Ambari-agent väntar på 30 sekunder för den region servern avslutas på vanligt sätt.</span><span class="sxs-lookup"><span data-stu-id="d6081-225">The Ambari agent waits for 30 seconds for the region server to shut down gracefully.</span></span> 
3. <span data-ttu-id="d6081-226">Om programmet fortsätter att ansluta till servern region, stängs servern inte omedelbart.</span><span class="sxs-lookup"><span data-stu-id="d6081-226">If your application continues to connect with the region server, the server won't shut down immediately.</span></span> <span data-ttu-id="d6081-227">30 sekunder timeout uppnås innan avstängning inträffar.</span><span class="sxs-lookup"><span data-stu-id="d6081-227">The 30-second timeout expires before shutdown occurs.</span></span> 
4. <span data-ttu-id="d6081-228">Efter 30 sekunder skickar agenten Ambari force-kill (`kill -9`) kommandot till region-servern.</span><span class="sxs-lookup"><span data-stu-id="d6081-228">After 30 seconds, the Ambari agent sends a force-kill (`kill -9`) command to the region server.</span></span> <span data-ttu-id="d6081-229">Du kan se det i loggen ambari-agent (i /var/log/directory respektive arbetsnoden):</span><span class="sxs-lookup"><span data-stu-id="d6081-229">You can see this in the ambari-agent log (in the /var/log/ directory of the respective worker node):</span></span>

   ```apache
           2017-03-21 13:22:09,171 - Execute['/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/current/hbase-regionserver/conf stop regionserver'] {'only_if': 'ambari-sudo.sh  -H -E t
           est -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1', 'on_timeout': '! ( ambari-sudo.sh  -H -E test -
           f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >/dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H 
           -E cat /var/run/hbase/hbase-hbase-regionserver.pid`', 'timeout': 30, 'user': 'hbase'}
           2017-03-21 13:22:40,268 - Executing '! ( ambari-sudo.sh  -H -E test -f /var/run/hbase/hbase-hbase-regionserver.pid && ps -p `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid` >
           /dev/null 2>&1 ) || ambari-sudo.sh -H -E kill -9 `ambari-sudo.sh  -H -E cat /var/run/hbase/hbase-hbase-regionserver.pid`'. Reason: Execution of 'ambari-sudo.sh su hbase -l -s /bin/bash -c 'export  
           PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/var/lib/ambari-agent ; /usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh --config /usr/hdp/curre
           nt/hbase-regionserver/conf stop regionserver was killed due timeout after 30 seconds
           2017-03-21 13:22:40,285 - File['/var/run/hbase/hbase-hbase-regionserver.pid'] {'action': ['delete']}
           2017-03-21 13:22:40,285 - Deleting File['/var/run/hbase/hbase-hbase-regionserver.pid']
   ```
<span data-ttu-id="d6081-230">På grund av abrupt avstängning kan den port som är associerade med processen inte släppas, även om region serverprocessen har stoppats.</span><span class="sxs-lookup"><span data-stu-id="d6081-230">Because of the abrupt shutdown, the port associated with the process might not be released, even though the region server process is stopped.</span></span> <span data-ttu-id="d6081-231">Den här situationen kan leda till en AddressBindException när region servern startas som visas i följande loggar.</span><span class="sxs-lookup"><span data-stu-id="d6081-231">This situation can lead to an AddressBindException when the region server is starting, as shown in the following logs.</span></span> <span data-ttu-id="d6081-232">Du kan kontrollera detta i den region server.log i katalogen /var/log/hbase på arbetsnoderna där region-servern inte kan startas.</span><span class="sxs-lookup"><span data-stu-id="d6081-232">You can verify this in the region-server.log in the /var/log/hbase directory on the worker nodes where the region server fails to start.</span></span> 

   ```apache

   2017-03-21 13:25:47,061 ERROR [main] regionserver.HRegionServerCommandLine: Region server exiting
   java.lang.RuntimeException: Failed construction of Regionserver: class org.apache.hadoop.hbase.regionserver.HRegionServer
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2636)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.start(HRegionServerCommandLine.java:64)
   at org.apache.hadoop.hbase.regionserver.HRegionServerCommandLine.run(HRegionServerCommandLine.java:87)
   at org.apache.hadoop.util.ToolRunner.run(ToolRunner.java:76)
   at org.apache.hadoop.hbase.util.ServerCommandLine.doMain(ServerCommandLine.java:126)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.main(HRegionServer.java:2651)
        
   Caused by: java.lang.reflect.InvocationTargetException
   at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
   at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:57)
   at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
   at java.lang.reflect.Constructor.newInstance(Constructor.java:526)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.constructRegionServer(HRegionServer.java:2634)
   ... 5 more
        
   Caused by: java.net.BindException: Problem binding to /10.2.0.4:16020 : Address already in use
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2497)
   at org.apache.hadoop.hbase.ipc.RpcServer$Listener.<init>(RpcServer.java:580)
   at org.apache.hadoop.hbase.ipc.RpcServer.<init>(RpcServer.java:1982)
   at org.apache.hadoop.hbase.regionserver.RSRpcServices.<init>(RSRpcServices.java:863)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.createRpcServices(HRegionServer.java:632)
   at org.apache.hadoop.hbase.regionserver.HRegionServer.<init>(HRegionServer.java:532)
   ... 10 more
        
   Caused by: java.net.BindException: Address already in use
   at sun.nio.ch.Net.bind0(Native Method)
   at sun.nio.ch.Net.bind(Net.java:463)
   at sun.nio.ch.Net.bind(Net.java:455)
   at sun.nio.ch.ServerSocketChannelImpl.bind(ServerSocketChannelImpl.java:223)
   at sun.nio.ch.ServerSocketAdaptor.bind(ServerSocketAdaptor.java:74)
   at org.apache.hadoop.hbase.ipc.RpcServer.bind(RpcServer.java:2495)
   ... 15 more
   ```

### <a name="resolution-steps"></a><span data-ttu-id="d6081-233">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d6081-233">Resolution steps</span></span>

1. <span data-ttu-id="d6081-234">Försök att minska belastningen på HBase region servrar innan du startar en omstart.</span><span class="sxs-lookup"><span data-stu-id="d6081-234">Try to reduce the load on the HBase region servers before you initiate a restart.</span></span> 
2. <span data-ttu-id="d6081-235">Du kan också (om steg 1 inte hjälper), försök att starta om region servrar på arbetsnoderna med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="d6081-235">Alternatively (if step 1 doesn't help), try to manually restart region servers on the worker nodes by using the following commands:</span></span>

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

