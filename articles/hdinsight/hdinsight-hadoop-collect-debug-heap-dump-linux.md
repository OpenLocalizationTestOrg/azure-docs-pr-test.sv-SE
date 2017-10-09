---
title: "aaaEnable heap Dumpar för Hadoop på HDInsight - Azure-tjänster | Microsoft Docs"
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
ms.openlocfilehash: 49e30f26e1a83f19e068e9da253b5548caec70d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a><span data-ttu-id="918fe-103">Aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight</span><span class="sxs-lookup"><span data-stu-id="918fe-103">Enable heap dumps for Hadoop services on Linux-based HDInsight</span></span>

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="918fe-104">Heap Dumpar innehåller en ögonblicksbild av hello programmets minne, inklusive hello värdena för variabler som för närvarande hello hello dump har skapats.</span><span class="sxs-lookup"><span data-stu-id="918fe-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="918fe-105">Så att de är användbara för att diagnostisera problem som uppstår vid körning.</span><span class="sxs-lookup"><span data-stu-id="918fe-105">So they are useful for diagnosing problems that occur at run-time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="918fe-106">hello fungerar stegen i det här dokumentet endast med HDInsight-kluster som använder Linux.</span><span class="sxs-lookup"><span data-stu-id="918fe-106">hello steps in this document only work with HDInsight clusters that use Linux.</span></span> <span data-ttu-id="918fe-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="918fe-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="918fe-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="918fe-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="918fe-109"><a name="whichServices"></a>Tjänster</span><span class="sxs-lookup"><span data-stu-id="918fe-109"><a name="whichServices"></a>Services</span></span>

<span data-ttu-id="918fe-110">Du kan aktivera heap Dumpar för hello följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="918fe-110">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="918fe-111">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="918fe-111">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="918fe-112">**hive** -hiveserver2 metastore, derbyserver</span><span class="sxs-lookup"><span data-stu-id="918fe-112">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="918fe-113">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="918fe-113">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="918fe-114">**yarn** -resurshanteraren, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="918fe-114">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="918fe-115">**hdfs** -datanode secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="918fe-115">**hdfs** - datanode, secondarynamenode, namenode</span></span>

<span data-ttu-id="918fe-116">Du kan också aktivera heap Dumpar för hello kartan och minska processer kördes av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="918fe-116">You can also enable heap dumps for hello map and reduce processes ran by HDInsight.</span></span>

## <span data-ttu-id="918fe-117"><a name="configuration"></a>Förstå heap dump konfiguration</span><span class="sxs-lookup"><span data-stu-id="918fe-117"><a name="configuration"></a>Understanding heap dump configuration</span></span>

<span data-ttu-id="918fe-118">Heap Dumpar aktiveras genom att skicka alternativ (ibland kallas bort, eller parametrar) toohello JVM när en tjänst har startats.</span><span class="sxs-lookup"><span data-stu-id="918fe-118">Heap dumps are enabled by passing options (sometimes known as opts, or parameters) toohello JVM when a service is started.</span></span> <span data-ttu-id="918fe-119">Du kan ändra dessa alternativ hello shell-skript används toostart hello service toopass för de flesta Hadoop-tjänster.</span><span class="sxs-lookup"><span data-stu-id="918fe-119">For most Hadoop services, you can modify hello shell script used toostart hello service toopass these options.</span></span>

<span data-ttu-id="918fe-120">I varje skript är en export för  **\* \_OPTS**, som innehåller alternativ för hello skickades toohello JVM.</span><span class="sxs-lookup"><span data-stu-id="918fe-120">In each script, there is an export for **\*\_OPTS**, which contains hello options passed toohello JVM.</span></span> <span data-ttu-id="918fe-121">Till exempel i hello **hadoop env.sh** skript hello rad som börjar med `export HADOOP_NAMENODE_OPTS=` innehåller hello alternativ för hello NameNode service.</span><span class="sxs-lookup"><span data-stu-id="918fe-121">For example, in hello **hadoop-env.sh** script, hello line that begins with `export HADOOP_NAMENODE_OPTS=` contains hello options for hello NameNode service.</span></span>

