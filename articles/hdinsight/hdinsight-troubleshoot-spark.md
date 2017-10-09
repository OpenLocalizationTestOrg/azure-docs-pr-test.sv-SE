---
title: "aaaTroubleshoot Spark med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du arbetar med Apache Spark och Azure HDInsight."
keywords: "Azure HDInsight Spark, vanliga frågor och svar, felsökning guide, vanliga problem, konfiguration, Ambari"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: 25D89586-DE5B-4268-B5D5-CC2CE12207ED
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: c9f910daf295462238a3143ae2589db6d383097f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-spark-by-using-azure-hdinsight"></a>Felsöka Spark med Azure HDInsight

Läs mer om hello de främsta problemen och sina lösningar när du arbetar med Apache Spark nyttolaster i Apache Ambari.

## <a name="how-do-i-configure-a-spark-application-by-using-ambari-on-clusters"></a>Hur konfigurerar jag ett Spark-program genom att använda Ambari på kluster

### <a name="resolution-steps"></a>Lösningssteg

tidigare hello konfigurationsvärden för den här proceduren har ställts in i HDInsight. toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception). 

1. I hello kluster väljer du **Spark2**.

    ![Markera klustret från listan](media/hdinsight-troubleshoot-spark/update-config-1.png)

2. Välj hello **konfigurationerna** fliken.

    ![Fliken hello-konfigurationer](media/hdinsight-troubleshoot-spark/update-config-2.png)

3. Markera i hello lista över konfigurationer, **anpassad-spark2-standarder**.

    ![Ange standardinställningar för anpassad spark](media/hdinsight-troubleshoot-spark/update-config-3.png)

4. Leta efter hello-värdet som du behöver tooadjust, som **spark.executor.memory**. I det här fallet hello värdet för **4608m** är för hög.

    ![Markera hello spark.executor.memory fält](media/hdinsight-troubleshoot-spark/update-config-4.png)

5. Ange hello värdet toohello rekommenderad inställning. Hej värdet **2 048 m** rekommenderas för den här inställningen.

    ![Ändra värdet too2048m](media/hdinsight-troubleshoot-spark/update-config-5.png)

6. Spara hello värde och spara hello konfiguration. Välj hello verktygsfältet **spara**.

    ![Spara inställningen hello och konfiguration](media/hdinsight-troubleshoot-spark/update-config-6a.png)

    Du meddelas om några konfigurationer behöver åtgärdas. Observera hello objekt och välj sedan **fortsätta ändå**. 

    ![Välj fortsätter ändå](media/hdinsight-troubleshoot-spark/update-config-6b.png)

    Skriv en anteckning om hello konfigurationsändringar och välj sedan **spara**.

    ![Ange en notering om hello ändringar](media/hdinsight-troubleshoot-spark/update-config-6c.png)

7. När en konfiguration sparas uppmanas du toorestart hello-tjänsten. Välj **starta om**.

    ![Välj omstart](media/hdinsight-troubleshoot-spark/update-config-7a.png)

    Bekräfta hello omstart.

    ![Välj bekräfta starta om alla](media/hdinsight-troubleshoot-spark/update-config-7b.png)

    Du kan granska hello processer som körs.

    ![Granska processer som körs](media/hdinsight-troubleshoot-spark/update-config-7c.png)

8. Du kan lägga till konfigurationer. Markera i hello lista över konfigurationer, **anpassad-spark2-standarder**, och välj sedan **Lägg till egenskap**.

    ![Välj Lägg till egenskap](media/hdinsight-troubleshoot-spark/update-config-8.png)

9. Definiera en ny egenskap. Du kan definiera en egenskap med hjälp av en dialogruta för specifika inställningar, till exempel hello-datatypen. Eller så du kan definiera flera egenskaper med hjälp av en definition av per rad. 

    I det här exemplet hello **spark.driver.memory** egenskapen har definierats med värdet **4g**.

    ![Definiera nya egenskap](media/hdinsight-troubleshoot-spark/update-config-9.png)

10. Spara hello konfigurationen och starta om tjänsten hello enligt beskrivningen i steg 6 och 7.

Dessa ändringar är hela klustret, men kan åsidosättas när du skickar hello Spark jobb.

### <a name="additional-reading"></a>Ytterligare resurser

[Jobbet för Spark i HDInsight-kluster](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Hur konfigurerar jag ett Spark-program med hjälp av en Jupyter-anteckningsbok på kluster

### <a name="resolution-steps"></a>Lösningssteg

1. toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).

