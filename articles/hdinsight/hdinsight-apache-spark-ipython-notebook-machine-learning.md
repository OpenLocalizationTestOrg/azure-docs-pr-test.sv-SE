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
# <a name="build-apache-spark-machine-learning-applications-on-azure-hdinsight"></a><span data-ttu-id="7117b-103">Skapa Apache Spark machine learning-program på Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-103">Build Apache Spark machine learning applications on Azure HDInsight</span></span>

<span data-ttu-id="7117b-104">Lär dig hur toobuild ett Apache Spark machine learning-program med ett Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7117b-104">Learn how toobuild an Apache Spark machine learning application using a Spark cluster on HDInsight.</span></span> <span data-ttu-id="7117b-105">Den här artikeln visar hur toouse hello Jupyter-anteckningsbok med hello klustret toobuild och testa det här programmet.</span><span class="sxs-lookup"><span data-stu-id="7117b-105">This article shows how toouse hello Jupyter notebook available with hello cluster toobuild and test this application.</span></span> <span data-ttu-id="7117b-106">hello programmet använder hello HVAC.csv exempeldata som är tillgänglig på alla kluster som standard.</span><span class="sxs-lookup"><span data-stu-id="7117b-106">hello application uses hello sample HVAC.csv data that is available on all clusters by default.</span></span>

<span data-ttu-id="7117b-107">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="7117b-107">**Prerequisites:**</span></span>

<span data-ttu-id="7117b-108">Du måste ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="7117b-108">You must have hello following:</span></span>

* <span data-ttu-id="7117b-109">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7117b-109">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="7117b-110">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="7117b-110">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> 

## <span data-ttu-id="7117b-111"><a name="data"></a>Förstå hello datauppsättning</span><span class="sxs-lookup"><span data-stu-id="7117b-111"><a name="data"></a>Understand hello data set</span></span>
<span data-ttu-id="7117b-112">Innan vi börja skapa hello program, låt oss förstå hello datastrukturen hello som vi skapa hello program och hello sorts analys gör vi på hello data.</span><span class="sxs-lookup"><span data-stu-id="7117b-112">Before we start building hello application, let us understand hello structure of hello data for which we build hello application and hello kind of analysis we will do on hello data.</span></span> 

<span data-ttu-id="7117b-113">I den här artikeln använder vi hello exempel **HVAC.csv** datafil som är tillgängliga i hello Azure Storage-konto som du har associerat med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7117b-113">In this article, we use hello sample **HVAC.csv** data file that is available in hello Azure Storage account that you associated with hello HDInsight cluster.</span></span> <span data-ttu-id="7117b-114">Inom hello lagringskonto hello-filen finns i **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="7117b-114">Within hello storage account, hello file is at **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span> <span data-ttu-id="7117b-115">Ladda ned och öppna hello CSV-filen tooget en ögonblicksbild av hello data.</span><span class="sxs-lookup"><span data-stu-id="7117b-115">Download and open hello CSV file tooget a snapshot of hello data.</span></span>  

<span data-ttu-id="7117b-116">![Ögonblicksbild av data som används för Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "ögonblicksbild av data som används för Spark machine learning-exempel")</span><span class="sxs-lookup"><span data-stu-id="7117b-116">![Snapshot of data used for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-understand-data.png "Snapshot of data used for Spark machine learning example")</span></span>

<span data-ttu-id="7117b-117">hello data visar hello mål temperatur- och hello faktiska temperaturen för en byggnad som har installerats för HVAC-system.</span><span class="sxs-lookup"><span data-stu-id="7117b-117">hello data shows hello target temperature and hello actual temperature of a building that has HVAC systems installed.</span></span> <span data-ttu-id="7117b-118">Vi antar hello **System** kolumn representerar hello system-ID och hello **SystemAge** kolumn representerar hello antalet år hello HVAC-system har på plats och hello byggnad.</span><span class="sxs-lookup"><span data-stu-id="7117b-118">Let's assume hello **System** column represents hello system ID and hello **SystemAge** column represents hello number of years hello HVAC system has been in place at hello building.</span></span>

<span data-ttu-id="7117b-119">Vi använder den här toopredict data om en byggnad ska vara högre temperaturer eller colder baserat på hello mål temperatur, en system-ID och system ålder.</span><span class="sxs-lookup"><span data-stu-id="7117b-119">We use this data toopredict whether a building will be hotter or colder based on hello target temperature, given a system ID and system age.</span></span>

