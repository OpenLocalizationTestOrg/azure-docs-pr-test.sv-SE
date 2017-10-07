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
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="d2718-104">Felsöka YARN med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2718-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="d2718-105">Läs mer om hello de främsta problemen och sina lösningar när du arbetar med Apache Hadoop YARN nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="d2718-105">Learn about hello top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="d2718-106">Hur skapar jag en ny YARN-kö på ett kluster</span><span class="sxs-lookup"><span data-stu-id="d2718-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="d2718-107">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d2718-107">Resolution steps</span></span> 

<span data-ttu-id="d2718-108">Använd hello följande steg i Ambari toocreate en ny YARN-kö och balansera hello kapacitet fördelas mellan alla hello köer.</span><span class="sxs-lookup"><span data-stu-id="d2718-108">Use hello following steps in Ambari toocreate a new YARN queue, and then balance hello capacity allocation among all hello queues.</span></span> 

<span data-ttu-id="d2718-109">I det här exemplet två befintliga köer (**standard** och **thriftsvr**) både ändras från 50% kapacitet too25% kapacitet som ger hello nya kön (spark) 50% kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="d2718-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity too25% capacity, which gives hello new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="d2718-110">Kö</span><span class="sxs-lookup"><span data-stu-id="d2718-110">Queue</span></span> | <span data-ttu-id="d2718-111">Kapacitet</span><span class="sxs-lookup"><span data-stu-id="d2718-111">Capacity</span></span> | <span data-ttu-id="d2718-112">Maximal kapacitet</span><span class="sxs-lookup"><span data-stu-id="d2718-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d2718-113">Standard</span><span class="sxs-lookup"><span data-stu-id="d2718-113">default</span></span> | <span data-ttu-id="d2718-114">25 %</span><span class="sxs-lookup"><span data-stu-id="d2718-114">25%</span></span> | <span data-ttu-id="d2718-115">50%</span><span class="sxs-lookup"><span data-stu-id="d2718-115">50%</span></span> |
| <span data-ttu-id="d2718-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="d2718-116">thrftsvr</span></span> | <span data-ttu-id="d2718-117">25 %</span><span class="sxs-lookup"><span data-stu-id="d2718-117">25%</span></span> | <span data-ttu-id="d2718-118">50%</span><span class="sxs-lookup"><span data-stu-id="d2718-118">50%</span></span> |
| <span data-ttu-id="d2718-119">Spark</span><span class="sxs-lookup"><span data-stu-id="d2718-119">spark</span></span> | <span data-ttu-id="d2718-120">50%</span><span class="sxs-lookup"><span data-stu-id="d2718-120">50%</span></span> | <span data-ttu-id="d2718-121">50%</span><span class="sxs-lookup"><span data-stu-id="d2718-121">50%</span></span> |