2. I hello första cellen i hello Jupyter notebook efter hello **%% konfigurera** direktiv, ange hello Spark konfigurationer i ett giltigt JSON-format. Ändra hello faktiska värden efter behov:

    ![Lägga till en konfiguration](media/hdinsight-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Ytterligare resurser

[Jobbet för Spark i HDInsight-kluster](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-livy-on-clusters"></a>Hur konfigurerar jag ett Spark-program med hjälp av Livius på kluster

### <a name="resolution-steps"></a>Lösningssteg

1. toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception). 

2. Skicka hello Spark programmet tooLivy med hjälp av REST-klient som cURL. Använda en liknande toohello följande i kommandot. Ändra hello faktiska värden efter behov:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Ytterligare resurser

[Jobbet för Spark i HDInsight-kluster](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters"></a>Hur konfigurerar ett program med hjälp av spark-skicka Spark på kluster

### <a name="resolution-steps"></a>Lösningssteg

1. toodetermine vilka Spark-konfigurationer måste toobe uppsättningen och toowhat värden, se [vad som orsakar en Spark OutofMemoryError undantag](#what-causes-a-spark-application-outofmemoryerror-exception).

2. Starta spark-shell med hjälp av ett kommando liknande toohello som följer. Ändra hello faktiska värdet för hello konfigurationer efter behov: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Ytterligare resurser

[Jobbet för Spark i HDInsight-kluster](https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="what-causes-a-spark-application-outofmemoryerror-exception"></a>Vad som orsakar en Spark OutofMemoryError undantag

### <a name="detailed-description"></a>Detaljerad beskrivning

hello Spark programmet misslyckas med följande typer av undantagsfel utan felhantering hello:

```apache
ERROR Executor: Exception in task 7.0 in stage 6.0 (TID 439) 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

```apache
ERROR SparkUncaughtExceptionHandler: Uncaught exception in thread Thread[Executor task launch worker-0,5,main] 

java.lang.OutOfMemoryError 
    at java.io.ByteArrayOutputStream.hugeCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.grow(Unknown Source) 
    at java.io.ByteArrayOutputStream.ensureCapacity(Unknown Source) 
    at java.io.ByteArrayOutputStream.write(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(Unknown Source) 
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject0(Unknown Source) 
    at java.io.ObjectOutputStream.writeObject(Unknown Source) 
    at org.apache.spark.serializer.JavaSerializationStream.writeObject(JavaSerializer.scala:44) 
    at org.apache.spark.serializer.JavaSerializerInstance.serialize(JavaSerializer.scala:101) 
    at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:239) 
    at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source) 
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source) 
    at java.lang.Thread.run(Unknown Source) 
```

### <a name="probable-cause"></a>Möjlig orsak

hello troligaste orsaken till det här undantaget är att det finns inte tillräckligt med minne för heap fördelas toohello Java virtuella datorer (JVMs). Dessa JVMs startas som executors eller drivrutiner som en del av hello Spark-program. 

### <a name="resolution-steps"></a>Lösningssteg

1. Fastställa hello maxstorleken för hello data hello Spark-program hanterar. Du kan göra en gissning baserat på hello maxstorleken för hello indata, hello mellanliggande data som produceras av omvandla hello indata och utdata för hello som skapas när programmet hello ytterligare förändrar hello mellanliggande data. Den här processen kan vara en iterativ om du inte göra en första formella gissning. 

2. Se till att du ska toouse har tillräckligt med resurser när det gäller minne och kärnor tooaccommodate hello Spark programmet hello HDInsight klustret. Du kan bestämma det genom att visa hello klustret mått avsnitt i hello YARN-Användargränssnittet för hello värden av **minne används** vs. **Minne totalt**, och **VCores används** vs. **Totalt antal VCores**.

3. Ange följande Spark hello konfigurationer tooappropriate värden som inte får överstiga 90% av hello tillgängligt minne och kärnor. hello-värden ska vara bra i hello minneskrav för hello Spark-program: 

    ```apache
    spark.executor.instances (Example: 8 for 8 executor count) 
    spark.executor.memory (Example: 4g for 4 GB) 
    spark.yarn.executor.memoryOverhead (Example: 384m for 384 MB) 
    spark.executor.cores (Example: 2 for 2 cores per executor) 
    spark.driver.memory (Example: 8g for 8GB) 
    spark.driver.cores (Example: 4 for 4 cores)   
    spark.yarn.driver.memoryOverhead (Example: 384m for 384MB) 
    ```

    tooget hello totalt minne som används av alla executors, kör hello följande kommando: 
    
    ```apache
    spark.executor.instances * (spark.executor.memory + spark.yarn.executor.memoryOverhead) 
    ```
    tooget hello totalt minne som används av hello drivrutin, kör hello följande kommando:
    
    ```apache
    spark.driver.memory + spark.yarn.driver.memoryOverhead
    ```

### <a name="additional-reading"></a>Ytterligare resurser

- [Översikt över Spark minne hantering](http://spark.apache.org/docs/latest/tuning.html#memory-management-overview)
- [Felsöka ett Spark-program på ett HDInsight-kluster](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/)

