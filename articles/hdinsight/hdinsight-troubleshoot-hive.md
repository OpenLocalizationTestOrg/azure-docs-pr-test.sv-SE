---
title: "aaaTroubleshoot Hive med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du arbetar med Apache Hive och Azure HDInsight."
keywords: "Azure HDInsight Hive, vanliga frågor och svar, felsökningsguide för vanliga frågor"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: ac459316e658d0b29eb66f5685f0bc7e693bb277
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a><span data-ttu-id="7aa5d-104">Felsöka Hive med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7aa5d-104">Troubleshoot Hive by using Azure HDInsight</span></span>

<span data-ttu-id="7aa5d-105">Läs mer om hello översta frågor och sina lösningar när du arbetar med Apache Hive nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-105">Learn about hello top questions and their resolutions when working with Apache Hive payloads in Apache Ambari.</span></span>


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a><span data-ttu-id="7aa5d-106">Hur gör exportera en Hive metastore och importera det till ett annat kluster</span><span class="sxs-lookup"><span data-stu-id="7aa5d-106">How do I export a Hive metastore and import it on another cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="7aa5d-107">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="7aa5d-107">Resolution steps</span></span>

1. <span data-ttu-id="7aa5d-108">Ansluta toohello HDInsight-kluster med hjälp av en klient med SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="7aa5d-108">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="7aa5d-109">Mer information finns i [ytterligare resurser](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7aa5d-109">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="7aa5d-110">Kör följande kommando på hello HDInsight-kluster som du vill tooexport hello metastore hello:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-110">Run hello following command on hello HDInsight cluster from which you want tooexport hello metastore:</span></span>

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  <span data-ttu-id="7aa5d-111">Det här kommandot genererar en fil med namnet allatables.sql.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-111">This command generates a file named allatables.sql.</span></span>

3. <span data-ttu-id="7aa5d-112">Kopiera hello filen alltables.sql toohello nya HDInsight-kluster och kör sedan följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-112">Copy hello file alltables.sql toohello new HDInsight cluster, and then run hello following command:</span></span>

  ```apache
  hive -f alltables.sql
  ```

<span data-ttu-id="7aa5d-113">hello koden i hello Lösningssteg förutsätter att data sökvägar på hello nytt kluster hello samma som hello datasökvägar på hello gamla klustret.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-113">hello code in hello resolution steps assumes that data paths on hello new cluster are hello same as hello data paths on hello old cluster.</span></span> <span data-ttu-id="7aa5d-114">Om hello datasökvägar är olika, kan du manuellt redigera hello genereras alltables.sql filen tooreflect ändringar.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-114">If hello data paths are different, you can manually edit hello generated alltables.sql file tooreflect any changes.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="7aa5d-115">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7aa5d-115">Additional reading</span></span>

- [<span data-ttu-id="7aa5d-116">Ansluta tooan HDInsight-kluster med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="7aa5d-116">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a><span data-ttu-id="7aa5d-117">Hur jag för att hitta Hive loggar på ett kluster</span><span class="sxs-lookup"><span data-stu-id="7aa5d-117">How do I locate Hive logs on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="7aa5d-118">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="7aa5d-118">Resolution steps</span></span>

1. <span data-ttu-id="7aa5d-119">Ansluta toohello HDInsight-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-119">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7aa5d-120">Mer information finns i **ytterligare resurser**.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-120">For more information, see **Additional reading**.</span></span>

2. <span data-ttu-id="7aa5d-121">Klientloggfiler för tooview Hive, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-121">tooview Hive client logs, use hello following command:</span></span>

  ```apache
  /tmp/<username>/hive.log 
  ```

3. <span data-ttu-id="7aa5d-122">tooview loggar för Hive metastore, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-122">tooview Hive metastore logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. <span data-ttu-id="7aa5d-123">tooview Hiveserver loggar, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-123">tooview Hiveserver logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a><span data-ttu-id="7aa5d-124">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7aa5d-124">Additional reading</span></span>

- [<span data-ttu-id="7aa5d-125">Ansluta tooan HDInsight-kluster med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="7aa5d-125">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a><span data-ttu-id="7aa5d-126">Hur jag för att starta hello Hive-gränssnittet med specifika konfigurationer i ett kluster</span><span class="sxs-lookup"><span data-stu-id="7aa5d-126">How do I launch hello Hive shell with specific configurations on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="7aa5d-127">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="7aa5d-127">Resolution steps</span></span>

1. <span data-ttu-id="7aa5d-128">Ange ett nyckel / värde-par för konfiguration när du startar hello Hive-gränssnittet.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-128">Specify a configuration key-value pair when you start hello Hive shell.</span></span> <span data-ttu-id="7aa5d-129">Mer information finns i [ytterligare resurser](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7aa5d-129">For more information, see [Additional reading](#additional-reading-end).</span></span>

  ```apache
  hive -hiveconf a=b 
  ```

2. <span data-ttu-id="7aa5d-130">toolist alla effektiva konfigurationer på Hive-gränssnittet, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-130">toolist all effective configurations on Hive shell, use hello following command:</span></span>

  ```apache
  hive> set;
  ```

  <span data-ttu-id="7aa5d-131">Till exempel använda hello följande toostart Hive-kommandogränssnittet utan felsökningsloggning aktiverats på hello-konsolen:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-131">For example, use hello following command toostart Hive shell with debug logging enabled on hello console:</span></span>

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a><span data-ttu-id="7aa5d-132">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7aa5d-132">Additional reading</span></span>

- [<span data-ttu-id="7aa5d-133">Egenskaper för hive-konfiguration</span><span class="sxs-lookup"><span data-stu-id="7aa5d-133">Hive configuration properties</span></span>](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <span data-ttu-id="7aa5d-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hur jag för att analysera Tez DAG data på ett kluster kritiska</span><span class="sxs-lookup"><span data-stu-id="7aa5d-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>How do I analyze Tez DAG data on a cluster-critical path</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="7aa5d-135">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="7aa5d-135">Resolution steps</span></span>
 
1. <span data-ttu-id="7aa5d-136">tooanalyze en Apache Tez dirigeras acykliskt diagram (DAG) i ett kluster-kritiskt diagram, Anslut toohello HDInsight-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-136">tooanalyze an Apache Tez directed acyclic graph (DAG) on a cluster-critical graph, connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7aa5d-137">Mer information finns i [ytterligare resurser](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="7aa5d-137">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="7aa5d-138">Kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-138">At a command prompt, run hello following command:</span></span>
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. <span data-ttu-id="7aa5d-139">toolist andra analyzers som kan använda tooanalyze Tez DAG använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-139">toolist other analyzers that can be used tooanalyze Tez DAG, use hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  <span data-ttu-id="7aa5d-140">Du måste ange ett exempelprogram som hello första argument.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-140">You must provide an example program as hello first argument.</span></span>

  <span data-ttu-id="7aa5d-141">Giltigt programnamn inkluderar:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-141">Valid program names include:</span></span>
    - <span data-ttu-id="7aa5d-142">**ContainerReuseAnalyzer**: Skriv ut behållaren återanvändning i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-142">**ContainerReuseAnalyzer**: Print container reuse details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-143">**CriticalPath**: hitta hello kritiska i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-143">**CriticalPath**: Find hello critical path of a DAG</span></span>
    - <span data-ttu-id="7aa5d-144">**LocalityAnalyzer**: Skriv ut ort i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-144">**LocalityAnalyzer**: Print locality details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-145">**ShuffleTimeAnalyzer**: analysera hello blanda tidsinformation i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-145">**ShuffleTimeAnalyzer**: Analyze hello shuffle time details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-146">**SkewAnalyzer**: analysera hello skeva information i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-146">**SkewAnalyzer**: Analyze hello skew details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-147">**SlowNodeAnalyzer**: skriva noden information i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-147">**SlowNodeAnalyzer**: Print node details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-148">**SlowTaskIdentifier**: Skriv ut långsam aktivitetsinformation i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-148">**SlowTaskIdentifier**: Print slow task details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-149">**SlowestVertexAnalyzer**: Skriv ut långsammaste hörn i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-149">**SlowestVertexAnalyzer**: Print slowest vertex details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-150">**SpillAnalyzer**: Skriv ut oljesanering detaljer i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-150">**SpillAnalyzer**: Print spill details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-151">**TaskConcurrencyAnalyzer**: Skriv ut hello samtidighet aktivitetsinformation i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-151">**TaskConcurrencyAnalyzer**: Print hello task concurrency details in a DAG</span></span>
    - <span data-ttu-id="7aa5d-152">**VertexLevelCriticalPathAnalyzer**: hitta hello kritiska nivån hörn i DAG</span><span class="sxs-lookup"><span data-stu-id="7aa5d-152">**VertexLevelCriticalPathAnalyzer**: Find hello critical path at vertex level in a DAG</span></span>


### <a name="additional-reading"></a><span data-ttu-id="7aa5d-153">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7aa5d-153">Additional reading</span></span>

- [<span data-ttu-id="7aa5d-154">Ansluta tooan HDInsight-kluster med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="7aa5d-154">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a><span data-ttu-id="7aa5d-155">Hur hämta Tez DAG data från ett kluster</span><span class="sxs-lookup"><span data-stu-id="7aa5d-155">How do I download Tez DAG data from a cluster</span></span>


#### <a name="resolution-steps"></a><span data-ttu-id="7aa5d-156">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="7aa5d-156">Resolution steps</span></span>

<span data-ttu-id="7aa5d-157">Det finns två sätt toocollect hello Tez DAG data:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-157">There are two ways toocollect hello Tez DAG data:</span></span>

- <span data-ttu-id="7aa5d-158">Hello kommandoraden:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-158">From hello command line:</span></span>
 
    <span data-ttu-id="7aa5d-159">Ansluta toohello HDInsight-kluster med hjälp av SSH.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-159">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="7aa5d-160">Kör följande kommando hello Kommandotolken hello:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-160">At hello command prompt, run hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- <span data-ttu-id="7aa5d-161">Använd hello Ambari Tez vy:</span><span class="sxs-lookup"><span data-stu-id="7aa5d-161">Use hello Ambari Tez view:</span></span>
   
  1. <span data-ttu-id="7aa5d-162">Gå tooAmbari.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-162">Go tooAmbari.</span></span> 
  2. <span data-ttu-id="7aa5d-163">Gå tooTez vy (under hello paneler ikon i hello längst upp till höger).</span><span class="sxs-lookup"><span data-stu-id="7aa5d-163">Go tooTez view (under hello tiles icon in hello upper-right corner).</span></span> 
  3. <span data-ttu-id="7aa5d-164">Välj hello DAG som du vill tooview.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-164">Select hello DAG you want tooview.</span></span>
  4. <span data-ttu-id="7aa5d-165">Välj **ladda ned data**.</span><span class="sxs-lookup"><span data-stu-id="7aa5d-165">Select **Download data**.</span></span>

### <span data-ttu-id="7aa5d-166"><a name="additional-reading-end"></a>Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="7aa5d-166"><a name="additional-reading-end"></a>Additional reading</span></span>

[<span data-ttu-id="7aa5d-167">Ansluta tooan HDInsight-kluster med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="7aa5d-167">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)






