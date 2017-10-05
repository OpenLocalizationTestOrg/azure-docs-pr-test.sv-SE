---
title: "Felsöka YARN med Azure HDInsight | Microsoft Docs"
description: "Få svar på vanliga frågor om hur du arbetar med Apache Hadoop YARN och Azure HDInsight."
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
ms.openlocfilehash: 63f2d88ad59661b7fbcffd0aaeb94c58d40bdb73
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-yarn-by-using-azure-hdinsight"></a><span data-ttu-id="d2154-104">Felsöka YARN med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d2154-104">Troubleshoot YARN by using Azure HDInsight</span></span>

<span data-ttu-id="d2154-105">Läs mer om de vanligaste problemen och sina lösningar när du arbetar med Apache Hadoop YARN nyttolaster i Apache Ambari.</span><span class="sxs-lookup"><span data-stu-id="d2154-105">Learn about the top issues and their resolutions when working with Apache Hadoop YARN payloads in Apache Ambari.</span></span>

## <a name="how-do-i-create-a-new-yarn-queue-on-a-cluster"></a><span data-ttu-id="d2154-106">Hur skapar jag en ny YARN-kö på ett kluster</span><span class="sxs-lookup"><span data-stu-id="d2154-106">How do I create a new YARN queue on a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="d2154-107">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d2154-107">Resolution steps</span></span> 

<span data-ttu-id="d2154-108">Använd följande steg i Ambari för att skapa en ny YARN-kö och balansera kapacitet fördelas mellan alla köer.</span><span class="sxs-lookup"><span data-stu-id="d2154-108">Use the following steps in Ambari to create a new YARN queue, and then balance the capacity allocation among all the queues.</span></span> 

<span data-ttu-id="d2154-109">I det här exemplet två befintliga köer (**standard** och **thriftsvr**) både ändras från 50% kapacitet till 25% kapacitet, som ger den nya kön (spark) 50% kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="d2154-109">In this example, two existing queues (**default** and **thriftsvr**) both are changed from 50% capacity to 25% capacity, which gives the new queue (spark) 50% capacity.</span></span>
| <span data-ttu-id="d2154-110">Kö</span><span class="sxs-lookup"><span data-stu-id="d2154-110">Queue</span></span> | <span data-ttu-id="d2154-111">Kapacitet</span><span class="sxs-lookup"><span data-stu-id="d2154-111">Capacity</span></span> | <span data-ttu-id="d2154-112">Maximal kapacitet</span><span class="sxs-lookup"><span data-stu-id="d2154-112">Maximum capacity</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d2154-113">Standard</span><span class="sxs-lookup"><span data-stu-id="d2154-113">default</span></span> | <span data-ttu-id="d2154-114">25 %</span><span class="sxs-lookup"><span data-stu-id="d2154-114">25%</span></span> | <span data-ttu-id="d2154-115">50%</span><span class="sxs-lookup"><span data-stu-id="d2154-115">50%</span></span> |
| <span data-ttu-id="d2154-116">thrftsvr</span><span class="sxs-lookup"><span data-stu-id="d2154-116">thrftsvr</span></span> | <span data-ttu-id="d2154-117">25 %</span><span class="sxs-lookup"><span data-stu-id="d2154-117">25%</span></span> | <span data-ttu-id="d2154-118">50%</span><span class="sxs-lookup"><span data-stu-id="d2154-118">50%</span></span> |
| <span data-ttu-id="d2154-119">Spark</span><span class="sxs-lookup"><span data-stu-id="d2154-119">spark</span></span> | <span data-ttu-id="d2154-120">50%</span><span class="sxs-lookup"><span data-stu-id="d2154-120">50%</span></span> | <span data-ttu-id="d2154-121">50%</span><span class="sxs-lookup"><span data-stu-id="d2154-121">50%</span></span> |

1. <span data-ttu-id="d2154-122">Välj den **Ambari Views** ikonen och välj sedan rutnät.</span><span class="sxs-lookup"><span data-stu-id="d2154-122">Select the **Ambari Views** icon, and then select the grid pattern.</span></span> <span data-ttu-id="d2154-123">Välj därefter **YARN Queue Manager**.</span><span class="sxs-lookup"><span data-stu-id="d2154-123">Next, select **YARN Queue Manager**.</span></span>

    ![Välj ikonen Ambari-vyer](media/hdinsight-troubleshoot-yarn/create-queue-1.png)
2. <span data-ttu-id="d2154-125">Välj den **standard** kön.</span><span class="sxs-lookup"><span data-stu-id="d2154-125">Select the **default** queue.</span></span>

    ![Välj standardkön](media/hdinsight-troubleshoot-yarn/create-queue-2.png)
3. <span data-ttu-id="d2154-127">För den **standard** kö, ändra den **kapacitet** från 50% 25%.</span><span class="sxs-lookup"><span data-stu-id="d2154-127">For the **default** queue, change the **capacity** from 50% to 25%.</span></span> <span data-ttu-id="d2154-128">För den **thriftsvr** kö, ändra den **kapacitet** 25%.</span><span class="sxs-lookup"><span data-stu-id="d2154-128">For the **thriftsvr** queue, change the **capacity** to 25%.</span></span>

    ![Ändra kapacitet till 25% för standard- och thriftsvr köer](media/hdinsight-troubleshoot-yarn/create-queue-3.png)
4. <span data-ttu-id="d2154-130">Om du vill skapa en ny kö **lägga till kön**.</span><span class="sxs-lookup"><span data-stu-id="d2154-130">To create a new queue, select **Add Queue**.</span></span>

    ![Välj Lägg till kön](media/hdinsight-troubleshoot-yarn/create-queue-4.png)