## <span data-ttu-id="7117b-120"><a name="app"></a>Skriva ett Spark machine learning-program med hjälp av Spark MLlib</span><span class="sxs-lookup"><span data-stu-id="7117b-120"><a name="app"></a>Write a Spark machine learning application using Spark MLlib</span></span>
<span data-ttu-id="7117b-121">Vi använder en Spark ML pipeline tooperform en klassificering av dokument i det här programmet.</span><span class="sxs-lookup"><span data-stu-id="7117b-121">In this application we use a Spark ML pipeline tooperform a document classification.</span></span> <span data-ttu-id="7117b-122">I hello pipeline vi dela hello dokument i ord, konvertera hello ord till en numerisk funktionen vector och slutligen bygger en förutsägelse-modell med hello funktionen angreppsmetoderna och etiketter.</span><span class="sxs-lookup"><span data-stu-id="7117b-122">In hello pipeline, we split hello document into words, convert hello words into a numerical feature vector, and finally build a prediction model using hello feature vectors and labels.</span></span> <span data-ttu-id="7117b-123">Utför följande steg toocreate hello programmet hello.</span><span class="sxs-lookup"><span data-stu-id="7117b-123">Perform hello following steps toocreate hello application.</span></span>

1. <span data-ttu-id="7117b-124">Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="7117b-124">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="7117b-125">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="7117b-125">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="7117b-126">Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="7117b-126">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="7117b-127">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="7117b-127">If prompted, enter hello admin credentials for hello cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7117b-128">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="7117b-128">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="7117b-129">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="7117b-129">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   > 
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   > 
   > 
3. <span data-ttu-id="7117b-130">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="7117b-130">Create a new notebook.</span></span> <span data-ttu-id="7117b-131">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="7117b-131">Click **New**, and then click **PySpark**.</span></span>
   
    <span data-ttu-id="7117b-132">![Skapa en Jupyter-anteckningsbok Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "skapa en Jupyter-anteckningsbok Spark machine learning-exempel")</span><span class="sxs-lookup"><span data-stu-id="7117b-132">![Create a Jupyter notebook for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-create-notebook.png "Create a Jupyter notebook for Spark machine learning example")</span></span>
4. <span data-ttu-id="7117b-133">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="7117b-133">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="7117b-134">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="7117b-134">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="7117b-135">![Ange ett namn för anteckningsboken Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "ange ett namn för anteckningsboken Spark machine learning-exempel")</span><span class="sxs-lookup"><span data-stu-id="7117b-135">![Provide a notebook name for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-notebook-name.png "Provide a notebook name for Spark machine learning example")</span></span>
5. <span data-ttu-id="7117b-136">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="7117b-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="7117b-137">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="7117b-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="7117b-138">Du kan starta genom att importera hello-typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7117b-138">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="7117b-139">Klistra in hello följande fragment i en tom cell och tryck sedan på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-139">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span> 
   
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
6. <span data-ttu-id="7117b-140">Du måste nu ladda hello data (hvac.csv), tolkas det och använda den tootrain hello modellen.</span><span class="sxs-lookup"><span data-stu-id="7117b-140">You must now load hello data (hvac.csv), parse it, and use it tootrain hello model.</span></span> <span data-ttu-id="7117b-141">För det här kan du definiera en funktion som kontrollerar om hello faktiska temperatur hello byggnaden är större än hello mål temperatur.</span><span class="sxs-lookup"><span data-stu-id="7117b-141">For this, you define a function that checks whether hello actual temperature of hello building is greater than hello target temperature.</span></span> <span data-ttu-id="7117b-142">Om hello faktiska temperatur är större än hello byggnaden är hot, med hello värdet **1.0**.</span><span class="sxs-lookup"><span data-stu-id="7117b-142">If hello actual temperature is greater, hello building is hot, denoted by hello value **1.0**.</span></span> <span data-ttu-id="7117b-143">Om hello faktiska temperatur är mindre hello byggnaden är kyla, med hello värdet **0,0**.</span><span class="sxs-lookup"><span data-stu-id="7117b-143">If hello actual temperature is lesser, hello building is cold, denoted by hello value **0.0**.</span></span> 
   
    <span data-ttu-id="7117b-144">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-144">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>

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


1. <span data-ttu-id="7117b-145">Konfigurera hello Spark maskininlärning pipeline som består av tre faser: tokenizer och hashingTF lr.</span><span class="sxs-lookup"><span data-stu-id="7117b-145">Configure hello Spark machine learning pipeline that consists of three stages: tokenizer, hashingTF, and lr.</span></span> <span data-ttu-id="7117b-146">Mer information om vad är en pipeline och hur det fungerar finns <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.</span><span class="sxs-lookup"><span data-stu-id="7117b-146">For more information about what is a pipeline and how it works see <a href="http://spark.apache.org/docs/latest/ml-guide.html#how-it-works" target="_blank">Spark machine learning pipeline</a>.</span></span>
   
    <span data-ttu-id="7117b-147">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-147">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        tokenizer = Tokenizer(inputCol="SystemInfo", outputCol="words")
        hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
        lr = LogisticRegression(maxIter=10, regParam=0.01)
        pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])
