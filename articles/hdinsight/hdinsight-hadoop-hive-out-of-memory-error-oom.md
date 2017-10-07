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
# <a name="fix-a-hive-out-of-memory-error-in-azure-hdinsight"></a>Åtgärda registreringsdata slut på minne i Azure HDInsight

Lär dig hur toofix registreringsdata slut på minne när bearbeta stora tabeller genom att konfigurera inställningar för Hive-minne.

## <a name="run-hive-query-against-large-tables"></a>Köra Hive-fråga mot stora tabeller

En kund körde en Hive-fråga:

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

Vissa olika delarna i den här frågan:

* T1 är ett alias tooa stor, tabell1 som har många typer av STRÄNGEN.
* Andra tabeller är inte som stora men har många kolumner.
* Alla tabeller som ansluter till varandra, i vissa fall med flera kolumner i tabell 1 och andra.

hello Hive-frågan tog 26 minuter toofinish på ett 24 nod A3 HDInsight-kluster. hello kunden upptäckt hello följande varningar:

    Warning: Map Join MAPJOIN[428][bigTable=?] in task 'Stage-21:MAPRED' is a cross product
    Warning: Shuffle Join JOIN[8][tables = [t1933775, t1932766]] in Stage 'Stage-4:MAPRED' is a cross product

Med hjälp av hello Tez-Körningsmotor. hello samma fråga kördes i 15 minuter och utlöste hello följande fel:

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

hello fel förblir när du använder en större virtuell dator (till exempel D12).


## <a name="debug-hello-out-of-memory-error"></a>Felsöka hello slut på minne

Vår support och teknikerna snappa tillsammans finns en hello problem som orsakar hello utanför minnesfel har en [kända problem som beskrivs i hello Apache JIRA](https://issues.apache.org/jira/browse/HIVE-8306):

    When hive.auto.convert.join.noconditionaltask = true we check noconditionaltask.size and if hello sum  of tables sizes in hello map join is less than noconditionaltask.size hello plan would generate a Map join, hello issue with this is that hello calculation doesnt take into account hello overhead introduced by different HashTable implementation as results if hello sum of input sizes is smaller than hello noconditionaltask size by a small margin queries will hit OOM.

Hej **hive.auto.convert.join.noconditionaltask** i hello hive-site.xml filen har ställts in för**SANT**:

    <property>
        <name>hive.auto.convert.join.noconditionaltask</name>
        <value>true</value>
        <description>
              Whether Hive enables hello optimization about converting common join into mapjoin based on hello input file size.
              If this parameter is on, and hello sum of size for n-1 of hello tables/partitions for a n-way join is smaller than the
              specified size, hello join is directly converted tooa mapjoin (there is no conditional task).
        </description>
      </property>

Det är troligt kartan koppling har hello orsaken till hello Java Heap utrymme våra av minnesfel. Enligt beskrivningen i hello blogginlägget [Hadoop Yarn minnesinställningarna i HDInsight](http://blogs.msdn.com/b/shanyu/archive/2014/07/31/hadoop-yarn-memory-settings-in-hdinsigh.aspx)när Tez-Körningsmotor är används hello heap utrymmet faktiskt tillhör toohello Tez behållare. Se hello följande bild som beskriver hello Tez behållare minne.

![Tez behållaren minne diagram: Hive slut på minne](./media/hdinsight-hadoop-hive-out-of-memory-error-oom/hive-out-of-memory-error-oom-tez-container-memory.png)

Hello blogginlägget föreslår hello följande två minnesinställningarna definiera hello behållaren minne för hello heap: **hive.tez.container.size** och **hive.tez.java.opts**. Av erfarenhet innebär inte hello slut på minne undantag hello behållare är för litet. Det innebär att hello Java heap-storlek (hive.tez.java.opts) är för liten. Så när du ser slut på minne kan du försöka tooincrease **hive.tez.java.opts**. Om det behövs kan du ha tooincrease **hive.tez.container.size**. Hej **java.opts** inställningen ska vara runt 80% av **container.size**.

> [!NOTE]
> hello inställningen **hive.tez.java.opts** måste alltid vara mindre än **hive.tez.container.size**.
> 
> 

Eftersom en D12 dator har 28GB minne, vi valt toouse en behållare storlek på 10GB (10240MB) och tilldela 80% toojava.opts:

    SET hive.tez.container.size=10240
    SET hive.tez.java.opts=-Xmx8192m

Med nya inställningar för hello körde hello frågan under 10 minuter.

## <a name="next-steps"></a>Nästa steg

Hämta ett OOM fel innebär inte nödvändigtvis hello behållare är för litet. I stället bör du konfigurera hello minnesinställningarna så att hello heap-storlek ökas och minst 80% av hello minnesstorlek för behållaren. För att optimera Hive-frågor finns [optimera Hive-frågor för Hadoop i HDInsight](hdinsight-hadoop-optimize-hive-query.md).
