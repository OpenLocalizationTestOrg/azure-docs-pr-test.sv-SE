---
title: "aaaTroubleshoot HBase med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du arbetar med HBase och Azure HDInsight."
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
ms.openlocfilehash: 5210184f8ea95628952a95df8c98f5b98e37c53e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hbase-by-using-azure-hdinsight"></a><span data-ttu-id="ebda8-103">Felsöka HBase med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ebda8-103">Troubleshoot HBase by using Azure HDInsight</span></span>

<span data-ttu-id="ebda8-104">Läs mer om hello de främsta problemen och sina lösningar när du arbetar med Apache HBase nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="ebda8-104">Learn about hello top issues and their resolutions when working with Apache HBase payloads in Apache Ambari.</span></span>

## <a name="how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions"></a><span data-ttu-id="ebda8-105">Hur kör hbck kommandot rapporter med flera otilldelade regioner</span><span class="sxs-lookup"><span data-stu-id="ebda8-105">How do I run hbck command reports with multiple unassigned regions</span></span>

<span data-ttu-id="ebda8-106">Ett vanligt felmeddelande som kan visas när du kör hello `hbase hbck` kommandot är ”flera regioner som otilldelade eller tomrum i hello kedja med regioner”.</span><span class="sxs-lookup"><span data-stu-id="ebda8-106">A common error message that you might see when you run hello `hbase hbck` command is "multiple regions being unassigned or holes in hello chain of regions."</span></span>

<span data-ttu-id="ebda8-107">I hello HBase Master UI ser du hello antalet regioner som obalanserade över alla region-servrar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-107">In hello HBase Master UI, you can see hello number of regions that are unbalanced across all region servers.</span></span> <span data-ttu-id="ebda8-108">Kör sedan `hbase hbck` kommandot toosee tomrum i hello region kedja.</span><span class="sxs-lookup"><span data-stu-id="ebda8-108">Then, you can run `hbase hbck` command toosee holes in hello region chain.</span></span>

<span data-ttu-id="ebda8-109">Hål kan orsakas av hello offline regioner, så korrigering hello tilldelningar först.</span><span class="sxs-lookup"><span data-stu-id="ebda8-109">Holes might be caused by hello offline regions, so fix hello assignments first.</span></span> 

<span data-ttu-id="ebda8-110">toobring Hej otilldelade regioner tillbaka tooa normala tillstånd, Slutför hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="ebda8-110">toobring hello unassigned regions back tooa normal state, complete hello following steps:</span></span>

1. <span data-ttu-id="ebda8-111">Logga in toohello HDInsight HBase-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="ebda8-111">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="ebda8-112">tooconnect med hello ZooKeeper shell, kör hello `hbase zkcli` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-112">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="ebda8-113">Kör hello `rmr /hbase/regions-in-transition` kommando eller hello `rmr /hbase-unsecure/regions-in-transition` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-113">Run hello `rmr /hbase/regions-in-transition` command or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="ebda8-114">tooexit från hello `hbase zkcli` shell använder hello `exit` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-114">tooexit from hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="ebda8-115">Öppna hello Apache Ambari UI och starta sedan om hello Active HBase Master-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ebda8-115">Open hello Apache Ambari UI, and then restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="ebda8-116">Kör hello `hbase hbck` kommandot igen (utan alternativ).</span><span class="sxs-lookup"><span data-stu-id="ebda8-116">Run hello `hbase hbck` command again (without any options).</span></span> <span data-ttu-id="ebda8-117">Kontrollera hello utdata från det här kommandot tooensure som alla regioner tilldelas.</span><span class="sxs-lookup"><span data-stu-id="ebda8-117">Check hello output of this command tooensure that all regions are being assigned.</span></span>


## <span data-ttu-id="ebda8-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>Hur kan jag åtgärda timeout problem när du använder hbck kommandon för region tilldelningar</span><span class="sxs-lookup"><span data-stu-id="ebda8-118"><a name="how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments"></a>How do I fix timeout issues when using hbck commands for region assignments</span></span>

### <a name="issue"></a><span data-ttu-id="ebda8-119">Problem</span><span class="sxs-lookup"><span data-stu-id="ebda8-119">Issue</span></span>

<span data-ttu-id="ebda8-120">En möjlig orsak för timeout problem när du använder hello `hbck` kommandot kan vara att det finns flera regioner i hello ”i ett övergångsstadium” tillstånd under lång tid.</span><span class="sxs-lookup"><span data-stu-id="ebda8-120">A potential cause for timeout issues when you use hello `hbck` command might be that several regions are in hello "in transition" state for a long time.</span></span> <span data-ttu-id="ebda8-121">Du kan se dessa regioner som offline i hello HBase Master UI.</span><span class="sxs-lookup"><span data-stu-id="ebda8-121">You can see those regions as offline in hello HBase Master UI.</span></span> <span data-ttu-id="ebda8-122">Eftersom ett stort antal regioner försöker tootransition, HBase Master kan timeout och att toobring dessa regioner online igen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-122">Because a high number of regions are attempting tootransition, HBase Master might timeout and be unable toobring those regions back online.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-123">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-123">Resolution steps</span></span>

