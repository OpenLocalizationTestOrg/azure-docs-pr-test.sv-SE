---
title: "aaaTroubleshoot YARN med hjälp av Azure HDInsight | Microsoft Docs"
description: "Få svar toocommon frågor om hur du arbetar med Apache Hadoop YARN och Azure HDInsight."
keywords: "Azure HDInsight, YARN, vanliga frågor och svar, felsökningsguide för vanliga frågor"
services: Azure HDInsight
documentationcenter: na
author: arijitt
manager: 
editor: 
ms.assetid: F76786A9-99AB-4B85-9B15-CA03528FC4CD
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: arijitt
ms.openlocfilehash: 800d9738cb27e05a64db470ee58565af3b85aa99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a>Felsöka YARN med Azure HDInsight

Läs mer om hello de främsta problemen och sina lösningar när du arbetar med Apache Hadoop YARN nyttolaster i Apache Ambari.

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a>Hur skapar jag en ny YARN-kö på ett kluster


### <a name="resolution-steps"></a>Lösningssteg 

Använd hello följande steg i Ambari toocreate en ny YARN-kö och balansera hello kapacitet fördelas mellan alla hello köer. 

I det här exemplet två befintliga köer (**standard** och **thriftsvr**) både ändras från 50% kapacitet too25% kapacitet som ger hello nya kön (spark) 50% kapaciteten.
| Kö | Kapacitet | Maximal kapacitet |
| --- | --- | --- | --- |
| Standard | 25 % | 50% |
| thrftsvr | 25 % | 50% |
| Spark | 50% | 50% |

1. Välj hello **Ambari Views** ikonen och välj sedan hello rutnät. Välj därefter **YARN Queue Manager**.

    ![Välj hello Ambari Views ikon](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. Välj hello **standard** kön.

    ![Välj hello standardkö](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. För hello **standard** kö, ändra hello **kapacitet** från 50% too25%. För hello **thriftsvr** kö, ändra hello **kapacitet** too25%.

    ![Ändra hello kapacitet too25% för hello standard och thriftsvr köer](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. Välj toocreate en ny kö **lägga till kön**.

    ![Välj Lägg till kön](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. Namnet hello ny kö.

    ![Namnet hello kön Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. Lämna hello **kapacitet** värdena vid 50% och välj sedan hello **åtgärder** knappen.

    ![Markera knappen för hello-åtgärder](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. Välj **spara och uppdatera köer**.

    ![Välj Spara och uppdatera köer](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

Dessa ändringar visas omedelbart på hello YARN-Användargränssnittet för Schemaläggaren.

### <a name="additional-reading"></a>Ytterligare resurser

- [YARN CapacityScheduler](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a>Hur hämta YARN-loggar från ett kluster


### <a name="resolution-steps"></a>Lösningssteg 

1. Ansluta toohello HDInsight-kluster med hjälp av en klient med SSH (Secure Shell). Mer information finns i [ytterligare resurser](#additional-reading-2).

2. toolist alla hello program-ID för hello YARN program som körs för närvarande kör hello följande kommando:

    ```apache
    yarn top
    ```
    hello ID visas i hello **APPLICATIONID** kolumn. Du kan hämta loggar från hello **APPLICATIONID** kolumn.

    ```apache
    YARN top - 18:00:07, up 19d, 0:14, 0 active users, queue(s): root
    NodeManager(s): 4 total, 4 active, 0 unhealthy, 0 decommissioned, 0 lost, 0 rebooted
    Queue(s) Applications: 2 running, 10 submitted, 0 pending, 8 completed, 0 killed, 0 failed
    Queue(s) Mem(GB): 97 available, 3 allocated, 0 pending, 0 reserved
    Queue(s) VCores: 58 available, 2 allocated, 0 pending, 0 reserved
    Queue(s) Containers: 2 allocated, 0 pending, 0 reserved

                      APPLICATIONID USER             TYPE      QUEUE   #CONT  #RCONT  VCORES RVCORES     MEM    RMEM  VCORESECS    MEMSECS %PROGR       TIME NAME
     application_1490377567345_0007 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628407    2442611  10.00   18:20:20 Thrift JDBC/ODBC Server
     application_1490377567345_0006 hive            spark  thriftsvr       1       0       1       0      1G      0G    1628430    2442645  10.00   18:20:20 Thrift JDBC/ODBC Server
    ```

3. toodownload YARN behållare händelseloggarna för alla program original använder hello följande kommando:
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    Detta kommando skapar en loggfil med namnet amlogs.txt. 

4. toodownload YARN-behållaren loggar för endast hello senaste programmet master, Använd hello följande kommando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    Detta kommando skapar en loggfil med namnet latestamlogs.txt. 

4. toodownload YARN behållare loggar för hello första två program original, Använd hello följande kommando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    Detta kommando skapar en loggfil med namnet first2amlogs.txt. 

5. händelseloggarna för alla YARN-behållare, använder du toodownload hello följande kommando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    Detta kommando skapar en loggfil med namnet logs.txt. 

6. toodownload hello YARN behållare log för en specifik behållare, Använd hello följande kommando:

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    Detta kommando skapar en loggfil med namnet containerlogs.txt.

### <a name="additional-reading-2"></a>Ytterligare resurser

- [Ansluta tooHDInsight (Hadoop) med hjälp av SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [Apache Hadoop YARN begrepp och program](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







