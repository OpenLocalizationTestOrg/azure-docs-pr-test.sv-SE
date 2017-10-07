---
title: "aaaBuild Apache Spark maskininlärning program på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur toobuild Apache Spark maskininlärning program på HDInsight Spark-kluster med Jupyter-anteckningsbok"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f584ca5e-abee-4b7c-ae58-2e45dfc56bf4
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 332bd89876f7ebf178f7573d6018d064edfe9a8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a>Skapa Apache Spark machine learning-program på Azure HDInsight

Lär dig hur toobuild ett Apache Spark machine learning-program med ett Spark-kluster i HDInsight. Den här artikeln visar hur toouse hello Jupyter-anteckningsbok med hello klustret toobuild och testa det här programmet. hello programmet använder hello HVAC.csv exempeldata som är tillgänglig på alla kluster som standard.

**Krav:**

Du måste ha hello följande:

* Ett Apache Spark-kluster i HDInsight. Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="data"></a>Förstå hello datauppsättning
Innan vi börja skapa hello program, låt oss förstå hello datastrukturen hello som vi skapa hello program och hello sorts analys gör vi på hello data. 

I den här artikeln använder vi hello exempel **HVAC.csv** datafil som är tillgängliga i hello Azure Storage-konto som du har associerat med hello HDInsight-kluster. Inom hello lagringskonto hello-filen finns i **\HdiSamples\HdiSamples\SensorSampleData\hvac**. Ladda ned och öppna hello CSV-filen tooget en ögonblicksbild av hello data.  

![Ögonblicksbild av data som används för Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "ögonblicksbild av data som används för Spark machine learning-exempel")

hello data visar hello mål temperatur- och hello faktiska temperaturen för en byggnad som har installerats för HVAC-system. Vi antar hello **System** kolumn representerar hello system-ID och hello **SystemAge** kolumn representerar hello antalet år hello HVAC-system har på plats och hello byggnad.

Vi använder den här toopredict data om en byggnad ska vara högre temperaturer eller colder baserat på hello mål temperatur, en system-ID och system ålder.

## <a name="app"></a>Skriva ett Spark machine learning-program med hjälp av Spark MLlib
Vi använder en Spark ML pipeline tooperform en klassificering av dokument i det här programmet. I hello pipeline vi dela hello dokument i ord, konvertera hello ord till en numerisk funktionen vector och slutligen bygger en förutsägelse-modell med hello funktionen angreppsmetoderna och etiketter. Utför följande steg toocreate hello programmet hello.

1. Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan). Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.   
2. Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**. Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.
   
   > [!NOTE]
   > Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren. Ersätt **KLUSTERNAMN** med hello namnet på klustret:
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. Skapa en ny anteckningsbok. Klicka på **Ny** och sedan på **PySpark**.
   
    ![Skapa en Jupyter-anteckningsbok Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "skapa en Jupyter-anteckningsbok Spark machine learning-exempel")
4. En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb. Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.
   
    ![Ange ett namn för anteckningsboken Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "ange ett namn för anteckningsboken Spark machine learning-exempel")
5. Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit. hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen. Du kan starta genom att importera hello-typer som krävs för det här scenariot. Klistra in hello följande fragment i en tom cell och tryck sedan på **SKIFT + RETUR**. 
   
        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
   
        import os
        import sys
        from pyspark.sql.types import *
   
        from pyspark.mllib.classification import LogisticRegressionWithSGD
        from pyspark.mllib.regression import LabeledPoint
        from numpy import array
6. Du måste nu ladda hello data (hvac.csv), tolkas det och använda den tootrain hello modellen. För det här kan du definiera en funktion som kontrollerar om hello faktiska temperatur hello byggnaden är större än hello mål temperatur. Om hello faktiska temperatur är större än hello byggnaden är hot, med hello värdet **1.0**. Om hello faktiska temperatur är mindre hello byggnaden är kyla, med hello värdet **0,0**. 
   
    Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.

        # List hello structure of data for better understanding. Because hello data will be
        # loaded as an array, this structure makes it easy toounderstand what each element
        # in hello array corresponds to

        # 0 Date
        # 1 Time
        # 2 TargetTemp
        # 3 ActualTemp
        # 4 System
        # 5 SystemAge
        # 6 BuildingID

        LabeledDocument = Row("BuildingID", "SystemInfo", "label")

        # Define a function that parses hello raw CSV file and returns an object of type LabeledDocument

        def parseDocument(line):
            values = [str(x) for x in line.split(',')]
            if (values[3] > values[2]):
                hot = 1.0
            else:
                hot = 0.0        

            textValue = str(values[4]) + " " + str(values[5])

            return LabeledDocument((values[6]), textValue, hot)

        # Load hello raw HVAC.csv file, parse it using hello function
        data = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        documents = data.filter(lambda s: "Date" not in s).map(parseDocument)
        training = documents.toDF()


