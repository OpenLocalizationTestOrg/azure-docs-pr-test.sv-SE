---
title: "aaaUse anpassade Maven-paket med Jupyter i Spark på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur tooconfigure Jupyter-anteckningsböcker med HDInsight Spark-kluster toouse anpassade Maven-paket."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight
> [!div class="op_single_selector"]
> * [Med hjälp av cell Magiskt tal](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Med skriptåtgärder](hdinsight-apache-spark-python-package-installation.md)
>
>

Lär dig hur tooconfigure en Jupyter-anteckningsbok i Apache Spark-kluster i HDInsight toouse externa community-bidragit **maven** paket som inte är inkluderat out box i hello kluster. 

Du kan söka hello [Maven databasen](http://search.maven.org/) hello fullständig lista över paket som är tillgängliga. Du kan också hämta en lista över tillgängliga paket från andra källor. Till exempel en fullständig lista över community-bidragit paket finns på [Spark paket](http://spark-packages.org/).

I den här artikeln får du lära dig hur toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paket med Jupyter-anteckningsbok hello.



## <a name="prerequisites"></a>Krav
Du måste ha hello följande:

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Använda externa paket med Jupyter-anteckningsböcker
1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   
2. Hello Spark-klusterbladet, klicka på **snabblänkar**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.

    > [!NOTE]
    > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. Skapa en ny anteckningsbok. Klicka på **ny**, och klicka sedan på **Spark**.
   
    ![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")

4. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.
   
    ![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "ange ett namn för anteckningsboken hello")

5. Du kommer att använda hello `%%configure` magiskt tooconfigure hello anteckningsboken toouse ett externa paket. Kontrollera att du anropa hello i bärbara datorer som använder externa paket, `%%configure` magiskt i hello första kodcellen. Detta säkerställer att hello kernel konfigurerade toouse hello paketet innan du startar hello-sessionen.

    >[!IMPORTANT] 
    >Om du glömmer tooconfigure hello kernel i hello första cellen kan du använda hello `%%configure` med hello `-f` parameter, men som kommer att startas om hello sessionen och alla pågår kommer att förloras.

    | HDInsight-version | Kommando |
    |-------------------|---------|
    |För HDInsight 3.3 och HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | För HDInsight 3.5 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. hello fragment ovan förväntar hello maven koordinaterna för hello externa paket i Maven centrallager. I det här kodstycket `com.databricks:spark-csv_2.10:1.4.0` är hello maven koordinaten för **spark-csv** paketet. Här är hur du skapar hello koordinater för ett paket.
   
    a. Leta upp hello paketet i hello Maven-databasen. Den här självstudiekursen kommer vi att använda [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Samla in hello värden för från hello databasen **GroupId**, **artefakt-ID**, och **Version**. Se till att hello-värden som du samlar in matchar ditt kluster. I det här fallet använder du en Scala 2.10 och Spark 1.4.0 paketet, men du kanske måste tooselect olika versioner för hello Scala eller Spark-version i klustret. Du kan hitta hello Scala version på ditt kluster genom att köra `scala.util.Properties.versionString` på hello Spark Jupyter kernel eller skicka Spark. Du kan hitta hello Spark-version på ditt kluster genom att köra `sc.version` i Jupyter-anteckningsböcker.
   
    ![Använda externa paket med Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "använda externa paket med Jupyter-anteckningsbok")
   
    c. Sammanfoga hello tre värden, avgränsade med kolon (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

7. Kör hello kodcellen med hello `%%configure` Magiskt tal. Detta konfigurerar hello underliggande Livius session toouse hello paket du angett. I hello efterföljande celler i hello anteckningsbok använda du nu hello paketet, enligt nedan.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. Sedan kan du köra hello kodavsnitt, som visas nedan, tooview hello data från hello dataframe du skapade i föregående steg i hello.
   
        df.show()
   
        df.select("Time").count()

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

* [Använda externa python-paket med Jupyter notebooks i Apache Spark-kluster i HDInsight Linux](hdinsight-apache-spark-python-package-installation.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)