1. <span data-ttu-id="ebda8-124">Logga in toohello HDInsight HBase-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="ebda8-124">Sign in toohello HDInsight HBase cluster by using SSH.</span></span>
2. <span data-ttu-id="ebda8-125">tooconnect med hello ZooKeeper shell, kör hello `hbase zkcli` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-125">tooconnect with hello ZooKeeper shell, run hello `hbase zkcli` command.</span></span>
3. <span data-ttu-id="ebda8-126">Kör hello `rmr /hbase/regions-in-transition` eller hello `rmr /hbase-unsecure/regions-in-transition` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-126">Run hello `rmr /hbase/regions-in-transition` or hello `rmr /hbase-unsecure/regions-in-transition` command.</span></span>
4. <span data-ttu-id="ebda8-127">tooexit hello `hbase zkcli` shell använder hello `exit` kommando.</span><span class="sxs-lookup"><span data-stu-id="ebda8-127">tooexit hello `hbase zkcli` shell, use hello `exit` command.</span></span>
5. <span data-ttu-id="ebda8-128">Starta om hello Active HBase Master i hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="ebda8-128">In hello Ambari UI, restart hello Active HBase Master service.</span></span>
6. <span data-ttu-id="ebda8-129">Kör hello `hbase hbck -fixAssignments` kommandot igen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-129">Run hello `hbase hbck -fixAssignments` command again.</span></span>

## <span data-ttu-id="ebda8-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>Hur jag force-inaktivera HDFS felsäkert läge i ett kluster</span><span class="sxs-lookup"><span data-stu-id="ebda8-130"><a name="how-do-i-force-disable-hdfs-safe-mode-in-a-cluster"></a>How do I force-disable HDFS safe mode in a cluster</span></span>

### <a name="issue"></a><span data-ttu-id="ebda8-131">Problem</span><span class="sxs-lookup"><span data-stu-id="ebda8-131">Issue</span></span>

<span data-ttu-id="ebda8-132">hello lokala Hadoop Distributed File System (HDFS) har fastnat i felsäkert läge på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ebda8-132">hello local Hadoop Distributed File System (HDFS) is stuck in safe mode on hello HDInsight cluster.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="ebda8-133">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="ebda8-133">Detailed description</span></span>

<span data-ttu-id="ebda8-134">Det här felet kan orsakas av ett fel när du kör följande kommando för HDFS hello:</span><span class="sxs-lookup"><span data-stu-id="ebda8-134">This error might be caused by a failure when you run hello following HDFS command:</span></span>

```apache
hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
```

<span data-ttu-id="ebda8-135">hello-fel kan uppstå när du försöker toorun hello kommandot ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ebda8-135">hello error you might see when you try toorun hello command looks like this:</span></span>

