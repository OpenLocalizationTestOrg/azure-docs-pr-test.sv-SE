---
title: "aaaFix registreringsdata slut på minne i Azure HDInsight | Microsoft Docs"
description: "Åtgärda registreringsdata slut på minne i HDInsight. hello kunden scenariot är en fråga för många stora tabeller."
keywords: "slut på minne fel, OOM, Hive inställningar"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 7bce3dff-9825-4fa0-a568-c52a9f7d1dad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/17/2017
ms.author: jgao
ms.openlocfilehash: 00a12969322c1e74434ba6593ffd098f342edd84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a><span data-ttu-id="1a6ac-105">Åtgärda registreringsdata slut på minne i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="1a6ac-105">Fix a Hive out of memory error in Azure HDInsight</span></span>

<span data-ttu-id="1a6ac-106">Lär dig hur toofix registreringsdata slut på minne när bearbeta stora tabeller genom att konfigurera inställningar för Hive-minne.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-106">Learn how toofix a Hive out of memory error when process large tables by configuring Hive memory settings.</span></span>

## <a name="run-hive-query-against-large-tables"></a><span data-ttu-id="1a6ac-107">Köra Hive-fråga mot stora tabeller</span><span class="sxs-lookup"><span data-stu-id="1a6ac-107">Run Hive query against large tables</span></span>