5. <span data-ttu-id="d2154-132">Namnge den nya kön.</span><span class="sxs-lookup"><span data-stu-id="d2154-132">Name the new queue.</span></span>

    ![Namnet kön Spark](media/hdinsight-troubleshoot-yarn/create-queue-5.png)  

6. <span data-ttu-id="d2154-134">Lämna den **kapacitet** värdena vid 50% och välj sedan den **åtgärder** knappen.</span><span class="sxs-lookup"><span data-stu-id="d2154-134">Leave the **capacity** values at 50%, and then select the **Actions** button.</span></span>

    ![Välj knappen åtgärder](media/hdinsight-troubleshoot-yarn/create-queue-6.png)  
7. <span data-ttu-id="d2154-136">Välj **spara och uppdatera köer**.</span><span class="sxs-lookup"><span data-stu-id="d2154-136">Select **Save and Refresh Queues**.</span></span>

    ![Välj Spara och uppdatera köer](media/hdinsight-troubleshoot-yarn/create-queue-7.png)  

<span data-ttu-id="d2154-138">Dessa ändringar visas omedelbart på YARN Scheduler-Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="d2154-138">These changes are visible immediately on the YARN Scheduler UI.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="d2154-139">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2154-139">Additional reading</span></span>

- [<span data-ttu-id="d2154-140">YARN CapacityScheduler</span><span class="sxs-lookup"><span data-stu-id="d2154-140">YARN CapacityScheduler</span></span>](https://hadoop.apache.org/docs/r2.7.2/hadoop-yarn/hadoop-yarn-site/CapacityScheduler.html)


## <a name="how-do-i-download-yarn-logs-from-a-cluster"></a><span data-ttu-id="d2154-141">Hur hämta YARN-loggar från ett kluster</span><span class="sxs-lookup"><span data-stu-id="d2154-141">How do I download YARN logs from a cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="d2154-142">Lösningssteg</span><span class="sxs-lookup"><span data-stu-id="d2154-142">Resolution steps</span></span> 

1. <span data-ttu-id="d2154-143">Ansluta till HDInsight-kluster med hjälp av en klient med SSH (Secure Shell).</span><span class="sxs-lookup"><span data-stu-id="d2154-143">Connect to the HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="d2154-144">Mer information finns i [ytterligare resurser](#additional-reading-2).</span><span class="sxs-lookup"><span data-stu-id="d2154-144">For more information, see [Additional reading](#additional-reading-2).</span></span>

2. <span data-ttu-id="d2154-145">Om du vill visa en lista över alla program-ID YARN-program som körs för närvarande, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-145">To list all the application IDs of the YARN applications that are currently running, run the following command:</span></span>

    ```apache
    yarn top
    ```
    <span data-ttu-id="d2154-146">ID: N visas i den **APPLICATIONID** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d2154-146">The IDs are listed in the **APPLICATIONID** column.</span></span> <span data-ttu-id="d2154-147">Du kan hämta loggar från den **APPLICATIONID** kolumn.</span><span class="sxs-lookup"><span data-stu-id="d2154-147">You can download logs from the **APPLICATIONID** column.</span></span>

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

3. <span data-ttu-id="d2154-148">Om du vill hämta YARN-loggar för behållaren för alla program original, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-148">To download YARN container logs for all application masters, use the following command:</span></span>
   
    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am ALL > amlogs.txt
    ```

    <span data-ttu-id="d2154-149">Detta kommando skapar en loggfil med namnet amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2154-149">This command creates a log file named amlogs.txt.</span></span> 

4. <span data-ttu-id="d2154-150">Hämta YARN-loggar för behållaren för endast det senaste programmet master, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-150">To download YARN container logs for only the latest application master, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am -1 > latestamlogs.txt
    ```

    <span data-ttu-id="d2154-151">Detta kommando skapar en loggfil med namnet latestamlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2154-151">This command creates a log file named latestamlogs.txt.</span></span> 

4. <span data-ttu-id="d2154-152">Hämta YARN-loggar för behållaren för de första två program original, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-152">To download YARN container logs for the first two application masters, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -am 1,2 > first2amlogs.txt 
    ```

    <span data-ttu-id="d2154-153">Detta kommando skapar en loggfil med namnet first2amlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2154-153">This command creates a log file named first2amlogs.txt.</span></span> 

5. <span data-ttu-id="d2154-154">Om du vill hämta alla loggar i YARN-behållare, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-154">To download all YARN container logs, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> > logs.txt
    ```

    <span data-ttu-id="d2154-155">Detta kommando skapar en loggfil med namnet logs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2154-155">This command creates a log file named logs.txt.</span></span> 

6. <span data-ttu-id="d2154-156">Om du vill hämta YARN-logg för behållare för en specifik behållare, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d2154-156">To download the YARN container log for a specific container, use the following command:</span></span>

    ```apache
    yarn logs -applicationIdn logs -applicationId <application_id> -containerId <container_id> > containerlogs.txt 
    ```

    <span data-ttu-id="d2154-157">Detta kommando skapar en loggfil med namnet containerlogs.txt.</span><span class="sxs-lookup"><span data-stu-id="d2154-157">This command creates a log file named containerlogs.txt.</span></span>

### <span data-ttu-id="d2154-158"><a name="additional-reading-2"></a>Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="d2154-158"><a name="additional-reading-2"></a>Additional reading</span></span>

- [<span data-ttu-id="d2154-159">Ansluta till HDInsight (Hadoop) med hjälp av SSH</span><span class="sxs-lookup"><span data-stu-id="d2154-159">Connect to HDInsight (Hadoop) by using SSH</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix)
- [<span data-ttu-id="d2154-160">Apache Hadoop YARN begrepp och program</span><span class="sxs-lookup"><span data-stu-id="d2154-160">Apache Hadoop YARN concepts and applications</span></span>](https://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/)







