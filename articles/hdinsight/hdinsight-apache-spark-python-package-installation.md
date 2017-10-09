---
title: "aaaScript åtgärden - installera Python-paket med Jupyter på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur toouse skript åtgärd tooconfigure Jupyter-anteckningsböcker med HDInsight Spark-kluster toouse externa python-paket."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Använd skriptåtgärder tooinstall externa Python-paket för Jupyter notebooks i Apache Spark-kluster i HDInsight
> [!div class="op_single_selector"]
> * [Med hjälp av cell Magiskt tal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Med skriptåtgärder](hdinsight-apache-spark-python-package-installation.md)
>
>

Lär dig hur toouse skriptåtgärder tooconfigure ett Apache Spark-kluster i HDInsight (Linux) toouse externa community-bidragit **python** paket som inte är inkluderat out box i hello kluster.

> [!NOTE]
> Du kan också konfigurera en Jupyter-anteckningsbok med hjälp av `%%configure` magiska toouse externa paket. Instruktioner finns i [använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

Du kan söka hello [paketindexet](https://pypi.python.org/pypi) hello fullständig lista över paket som är tillgängliga. Du kan också hämta en lista över tillgängliga paket från andra källor. Du kan till exempel installera paket som är tillgängliga via [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) eller [conda bedömningar](https://conda-forge.github.io/feedstocks.html).

I den här artikeln får du lära dig hur tooinstall hello [TensorFlow](https://www.tensorflow.org/) paketet med hjälp av skript Actoin på klustret och använda den via hello Jupyter-anteckningsbok.

## <a name="prerequisites"></a>Krav
Du måste ha hello följande:

* En Azure-prenumeration. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

   > [!NOTE]
   > Om du inte redan har ett Spark-kluster i HDInsight Linux kan du köra skriptåtgärder när klustret skapas. Besök hello dokumentationen om [hur toouse anpassade skript åtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a>Använda externa paket med Jupyter-anteckningsböcker

1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   

2. Hello Spark-klusterbladet, klicka på **skriptåtgärder** under **användning**. Kör hello anpassad åtgärd som installerar TensorFlow i hello huvudnoderna och hello arbetsnoder. hello bash-skript kan refereras från: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh besök hello dokumentation om [hur toouse anpassade skript åtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Det finns två python installationer i hello kluster. Spark använder hello Anaconda python installationen finns på `/usr/bin/anaconda/bin`. Referera till den installationen i din anpassade åtgärder via `/usr/bin/anaconda/bin/pip` och `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Öppna en PySpark Jupyter-anteckningsbok

    ![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")

4. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.

    ![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "ange ett namn för anteckningsboken hello")

5. Du kommer nu `import tensorflow` och kör ett hello world-exempel. 

    Koden toocopy:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    hello resultatet ser ut så här:
    
    ![TensorFlow kodkörning](./media/hdinsight-apache-spark-python-package-installation/execution.png "köra TensorFlow kod")



## <a name="seealso"></a>Se även
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
* [Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