```apache
hdiuser@hn0-spark2:~$ hdfs dfs -D "fs.default.name=hdfs://mycluster/" -mkdir /temp
17/04/05 16:20:52 WARN retry.RetryInvocationHandler: Exception while invoking ClientNamenodeProtocolTranslatorPB.mkdirs over hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net/10.0.0.22:8020. Not retrying because try once and fail.
org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.hdfs.server.namenode.SafeModeException): Cannot create directory /temp. Name node is in safe mode.
It was turned on manually. Use "hdfs dfsadmin -safemode leave" tooturn safe mode off.
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

### <a name="probable-cause"></a><span data-ttu-id="ebda8-136">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="ebda8-136">Probable cause</span></span>

<span data-ttu-id="ebda8-137">Hej HDInsight-kluster har skalats ned tooa mycket få noder.</span><span class="sxs-lookup"><span data-stu-id="ebda8-137">hello HDInsight cluster has been scaled down tooa very few nodes.</span></span> <span data-ttu-id="ebda8-138">hello antalet noder är nedan eller stänga toohello HDFS replikering faktor.</span><span class="sxs-lookup"><span data-stu-id="ebda8-138">hello number of nodes is below or close toohello HDFS replication factor.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-139">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-139">Resolution steps</span></span> 

1. <span data-ttu-id="ebda8-140">Hämta hello status för hello HDFS på hello HDInsight-kluster genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="ebda8-140">Get hello status of hello HDFS on hello HDInsight cluster by running hello following commands:</span></span>

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
2. <span data-ttu-id="ebda8-141">Du kan också kontrollera hello integriteten hos hello HDFS på hello HDInsight-kluster hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="ebda8-141">You also can check hello integrity of hello HDFS on hello HDInsight cluster by using hello following commands:</span></span>

   ```apache
   hdiuser@hn0-spark2:~$ hdfs fsck -D "fs.default.name=hdfs://mycluster/" /
   ```

   ```apache
   Connecting toonamenode via http://hn0-spark2.2oyzcdm4sfjuzjmj5dnmvscjpg.dx.internal.cloudapp.net:30070/fsck?ugi=hdiuser&path=%2F
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

   hello filesystem under path '/' is HEALTHY
   ```

3. <span data-ttu-id="ebda8-142">Om du anser att det inte finns några saknas, är skadad eller under-replikerade block eller att dessa block kan ignoreras Kör hello efter kommandot tootake hello namn noden ur felsäkert läge:</span><span class="sxs-lookup"><span data-stu-id="ebda8-142">If you determine that there are no missing, corrupt, or under-replicated blocks, or that those blocks can be ignored, run hello following command tootake hello name node out of safe mode:</span></span>

   ```apache
   hdfs dfsadmin -D "fs.default.name=hdfs://mycluster/" -safemode leave
   ```


## <a name="how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix"></a><span data-ttu-id="ebda8-143">Hur kan jag åtgärda JDBC eller SQLLine anslutningen problem med Apache Phoenix</span><span class="sxs-lookup"><span data-stu-id="ebda8-143">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-144">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-144">Resolution steps</span></span>

<span data-ttu-id="ebda8-145">Du måste ange hello IP-adressen för en aktiv nod ZooKeeper tooconnect med Phoenix.</span><span class="sxs-lookup"><span data-stu-id="ebda8-145">tooconnect with Phoenix, you must provide hello IP address of an active ZooKeeper node.</span></span> <span data-ttu-id="ebda8-146">Se till att hello ZooKeeper försöker tjänsten toowhich sqlline.py tooconnect är igång.</span><span class="sxs-lookup"><span data-stu-id="ebda8-146">Ensure that hello ZooKeeper service toowhich sqlline.py is trying tooconnect is up and running.</span></span>
1. <span data-ttu-id="ebda8-147">Logga in toohello HDInsight-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="ebda8-147">Sign in toohello HDInsight cluster by using SSH.</span></span>
2. <span data-ttu-id="ebda8-148">Ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ebda8-148">Enter hello following command:</span></span>
                
   ```apache
           "/usr/hdp/current/phoenix-client/bin/sqlline.py <IP of machine where Active Zookeeper is running"
   ```

   > [!Note] 
   > <span data-ttu-id="ebda8-149">Du kan hämta hello IP-adressen för hello aktiva ZooKeeper noden från hello Ambari UI.</span><span class="sxs-lookup"><span data-stu-id="ebda8-149">You can get hello IP address of hello active ZooKeeper node from hello Ambari UI.</span></span> <span data-ttu-id="ebda8-150">Gå för**HBase** > **snabblänkar** > **ZK\* (aktiv)** > **Zookeeper Info**.</span><span class="sxs-lookup"><span data-stu-id="ebda8-150">Go too**HBase** > **Quick Links** > **ZK\* (Active)** > **Zookeeper Info**.</span></span> 

3. <span data-ttu-id="ebda8-151">Om hello sqlline.py ansluter tooPhoenix och inte inte timeout, kommando hello kör följande toovalidate hello tillgänglighet och hälsotillståndet för Phoenix:</span><span class="sxs-lookup"><span data-stu-id="ebda8-151">If hello sqlline.py connects tooPhoenix and does not timeout, run hello following command toovalidate hello availability and health of Phoenix:</span></span>

   ```apache
           !tables
           !quit
   ```      
4. <span data-ttu-id="ebda8-152">Om det här kommandot fungerar finns det inga problem.</span><span class="sxs-lookup"><span data-stu-id="ebda8-152">If this command works, there is no issue.</span></span> <span data-ttu-id="ebda8-153">hello IP-adressen av hello användare kan vara felaktig.</span><span class="sxs-lookup"><span data-stu-id="ebda8-153">hello IP address provided by hello user might be incorrect.</span></span> <span data-ttu-id="ebda8-154">Om hello kommando pausar en längre tid och visar hello följande fel kan du dock fortsätta toostep 5.</span><span class="sxs-lookup"><span data-stu-id="ebda8-154">However, if hello command pauses for an extended time and then displays hello following error, continue toostep 5.</span></span>

   ```apache
           Error while connecting toosqlline.py (Hbase - phoenix) Setting property: [isolation, TRANSACTION_READ_COMMITTED] issuing: !connect jdbc:phoenix:10.2.0.7 none none org.apache.phoenix.jdbc.PhoenixDriver Connecting toojdbc:phoenix:10.2.0.7 SLF4J: Class path contains multiple SLF4J bindings. 
   ```

5. <span data-ttu-id="ebda8-155">Kör följande kommandon från hello huvudnod (hn0) toodiagnose hello villkoret hello Phoenix SYSTEM hello. KATALOGEN tabell:</span><span class="sxs-lookup"><span data-stu-id="ebda8-155">Run hello following commands from hello head node (hn0) toodiagnose hello condition of hello Phoenix SYSTEM.CATALOG table:</span></span>

   ```apache
            hbase shell
                
           count 'SYSTEM.CATALOG'
   ```

   <span data-ttu-id="ebda8-156">hello kommandot ska returnera ett felmeddelande liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="ebda8-156">hello command should return an error similar toohello following:</span></span> 

   ```apache
           ERROR: org.apache.hadoop.hbase.NotServingRegionException: Region SYSTEM.CATALOG,,1485464083256.c0568c94033870c517ed36c45da98129. is not online on 10.2.0.5,16020,1489466172189) 
   ```
6. <span data-ttu-id="ebda8-157">Hello Ambari UI, utföra hello följa steg toorestart hello HMaster service på alla ZooKeeper-noder:</span><span class="sxs-lookup"><span data-stu-id="ebda8-157">In hello Ambari UI, complete hello following steps toorestart hello HMaster service on all ZooKeeper nodes:</span></span>

    1. <span data-ttu-id="ebda8-158">I hello **sammanfattning** avsnitt av HBase, gå för**HBase** > **Active HBase Master**.</span><span class="sxs-lookup"><span data-stu-id="ebda8-158">In hello **Summary** section of HBase, go too**HBase** > **Active HBase Master**.</span></span> 
    2. <span data-ttu-id="ebda8-159">I hello **komponenter** och starta om hello HBase Master-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ebda8-159">In hello **Components** section, restart hello HBase Master service.</span></span>
    3. <span data-ttu-id="ebda8-160">Upprepa dessa steg för alla återstående **vänteläge HBase Master** tjänster.</span><span class="sxs-lookup"><span data-stu-id="ebda8-160">Repeat these steps for all remaining **Standby HBase Master** services.</span></span> 

<span data-ttu-id="ebda8-161">Det kan ta upp toofive minuter för hello HBase Master service toostabilize och hello återställningen är klar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-161">It can take up toofive minutes for hello HBase Master service toostabilize and finish hello recovery process.</span></span> <span data-ttu-id="ebda8-162">Upprepa hello sqlline.py kommandon tooconfirm som hello systemet efter några minuter. KATALOGEN tabell är igång och att den kan efterfrågas.</span><span class="sxs-lookup"><span data-stu-id="ebda8-162">After a few minutes, repeat hello sqlline.py commands tooconfirm that hello SYSTEM.CATALOG table is up, and that it can be queried.</span></span> 

<span data-ttu-id="ebda8-163">När hello SYSTEM. KATALOGEN tabellen är tillbaka toonormal, hello anslutningen problemet tooPhoenix bör lösas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ebda8-163">When hello SYSTEM.CATALOG table is back toonormal, hello connectivity issue tooPhoenix should be automatically resolved.</span></span>


## <a name="what-causes-a-master-server-toofail-toostart"></a><span data-ttu-id="ebda8-164">Vad som orsakar en huvudserver toofail toostart</span><span class="sxs-lookup"><span data-stu-id="ebda8-164">What causes a master server toofail toostart</span></span>

### <a name="error"></a><span data-ttu-id="ebda8-165">Fel</span><span class="sxs-lookup"><span data-stu-id="ebda8-165">Error</span></span> 

<span data-ttu-id="ebda8-166">En atomisk namnbyte felet inträffar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-166">An atomic renaming failure occurs.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="ebda8-167">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="ebda8-167">Detailed description</span></span>

<span data-ttu-id="ebda8-168">Under hello startprocessen slutförs HMaster många initieringssstegen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-168">During hello startup process, HMaster completes many initialization steps.</span></span> <span data-ttu-id="ebda8-169">Dessa inkluderar flytta data från grunden hello (tmp) toohello data-mappen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-169">These include moving data from hello scratch (.tmp) folder toohello data folder.</span></span> <span data-ttu-id="ebda8-170">HMaster kontrollerar också hello write-ahead-loggar (WALs) mappen toosee om det inte finns några servrar svarar region, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ebda8-170">HMaster also looks at hello write-ahead logs (WALs) folder toosee if there are any unresponsive region servers, and so on.</span></span> 

<span data-ttu-id="ebda8-171">Under start HMaster har en grundläggande `list` på dessa mappar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-171">During startup, HMaster does a basic `list` command on these folders.</span></span> <span data-ttu-id="ebda8-172">Om HMaster ser en oväntad fil i någon av dessa mappar när som helst, genererar ett undantag och startar inte.</span><span class="sxs-lookup"><span data-stu-id="ebda8-172">If at any time, HMaster sees an unexpected file in any of these folders, it throws an exception and doesn't start.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="ebda8-173">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="ebda8-173">Probable cause</span></span>

<span data-ttu-id="ebda8-174">Försök tooidentify hello tidslinjen hello filen skapas i hello region server-loggar och se om det fanns en process krasch runt hello tid hello filen skapades.</span><span class="sxs-lookup"><span data-stu-id="ebda8-174">In hello region server logs, try tooidentify hello timeline of hello file creation, and then see if there was a process crash around hello time hello file was created.</span></span> <span data-ttu-id="ebda8-175">(Kontakta HBase support tooassist du göra detta.) Det här hjälper oss tillhandahålla mer robusta mekanismer så att du kan undvika träffa programfelet och se till att rätt processen avstängningar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-175">(Contact HBase support tooassist you in doing this.) This helps us provide more robust mechanisms, so that you can avoid hitting this bug, and ensure graceful process shutdowns.</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-176">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-176">Resolution steps</span></span>

<span data-ttu-id="ebda8-177">Kontrollera hello anropsstacken och försök toodetermine vilken mapp den orsakar hello problem (till exempel kanske inte hello WALs mappen eller hello tmp).</span><span class="sxs-lookup"><span data-stu-id="ebda8-177">Check hello call stack and try toodetermine which folder might be causing hello problem (for instance, it might be hello WALs folder or hello .tmp folder).</span></span> <span data-ttu-id="ebda8-178">I Cloud Explorer eller med hjälp av HDFS-kommandon och försök sedan toolocate hello problemfilen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-178">Then, in Cloud Explorer or by using HDFS commands, try toolocate hello problem file.</span></span> <span data-ttu-id="ebda8-179">Detta är normalt en \*-renamePending.json-filen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-179">Usually, this is a \*-renamePending.json file.</span></span> <span data-ttu-id="ebda8-180">(hello \*-renamePending.json-filen är en journal som har använt tooimplement hello namnbyte åtgärden i hello WASB-drivrutinen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-180">(hello \*-renamePending.json file is a journal file that's used tooimplement hello atomic rename operation in hello WASB driver.</span></span> <span data-ttu-id="ebda8-181">Förfallodatum toobugs i den här implementeringen dessa filer kan lämnas efter egna processer som kraschat osv.) Force-ta bort den här filen i Cloud Explorer eller med hjälp av HDFS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="ebda8-181">Due toobugs in this implementation, these files can be left over after process crashes, and so on.) Force-delete this file either in Cloud Explorer or by using HDFS commands.</span></span> 

<span data-ttu-id="ebda8-182">Ibland kanske en temporär fil som heter ungefär *$$$. $$$* på den här platsen.</span><span class="sxs-lookup"><span data-stu-id="ebda8-182">Sometimes, there might also be a temporary file named something like *$$$.$$$* at this location.</span></span> <span data-ttu-id="ebda8-183">Du har toouse HDFS `ls` kommandot toosee den här filen, du kan inte se hello-filen i Cloud Explorer.</span><span class="sxs-lookup"><span data-stu-id="ebda8-183">You have toouse HDFS `ls` command toosee this file; you cannot see hello file in Cloud Explorer.</span></span> <span data-ttu-id="ebda8-184">toodelete den här filen, Använd hello HDFS kommandot `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span><span class="sxs-lookup"><span data-stu-id="ebda8-184">toodelete this file, use hello HDFS command `hdfs dfs -rm /\<path>\/\$\$\$.\$\$\$`.</span></span>  

