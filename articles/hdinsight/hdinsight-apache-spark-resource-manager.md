---
title: "aaaManage resurser för Apache Spark-kluster i Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hantera resurser för Spark-kluster i Azure HDInsight för bättre prestanda."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9da7d4e3-458e-4296-a628-77b14643f7e4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: e18682a24f77494db884105f9db03c0a350ddad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Hantera resurser för Apache Spark-kluster i Azure HDInsight 

I den här artikeln du lära dig hur tooaccess hello gränssnitt som Ambari UI, YARN-Användargränssnittet och hello Spark historik Server som är associerade med Spark-kluster. Du kommer även information om hur tootune hello klusterkonfiguration för optimala prestanda.

**Krav:**

Du måste ha hello följande:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-do-i-launch-hello-ambari-web-ui"></a>Hur jag för att starta hello Ambari-Webbgränssnittet?
1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.
2. Hello Spark-klusterbladet, klicka på **instrumentpanelen**. När du uppmanas ange hello administratörsautentiseringsuppgifter för hello Spark-kluster.

    ![Starta Ambari](./media/hdinsight-apache-spark-resource-manager/hdinsight-launch-cluster-dashboard.png "starta hanteraren för filserverresurser")
3. Detta ska starta hello Ambari-Webbgränssnittet, enligt nedan.

    ![Ambari-webbgränssnittet](./media/hdinsight-apache-spark-resource-manager/ambari-web-ui.png "Ambari-webbgränssnittet")   

## <a name="how-do-i-launch-hello-spark-history-server"></a>Hur jag för att starta hello Spark historik Server?
1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).
2. Från hello klustret bladet under **snabblänkar**, klickar du på **Klusterinstrumentpanel**. I hello **Klusterinstrumentpanel** bladet, klickar du på **Spark historik Server**.

    ![Väck historik Server](./media/hdinsight-apache-spark-resource-manager/launch-history-server.png "Väck historik Server")

    När du uppmanas ange hello administratörsautentiseringsuppgifter för hello Spark-kluster.

## <a name="how-do-i-launch-hello-yarn-ui"></a>Hur jag för att starta hello Yarn-Användargränssnittet?
Du kan använda hello YARN-Användargränssnittet toomonitor program som körs på hello Spark-kluster.

1. Hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **YARN**.

    ![Starta YARN-Användargränssnittet](./media/hdinsight-apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > Alternativt kan du starta hello YARN-Användargränssnittet från hello Ambari UI. toolaunch hello Ambari UI från hello kluster-bladet, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **instrumentpanelen för HDInsight-klustret**. Hello Ambari UI, klicka på **YARN**, klickar du på **snabblänkar**, klicka på hello active resource manager och klicka sedan på **ResourceManager UI**.
   >
   >

## <a name="what-is-hello-optimum-cluster-configuration-toorun-spark-applications"></a>Vad är hello optimala klustret configuration toorun Spark-program?
hello tre viktiga parametrar som kan användas för konfiguration av Spark beroende på kraven för application är `spark.executor.instances`, `spark.executor.cores`, och `spark.executor.memory`. En utförare är en process startas för ett Spark-program. Det körs på arbetsnoden hello och är ansvarig toocarry hello uppgifter för hello program. hello standardantalet executors och hello utföraren storlekar för varje kluster beräknas baserat på hello antalet arbetarnoder och hello worker nodstorlek. Dessa lagras i `spark-defaults.conf` på hello head klusternoder.

hello tre konfigurationsparametrar kan konfigureras på hello klusternivå (för alla program som körs på klustret hello) eller kan anges för varje enskilt program.

### <a name="change-hello-parameters-using-ambari-ui"></a>Ändra hello parametrar med Ambari UI
1. Hej Ambari UI klickar du på **Spark**, klickar du på **konfigurationerna**, och expandera sedan **anpassad spark-standarder**.

    ![Ange parametrar med Ambari](./media/hdinsight-apache-spark-resource-manager/set-parameters-using-ambari.png)
2. hello standardvärdena är bra toohave 4 Spark-program som körs samtidigt på hello klustret. Du kan ändringar värdena från hello användargränssnitt som visas nedan.

    ![Ange parametrar med Ambari](./media/hdinsight-apache-spark-resource-manager/set-executor-parameters.png)
3. Klicka på **spara** toosave hello konfigurationsändringar. Överst hello på hello sidan, uppmanas du att alla hello toorestart tjänster som påverkas. Klicka på **starta om**.

    ![Starta om tjänsterna](./media/hdinsight-apache-spark-resource-manager/restart-services.png)

### <a name="change-hello-parameters-for-an-application-running-in-jupyter-notebook"></a>Ändra hello parametrar för ett program som körs i Jupyter-anteckningsbok
För program som körs i hello Jupyter-anteckningsbok, kan du använda hello `%%configure` magiska toomake hello konfigurationsändringar. Vi rekommenderar måste du göra dessa ändringar hello början av hello program innan du kör din första kodcellen. Detta säkerställer att hello-konfigurationen är tillämpade toohello Livius sessionen när den skapas. Om du vill att toochange hello konfiguration i ett senare skede hello program måste du använda hello `-f` parameter. Men genom att göra så att alla vidare i hello går programmet förlorade.

hello kodfragmentet nedan visar hur toochange hello konfigurationen för ett program som körs i Jupyter.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Konfigurationsparametrar måste överföras i som en JSON-sträng och måste vara hello nästa rad efter hello magic enligt hello exempel kolumn.

### <a name="change-hello-parameters-for-an-application-submitted-using-spark-submit"></a>Ändra hello parametrar för ett program som skickas med skicka spark-
Följande kommando är ett exempel på hur toochange hello konfigurationsparametrar för ett batch-program som har skickats med `spark-submit`.

    spark-submit --class <hello application class tooexecute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-hello-parameters-for-an-application-submitted-using-curl"></a>Ändra hello parametrar för ett program som skickas med cURL
Följande kommando är ett exempel på hur toochange hello konfigurationsparametrar för ett batch-program som har skickats med hjälp av cURL.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<hello application class tooexecute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="how-do-i-change-these-parameters-on-a-spark-thrift-server"></a>Hur ändrar parametrarna på en Spark Thrift-servern?
Spark Thrift-servern tillhandahåller JDBC/ODBC-åtkomst tooa Spark-kluster och har använt tooservice Spark SQL-frågor. Verktyg som Power BI Tableau osv. använda ODBC-protokollet toocommunicate med Spark Thrift-servern tooexecute Spark SQL-frågor som ett Spark-program. När ett Spark-kluster skapas två instanser av hello Spark Thrift-servern är igång, en på varje huvudnod. Varje Spark Thrift-servern visas som ett Spark-program i hello YARN-Användargränssnittet.

Spark Thrift-servern använder Väck dynamiska utförare allokering och därför hello `spark.executor.instances` används inte. Spark Thrift-servern används i stället `spark.dynamicAllocation.minExecutors` och `spark.dynamicAllocation.maxExecutors` toospecify hello utföraren count. Hej konfigurationsparametrar `spark.executor.cores` och `spark.executor.memory` är används toomodify hello utföraren storlek. Du kan ändra parametrarna som visas nedan.

* Expandera hello **avancerade spark-thrift-sparkconf** kategori tooupdate hello parametrar `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, och `spark.executor.memory`.

    ![Konfigurera Spark thrift-servern](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-1.png)    
* Expandera hello **anpassad spark-thrift-sparkconf** kategori tooupdate hello parametern `spark.executor.cores`.

    ![Konfigurera Spark thrift-servern](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="how-do-i-change-hello-driver-memory-of-hello-spark-thrift-server"></a>Hur ändrar jag hello drivrutinsminne hello Spark Thrift-servern?
Spark Thrift serverminne drivrutinen tillhandahålls konfigurerade too25% av hello huvudnod RAM-storlek, hello totalt RAM-minne hello huvudnod är större än 14GB. Du kan använda hello Ambari UI minneskonfigurationen toochange hello drivrutin, enligt nedan.

* Hej Ambari UI klickar du på **Spark**, klickar du på **konfigurationerna**, expandera **avancerade spark-env**, och ange sedan hello värde för **spark_thrift_cmd_opts**.

    ![Konfigurera Spark thrift-servern RAM-minne](./media/hdinsight-apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="i-do-not-use-bi-with-spark-cluster-how-do-i-take-hello-resources-back"></a>Jag använder inte BI med Spark-kluster. Hur jag återta hello resurser?
Eftersom vi använder Spark dynamisk allokering är hello resurser som förbrukas av thrift-servern hello resurser för hello två program huvudservrar. dessa resurser måste du stoppa hello Thrift-servertjänster som körs på klustret hello tooreclaim.

1. Hej Ambari UI från hello till vänster och klicka på **Spark**.
2. Klicka på nästa sida hello **Spark Thrift servrar**.

    ![Starta om thrift-servern](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-1.png)
3. Du bör se hello två headnodes vilka hello Spark Thrift-servern körs. Klicka på en hello headnodes.

    ![Starta om thrift-servern](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-2.png)
4. hello nästa sida visas alla hello-tjänster som körs på den headnode. Klicka på hello nedrullningsbara knappen Nästa tooSpark Thrift-servern hello listan och klicka sedan på **stoppa**.

    ![Starta om thrift-servern](./media/hdinsight-apache-spark-resource-manager/restart-thrift-server-3.png)
5. Upprepa dessa steg på hello samt andra headnode.

## <a name="my-jupyter-notebooks-are-not-running-as-expected-how-can-i-restart-hello-service"></a>Jupyter-anteckningsböcker körs inte som förväntat. Hur kan jag starta om hello tjänsten?
Starta hello Ambari-Webbgränssnittet som ovan. Hello vänstra navigationsfönstret klickar du på **Jupyter**, klickar du på **tjänståtgärder**, och klicka sedan på **starta om alla**. Detta startar hello Jupyter-tjänsten på alla hello headnodes.

    ![Restart Jupyter](./media/hdinsight-apache-spark-resource-manager/restart-jupyter.png "Restart Jupyter")

## <a name="how-do-i-know-if-i-am-running-out-of-resources"></a>Hur vet jag om jag har slut på resurser?
Starta hello Yarn-Användargränssnittet som du ser ovan. Kontrollera värdena för i klustret mått tabell ovanpå hello-skärmen, **minne används** och **minne totalt** kolumner. Om hello 2 värdena är mycket nära, kanske det inte finns tillräckligt med resurser toostart hello nästa program. hello samma gäller toohello **VCores används** och **VCores totalt** kolumner. Dessutom i hello huvudsakliga visas om det finns ett program svaret inte i **godkända** tillstånd och inte att omvandla till **kör** eller **misslyckades** tillstånd, detta kan också vara ett tecken att det blir inte tillräckligt med resurser toostart.

    ![Resource Limit](./media/hdinsight-apache-spark-resource-manager/resource-limit.png "Resource Limit")

## <a name="how-do-i-kill-a-running-application-toofree-up-resource"></a>Hur avsluta en körs programmet toofree resurs?
1. I hello Yarn-Användargränssnittet från hello vänstra panelen, klickar du på **kör**. Hello listan över program som körs och fastställa hello programmet toobe avslutats och klicka på hello **ID**.

    ![Avsluta App1](./media/hdinsight-apache-spark-resource-manager/kill-app1.png "Kill App1")

2. Klicka på **avsluta programmet** på hello längst upp till höger Klicka **OK**.

    ![Avsluta App2](./media/hdinsight-apache-spark-resource-manager/kill-app2.png "Kill App2")

## <a name="see-also"></a>Se även
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>För dataanalytiker

* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight-telemetridataanalys i HDInsight](hdinsight-spark-analyze-application-insight-logs.md)
* [Använda Caffe för distribuerade djup learning på Azure HDInsight Spark](hdinsight-deep-learning-caffe-spark.md)

### <a name="for-spark-developers"></a>För Spark-utvecklare

* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)