1. Konfigurera hello Spark maskininlärning pipeline som består av tre faser: tokenizer och hashingTF lr. Mer information om vad är en pipeline och hur det fungerar finns <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.
   
    Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. Passa in hello pipeline toohello utbildning dokument. Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.
   
        model = pipeline.fit(training)
3. Kontrollera hello utbildning dokumentet toocheckpoint förloppet med hello program. Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.
   
        training.show()
   
    Detta bör ge hello utdata liknande toohello följande:
   
        +----------+----------+-----+
        |BuildingID|SystemInfo|label|
        +----------+----------+-----+
        |         4|     13 20|  0.0|
        |        17|      3 20|  0.0|
        |        18|     17 20|  1.0|
        |        15|      2 23|  0.0|
        |         3|      16 9|  1.0|
        |         4|     13 28|  0.0|
        |         2|     12 24|  0.0|
        |        16|     20 26|  1.0|
        |         9|      16 9|  1.0|
        |        12|       6 5|  0.0|
        |        15|     10 17|  1.0|
        |         7|      2 11|  0.0|
        |        15|      14 2|  1.0|
        |         6|       3 2|  0.0|
        |        20|     19 22|  0.0|
        |         8|     19 11|  0.0|
        |         6|      15 7|  0.0|
        |        13|      12 5|  0.0|
        |         4|      8 22|  0.0|
        |         7|      17 5|  0.0|
        +----------+----------+-----+

    Gå tillbaka och kontrollera hello utdata mot hello raw CSV-filen. Hello första rad hello CSV-fil har till exempel data:

    ![Utdata ögonblicksbild Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "utdata ögonblicksbild Spark machine learning-exempel")

    Observera hur hello faktiska temperatur är mindre än hello mål temperatur föreslå hello byggnad Kall. Därför hello värde för hello utbildning utdata och **etikett** i hello första raden är **0,0**, vilket innebär att hello byggnad inte är aktivt.

1. Förbered en datauppsättning toorun hello tränad modell mot. toodo så vi skulle vidarebefordra en system-ID och system ålder (betecknas som **SystemInfo** i hello utbildning utdata), och hello modell ska förutsäga om hello skapar med den system-ID och system åldern skulle högre temperaturer (betecknas med 1.0) eller kallare ( betecknas med 0,0).
   
   Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. Slutligen göra förutsägelser på hello testdata. Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. Du bör se en utdata liknande toohello följande:
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   Hello första raden i hello förutsägelse, du kan visa för ett HVAC-system med ID 20 och 25 år system ålder hello byggnad kommer att varm (**förutsägelse = 1.0**). hello första värde för DenseVector (0.49999) motsvarar toohello förutsägelse 0,0 och andra hello-värdet (0.5001) motsvarar toohello förutsägelse 1.0. I hello utdata, även om andra hello-värdet är bara marginellt högre visar hello modellen **förutsägelse = 1.0**.
4. När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser. toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**. Den här stängs och Stäng hello bärbar dator.

## <a name="anaconda"></a>Använd Anaconda scikit-Läs bibliotek för Spark machine learning
Apache Spark-kluster i HDInsight innehåller Anaconda-bibliotek. Detta omfattar även hello **scikit-Läs** bibliotek för machine learning. hello-bibliotek innehåller också olika datauppsättningar som du kan använda toobuild exempelprogrammen direkt från en Jupyter-anteckningsbok. För exempel på hur du använder hello scikit-Läs biblioteket, se [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).

## <a name="seealso"></a>Se även
* [Översikt: Apache Spark i Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
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

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