1. <span data-ttu-id="d2718-122">Välj hello **Ambari Views** ikonen och välj sedan hello rutnät.</span><span class="sxs-lookup"><span data-stu-id="d2718-122">Select hello **Ambari Views** icon, and then select hello grid pattern.</span></span> <span data-ttu-id="d2718-123">Välj därefter **YARN Queue Manager**.</span><span class="sxs-lookup"><span data-stu-id="d2718-123">Next, select **YARN Queue Manager**.</span></span>

    ![Välj hello Ambari Views ikon](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="d2718-125">Välj hello **standard** kön.</span><span class="sxs-lookup"><span data-stu-id="d2718-125">Select hello **default** queue.</span></span>

    ![Välj hello standardkö](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="d2718-127">För hello **standard** kö, ändra hello **kapacitet** från 50% too25%.</span><span class="sxs-lookup"><span data-stu-id="d2718-127">For hello **default** queue, change hello **capacity** from 50% too25%.</span></span> <span data-ttu-id="d2718-128">För hello **thriftsvr** kö, ändra hello **kapacitet** too25%.</span><span class="sxs-lookup"><span data-stu-id="d2718-128">For hello **thriftsvr** queue, change hello **capacity** too25%.</span></span>

    ![Ändra hello kapacitet too25% för hello standard och thriftsvr köer](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="d2718-130">Välj toocreate en ny kö **lägga till kön**.</span><span class="sxs-lookup"><span data-stu-id="d2718-130">toocreate a new queue, select **Add Queue**.</span></span>

    ![Välj Lägg till kön](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="d2718-132">Namnet hello ny kö.</span><span class="sxs-lookup"><span data-stu-id="d2718-132">Name hello new queue.</span></span>

    ![Namnet hello kön Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="d2718-134">Lämna hello **kapacitet** värdena vid 50% och välj sedan hello **åtgärder** knappen.</span><span class="sxs-lookup"><span data-stu-id="d2718-134">Leave hello **capacity** values at 50%, and then select hello **Actions** button.</span></span>

    ![Markera knappen för hello-åtgärder](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="d2718-136">Välj **spara och uppdatera köer**.</span><span class="sxs-lookup"><span data-stu-id="d2718-136">Select **Save and Refresh Queues**.</span></span>

    ![Välj Spara och uppdatera köer](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="d2718-138">Dessa ändringar visas omedelbart på hello YARN-Användargränssnittet för Schemaläggaren.</span><span class="sxs-lookup"><span data-stu-id="d2718-138">These changes are visible immediately on hello YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="d2718-139">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2718-139">Additional reading</span></span>

- [<span data-ttu-id="d2718-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="d2718-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="d2718-141">Hur hämta YARN-loggar från ett kluster</span><span class="sxs-lookup"><span data-stu-id="d2718-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="d2718-142">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d2718-142">Resolution steps</span></span> 

1. <span data-ttu-id="d2718-143">Ansluta toohello HDInsight-kluster med hjälp av en klient med SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="d2718-143">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="d2718-144">Mer information finns i [ytterligare resurser](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="d2718-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="d2718-145">toolist alla hello program-ID för hello YARN program som körs för närvarande kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-145">toolist all hello application IDs of hello YARN applications that are currently running, run hello following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="d2718-146">hello ID visas i hello **APPLICATIONID** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d2718-146">hello IDs are listed in hello **APPLICATIONID** column.</span></span> <span data-ttu-id="d2718-147">Du kan hämta loggar från hello **APPLICATIONID** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d2718-147">You can download logs from hello **APPLICATIONID** column.</span></span>

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

3. <span data-ttu-id="d2718-148">toodownload YARN behållare händelseloggarna för alla program original använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-148">toodownload YARN container logs for all application masters, use hello following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="d2718-149">Detta kommando skapar en loggfil med namnet amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2718-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="d2718-150">toodownload YARN-behållaren loggar för endast hello senaste programmet master, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-150">toodownload YARN container logs for only hello latest application master, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="d2718-151">Detta kommando skapar en loggfil med namnet latestamlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2718-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="d2718-152">toodownload YARN behållare loggar för hello första två program original, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-152">toodownload YARN container logs for hello first two application masters, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="d2718-153">Detta kommando skapar en loggfil med namnet first2amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2718-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="d2718-154">händelseloggarna för alla YARN-behållare, använder du toodownload hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-154">toodownload all YARN container logs, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="d2718-155">Detta kommando skapar en loggfil med namnet logs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2718-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="d2718-156">toodownload hello YARN behållare log för en specifik behållare, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2718-156">toodownload hello YARN container log for a specific container, use hello following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="d2718-157">Detta kommando skapar en loggfil med namnet containerlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2718-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="d2718-158"><a name="additional-reading-2"></a>Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2718-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="d2718-159">Ansluta tooHDInsight (Hadoop) med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="d2718-159">Connect tooHDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="d2718-160">Apache Hadoop YARN begrepp och program</span><span class="sxs-lookup"><span data-stu-id="d2718-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







