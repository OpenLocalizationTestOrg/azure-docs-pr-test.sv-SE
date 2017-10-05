---
title: "Aktivera heap Dumpar för Hadoop på HDInsight - Azure-tjänster | Microsoft Docs"
description: "Aktivera heap Dumpar Hadoop från Linux-baserade HDInsight-kluster för felsökning och analys-tjänster."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8f151adb-f687-41e4-aca0-82b551953725
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 59942e989d622c2486edf181d76e13344c71e6f9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="b858f-103">Aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="b858f-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="b858f-104">Heap Dumpar innehåller en ögonblicksbild av programmets minne, inklusive värdena för variabler vid tiden för dumpen skapades.</span><span class="sxs-lookup"><span data-stu-id="b858f-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="b858f-105">Så att de är användbara för att diagnostisera problem som uppstår vid körning.</span><span class="sxs-lookup"><span data-stu-id="b858f-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b858f-106">Stegen i det här dokumentet fungerar endast med HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="b858f-106">The steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="b858f-107">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="b858f-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="b858f-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="b858f-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="b858f-109"><a name="whichServices"></a>Tjänster</span><span class="sxs-lookup"><span data-stu-id="b858f-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="b858f-110">Du kan aktivera heap Dumpar för följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="b858f-110">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="b858f-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="b858f-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="b858f-112">**hive** -hiveserver2 metastore, derbyserver</span><span class="sxs-lookup"><span data-stu-id="b858f-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="b858f-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="b858f-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="b858f-114">**yarn** -resurshanteraren, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="b858f-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="b858f-115">**hdfs** -datanode secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="b858f-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="b858f-116">Du kan också aktivera heap Dumpar för kartan och minska processer kördes av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b858f-116">You can also enable heap dumps for the map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="b858f-117"><a name="configuration"></a>Förstå heap dump konfiguration</span><span class="sxs-lookup"><span data-stu-id="b858f-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="b858f-118">Heap Dumpar aktiveras genom att skicka alternativ (ibland kallas bort, eller parametrar) till JVM när en tjänst har startats.</span><span class="sxs-lookup"><span data-stu-id="b858f-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) to the JVM when a service is started.</span></span> <span data-ttu-id="b858f-119">Du kan ändra shell-skript som används för att starta tjänsten om du vill skicka dessa alternativ för de flesta Hadoop-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b858f-119">For most Hadoop services, you can modify the shell script used to start the service to pass these options.</span></span>

<span data-ttu-id="b858f-120">I varje skript är en export för  **\* \_OPTS**, som innehåller de alternativ som angavs för JVM.</span><span class="sxs-lookup"><span data-stu-id="b858f-120">In each script, there is an export for **\*\_OPTS**, which contains the options passed to the JVM.</span></span> <span data-ttu-id="b858f-121">Till exempel i den **hadoop env.sh** skript rad som börjar med `export HADOOP_NAMENODE_OPTS=` innehåller alternativ för tjänsten NameNode.</span><span class="sxs-lookup"><span data-stu-id="b858f-121">For example, in the **hadoop-env.sh** script, the line that begins with `export HADOOP_NAMENODE_OPTS=` contains the options for the NameNode service.</span></span>