<span data-ttu-id="ebda8-185">När du har kört dessa kommandon ska HMaster starta direkt.</span><span class="sxs-lookup"><span data-stu-id="ebda8-185">After you've run these commands, HMaster should start immediately.</span></span> 

### <a name="error"></a><span data-ttu-id="ebda8-186">Fel</span><span class="sxs-lookup"><span data-stu-id="ebda8-186">Error</span></span>

<span data-ttu-id="ebda8-187">Ingen serveradressen listas i *hbase: meta* för region xxx.</span><span class="sxs-lookup"><span data-stu-id="ebda8-187">No server address is listed in *hbase: meta* for region xxx.</span></span>

### <a name="detailed-description"></a><span data-ttu-id="ebda8-188">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="ebda8-188">Detailed description</span></span>

<span data-ttu-id="ebda8-189">Du kan se ett meddelande på ditt Linux-kluster som anger att hello *hbase: meta* tabellen är inte online.</span><span class="sxs-lookup"><span data-stu-id="ebda8-189">You might see a message on your Linux cluster that indicates that hello *hbase: meta* table is not online.</span></span> <span data-ttu-id="ebda8-190">Kör `hbck` kan rapportera som ”hbase: meta tabell replicaId 0 hittades inte på en region”.</span><span class="sxs-lookup"><span data-stu-id="ebda8-190">Running `hbck` might report that "hbase: meta table replicaId 0 is not found on any region."</span></span> <span data-ttu-id="ebda8-191">hello problemet kan vara att HMaster inte kunde initiera när du har startat om HBase.</span><span class="sxs-lookup"><span data-stu-id="ebda8-191">hello problem might be that HMaster could not initialize after you restarted HBase.</span></span> <span data-ttu-id="ebda8-192">Hej HMaster loggar du kan se hello-meddelande ”: ingen serveradress som anges i hbase: metadata för region hbase: säkerhetskopiering \<regionnamn\>”.</span><span class="sxs-lookup"><span data-stu-id="ebda8-192">In hello HMaster logs, you might see hello message: "No server address listed in hbase: meta for region hbase: backup \<region name\>".</span></span>  

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-193">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-193">Resolution steps</span></span>

