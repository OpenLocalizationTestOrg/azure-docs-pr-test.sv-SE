---
title: aaaTroubleshoot problem med Apache Spark-klustret i Azure HDInsight | Microsoft Docs
description: "Lär dig mer om problem relaterade tooApache Spark-kluster i Azure HDInsight och hur toowork runt de."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 610c4103-ffc8-4ec0-ad06-fdaf3c4d7c10
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 7373b90524ae5dbb10ab8ded593aa38d12c14b55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="known-issues-for-apache-spark-cluster-on-hdinsight"></a>Kända problem med Apache Spark-kluster i HDInsight

Det här dokumentet håller reda på alla hello kända problem för hello HDInsight Spark förhandsversion.  

## <a name="livy-leaks-interactive-session"></a>Livius läcker interaktiva sessionen
När Livius startas (från Ambari eller på grund av tooheadnode 0 virtuella omstart) med en interaktiv session fortfarande lever läcka en interaktiv jobbet session. Nya jobb kan därför fastnat i hello godkända tillstånd och kan inte startas.

**Lösning:**

Använd följande procedur tooworkaround hello problemet hello:

1. SSH i headnode. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

2. Kör hello efter kommandot toofind hello program-ID för hello interaktiva jobb startas via Livius. 
   
        yarn application –list
   
    hello standardnamnen jobbet kommer att Livius om hello-jobb startades med en interaktiv session Livius med inga explicit namn som angetts för hello Livius-session som har startats med Jupyter-anteckningsbok hello jobbnamn startar med remotesparkmagics_ *. 
3. Kör följande kommando tookill hello dessa jobb. 
   
        yarn application –kill <Application ID>

Nya jobb ska börja köras. 

## <a name="spark-history-server-not-started"></a>Spark historik Server inte har startats
Spark historik Server startas inte automatiskt efter ett kluster skapas.  

**Lösning:** 

Starta hello historik server manuellt från Ambari.

## <a name="permission-issue-in-spark-log-directory"></a>Problem i Spark loggkatalogen
När hdiuser skickar ett jobb med spark-submit, det finns ett fel java.io.FileNotFoundException: /var/log/spark/sparkdriver_hdiuser.log (behörighet nekas) och hello drivrutinen inte loggen skapas. 

**Lösning:**

1. Lägga till hdiuser toohello Hadoop grupp. 
2. Ange 777 behörigheter på /var/log/spark när klustret skapas. 
3. Uppdatera hello spark loggplatsen med Ambari toobe en katalog med 777 behörigheter.  
4. Kör spark-skicka som sudo.  

## <a name="spark-phoenix-connector-is-not-supported"></a>Spark Phoenix anslutningen stöds inte

Hello Spark Phoenix anslutningen stöds för närvarande inte med ett HDInsight Spark-kluster.

**Lösning:**

I stället måste du använda hello Spark-HBase-anslutningen. Instruktioner finns i [hur toouse Spark HBase connector](https://blogs.msdn.microsoft.com/azuredatalake/2016/07/25/hdinsight-how-to-use-spark-hbase-connector/).

## <a name="issues-related-toojupyter-notebooks"></a>Problem med tooJupyter bärbara datorer
Följande är några kända problem relaterade tooJupyter bärbara datorer.

### <a name="notebooks-with-non-ascii-characters-in-filenames"></a>Bärbara datorer med icke-ASCII-tecken i filnamn
Jupyter-anteckningsböcker som kan användas i HDInsight Spark-kluster ska inte ha icke-ASCII-tecken i filnamn. Om du försöker tooupload en fil genom hello Jupyter UI som har ett icke-ASCII-filnamn, kommer inte tyst (som är Jupyter kan du inte överföra hello-fil, men det kommer inte att utlösa visas fel antingen). 

### <a name="error-while-loading-notebooks-of-larger-sizes"></a>Fel vid inläsning av bärbara datorer av större storlek
Ett fel kan uppstå  **`Error loading notebook`**  när du läser in anteckningsböcker som är större.  

**Lösning:**

Om du får det här felet kan innebär det inte dina data är skadad eller förlorade.  Dina anteckningsböcker finns kvar på disken i `/var/lib/jupyter`, och du kan SSH till hello klustret tooaccess dem. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

När du har anslutit toohello kluster med SSH kopiera du dina anteckningsböcker från ditt kluster tooyour lokal dator (med SCP eller WinSCP) som en säkerhetskopiering tooprevent hello förlust av viktiga data i hello anteckningsbok. Du kan sedan SSH-tunnel i din headnode på port 8001 tooaccess Jupyter utan att gå via hello gateway.  Därifrån kan du rensa hello utdata från din bärbara dator och spara om den toominimize hello anteckningsboken storlek.

tooprevent det här felet inträffar i hello framtida, måste du följa några rekommendationer:

* Det är viktigt tookeep hello anteckningsboken storlek små. Alla utdata från Spark-jobb som skickas tillbaka tooJupyter sparas i hello bärbar dator.  Det är bäst med Jupyter i allmänna tooavoid kör `.collect()` på stora RDDS eller dataframes; om du vill toopeek på ett RDD innehållet i stället körs `.take()` eller `.sample()` så att dina utdata inte ger för stort.
* Dessutom när du sparar en bärbar dator, utdata avmarkera alla celler tooreduce hello storlek.

### <a name="notebook-initial-startup-takes-longer-than-expected"></a>Bärbar dator första start tar längre tid än väntat
Den första koden instruktionen i Jupyter-anteckningsbok med Spark magic kan ta mer än en minut.  

**Förklaring:**

Detta beror på att när hello första kodcellen körs. I bakgrunden hello detta startar sessionskonfigurationen och Spark, SQL och Hive sammanhang anges. När dessa kontexter ställs hello första satsen körs och det ger hello intryck av att hello instruktionen tog toocomplete en lång tid.

### <a name="jupyter-notebook-timeout-in-creating-hello-session"></a>Jupyter-anteckningsbok timeout i hello session skapas
När Spark-kluster har slut på resurser, kommer hello Spark och Pyspark-kernel i hello Jupyter-anteckningsbok att tidsgränsen nåddes vid försök toocreate hello session. 

**Åtgärder:** 

1. Fri resurser i Spark-kluster genom att:
   
   * Stoppa andra Spark bärbara datorer genom att gå toohello Stäng och stopp-menyn eller klicka Stäng i hello anteckningsboken explorer.
   * Stoppa andra Spark-program från YARN.
2. Starta om hello anteckningsboken skulle toostart upp. Tillräckligt med resurser ska vara tillgänglig för du toocreate nu en session.

## <a name="see-also"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

