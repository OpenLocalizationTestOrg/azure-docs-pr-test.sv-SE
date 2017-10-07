---
title: "aaaMicrosoft kognitiva Toolkit med Azure HDInsight Spark för djup learning | Microsoft Docs"
description: "Information om hur en tränad Microsoft kognitiva Toolkit djup learning modellen kan vara tillämpade tooa dataset med hello Spark Python API i ett Azure HDInsight Spark-kluster."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a>Använd Microsoft kognitiva Toolkit djup Lär modell med Azure HDInsight Spark-kluster

Du hello följande steg i den här artikeln.

1. Köra ett anpassat skript tooinstall Microsoft kognitiva Toolkit på ett Azure HDInsight Spark-kluster.

2. Överför en Jupyter-anteckningsbok toohello Spark-kluster toosee hur tooapply en tränad Microsoft kognitiva Toolkit djup learning modellen toofiles i ett Azure Blob Storage-konto med hjälp av hello [Spark Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)

## <a name="prerequisites"></a>Krav

* **En Azure-prenumeration**. Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration. Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).

* **Azure HDInsight Spark-kluster**. Skapa ett Spark 2.0-kluster i den här artikeln. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="how-does-this-solution-flow"></a>Hur går den här lösningen?

Den här lösningen delas mellan den här artikeln och en Jupyter-anteckningsbok som du överför som en del av den här kursen. Du har slutfört hello följa stegen i den här artikeln:

* Kör en skriptåtgärd på ett HDInsight Spark-kluster tooinstall Microsoft kognitiva Toolkit och Python-paket.
* Överför hello Jupyter-anteckningsbok som kör hello lösning toohello HDInsight Spark-kluster.

hello beskrivs följande stegen i hello Jupyter-anteckningsbok.

- Läsa in exempel bilder i en distribuerad Spark Resiliant Dataset eller RDD
   - Läsa in moduler och definiera förinställningar
   - Hämta hello dataset lokalt på hello Spark-kluster
   - Konvertera hello dataset till en RDD
- Poäng hello bilder med hjälp av en trained model kognitiva Toolkit
   - Hämta hello tränats kognitiva Toolkit modellen toohello Spark-kluster
   - Definiera funktioner toobe som används av arbetsnoder
   - Poäng hello bilder på arbetsnoder
   - Utvärdera modellen noggrannhet


## <a name="install-microsoft-cognitive-toolkit"></a>Installera Microsoft kognitiva Toolkit

Du kan installera Microsoft kognitiva Toolkit på ett Spark-kluster med skriptåtgärder. Skriptåtgärder använder anpassade skript tooinstall komponenter på hello-kluster som inte är tillgängliga som standard. Du kan använda hello anpassat skript från hello Azure-portalen genom att använda HDInsight .NET SDK eller med hjälp av Azure PowerShell. Du kan också använda hello skriptet tooinstall hello toolkit antingen som en del av klustret har skapats eller när hello klustret är igång. 

I den här artikeln använder vi hello portal tooinstall hello toolkit efter hello klustret har skapats. Andra sätt toorun hello anpassade skript, se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).

### <a name="using-hello-azure-portal"></a>Med hjälp av hello Azure-portalen

Anvisningar för hur toouse hello Azure Portal toorun skript åtgärd finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Kontrollera att du anger hello följande indata tooinstall Microsoft kognitiva Toolkit.

* Ange ett värde för hello Skriptnamn för åtgärden.

* För **Bash skript URI**, ange `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.

* Kontrollera att du kör skriptet hello endast på hello head- och arbetsroller noder och avmarkera alla hello andra kryssrutorna.

* Klicka på **Skapa**.

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a>Överför hello Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster

toouse hello Microsoft kognitiva Toolkit med hello Azure HDInsight Spark-kluster, måste du ladda hello Jupyter-anteckningsbok **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark-kluster. Den här anteckningsboken finns på GitHub på [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).

1. Klona hello GitHub-lagringsplatsen [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration). Läs instruktionerna tooclone [kloning av en databas](https://help.github.com/articles/cloning-a-repository/).

2. Hello Azure-portalen, öppna hello Spark-klusterbladet som du redan etablerats, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.

    Du kan också starta hello Jupyter-anteckningsbok genom att gå toohello URL `https://<clustername>.azurehdinsight.net/jupyter/`. Ersätt \<klusternamn > med hello namnet på ditt HDInsight-kluster.

3. Hej Jupyter-anteckningsbok, klicka på **överför** i hello övre högra hörnet och gå sedan toohello plats där du har klonat hello GitHub-lagringsplatsen.

    ![Överför Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "överför Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster")

4. Klicka på **överför** igen.

5. När hello anteckningsboken har överförts klickar hello namnet på hello anteckningsboken och följ sedan instruktionerna för hello i hello anteckningsbok själva på hur tooload hello datauppsättning och utföra hello kursen.

## <a name="see-also"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [Application Insight-telemetridataanalys i HDInsight](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