1. <span data-ttu-id="ebda8-194">Ange hello följande kommandon (ändra faktiska värden i tillämpliga fall) i hello HBase-gränssnittet:</span><span class="sxs-lookup"><span data-stu-id="ebda8-194">In hello HBase shell, enter hello following commands (change actual values as applicable):</span></span>  

   ```apache
   > scan 'hbase:meta'  
   ```

   ```apache
   > delete 'hbase:meta','hbase:backup <region name>','<column name>'  
   ```

2. <span data-ttu-id="ebda8-195">Ta bort hello *hbase: namnområdet* post.</span><span class="sxs-lookup"><span data-stu-id="ebda8-195">Delete hello *hbase: namespace* entry.</span></span> <span data-ttu-id="ebda8-196">Den här posten kan vara hello samma fel som rapporteras när hello *hbase: namnområdet* tabell genomsöks.</span><span class="sxs-lookup"><span data-stu-id="ebda8-196">This entry might be hello same error that's being reported when hello *hbase: namespace* table is scanned.</span></span>

3. <span data-ttu-id="ebda8-197">toobring in HBase i ett körningstillstånd i hello Ambari UI, starta om hello Active HMaster.</span><span class="sxs-lookup"><span data-stu-id="ebda8-197">toobring up HBase in a running state, in hello Ambari UI, restart hello Active HMaster service.</span></span>  