2. <span data-ttu-id="7117b-148">Passa in hello pipeline toohello utbildning dokument.</span><span class="sxs-lookup"><span data-stu-id="7117b-148">Fit hello pipeline toohello training document.</span></span> <span data-ttu-id="7117b-149">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-149">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        model = pipeline.fit(training)
3. <span data-ttu-id="7117b-150">Kontrollera hello utbildning dokumentet toocheckpoint förloppet med hello program.</span><span class="sxs-lookup"><span data-stu-id="7117b-150">Verify hello training document toocheckpoint your progress with hello application.</span></span> <span data-ttu-id="7117b-151">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-151">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        training.show()
   
    <span data-ttu-id="7117b-152">Detta bör ge hello utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7117b-152">This should give hello output similar toohello following:</span></span>
   
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

    <span data-ttu-id="7117b-153">Gå tillbaka och kontrollera hello utdata mot hello raw CSV-filen.</span><span class="sxs-lookup"><span data-stu-id="7117b-153">Go back and verify hello output against hello raw CSV file.</span></span> <span data-ttu-id="7117b-154">Hello första rad hello CSV-fil har till exempel data:</span><span class="sxs-lookup"><span data-stu-id="7117b-154">For example, hello first row hello CSV file has this data:</span></span>

    <span data-ttu-id="7117b-155">![Utdata ögonblicksbild Spark machine learning exempel](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "utdata ögonblicksbild Spark machine learning-exempel")</span><span class="sxs-lookup"><span data-stu-id="7117b-155">![Output data snapshot for Spark machine learning example](./media/hdinsight-apache-spark-ipython-notebook-machine-learning/spark-machine-learning-output-data.png "Output data snapshot for Spark machine learning example")</span></span>

    <span data-ttu-id="7117b-156">Observera hur hello faktiska temperatur är mindre än hello mål temperatur föreslå hello byggnad Kall.</span><span class="sxs-lookup"><span data-stu-id="7117b-156">Notice how hello actual temperature is less than hello target temperature suggesting hello building is cold.</span></span> <span data-ttu-id="7117b-157">Därför hello värde för hello utbildning utdata och **etikett** i hello första raden är **0,0**, vilket innebär att hello byggnad inte är aktivt.</span><span class="sxs-lookup"><span data-stu-id="7117b-157">Hence in hello training output, hello value for **label** in hello first row is **0.0**, which means hello building is not hot.</span></span>

1. <span data-ttu-id="7117b-158">Förbered en datauppsättning toorun hello tränad modell mot.</span><span class="sxs-lookup"><span data-stu-id="7117b-158">Prepare a data set toorun hello trained model against.</span></span> <span data-ttu-id="7117b-159">toodo så vi skulle vidarebefordra en system-ID och system ålder (betecknas som **SystemInfo** i hello utbildning utdata), och hello modell ska förutsäga om hello skapar med den system-ID och system åldern skulle högre temperaturer (betecknas med 1.0) eller kallare ( betecknas med 0,0).</span><span class="sxs-lookup"><span data-stu-id="7117b-159">toodo so, we would pass on a system ID and system age (denoted as **SystemInfo** in hello training output), and hello model would predict whether hello building with that system ID and system age would be hotter (denoted by 1.0) or cooler (denoted by 0.0).</span></span>
   
   <span data-ttu-id="7117b-160">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-160">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
       # SystemInfo here is a combination of system ID followed by system age
       Document = Row("id", "SystemInfo")
       test = sc.parallelize([(1L, "20 25"),
                     (2L, "4 15"),
                     (3L, "16 9"),
                     (4L, "9 22"),
                     (5L, "17 10"),
                     (6L, "7 22")]) \
           .map(lambda x: Document(*x)).toDF() 
2. <span data-ttu-id="7117b-161">Slutligen göra förutsägelser på hello testdata.</span><span class="sxs-lookup"><span data-stu-id="7117b-161">Finally, make predictions on hello test data.</span></span> <span data-ttu-id="7117b-162">Klistra in hello följande fragment i en tom cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7117b-162">Paste hello following snippet in an empty cell and press **SHIFT + ENTER**.</span></span>
   
        # Make predictions on test documents and print columns of interest
        prediction = model.transform(test)
        selected = prediction.select("SystemInfo", "prediction", "probability")
        for row in selected.collect():
            print row