<span data-ttu-id="b858f-122">Mappa och minska processer är något annorlunda, eftersom dessa åtgärder är en underordnad process för MapReduce-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b858f-122">Map and reduce processes are slightly different, as these operations are a child process of the MapReduce service.</span></span> <span data-ttu-id="b858f-123">Varje mappa eller minska processen körs i en underordnad behållare och det finns två poster som innehåller JVM-alternativ.</span><span class="sxs-lookup"><span data-stu-id="b858f-123">Each map or reduce process runs in a child container, and there are two entries that contain the JVM options.</span></span> <span data-ttu-id="b858f-124">Både i **mapred site.xml**:</span><span class="sxs-lookup"><span data-stu-id="b858f-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="b858f-125">**mapreduce.Admin.Map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="b858f-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="b858f-126">**mapreduce.Admin.Reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="b858f-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="b858f-127">Vi rekommenderar att du använder Ambari för att ändra inställningar för både skript och mapred site.xml, som Ambari referens replikera ändringar på noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="b858f-127">We recommend using Ambari to modify both the scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in the cluster.</span></span> <span data-ttu-id="b858f-128">Finns det [med Ambari](#using-ambari) avsnittet specifika anvisningar.</span><span class="sxs-lookup"><span data-stu-id="b858f-128">See the [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="b858f-129">Aktivera heap dumps</span><span class="sxs-lookup"><span data-stu-id="b858f-129">Enable heap dumps</span></span>

<span data-ttu-id="b858f-130">Följande alternativ kan heap minnesdumpar när en OutOfMemoryError inträffar:</span><span class="sxs-lookup"><span data-stu-id="b858f-130">The following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="b858f-131">Den  **+**  anger att det här alternativet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="b858f-131">The **+** indicates that this option is enabled.</span></span> <span data-ttu-id="b858f-132">Standardvärdet är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="b858f-132">The default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="b858f-133">Heap Dumpar har inte aktiverats för Hadoop-tjänster på HDInsight standard dumpfiler kan vara stora.</span><span class="sxs-lookup"><span data-stu-id="b858f-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as the dump files can be large.</span></span> <span data-ttu-id="b858f-134">Om du aktiverar dem för felsökning kan du komma ihåg att inaktivera dem när du har försökt återskapa problemet och samlat in dumpfiler.</span><span class="sxs-lookup"><span data-stu-id="b858f-134">If you do enable them for troubleshooting, remember to disable them once you have reproduced the problem and gathered the dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="b858f-135">Dump plats</span><span class="sxs-lookup"><span data-stu-id="b858f-135">Dump location</span></span>

<span data-ttu-id="b858f-136">Standardplatsen för dumpfilen är den aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="b858f-136">The default location for the dump file is the current working directory.</span></span> <span data-ttu-id="b858f-137">Du kan kontrollera om filen sparas med hjälp av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="b858f-137">You can control where the file is stored using the following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="b858f-138">Till exempel använder `-XX:HeapDumpPath=/tmp` orsakar minnesdumpar lagras i tmp.</span><span class="sxs-lookup"><span data-stu-id="b858f-138">For example, using `-XX:HeapDumpPath=/tmp` causes the dumps to be stored in the /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="b858f-139">Skript</span><span class="sxs-lookup"><span data-stu-id="b858f-139">Scripts</span></span>

<span data-ttu-id="b858f-140">Du kan även utlösa ett skript när en **OutOfMemoryError** inträffar.</span><span class="sxs-lookup"><span data-stu-id="b858f-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="b858f-141">Till exempel utlösa en avisering så att du vet att felet har uppstått.</span><span class="sxs-lookup"><span data-stu-id="b858f-141">For example, triggering a notification so you know that the error has occurred.</span></span> <span data-ttu-id="b858f-142">Använd följande alternativ för att utlösa ett skript på en __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="b858f-142">Use the following option to trigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="b858f-143">Eftersom Hadoop är ett distribuerat system, måste alla skript som används placeras på alla noder i klustret som tjänsten körs på.</span><span class="sxs-lookup"><span data-stu-id="b858f-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in the cluster that the service runs on.</span></span>
> 
> <span data-ttu-id="b858f-144">Skriptet måste också vara på en plats som är tillgänglig för det konto som tjänsten körs som och måste ange behörighet att köra.</span><span class="sxs-lookup"><span data-stu-id="b858f-144">The script must also be in a location that is accessible by the account the service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="b858f-145">Exempelvis kanske du vill lagra skript i `/usr/local/bin` och använda `chmod go+rx /usr/local/bin/filename.sh` att bevilja Läs- och körningsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="b858f-145">For example, you may wish to store scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` to grant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="b858f-146">Med Ambari</span><span class="sxs-lookup"><span data-stu-id="b858f-146">Using Ambari</span></span>

<span data-ttu-id="b858f-147">Om du vill ändra konfigurationen för en tjänst använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="b858f-147">To modify the configuration for a service, use the following steps:</span></span>

1. <span data-ttu-id="b858f-148">Öppna Ambari webbgränssnittet för klustret.</span><span class="sxs-lookup"><span data-stu-id="b858f-148">Open the Ambari web UI for your cluster.</span></span> <span data-ttu-id="b858f-149">Webbadressen är https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="b858f-149">The URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="b858f-150">När du uppmanas, autentisera till platsen med HTTP-kontonamnet (standard: admin) och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="b858f-150">When prompted, authenticate to the site using the HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b858f-151">Du kan uppmanas att en gång av Ambari användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b858f-151">You may be prompted a second time by Ambari for the user name and password.</span></span> <span data-ttu-id="b858f-152">I så fall, ange samma kontonamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="b858f-152">If so, enter the same account name and password</span></span>

2. <span data-ttu-id="b858f-153">Använd listan över till vänster och markera området tjänst som du vill ändra.</span><span class="sxs-lookup"><span data-stu-id="b858f-153">Using the list of on the left, select the service area you want to modify.</span></span> <span data-ttu-id="b858f-154">Till exempel **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="b858f-154">For example, **HDFS**.</span></span> <span data-ttu-id="b858f-155">I området center väljer du den **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="b858f-155">In the center area, select the **Configs** tab.</span></span>

    ![Bild av Ambari web HDFS konfigurationerna fliken](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="b858f-157">Med den **Filter...**  posten, ange **bort**.</span><span class="sxs-lookup"><span data-stu-id="b858f-157">Using the **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="b858f-158">Endast objekt som innehåller den här texten visas.</span><span class="sxs-lookup"><span data-stu-id="b858f-158">Only items containing this text are displayed.</span></span>

    ![Filtrerad lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="b858f-160">Hitta de  **\* \_OPTS** post för tjänsten som du vill aktivera heap Dumpar för och Lägg till de alternativ som du vill aktivera.</span><span class="sxs-lookup"><span data-stu-id="b858f-160">Find the **\*\_OPTS** entry for the service you want to enable heap dumps for, and add the options you wish to enable.</span></span> <span data-ttu-id="b858f-161">I följande bild, jag har lagt till `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` till den **HADOOP\_NAMENODE\_OPTS** post:</span><span class="sxs-lookup"><span data-stu-id="b858f-161">In the following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` to the **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![HADOOP_NAMENODE_OPTS med - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="b858f-163">När Aktivera heap Dumpar för kartan eller minska underordnad process titta för fält med namnet **mapreduce.admin.map.child.java.opts** och **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="b858f-163">When enabling heap dumps for the map or reduce child process, look for the fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="b858f-164">Använd den **spara** för att spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b858f-164">Use the **Save** button to save the changes.</span></span> <span data-ttu-id="b858f-165">Du kan ange en kort anteckning om ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b858f-165">You can enter a short note describing the changes.</span></span>

5. <span data-ttu-id="b858f-166">När ändringarna har tillämpats på **omstart krävs** ikon visas bredvid en eller flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="b858f-166">Once the changes have been applied, the **Restart required** icon appears beside one or more services.</span></span>

    ![Starta om nödvändiga ikon och starta om knappen](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="b858f-168">Markera varje tjänst som måste startas om och Använd den **tjänståtgärder** för att **aktivera på underhållsläge**.</span><span class="sxs-lookup"><span data-stu-id="b858f-168">Select each service that needs a restart, and use the **Service Actions** button to **Turn On Maintenance Mode**.</span></span> <span data-ttu-id="b858f-169">Underhållsläge förhindrar att aviseringar som genereras från tjänsten när du startar om.</span><span class="sxs-lookup"><span data-stu-id="b858f-169">Maintenance mode prevents alerts from being generated from the service when you restart it.</span></span>

    ![Aktivera menyn för underhåll läge](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="b858f-171">När du har aktiverat underhållsläget kan använda den **starta om** knappen för tjänsten **starta om alla påverkas**</span><span class="sxs-lookup"><span data-stu-id="b858f-171">Once you have enabled maintenance mode, use the **Restart** button for the service to **Restart All Effected**</span></span>

    ![Starta om alla berörda post](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="b858f-173">posterna för den **starta om** knappen kan vara olika för andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="b858f-173">the entries for the **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="b858f-174">När tjänsten har startats om, använda den **tjänståtgärder** för att **aktivera inaktivera underhållsläge**.</span><span class="sxs-lookup"><span data-stu-id="b858f-174">Once the services have been restarted, use the **Service Actions** button to **Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="b858f-175">Den här Ambari att återuppta övervakning för aviseringar för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b858f-175">This Ambari to resume monitoring for alerts for the service.</span></span>