4. <span data-ttu-id="ebda8-198">Kör följande kommando hello i hello HBase-gränssnittet, toobring alla offline tabeller:</span><span class="sxs-lookup"><span data-stu-id="ebda8-198">In hello HBase shell, toobring up all offline tables, run hello following command:</span></span>

   ```apache 
   hbase hbck -ignorePreCheckPermission -fixAssignments 
   ```

### <a name="additional-reading"></a><span data-ttu-id="ebda8-199">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="ebda8-199">Additional reading</span></span>

[<span data-ttu-id="ebda8-200">Det går inte tooprocess hello HBase-tabell</span><span class="sxs-lookup"><span data-stu-id="ebda8-200">Unable tooprocess hello HBase table</span></span>](http://stackoverflow.com/questions/4794092/unable-to-access-hbase-table)


### <a name="error"></a><span data-ttu-id="ebda8-201">Fel</span><span class="sxs-lookup"><span data-stu-id="ebda8-201">Error</span></span>

<span data-ttu-id="ebda8-202">HMaster tidsgränsen uppnås med ett allvarligt undantag liknande too"java.io.IOException: för lång tid 300000ms väntar på att namnområdet tabell toobe tilldelade”.</span><span class="sxs-lookup"><span data-stu-id="ebda8-202">HMaster times out with a fatal exception similar too"java.io.IOException: Timedout 300000ms waiting for namespace table toobe assigned."</span></span>

### <a name="detailed-description"></a><span data-ttu-id="ebda8-203">Detaljerad beskrivning</span><span class="sxs-lookup"><span data-stu-id="ebda8-203">Detailed description</span></span>

<span data-ttu-id="ebda8-204">Det här problemet kan uppstå om du har många tabeller och regioner som inte har tömts när du startar om HMaster-tjänster.</span><span class="sxs-lookup"><span data-stu-id="ebda8-204">You might experience this issue if you have many tables and regions that have not been flushed when you restart your HMaster services.</span></span> <span data-ttu-id="ebda8-205">Starta om misslyckas och hello föregående felmeddelande visas.</span><span class="sxs-lookup"><span data-stu-id="ebda8-205">Restart might fail, and you'll see hello preceding error message.</span></span>  

### <a name="probable-cause"></a><span data-ttu-id="ebda8-206">Möjlig orsak</span><span class="sxs-lookup"><span data-stu-id="ebda8-206">Probable cause</span></span>

<span data-ttu-id="ebda8-207">Detta är ett känt problem med hello HMaster service.</span><span class="sxs-lookup"><span data-stu-id="ebda8-207">This is a known issue with hello HMaster service.</span></span> <span data-ttu-id="ebda8-208">Allmän klustret startades uppgifter kan ta lång tid.</span><span class="sxs-lookup"><span data-stu-id="ebda8-208">General cluster startup tasks can take a long time.</span></span> <span data-ttu-id="ebda8-209">HMaster stängs av eftersom hello namnområde tabell ännu inte tilldelats.</span><span class="sxs-lookup"><span data-stu-id="ebda8-209">HMaster shuts down because hello namespace table isn’t yet assigned.</span></span> <span data-ttu-id="ebda8-210">Detta inträffar endast i situationer där stora mängden unflushed data finns och en tidsgräns på fem minuter är inte tillräcklig.</span><span class="sxs-lookup"><span data-stu-id="ebda8-210">This occurs only in scenarios in which large amount of unflushed data exists, and a timeout of five minutes is not sufficient.</span></span>
  
### <a name="resolution-steps"></a><span data-ttu-id="ebda8-211">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-211">Resolution steps</span></span>

1. <span data-ttu-id="ebda8-212">Hello Ambari UI, gå för**HBase** > **konfigurationerna**.</span><span class="sxs-lookup"><span data-stu-id="ebda8-212">In hello Ambari UI, go too**HBase** > **Configs**.</span></span> <span data-ttu-id="ebda8-213">Lägg till följande inställning hello i hello anpassade hbase-site.xml filen:</span><span class="sxs-lookup"><span data-stu-id="ebda8-213">In hello custom hbase-site.xml file, add hello following setting:</span></span> 

   ```apache
   Key: hbase.master.namespace.init.timeout Value: 2400000  
   ```

2. <span data-ttu-id="ebda8-214">Starta om tjänsterna för hello krävs (HMaster och eventuellt andra HBase-tjänster).</span><span class="sxs-lookup"><span data-stu-id="ebda8-214">Restart hello required services (HMaster, and possibly other HBase services).</span></span>  


## <a name="what-causes-a-restart-failure-on-a-region-server"></a><span data-ttu-id="ebda8-215">Vad som orsakar en omstart av fel på en region-server</span><span class="sxs-lookup"><span data-stu-id="ebda8-215">What causes a restart failure on a region server</span></span>

### <a name="issue"></a><span data-ttu-id="ebda8-216">Problem</span><span class="sxs-lookup"><span data-stu-id="ebda8-216">Issue</span></span>

<span data-ttu-id="ebda8-217">Starta om fel inträffar på en region-server kan förhindras genom följande bästa praxis.</span><span class="sxs-lookup"><span data-stu-id="ebda8-217">A restart failure on a region server might be prevented by following best practices.</span></span> <span data-ttu-id="ebda8-218">Vi rekommenderar att du pausar arbetsbelastning aktivitet när du planerar toorestart HBase region servrar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-218">We recommend that you pause heavy workload activity when you are planning toorestart HBase region servers.</span></span> <span data-ttu-id="ebda8-219">Om ett program kvarstår tooconnect med region servrar när shutdown pågår blir hello region server omstarten långsammare med flera minuter.</span><span class="sxs-lookup"><span data-stu-id="ebda8-219">If an application continues tooconnect with region servers when shutdown is in progress, hello region server restart operation will be slower by several minutes.</span></span> <span data-ttu-id="ebda8-220">Det är också en bra idé toofirst tömning alla hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="ebda8-220">Also, it's a good idea toofirst flush all hello tables.</span></span> <span data-ttu-id="ebda8-221">En referens för hur tooflush tabeller, se [HDInsight HBase: hur tooimprove hello HBase-kluster omstart av lokaliseraren tabeller](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span><span class="sxs-lookup"><span data-stu-id="ebda8-221">For a reference on how tooflush tables, see [HDInsight HBase: How tooimprove hello HBase cluster restart time by flushing tables](https://blogs.msdn.microsoft.com/azuredatalake/2016/09/19/hdinsight-hbase-how-to-improve-hbase-cluster-restart-time-by-flushing-tables/).</span></span>

<span data-ttu-id="ebda8-222">Om du har initierat hello omstarten på HBase region servrar från hello Ambari UI kan se du direkt hello region servrar fungerar korrekt, men de inte startas om direkt.</span><span class="sxs-lookup"><span data-stu-id="ebda8-222">If you initiate hello restart operation on HBase region servers from hello Ambari UI, you immediately see that hello region servers went down, but they don't restart right away.</span></span> 

<span data-ttu-id="ebda8-223">Här är vad som händer bakgrunden hello:</span><span class="sxs-lookup"><span data-stu-id="ebda8-223">Here's what's happening behind hello scenes:</span></span> 

1. <span data-ttu-id="ebda8-224">Hej Ambari agenten skickar en stoppa begäran toohello region server.</span><span class="sxs-lookup"><span data-stu-id="ebda8-224">hello Ambari agent sends a stop request toohello region server.</span></span>
2. <span data-ttu-id="ebda8-225">Hej Ambari-agent väntar på 30 sekunder för hello region server tooshut ned avslutas.</span><span class="sxs-lookup"><span data-stu-id="ebda8-225">hello Ambari agent waits for 30 seconds for hello region server tooshut down gracefully.</span></span> 
3. <span data-ttu-id="ebda8-226">Om ditt program kvarstår tooconnect med hello region server stängdes inte hello servern av omedelbart.</span><span class="sxs-lookup"><span data-stu-id="ebda8-226">If your application continues tooconnect with hello region server, hello server won't shut down immediately.</span></span> <span data-ttu-id="ebda8-227">hello 30 sekunder timeout uppnås innan avstängning inträffar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-227">hello 30-second timeout expires before shutdown occurs.</span></span> 
4. <span data-ttu-id="ebda8-228">Efter 30 sekunder hello Ambari agenten skickar en tvingad kill (`kill -9`) kommandot toohello region server.</span><span class="sxs-lookup"><span data-stu-id="ebda8-228">After 30 seconds, hello Ambari agent sends a force-kill (`kill -9`) command toohello region server.</span></span> <span data-ttu-id="ebda8-229">Du kan se det i loggen för hello ambari-agent (i hello /var/log/directory hello respektive arbetsnoden):</span><span class="sxs-lookup"><span data-stu-id="ebda8-229">You can see this in hello ambari-agent log (in hello /var/log/ directory of hello respective worker node):</span></span>

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
<span data-ttu-id="ebda8-230">På grund av hello abrupt avstängning, kan hello-port som är kopplad till hello process inte släppas, även om hello region serverprocessen har stoppats.</span><span class="sxs-lookup"><span data-stu-id="ebda8-230">Because of hello abrupt shutdown, hello port associated with hello process might not be released, even though hello region server process is stopped.</span></span> <span data-ttu-id="ebda8-231">Den här situationen kan leda tooan AddressBindException när hello region servern startas enligt hello följande loggar.</span><span class="sxs-lookup"><span data-stu-id="ebda8-231">This situation can lead tooan AddressBindException when hello region server is starting, as shown in hello following logs.</span></span> <span data-ttu-id="ebda8-232">Du kan kontrollera detta i hello region-server.log i hello /var/log/hbase katalog på hello arbetarnoder där hello region servern misslyckas toostart.</span><span class="sxs-lookup"><span data-stu-id="ebda8-232">You can verify this in hello region-server.log in hello /var/log/hbase directory on hello worker nodes where hello region server fails toostart.</span></span> 

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
        
   Caused by: java.net.BindException: Problem binding too/10.2.0.4:16020 : Address already in use
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

### <a name="resolution-steps"></a><span data-ttu-id="ebda8-233">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="ebda8-233">Resolution steps</span></span>

1. <span data-ttu-id="ebda8-234">Försök tooreduce hello belastningen på hello HBase region servrar innan du startar en omstart.</span><span class="sxs-lookup"><span data-stu-id="ebda8-234">Try tooreduce hello load on hello HBase region servers before you initiate a restart.</span></span> 
2. <span data-ttu-id="ebda8-235">Du kan också kommandon (om steg 1 inte hjälper), försök toomanually omstart region servrar på hello arbetarnoder med hjälp av hello följande:</span><span class="sxs-lookup"><span data-stu-id="ebda8-235">Alternatively (if step 1 doesn't help), try toomanually restart region servers on hello worker nodes by using hello following commands:</span></span>

   ```apache
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh stop regionserver"
   sudo su - hbase -c "/usr/hdp/current/hbase-regionserver/bin/hbase-daemon.sh start regionserver"   
   ```

