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
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a>Felsöka Hive med Azure HDInsight

Läs mer om hello översta frågor och sina lösningar när du arbetar med Apache Hive nyttolaster i Apache Ambari.


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a>Hur gör exportera en Hive metastore och importera det till ett annat kluster


### <a name="resolution-steps"></a>Lösningssteg

1. Ansluta toohello HDInsight-kluster med hjälp av en klient med SSH (Secure Shell). Mer information finns i [ytterligare resurser](#additional-reading-end).

2. Kör följande kommando på hello HDInsight-kluster som du vill tooexport hello metastore hello:

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  Det här kommandot genererar en fil med namnet allatables.sql.

3. Kopiera hello filen alltables.sql toohello nya HDInsight-kluster och kör sedan följande kommando hello:

  ```apache
  hive -f alltables.sql
  ```

hello koden i hello Lösningssteg förutsätter att data sökvägar på hello nytt kluster hello samma som hello datasökvägar på hello gamla klustret. Om hello datasökvägar är olika, kan du manuellt redigera hello genereras alltables.sql filen tooreflect ändringar.

### <a name="additional-reading"></a>Ytterligare resurser

- [Ansluta tooan HDInsight-kluster med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a>Hur jag för att hitta Hive loggar på ett kluster

### <a name="resolution-steps"></a>Lösningssteg

1. Ansluta toohello HDInsight-kluster med hjälp av SSH. Mer information finns i **ytterligare resurser**.

2. Klientloggfiler för tooview Hive, Använd hello följande kommando:

  ```apache
  /tmp/<username>/hive.log 
  ```

3. tooview loggar för Hive metastore, Använd hello följande kommando:

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. tooview Hiveserver loggar, Använd hello följande kommando:

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a>Ytterligare resurser

- [Ansluta tooan HDInsight-kluster med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a>Hur jag för att starta hello Hive-gränssnittet med specifika konfigurationer i ett kluster

### <a name="resolution-steps"></a>Lösningssteg

1. Ange ett nyckel / värde-par för konfiguration när du startar hello Hive-gränssnittet. Mer information finns i [ytterligare resurser](#additional-reading-end).

  ```apache
  hive -hiveconf a=b 
  ```

2. toolist alla effektiva konfigurationer på Hive-gränssnittet, Använd hello följande kommando:

  ```apache
  hive> set;
  ```

  Till exempel använda hello följande toostart Hive-kommandogränssnittet utan felsökningsloggning aktiverats på hello-konsolen:

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a>Ytterligare resurser

- [Egenskaper för hive-konfiguration](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hur jag för att analysera Tez DAG data på ett kluster kritiska


### <a name="resolution-steps"></a>Lösningssteg
 
1. tooanalyze en Apache Tez dirigeras acykliskt diagram (DAG) i ett kluster-kritiskt diagram, Anslut toohello HDInsight-kluster med hjälp av SSH. Mer information finns i [ytterligare resurser](#additional-reading-end).

2. Kör hello följande kommando vid en kommandotolk:
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. toolist andra analyzers som kan använda tooanalyze Tez DAG använder hello följande kommando:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  Du måste ange ett exempelprogram som hello första argument.

  Giltigt programnamn inkluderar:
    - **ContainerReuseAnalyzer**: Skriv ut behållaren återanvändning i DAG
    - **CriticalPath**: hitta hello kritiska i DAG
    - **LocalityAnalyzer**: Skriv ut ort i DAG
    - **ShuffleTimeAnalyzer**: analysera hello blanda tidsinformation i DAG
    - **SkewAnalyzer**: analysera hello skeva information i DAG
    - **SlowNodeAnalyzer**: skriva noden information i DAG
    - **SlowTaskIdentifier**: Skriv ut långsam aktivitetsinformation i DAG
    - **SlowestVertexAnalyzer**: Skriv ut långsammaste hörn i DAG
    - **SpillAnalyzer**: Skriv ut oljesanering detaljer i DAG
    - **TaskConcurrencyAnalyzer**: Skriv ut hello samtidighet aktivitetsinformation i DAG
    - **VertexLevelCriticalPathAnalyzer**: hitta hello kritiska nivån hörn i DAG


### <a name="additional-reading"></a>Ytterligare resurser

- [Ansluta tooan HDInsight-kluster med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a>Hur hämta Tez DAG data från ett kluster


#### <a name="resolution-steps"></a>Lösningssteg

Det finns två sätt toocollect hello Tez DAG data:

- Hello kommandoraden:
 
    Ansluta toohello HDInsight-kluster med hjälp av SSH. Kör följande kommando hello Kommandotolken hello:

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- Använd hello Ambari Tez vy:
   
  1. Gå tooAmbari. 
  2. Gå tooTez vy (under hello paneler ikon i hello längst upp till höger). 
  3. Välj hello DAG som du vill tooview.
  4. Välj **ladda ned data**.

### <a name="additional-reading-end"></a>Ytterligare resurser

[Ansluta tooan HDInsight-kluster med hjälp av SSH](hdinsight-hadoop-linux-use-ssh-unix.md)






