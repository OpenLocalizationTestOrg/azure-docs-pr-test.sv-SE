---
title: "aaaAzure HDInsight felsökning guider | Microsoft Docs"
description: "Felsöka Hadoop-arbetsbelastningar med Azure HDInsight. Steg för steg-dokumentationen visar hur toouse HDInsight toosolve vanliga problem med Hive, Spark, YARN, HBase, HDFS och Storm."
services: hdinsight
author: arijitt
manager: arijitt
layout: LandingPage
ms.assetid: 
ms.service: hdinsight
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing - page
ms.date: 7/06/2017
ms.author: arijitt
ms.openlocfilehash: 6d801389cba738345b36364302c62008b8318018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-by-using-azure-hdinsight"></a>Felsöka med Azure HDInsight

| Apache arbetsbelastning | Övre frågor |
|---|---|
|![HBase](./media/hdinsight-troubleshoot-guide/HBASE.png)<br>[Felsöka HBase](hdinsight-troubleshoot-hbase.md)|<br>[Hur kör hbck kommandot rapporter med flera otilldelade regioner](hdinsight-troubleshoot-hbase.md#how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions)<br><br>[Hur kan jag åtgärda timeout problem när du använder hbck kommandon för region tilldelningar](hdinsight-troubleshoot-hbase.md#how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments)<br><br>[Hur jag force-inaktivera HDFS felsäkert läge i ett kluster](hdinsight-troubleshoot-hbase.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)<br><br>[Hur kan jag åtgärda JDBC eller SQLLine anslutningen problem med Apache Phoenix](hdinsight-troubleshoot-hbase.md#how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix)<br><br>[Vad som orsakar en huvudserver toofail toostart](hdinsight-troubleshoot-hbase.md#what-causes-a-master-server-to-fail-to-start)<br><br>[Vad som orsakar en omstart av fel på en region-server](hdinsight-troubleshoot-hbase.md#what-causes-a-restart-failure-on-a-region-server)|
|![HDFS](./media/hdinsight-troubleshoot-guide/HDFS.png)<br>[Felsöka HDFS](hdinsight-troubleshoot-hdfs.md)|<br>[Hur kommer jag åt en lokala HDFS från i ett kluster](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[Hur jag force-inaktivera HDFS felsäkert läge i ett kluster](hdinsight-troubleshoot-hdfs.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)|
|![Hive](./media/hdinsight-troubleshoot-guide/HIVE.png)<br>[Felsöka HBase](hdinsight-troubleshoot-hive.md)|<br>[Hur gör exportera en Hive metastore och importera det till ett annat kluster](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[Hur jag för att hitta Hive loggar på ett kluster](hdinsight-troubleshoot-hive.md#how-do-i-locate-hive-logs-on-a-cluster)<br><br>[Hur jag för att starta hello Hive-gränssnittet med specifika konfigurationer i ett kluster](hdinsight-troubleshoot-hive.md#how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[Hur jag för att analysera Tez DAG data på ett kluster kritiska](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[Hur hämta Tez DAG data från ett kluster](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|![Spark](./media/hdinsight-troubleshoot-guide/SPARK.png)<br>[Felsöka Spark](hdinsight-troubleshoot-SPARK.md)|<br>[Hur konfigurerar jag ett Spark-program genom att använda Ambari på kluster](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-ambari-on-clusters)<br><br>[Hur konfigurerar jag ett Spark-program med hjälp av en Jupyter-anteckningsbok på kluster](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters)<br><br>[Hur konfigurerar jag ett Spark-program med hjälp av Livius på kluster](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-livy-on-clusters)<br><br>[Hur konfigurerar ett program med hjälp av spark-skicka Spark på kluster](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters)<br><br>[Vad som orsakar en Spark OutOfMemoryError undantag](hdinsight-troubleshoot-spark.md#what-causes-a-spark-application-outofmemoryerror-exception)|
|![Storm](./media/hdinsight-troubleshoot-guide/STORM.png)<br>[Felsöka Storm](hdinsight-troubleshoot-STORM.md)|<br>[Hur kommer jag åt hello Storm-Användargränssnittet på ett kluster](hdinsight-troubleshoot-storm.md#how-do-i-access-the-storm-ui-on-a-cluster)<br><br>[Hur överföra Storm hubb kanal kontrollpunkt händelseinformation från en topologi tooanother](hdinsight-troubleshoot-storm.md#how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[Hur jag för att hitta Storm binärfiler i ett kluster](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[Hur vet hello topologi för distribution av ett Storm-kluster](hdinsight-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[Hur jag för att hitta Storm event hub kanal binärfiler för utveckling](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-event-hub-spout-binaries-for-development)<br><br>[Hur jag för att hitta Storm Log4J konfigurationsfiler på kluster](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-log4j-configuration-files-on-clusters)|
|![YARN](./media/hdinsight-troubleshoot-guide/YARN.png)<br>[Felsöka YARN](hdinsight-troubleshoot-YARN.md)|<br>[Hur skapar jag en ny YARN-kö på ett kluster](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[Hur hämta YARN-loggar från ett kluster](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a>HDInsight felsökningsresurser

| Information om | Se dessa artiklar |
| --- | --- |
| HDInsight på Linux- och optimering | - [Information om hur du använder HDInsight på Linux](hdinsight-hadoop-linux-information.md)<br>- [Hadoop-minne och prestanda felsökning](hdinsight-hadoop-stack-trace-error-messages.md)<br>- [Prestanda för hive-frågor](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/) |
| Loggar och minnesdumpar | - [Åtkomst programloggar Hadoop YARN på Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)<br>- [Aktivera heap Dumpar för Hadoop-tjänster på Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)<br>- [Analysera HDInsight loggar](hdinsight-debug-jobs.md)|
| Fel | - [Förstå och lösa WebHCat-fel](hdinsight-hadoop-templeton-webhcat-debug-errors.md)<br>- [Hive inställningar toofix OutofMemory-fel](hdinsight-hadoop-hive-out-of-memory-error-oom.md) |
| Verktyg | - [Använda Ambari Views toodebug Tez jobb](hdinsight-debug-ambari-tez-view.md)<br>- [Optimera Hive-frågor](hdinsight-hadoop-optimize-hive-query.md) |