<span data-ttu-id="918fe-122">Mappa och minska processer är något annorlunda, eftersom dessa åtgärder är en underordnad process för hello MapReduce-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="918fe-122">Map and reduce processes are slightly different, as these operations are a child process of hello MapReduce service.</span></span> <span data-ttu-id="918fe-123">Varje mappa eller minska processen körs i en underordnad behållare och det finns två poster som innehåller hello JVM alternativ.</span><span class="sxs-lookup"><span data-stu-id="918fe-123">Each map or reduce process runs in a child container, and there are two entries that contain hello JVM options.</span></span> <span data-ttu-id="918fe-124">Både i **mapred site.xml**:</span><span class="sxs-lookup"><span data-stu-id="918fe-124">Both contained in **mapred-site.xml**:</span></span>

* <span data-ttu-id="918fe-125">**mapreduce.Admin.Map.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="918fe-125">**mapreduce.admin.map.child.java.opts**</span></span>
* <span data-ttu-id="918fe-126">**mapreduce.Admin.Reduce.child.Java.opts**</span><span class="sxs-lookup"><span data-stu-id="918fe-126">**mapreduce.admin.reduce.child.java.opts**</span></span>

> [!NOTE]
> <span data-ttu-id="918fe-127">Vi rekommenderar med Ambari toomodify båda hello skript och mapred site.xml inställningar som Ambari hantera replikera ändringar på noderna i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="918fe-127">We recommend using Ambari toomodify both hello scripts and mapred-site.xml settings, as Ambari handle replicating changes across nodes in hello cluster.</span></span> <span data-ttu-id="918fe-128">Se hello [med Ambari](#using-ambari) avsnittet specifika anvisningar.</span><span class="sxs-lookup"><span data-stu-id="918fe-128">See hello [Using Ambari](#using-ambari) section for specific steps.</span></span>

### <a name="enable-heap-dumps"></a><span data-ttu-id="918fe-129">Aktivera heap dumps</span><span class="sxs-lookup"><span data-stu-id="918fe-129">Enable heap dumps</span></span>

<span data-ttu-id="918fe-130">hello kan följande alternativ heap minnesdumpar när en OutOfMemoryError inträffar:</span><span class="sxs-lookup"><span data-stu-id="918fe-130">hello following option enables heap dumps when an OutOfMemoryError occurs:</span></span>

    -XX:+HeapDumpOnOutOfMemoryError

<span data-ttu-id="918fe-131">Hej  **+**  anger att det här alternativet är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="918fe-131">hello **+** indicates that this option is enabled.</span></span> <span data-ttu-id="918fe-132">hello standard är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="918fe-132">hello default is disabled.</span></span>

> [!WARNING]
> <span data-ttu-id="918fe-133">Heap Dumpar har inte aktiverats för Hadoop-tjänster på HDInsight standard hello dumpfiler kan vara stora.</span><span class="sxs-lookup"><span data-stu-id="918fe-133">Heap dumps are not enabled for Hadoop services on HDInsight by default, as hello dump files can be large.</span></span> <span data-ttu-id="918fe-134">Om du aktiverar dem för att felsöka, Kom ihåg toodisable dem när du har återskapat hello problemet och insamlade hello dumpfiler.</span><span class="sxs-lookup"><span data-stu-id="918fe-134">If you do enable them for troubleshooting, remember toodisable them once you have reproduced hello problem and gathered hello dump files.</span></span>

### <a name="dump-location"></a><span data-ttu-id="918fe-135">Dump plats</span><span class="sxs-lookup"><span data-stu-id="918fe-135">Dump location</span></span>

<span data-ttu-id="918fe-136">hello standardplatsen för hello dumpfilen är hello aktuella arbetskatalogen.</span><span class="sxs-lookup"><span data-stu-id="918fe-136">hello default location for hello dump file is hello current working directory.</span></span> <span data-ttu-id="918fe-137">Du kan styra om hello filen lagras med hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="918fe-137">You can control where hello file is stored using hello following option:</span></span>

    -XX:HeapDumpPath=/path

<span data-ttu-id="918fe-138">Till exempel använder `-XX:HeapDumpPath=/tmp` orsakar hello Dumpar toobe lagras i hello tmp.</span><span class="sxs-lookup"><span data-stu-id="918fe-138">For example, using `-XX:HeapDumpPath=/tmp` causes hello dumps toobe stored in hello /tmp directory.</span></span>

### <a name="scripts"></a><span data-ttu-id="918fe-139">Skript</span><span class="sxs-lookup"><span data-stu-id="918fe-139">Scripts</span></span>

<span data-ttu-id="918fe-140">Du kan även utlösa ett skript när en **OutOfMemoryError** inträffar.</span><span class="sxs-lookup"><span data-stu-id="918fe-140">You can also trigger a script when an **OutOfMemoryError** occurs.</span></span> <span data-ttu-id="918fe-141">Till exempel har utlösa en avisering så att du vet att hello-fel uppstått.</span><span class="sxs-lookup"><span data-stu-id="918fe-141">For example, triggering a notification so you know that hello error has occurred.</span></span> <span data-ttu-id="918fe-142">Använd hello följande alternativ tootrigger ett skript på en __OutOfMemoryError__:</span><span class="sxs-lookup"><span data-stu-id="918fe-142">Use hello following option tootrigger a script on an __OutOfMemoryError__:</span></span>

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> <span data-ttu-id="918fe-143">Eftersom Hadoop är ett distribuerat system, måste alla skript som används placeras på alla noder i klustret för hello som hello-tjänsten körs på.</span><span class="sxs-lookup"><span data-stu-id="918fe-143">Since Hadoop is a distributed system, any script used must be placed on all nodes in hello cluster that hello service runs on.</span></span>
> 
> <span data-ttu-id="918fe-144">hello skriptet måste också vara på en plats som kan nås av hello konto hello-tjänsten körs som och måste innehålla körbehörighet.</span><span class="sxs-lookup"><span data-stu-id="918fe-144">hello script must also be in a location that is accessible by hello account hello service runs as, and must provide execute permissions.</span></span> <span data-ttu-id="918fe-145">Till exempel gärna toostore skript i `/usr/local/bin` och använda `chmod go+rx /usr/local/bin/filename.sh` toogrant Läs- och körningsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="918fe-145">For example, you may wish toostore scripts in `/usr/local/bin` and use `chmod go+rx /usr/local/bin/filename.sh` toogrant read and execute permissions.</span></span>

## <a name="using-ambari"></a><span data-ttu-id="918fe-146">Med Ambari</span><span class="sxs-lookup"><span data-stu-id="918fe-146">Using Ambari</span></span>

<span data-ttu-id="918fe-147">toomodify hello konfiguration för en tjänst, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="918fe-147">toomodify hello configuration for a service, use hello following steps:</span></span>

1. <span data-ttu-id="918fe-148">Öppna hello Ambari-webbgränssnittet för klustret.</span><span class="sxs-lookup"><span data-stu-id="918fe-148">Open hello Ambari web UI for your cluster.</span></span> <span data-ttu-id="918fe-149">hello-URL är https://YOURCLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="918fe-149">hello URL is https://YOURCLUSTERNAME.azurehdinsight.net.</span></span>

    <span data-ttu-id="918fe-150">När du uppmanas att autentisera toohello plats med hello HTTP kontonamn (standard: admin) och lösenord för klustret.</span><span class="sxs-lookup"><span data-stu-id="918fe-150">When prompted, authenticate toohello site using hello HTTP account name (default: admin) and password for your cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="918fe-151">Du kan uppmanas en gång av Ambari hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="918fe-151">You may be prompted a second time by Ambari for hello user name and password.</span></span> <span data-ttu-id="918fe-152">I så fall, ange hello samma kontonamn och lösenord</span><span class="sxs-lookup"><span data-stu-id="918fe-152">If so, enter hello same account name and password</span></span>

2. <span data-ttu-id="918fe-153">Markera med hello lista över hello vänster hello service område som du vill toomodify.</span><span class="sxs-lookup"><span data-stu-id="918fe-153">Using hello list of on hello left, select hello service area you want toomodify.</span></span> <span data-ttu-id="918fe-154">Till exempel **HDFS**.</span><span class="sxs-lookup"><span data-stu-id="918fe-154">For example, **HDFS**.</span></span> <span data-ttu-id="918fe-155">Välj hello i hello center **konfigurationerna** fliken.</span><span class="sxs-lookup"><span data-stu-id="918fe-155">In hello center area, select hello **Configs** tab.</span></span>

    ![Bild av Ambari web HDFS konfigurationerna fliken](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. <span data-ttu-id="918fe-157">Med hjälp av hello **Filter...**  posten, ange **bort**.</span><span class="sxs-lookup"><span data-stu-id="918fe-157">Using hello **Filter...** entry, enter **opts**.</span></span> <span data-ttu-id="918fe-158">Endast objekt som innehåller den här texten visas.</span><span class="sxs-lookup"><span data-stu-id="918fe-158">Only items containing this text are displayed.</span></span>

    ![Filtrerad lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. <span data-ttu-id="918fe-160">Hitta hello  **\* \_OPTS** post för Hej tjänst du vill tooenable heap Dumpar för och lägga till hello alternativen du vill tooenable.</span><span class="sxs-lookup"><span data-stu-id="918fe-160">Find hello **\*\_OPTS** entry for hello service you want tooenable heap dumps for, and add hello options you wish tooenable.</span></span> <span data-ttu-id="918fe-161">I följande bild hello, jag har lagt till `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** post:</span><span class="sxs-lookup"><span data-stu-id="918fe-161">In hello following image, I've added `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** entry:</span></span>

    ![HADOOP_NAMENODE_OPTS med - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > <span data-ttu-id="918fe-163">När du aktiverar heap Dumpar för hello mappa eller minska underordnad process, Sök efter hello-fält med namnet **mapreduce.admin.map.child.java.opts** och **mapreduce.admin.reduce.child.java.opts**.</span><span class="sxs-lookup"><span data-stu-id="918fe-163">When enabling heap dumps for hello map or reduce child process, look for hello fields named **mapreduce.admin.map.child.java.opts** and **mapreduce.admin.reduce.child.java.opts**.</span></span>

    <span data-ttu-id="918fe-164">Använd hello **spara** knappen toosave hello ändras.</span><span class="sxs-lookup"><span data-stu-id="918fe-164">Use hello **Save** button toosave hello changes.</span></span> <span data-ttu-id="918fe-165">Du kan ange en kort anteckning om hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="918fe-165">You can enter a short note describing hello changes.</span></span>

5. <span data-ttu-id="918fe-166">När hello ändringarna har tillämpats hello **omstart krävs** ikon visas bredvid en eller flera tjänster.</span><span class="sxs-lookup"><span data-stu-id="918fe-166">Once hello changes have been applied, hello **Restart required** icon appears beside one or more services.</span></span>

    ![Starta om nödvändiga ikon och starta om knappen](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. <span data-ttu-id="918fe-168">Markera varje tjänst som måste startas om och Använd hello **tjänståtgärder** knappen för**aktivera på underhållsläge**.</span><span class="sxs-lookup"><span data-stu-id="918fe-168">Select each service that needs a restart, and use hello **Service Actions** button too**Turn On Maintenance Mode**.</span></span> <span data-ttu-id="918fe-169">Underhållsläge förhindrar att aviseringar som genereras från hello-tjänsten när du startar om.</span><span class="sxs-lookup"><span data-stu-id="918fe-169">Maintenance mode prevents alerts from being generated from hello service when you restart it.</span></span>

    ![Aktivera menyn för underhåll läge](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. <span data-ttu-id="918fe-171">När du har aktiverat underhållsläget använda hello **starta om** knappen för hello-tjänsten för**starta om alla påverkas**</span><span class="sxs-lookup"><span data-stu-id="918fe-171">Once you have enabled maintenance mode, use hello **Restart** button for hello service too**Restart All Effected**</span></span>

    ![Starta om alla berörda post](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > <span data-ttu-id="918fe-173">Hej poster för hello **starta om** knappen kan vara olika för andra tjänster.</span><span class="sxs-lookup"><span data-stu-id="918fe-173">hello entries for hello **Restart** button may be different for other services.</span></span>

8. <span data-ttu-id="918fe-174">När hello tjänster har startats om, kan du använda hello **tjänståtgärder** knappen för**aktivera inaktivera underhållsläge**.</span><span class="sxs-lookup"><span data-stu-id="918fe-174">Once hello services have been restarted, use hello **Service Actions** button too**Turn Off Maintenance Mode**.</span></span> <span data-ttu-id="918fe-175">Den här Ambari tooresume övervakning för aviseringar för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="918fe-175">This Ambari tooresume monitoring for alerts for hello service.</span></span>