3. <span data-ttu-id="7117b-163">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="7117b-163">You should see an output similar toohello following:</span></span>
   
       Row(SystemInfo=u'20 25', prediction=1.0, probability=DenseVector([0.4999, 0.5001]))
       Row(SystemInfo=u'4 15', prediction=0.0, probability=DenseVector([0.5016, 0.4984]))
       Row(SystemInfo=u'16 9', prediction=1.0, probability=DenseVector([0.4785, 0.5215]))
       Row(SystemInfo=u'9 22', prediction=1.0, probability=DenseVector([0.4549, 0.5451]))
       Row(SystemInfo=u'17 10', prediction=1.0, probability=DenseVector([0.4925, 0.5075]))
       Row(SystemInfo=u'7 22', prediction=0.0, probability=DenseVector([0.5015, 0.4985]))
   
   <span data-ttu-id="7117b-164">Hello första raden i hello förutsägelse, du kan visa för ett HVAC-system med ID 20 och 25 år system ålder hello byggnad kommer att varm (**förutsägelse = 1.0**).</span><span class="sxs-lookup"><span data-stu-id="7117b-164">From hello first row in hello prediction, you can see that for an HVAC system with ID 20 and system age of 25 years, hello building will be hot (**prediction=1.0**).</span></span> <span data-ttu-id="7117b-165">hello första värde för DenseVector (0.49999) motsvarar toohello förutsägelse 0,0 och andra hello-värdet (0.5001) motsvarar toohello förutsägelse 1.0.</span><span class="sxs-lookup"><span data-stu-id="7117b-165">hello first value for DenseVector (0.49999) corresponds toohello  prediction 0.0 and hello second value (0.5001) corresponds toohello prediction 1.0.</span></span> <span data-ttu-id="7117b-166">I hello utdata, även om andra hello-värdet är bara marginellt högre visar hello modellen **förutsägelse = 1.0**.</span><span class="sxs-lookup"><span data-stu-id="7117b-166">In hello output, even though hello second value is only marginally higher, hello model shows **prediction=1.0**.</span></span>
4. <span data-ttu-id="7117b-167">När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser.</span><span class="sxs-lookup"><span data-stu-id="7117b-167">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="7117b-168">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="7117b-168">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="7117b-169">Den här stängs och Stäng hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="7117b-169">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="7117b-170"><a name="anaconda"></a>Använd Anaconda scikit-Läs bibliotek för Spark machine learning</span><span class="sxs-lookup"><span data-stu-id="7117b-170"><a name="anaconda"></a>Use Anaconda scikit-learn library for Spark machine learning</span></span>
<span data-ttu-id="7117b-171">Apache Spark-kluster i HDInsight innehåller Anaconda-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7117b-171">Apache Spark clusters on HDInsight include Anaconda libraries.</span></span> <span data-ttu-id="7117b-172">Detta omfattar även hello **scikit-Läs** bibliotek för machine learning.</span><span class="sxs-lookup"><span data-stu-id="7117b-172">This also includes hello **scikit-learn** library for machine learning.</span></span> <span data-ttu-id="7117b-173">hello-bibliotek innehåller också olika datauppsättningar som du kan använda toobuild exempelprogrammen direkt från en Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="7117b-173">hello library also includes various data sets that you can use toobuild sample applications directly from a Jupyter notebook.</span></span> <span data-ttu-id="7117b-174">För exempel på hur du använder hello scikit-Läs biblioteket, se [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span><span class="sxs-lookup"><span data-stu-id="7117b-174">For examples on using hello scikit-learn library, see [http://scikit-learn.org/stable/auto_examples/index.html](http://scikit-learn.org/stable/auto_examples/index.html).</span></span>

## <span data-ttu-id="7117b-175"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="7117b-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="7117b-176">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="7117b-177">Scenarier</span><span class="sxs-lookup"><span data-stu-id="7117b-177">Scenarios</span></span>
* [<span data-ttu-id="7117b-178">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="7117b-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="7117b-179">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="7117b-179">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="7117b-180">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="7117b-180">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="7117b-181">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-181">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="7117b-182">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="7117b-182">Create and run applications</span></span>
* [<span data-ttu-id="7117b-183">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="7117b-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="7117b-184">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="7117b-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="7117b-185">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="7117b-185">Tools and extensions</span></span>
* [<span data-ttu-id="7117b-186">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="7117b-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="7117b-187">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="7117b-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="7117b-188">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="7117b-189">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="7117b-190">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="7117b-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="7117b-191">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7117b-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="7117b-192">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="7117b-192">Manage resources</span></span>
* [<span data-ttu-id="7117b-193">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="7117b-194">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7117b-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-weblogs-sample]: hdinsight-hive-analyze-website-log.md
[hdinsight-sensor-data-sample]: hdinsight-hive-analyze-sensor-data.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
