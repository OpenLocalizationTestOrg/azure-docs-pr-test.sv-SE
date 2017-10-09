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
# <a name="enable-heap-dumps-for-hadoop-services-on-linux-based-hdinsight"></a>Aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight

[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heap Dumpar innehåller en ögonblicksbild av hello programmets minne, inklusive hello värdena för variabler som för närvarande hello hello dump har skapats. Så att de är användbara för att diagnostisera problem som uppstår vid körning.

> [!IMPORTANT]
> hello fungerar stegen i det här dokumentet endast med HDInsight-kluster som använder Linux. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="whichServices"></a>Tjänster

Du kan aktivera heap Dumpar för hello följande tjänster:

* **hcatalog** -tempelton
* **hive** -hiveserver2 metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resurshanteraren, nodemanager, timelineserver
* **hdfs** -datanode secondarynamenode, namenode

Du kan också aktivera heap Dumpar för hello kartan och minska processer kördes av HDInsight.

## <a name="configuration"></a>Förstå heap dump konfiguration

Heap Dumpar aktiveras genom att skicka alternativ (ibland kallas bort, eller parametrar) toohello JVM när en tjänst har startats. Du kan ändra dessa alternativ hello shell-skript används toostart hello service toopass för de flesta Hadoop-tjänster.

I varje skript är en export för  **\* \_OPTS**, som innehåller alternativ för hello skickades toohello JVM. Till exempel i hello **hadoop env.sh** skript hello rad som börjar med `export HADOOP_NAMENODE_OPTS=` innehåller hello alternativ för hello NameNode service.

Mappa och minska processer är något annorlunda, eftersom dessa åtgärder är en underordnad process för hello MapReduce-tjänsten. Varje mappa eller minska processen körs i en underordnad behållare och det finns två poster som innehåller hello JVM alternativ. Både i **mapred site.xml**:

* **mapreduce.Admin.Map.child.Java.opts**
* **mapreduce.Admin.Reduce.child.Java.opts**

> [!NOTE]
> Vi rekommenderar med Ambari toomodify båda hello skript och mapred site.xml inställningar som Ambari hantera replikera ändringar på noderna i klustret hello. Se hello [med Ambari](#using-ambari) avsnittet specifika anvisningar.

### <a name="enable-heap-dumps"></a>Aktivera heap dumps

hello kan följande alternativ heap minnesdumpar när en OutOfMemoryError inträffar:

    -XX:+HeapDumpOnOutOfMemoryError

Hej  **+**  anger att det här alternativet är aktiverat. hello standard är inaktiverad.

> [!WARNING]
> Heap Dumpar har inte aktiverats för Hadoop-tjänster på HDInsight standard hello dumpfiler kan vara stora. Om du aktiverar dem för att felsöka, Kom ihåg toodisable dem när du har återskapat hello problemet och insamlade hello dumpfiler.

### <a name="dump-location"></a>Dump plats

hello standardplatsen för hello dumpfilen är hello aktuella arbetskatalogen. Du kan styra om hello filen lagras med hello följande alternativ:

    -XX:HeapDumpPath=/path

Till exempel använder `-XX:HeapDumpPath=/tmp` orsakar hello Dumpar toobe lagras i hello tmp.

### <a name="scripts"></a>Skript

Du kan även utlösa ett skript när en **OutOfMemoryError** inträffar. Till exempel har utlösa en avisering så att du vet att hello-fel uppstått. Använd hello följande alternativ tootrigger ett skript på en __OutOfMemoryError__:

    -XX:OnOutOfMemoryError=/path/to/script

> [!NOTE]
> Eftersom Hadoop är ett distribuerat system, måste alla skript som används placeras på alla noder i klustret för hello som hello-tjänsten körs på.
> 
> hello skriptet måste också vara på en plats som kan nås av hello konto hello-tjänsten körs som och måste innehålla körbehörighet. Till exempel gärna toostore skript i `/usr/local/bin` och använda `chmod go+rx /usr/local/bin/filename.sh` toogrant Läs- och körningsbehörighet.

## <a name="using-ambari"></a>Med Ambari

toomodify hello konfiguration för en tjänst, Använd hello följande steg:

1. Öppna hello Ambari-webbgränssnittet för klustret. hello-URL är https://YOURCLUSTERNAME.azurehdinsight.net.

    När du uppmanas att autentisera toohello plats med hello HTTP kontonamn (standard: admin) och lösenord för klustret.

   > [!NOTE]
   > Du kan uppmanas en gång av Ambari hello användarnamn och lösenord. I så fall, ange hello samma kontonamn och lösenord

2. Markera med hello lista över hello vänster hello service område som du vill toomodify. Till exempel **HDFS**. Välj hello i hello center **konfigurationerna** fliken.

    ![Bild av Ambari web HDFS konfigurationerna fliken](./media/hdinsight-hadoop-heap-dump-linux/serviceconfig.png)

3. Med hjälp av hello **Filter...**  posten, ange **bort**. Endast objekt som innehåller den här texten visas.

    ![Filtrerad lista](./media/hdinsight-hadoop-heap-dump-linux/filter.png)

4. Hitta hello  **\* \_OPTS** post för Hej tjänst du vill tooenable heap Dumpar för och lägga till hello alternativen du vill tooenable. I följande bild hello, jag har lagt till `-XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/` toohello **HADOOP\_NAMENODE\_OPTS** post:

    ![HADOOP_NAMENODE_OPTS med - XX: + HeapDumpOnOutOfMemoryError - XX: = HeapDumpPath/tmp /](./media/hdinsight-hadoop-heap-dump-linux/opts.png)

   > [!NOTE]
   > När du aktiverar heap Dumpar för hello mappa eller minska underordnad process, Sök efter hello-fält med namnet **mapreduce.admin.map.child.java.opts** och **mapreduce.admin.reduce.child.java.opts**.

    Använd hello **spara** knappen toosave hello ändras. Du kan ange en kort anteckning om hello ändringar.

5. När hello ändringarna har tillämpats hello **omstart krävs** ikon visas bredvid en eller flera tjänster.

    ![Starta om nödvändiga ikon och starta om knappen](./media/hdinsight-hadoop-heap-dump-linux/restartrequiredicon.png)

6. Markera varje tjänst som måste startas om och Använd hello **tjänståtgärder** knappen för**aktivera på underhållsläge**. Underhållsläge förhindrar att aviseringar som genereras från hello-tjänsten när du startar om.

    ![Aktivera menyn för underhåll läge](./media/hdinsight-hadoop-heap-dump-linux/maintenancemode.png)

7. När du har aktiverat underhållsläget använda hello **starta om** knappen för hello-tjänsten för**starta om alla påverkas**

    ![Starta om alla berörda post](./media/hdinsight-hadoop-heap-dump-linux/restartbutton.png)

   > [!NOTE]
   > Hej poster för hello **starta om** knappen kan vara olika för andra tjänster.

8. När hello tjänster har startats om, kan du använda hello **tjänståtgärder** knappen för**aktivera inaktivera underhållsläge**. Den här Ambari tooresume övervakning för aviseringar för hello-tjänsten.