<span data-ttu-id="1a6ac-108">En kund körde en Hive-fråga:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-108">A customer ran a Hive query:</span></span>

    SELECT
        COUNT (T1.COLUMN1) as DisplayColumn1,
        …
        …
        ….
    FROM
        TABLE1 T1,
        TABLE2 T2,
        TABLE3 T3,
        TABLE5 T4,
        TABLE6 T5,
        TABLE7 T6
    where (T1.KEY1 = T2.KEY1….
        …
        …

<span data-ttu-id="1a6ac-109">Vissa olika delarna i den här frågan:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-109">Some nuances of this query:</span></span>

* <span data-ttu-id="1a6ac-110">T1 är ett alias tooa stor, tabell1 som har många typer av STRÄNGEN.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-110">T1 is an alias tooa big table, TABLE1, which has lots of STRING column types.</span></span>
* <span data-ttu-id="1a6ac-111">Andra tabeller är inte som stora men har många kolumner.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-111">Other tables are not that big but do have many columns.</span></span>
* <span data-ttu-id="1a6ac-112">Alla tabeller som ansluter till varandra, i vissa fall med flera kolumner i tabell 1 och andra.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-112">All tables are joining each other, in some cases with multiple columns in TABLE1 and others.</span></span>

<span data-ttu-id="1a6ac-113">hello Hive-frågan tog 26 minuter toofinish på ett 24 nod A3 HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-113">hello Hive query took 26 minutes toofinish on a 24 node A3 HDInsight cluster.</span></span> <span data-ttu-id="1a6ac-114">hello kunden upptäckt hello följande varningar:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-114">hello customer noticed hello following warning messages:</span></span>

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

<span data-ttu-id="1a6ac-115">Med hjälp av hello Tez-Körningsmotor.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-115">By using hello Tez execution engine.</span></span> <span data-ttu-id="1a6ac-116">hello samma fråga kördes i 15 minuter och utlöste hello följande fel:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-116">hello same query ran for 15 minutes, and then threw hello following error:</span></span>

    Status: Failed
    Vertex failed, vertexName=Map 5, vertexId=vertex_1443634917922_0008_1_05, diagnostics=[Task failed, taskId=task_1443634917922_0008_1_05_000006, diagnostics=[TaskAttempt 0 failed, info=[Error: Failure while running task:java.lang.RuntimeException: java.lang.OutOfMemoryError: Java heap space
        at
    org.apache.hadoop.hive.ql.exec.tez.TezProcessor.initializeAndRunProcessor(TezProcessor.java:172)
        at org.apache.hadoop.hive.ql.exec.tez.TezProcessor.run(TezProcessor.java:138)
        at
    org.apache.tez.runtime.LogicalIOProcessorRuntimeTask.run(LogicalIOProcessorRuntimeTask.java:324)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:176)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable$1.run(TezTaskRunner.java:168)
        at java.security.AccessController.doPrivileged(Native Method)
        at javax.security.auth.Subject.doAs(Subject.java:415)
        at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1628)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:168)
        at
    org.apache.tez.runtime.task.TezTaskRunner$TaskRunnerCallable.call(TezTaskRunner.java:163)
        at java.util.concurrent.FutureTask.run(FutureTask.java:262)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
    Caused by: java.lang.OutOfMemoryError: Java heap space

<span data-ttu-id="1a6ac-117">hello fel förblir när du använder en större virtuell dator (till exempel D12).</span><span class="sxs-lookup"><span data-stu-id="1a6ac-117">hello error remains when using a bigger virtual machine (for example, D12).</span></span>


## <a name="debug-hello-out-of-memory-error"></a><span data-ttu-id="1a6ac-118">Felsöka hello slut på minne</span><span class="sxs-lookup"><span data-stu-id="1a6ac-118">Debug hello out of memory error</span></span>

<span data-ttu-id="1a6ac-119">Vår support och teknikerna snappa tillsammans finns en hello problem som orsakar hello utanför minnesfel har en [kända problem som beskrivs i hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span><span class="sxs-lookup"><span data-stu-id="1a6ac-119">Our support and engineering teams together found one of hello issues causing hello out of memory error was a [known issue described in hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):</span></span>

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

<span data-ttu-id="1a6ac-120">Hej **hive.auto.convert.join.noconditionaltask** i hello hive-site.xml filen har ställts in för**SANT**:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-120">hello **hive.auto.convert.join.noconditionaltask** in hello hive-site.xml file was set too**true**:</span></span>

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

<span data-ttu-id="1a6ac-121">Det är troligt kartan koppling har hello orsaken till hello Java Heap utrymme våra av minnesfel.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-121">It is likely map join was hello cause of hello Java Heap Space our of memory error.</span></span> <span data-ttu-id="1a6ac-122">Enligt beskrivningen i hello blogginlägget [Hadoop Yarn minnesinställningarna i HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)när Tez-Körningsmotor är används hello heap utrymmet faktiskt tillhör toohello Tez behållare.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-122">As explained in hello blog post [Hadoop Yarn memory settings in HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx), when Tez execution engine is used hello heap space used actually belongs toohello Tez container.</span></span> <span data-ttu-id="1a6ac-123">Se hello följande bild som beskriver hello Tez behållare minne.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-123">See hello following image describing hello Tez container memory.</span></span>

![Tez behållaren minne diagram: Hive slut på minne](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

<span data-ttu-id="1a6ac-125">Hello blogginlägget föreslår hello följande två minnesinställningarna definiera hello behållaren minne för hello heap: **hive.tez.container.size** och **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-125">As hello blog post suggests, hello following two memory settings define hello container memory for hello heap: **hive.tez.container.size** and **hive.tez.java.opts**.</span></span> <span data-ttu-id="1a6ac-126">Av erfarenhet innebär inte hello slut på minne undantag hello behållare är för litet.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-126">From our experience, hello out of memory exception does not mean hello container size is too small.</span></span> <span data-ttu-id="1a6ac-127">Det innebär att hello Java heap-storlek (hive.tez.java.opts) är för liten.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-127">It means hello Java heap size (hive.tez.java.opts) is too small.</span></span> <span data-ttu-id="1a6ac-128">Så när du ser slut på minne kan du försöka tooincrease **hive.tez.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-128">So whenever you see out of memory, you can try tooincrease **hive.tez.java.opts**.</span></span> <span data-ttu-id="1a6ac-129">Om det behövs kan du ha tooincrease **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-129">If needed you might have tooincrease **hive.tez.container.size**.</span></span> <span data-ttu-id="1a6ac-130">Hej **java.opts** inställningen ska vara runt 80% av **container.size**.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-130">hello **java.opts** setting should be around 80% of **container.size**.</span></span>

> [!NOTE]
> <span data-ttu-id="1a6ac-131">hello inställningen **hive.tez.java.opts** måste alltid vara mindre än **hive.tez.container.size**.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-131">hello setting **hive.tez.java.opts** must always be smaller than **hive.tez.container.size**.</span></span>
> 
> 

<span data-ttu-id="1a6ac-132">Eftersom en D12 dator har 28GB minne, vi valt toouse en behållare storlek på 10GB (10240MB) och tilldela 80% toojava.opts:</span><span class="sxs-lookup"><span data-stu-id="1a6ac-132">Because a D12 machine has 28GB memory, we decided toouse a container size of 10GB (10240MB) and assign 80% toojava.opts:</span></span>

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

<span data-ttu-id="1a6ac-133">Med nya inställningar för hello körde hello frågan under 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-133">With hello new settings, hello query successfully ran in under 10 minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a6ac-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a6ac-134">Next steps</span></span>

<span data-ttu-id="1a6ac-135">Hämta ett OOM fel innebär inte nödvändigtvis hello behållare är för litet.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-135">Getting an OOM error doesn't necessarily mean hello container size is too small.</span></span> <span data-ttu-id="1a6ac-136">I stället bör du konfigurera hello minnesinställningarna så att hello heap-storlek ökas och minst 80% av hello minnesstorlek för behållaren.</span><span class="sxs-lookup"><span data-stu-id="1a6ac-136">Instead, you should configure hello memory settings so that hello heap size is increased and is at least 80% of hello container memory size.</span></span> <span data-ttu-id="1a6ac-137">För att optimera Hive-frågor finns [optimera Hive-frågor för Hadoop i HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span><span class="sxs-lookup"><span data-stu-id="1a6ac-137">For optimizing Hive queries, see [Optimize Hive queries for Hadoop in HDInsight](hdinsight-hadoop-optimize-hive-query.md).</span></span>
