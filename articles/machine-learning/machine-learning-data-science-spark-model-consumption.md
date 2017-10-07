---
title: "aaaOperationalize Spark-inbyggda maskininlärning modeller | Microsoft Docs"
description: "Hur tooload och poäng maskininlärningsmodeller lagras i Azure Blob Storage (WASB) med Python."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 626305a2-0abf-4642-afb0-dad0f6bd24e9
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: c5fadcb13257b94dcb28a522be454f6e03dfa991
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a><span data-ttu-id="8f59a-103">Operationalisera Spark-inbyggda machine learning-modeller</span><span class="sxs-lookup"><span data-stu-id="8f59a-103">Operationalize Spark-built machine learning models</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="8f59a-104">Det här avsnittet visar hur toooperationalize en sparad maskininlärningsmodell (ML) använder Python på HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-104">This topic shows how toooperationalize a saved machine learning model (ML) using Python on HDInsight Spark clusters.</span></span> <span data-ttu-id="8f59a-105">Beskriver hur tooload datorn learning-modeller som har skapats med Spark MLlib och lagras i Azure Blob Storage (WASB) och hur tooscore dem med datauppsättningar som också har lagrats i WASB.</span><span class="sxs-lookup"><span data-stu-id="8f59a-105">It describes how tooload machine learning models that have been built using Spark MLlib and stored in Azure Blob Storage (WASB), and how tooscore them with datasets that have also been stored in WASB.</span></span> <span data-ttu-id="8f59a-106">Den visar hur toopre processen hello indata, transformera funktioner med hello indexering och koda funktioner i hello MLlib toolkit och hur toocreate ett märkt point data-objekt som kan användas som indata för bedömningen med hello ML-modell.</span><span class="sxs-lookup"><span data-stu-id="8f59a-106">It shows how toopre-process hello input data, transform features using hello indexing and encoding functions in hello MLlib toolkit, and how toocreate a labeled point data object that can be used as input for scoring with hello ML models.</span></span> <span data-ttu-id="8f59a-107">hello modeller som används för resultatfunktioner inkluderar linjär Regression, Logistic Regression, slumpmässiga skog modeller, och toning förstärkning trädet.</span><span class="sxs-lookup"><span data-stu-id="8f59a-107">hello models used for scoring include Linear Regression, Logistic Regression, Random Forest Models, and Gradient Boosting Tree Models.</span></span>

## <a name="spark-clusters-and-jupyter-notebooks"></a><span data-ttu-id="8f59a-108">Spark-kluster och Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="8f59a-108">Spark clusters and Jupyter notebooks</span></span>
<span data-ttu-id="8f59a-109">Konfigurationsstegen och hello kod toooperationalize ML-modell finns i den här genomgången för att använda ett kluster i HDInsight Spark 1.6 samt ett 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-109">Setup steps and hello code toooperationalize an ML model are provided in this walkthrough for using an HDInsight Spark 1.6 cluster as well as a Spark 2.0 cluster.</span></span> <span data-ttu-id="8f59a-110">hello-koden för dessa procedurer finns också i Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="8f59a-110">hello code for these procedures is also provided in Jupyter notebooks.</span></span>

### <a name="notebook-for-spark-16"></a><span data-ttu-id="8f59a-111">Bärbar dator för Spark 1.6</span><span class="sxs-lookup"><span data-stu-id="8f59a-111">Notebook for Spark 1.6</span></span>
<span data-ttu-id="8f59a-112">Hej [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter-anteckningsbok visar hur toooperationalize en sparad modell med Python på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-112">hello [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook shows how toooperationalize a saved model using Python on HDInsight clusters.</span></span> 

### <a name="notebook-for-spark-20"></a><span data-ttu-id="8f59a-113">Bärbar dator för Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="8f59a-113">Notebook for Spark 2.0</span></span>
<span data-ttu-id="8f59a-114">toomodify hello Jupyter-anteckningsboken för Spark 1.6 toouse med ett kluster i HDInsight Spark 2.0, Ersätt hello Python kodfilen med [filen](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span><span class="sxs-lookup"><span data-stu-id="8f59a-114">toomodify hello Jupyter notebook for Spark 1.6 toouse with an HDInsight Spark 2.0 cluster, replace hello Python code file with [this file](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span></span> <span data-ttu-id="8f59a-115">Den här koden visar hur tooconsume hello modeller som skapats i Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="8f59a-115">This code shows how tooconsume hello models created in Spark 2.0.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="8f59a-116">Krav</span><span class="sxs-lookup"><span data-stu-id="8f59a-116">Prerequisites</span></span>

1. <span data-ttu-id="8f59a-117">Du behöver ett Azure-konto och en Spark 1.6 (eller Spark 2.0) HDInsight-kluster toocomplete den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="8f59a-117">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="8f59a-118">Se hello [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md) anvisningar för hur toosatisfy dessa krav.</span><span class="sxs-lookup"><span data-stu-id="8f59a-118">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="8f59a-119">Avsnittet innehåller också en beskrivning av hello NYC 2013 Taxi data används här och anvisningar om hur tooexecute kod från en Jupyter-anteckningsbok på hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-119">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 
2. <span data-ttu-id="8f59a-120">Du måste också skapa hello machine learning-modeller toobe bedömas här genom att utföra hello [datagranskning och modellering med Spark](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet för hello 1.6 Spark-kluster eller hello Spark 2.0 bärbara datorer.</span><span class="sxs-lookup"><span data-stu-id="8f59a-120">You must also create hello machine learning models toobe scored here by working through hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic for hello Spark 1.6 cluster or hello Spark 2.0 notebooks.</span></span> 
3. <span data-ttu-id="8f59a-121">hello Spark 2.0 bärbara datorer använder en ytterligare datamängd för hello klassificering hello välkända flygbolag i tid avvikelse dataset från 2011 och 2012.</span><span class="sxs-lookup"><span data-stu-id="8f59a-121">hello Spark 2.0 notebooks use an additional data set for hello classification task, hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="8f59a-122">En beskrivning av hello anteckningsböcker och länkar toothem finns i hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för hello GitHub-lagringsplats som innehåller dessa.</span><span class="sxs-lookup"><span data-stu-id="8f59a-122">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="8f59a-123">Dessutom hello koden här hello länkade anteckningsböcker är generisk och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-123">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="8f59a-124">Om du inte använder HDInsight Spark hello konfiguration och hantering av steg kan vara skiljer sig från vad som anges här.</span><span class="sxs-lookup"><span data-stu-id="8f59a-124">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="8f59a-125">: Lagringsplatser, bibliotek och hello förinställning Spark-kontexten</span><span class="sxs-lookup"><span data-stu-id="8f59a-125">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="8f59a-126">Spark är kan tooread och skriva tooan Azure Storage Blob (WASB).</span><span class="sxs-lookup"><span data-stu-id="8f59a-126">Spark is able tooread and write tooan Azure Storage Blob (WASB).</span></span> <span data-ttu-id="8f59a-127">Så något av dina befintliga data som lagras där kan bearbetas med Spark och hello resultatet lagras igen i WASB.</span><span class="sxs-lookup"><span data-stu-id="8f59a-127">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="8f59a-128">toosave modeller eller filer i WASB hello sökväg behöver toobe som angetts på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="8f59a-128">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="8f59a-129">hello standard behållaren kopplade toohello Spark-kluster kan refereras med en sökväg som börjar med: *”wasb / /”*.</span><span class="sxs-lookup"><span data-stu-id="8f59a-129">hello default container attached toohello Spark cluster can be referenced using a path beginning with: *"wasb//"*.</span></span> <span data-ttu-id="8f59a-130">hello följande kodexempel anger hello platsen för hello data toobe läsa och sparas hello sökväg för hello modellen lagring directory toowhich hello modellen utdata.</span><span class="sxs-lookup"><span data-stu-id="8f59a-130">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved.</span></span> 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="8f59a-131">Ange katalogsökvägar för lagringsplatser i WASB</span><span class="sxs-lookup"><span data-stu-id="8f59a-131">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="8f59a-132">Modeller sparas i ”: wasb: / / / användare/remoteuser/NYCTaxi/modeller”.</span><span class="sxs-lookup"><span data-stu-id="8f59a-132">Models are saved in: "wasb:///user/remoteuser/NYCTaxi/Models".</span></span> <span data-ttu-id="8f59a-133">Om den här sökvägen inte är korrekt, är inte läsa in modeller för resultatfunktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-133">If this path is not set properly, models are not loaded for scoring.</span></span>

<span data-ttu-id="8f59a-134">Hej poängsatta resultaten har sparats i ”: wasb: / / / användare/remoteuser/NYCTaxi/ScoredResults”.</span><span class="sxs-lookup"><span data-stu-id="8f59a-134">hello scored results have been saved in: "wasb:///user/remoteuser/NYCTaxi/ScoredResults".</span></span> <span data-ttu-id="8f59a-135">Om hello sökvägen toofolder är felaktigt sparas inte resultat i den mappen.</span><span class="sxs-lookup"><span data-stu-id="8f59a-135">If hello path toofolder is incorrect, results are not saved in that folder.</span></span>   

> [!NOTE]
> <span data-ttu-id="8f59a-136">hello filplatser sökvägen kan kopieras och klistras in i hello platshållare i den här koden från hello utdata från hello sista cellen i hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="8f59a-136">hello file path locations can be copied and pasted into hello placeholders in this code from hello output of hello last cell of hello **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.</span></span>   
> 
> 

<span data-ttu-id="8f59a-137">Här är hello kodsökvägar tooset directory:</span><span class="sxs-lookup"><span data-stu-id="8f59a-137">Here is hello code tooset directory paths:</span></span> 

    # LOCATION OF DATA tooBE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE hello LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR hello MODELS tooBE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="8f59a-138">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-138">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-139">datetime.datetime (2016, 4, 25, 23, 56, 19, 229403)</span><span class="sxs-lookup"><span data-stu-id="8f59a-139">datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="8f59a-140">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="8f59a-140">Import libraries</span></span>
<span data-ttu-id="8f59a-141">Ange spark kontext och importera nödvändiga bibliotek med följande kod hello</span><span class="sxs-lookup"><span data-stu-id="8f59a-141">Set spark context and import necessary libraries with hello following code</span></span>

    #IMPORT LIBRARIES
    import pyspark
    from pyspark import SparkConf
    from pyspark import SparkContext
    from pyspark.sql import SQLContext
    import matplotlib
    import matplotlib.pyplot as plt
    from pyspark.sql import Row
    from pyspark.sql.functions import UserDefinedFunction
    from pyspark.sql.types import *
    import atexit
    from numpy import array
    import numpy as np
    import datetime


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="8f59a-142">Förinställda Spark-kontexten och PySpark användbara</span><span class="sxs-lookup"><span data-stu-id="8f59a-142">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="8f59a-143">Hej PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext.</span><span class="sxs-lookup"><span data-stu-id="8f59a-143">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="8f59a-144">Så behöver du inte tooset hello Spark- eller Hive kontexter explicit innan du börjar arbeta med hello-program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="8f59a-144">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="8f59a-145">Dessa är tillgängliga för dig som standard.</span><span class="sxs-lookup"><span data-stu-id="8f59a-145">These are available for you by default.</span></span> <span data-ttu-id="8f59a-146">Dessa kontexter är:</span><span class="sxs-lookup"><span data-stu-id="8f59a-146">These contexts are:</span></span>

* <span data-ttu-id="8f59a-147">SC - Spark</span><span class="sxs-lookup"><span data-stu-id="8f59a-147">sc - for Spark</span></span> 
* <span data-ttu-id="8f59a-148">sqlContext - för Hive</span><span class="sxs-lookup"><span data-stu-id="8f59a-148">sqlContext - for Hive</span></span>

<span data-ttu-id="8f59a-149">Hej PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%.</span><span class="sxs-lookup"><span data-stu-id="8f59a-149">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="8f59a-150">Det finns två kommandon som används i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="8f59a-150">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="8f59a-151">**%% lokala** angetts hello koden i efterföljande rader körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="8f59a-151">**%%local** Specified that hello code in subsequent lines is executed locally.</span></span> <span data-ttu-id="8f59a-152">Koden måste vara giltig Python-kod.</span><span class="sxs-lookup"><span data-stu-id="8f59a-152">Code must be valid Python code.</span></span>
* <span data-ttu-id="8f59a-153">**%% sql -o<variable name>**</span><span class="sxs-lookup"><span data-stu-id="8f59a-153">**%%sql -o <variable name>**</span></span> 
* <span data-ttu-id="8f59a-154">Kör en Hive-fråga mot hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="8f59a-154">Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="8f59a-155">Om hello -o parametern överförs hello resultatet av hello fråga sparas i hello %% lokala Python kontext som en Pandas dataframe.</span><span class="sxs-lookup"><span data-stu-id="8f59a-155">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas dataframe.</span></span>

<span data-ttu-id="8f59a-156">För mer information om hello kärnor för Jupyter-anteckningsböcker och hello fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="8f59a-156">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a><span data-ttu-id="8f59a-157">Mata in data och skapa en rensade data-ram</span><span class="sxs-lookup"><span data-stu-id="8f59a-157">Ingest data and create a cleaned data frame</span></span>
<span data-ttu-id="8f59a-158">Det här avsnittet innehåller ett antal uppgifter krävs tooingest hello data toobe bedömas hello kod.</span><span class="sxs-lookup"><span data-stu-id="8f59a-158">This section contains hello code for a series of tasks required tooingest hello data toobe scored.</span></span> <span data-ttu-id="8f59a-159">Formatet hello data läses i en domänansluten 0,1% exemplet hello taxi resa och avgiften-fil (som lagras som en TSV-fil), och skapar sedan en ren data ram.</span><span class="sxs-lookup"><span data-stu-id="8f59a-159">Read in a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file), format hello data, and then creates a clean data frame.</span></span>

<span data-ttu-id="8f59a-160">hello taxi resa och avgiften filer kopplas baserat på hello-procedur som angetts i den: [hello Team av vetenskapliga data i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8f59a-160">hello taxi trip and fare files were joined based on hello procedure provided in the: [hello Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) topic.</span></span>

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_test_file.first()
    fields = [StructField(field_name, StringType(), True) for field_name in schema_string.split('\t')]
    fields[7].dataType = IntegerType() #Pickup hour
    fields[8].dataType = IntegerType() # Pickup week
    fields[9].dataType = IntegerType() # Weekday
    fields[10].dataType = IntegerType() # Passenger count
    fields[11].dataType = FloatType() # Trip time in secs
    fields[12].dataType = FloatType() # Trip distance
    fields[19].dataType = FloatType() # Fare amount
    fields[20].dataType = FloatType() # Surcharge
    fields[21].dataType = FloatType() # Mta_tax
    fields[22].dataType = FloatType() # Tip amount
    fields[23].dataType = FloatType() # Tolls amount
    fields[24].dataType = FloatType() # Total amount
    fields[25].dataType = IntegerType() # Tipped or not
    fields[26].dataType = IntegerType() # Tip class
    taxi_schema = StructType(fields)

    # CREATE DATA FRAME
    taxi_df_test = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_test_cleaned = taxi_df_test.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_cleaned.cache()
    taxi_df_test_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_test_cleaned.registerTempTable("taxi_test")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="8f59a-161">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-161">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-162">Tidsåtgång tooexecute ovanför cellen: 46.37 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-162">Time taken tooexecute above cell: 46.37 seconds</span></span>

## <a name="prepare-data-for-scoring-in-spark"></a><span data-ttu-id="8f59a-163">Förbered data för Poängberäkningen i Spark</span><span class="sxs-lookup"><span data-stu-id="8f59a-163">Prepare data for scoring in Spark</span></span>
<span data-ttu-id="8f59a-164">Det här avsnittet visar hur tooindex, koda och skala kategoriska funktioner tooprepare dem för använder i MLlib övervakad inlärning algoritmer för klassificering och regression.</span><span class="sxs-lookup"><span data-stu-id="8f59a-164">This section shows how tooindex, encode, and scale categorical features tooprepare them for use in MLlib supervised learning algorithms for classification and regression.</span></span>

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a><span data-ttu-id="8f59a-165">Funktionen omvandling: indexera och koda kategoriska funktioner för indata i modeller för poäng.</span><span class="sxs-lookup"><span data-stu-id="8f59a-165">Feature transformation: index and encode categorical features for input into models for scoring</span></span>
<span data-ttu-id="8f59a-166">Det här avsnittet visas hur tooindex kategoriska data med hjälp av en `StringIndexer` och koda funktioner med `OneHotEncoder` indata för hello modeller.</span><span class="sxs-lookup"><span data-stu-id="8f59a-166">This section shows how tooindex categorical data using a `StringIndexer` and encode features with `OneHotEncoder` input into hello models.</span></span>

<span data-ttu-id="8f59a-167">Hej [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kodar en strängkolumn för etiketter tooa kolumnen av etikett index.</span><span class="sxs-lookup"><span data-stu-id="8f59a-167">hello [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) encodes a string column of labels tooa column of label indices.</span></span> <span data-ttu-id="8f59a-168">hello index sorteras efter etikett frekvenser.</span><span class="sxs-lookup"><span data-stu-id="8f59a-168">hello indices are ordered by label frequencies.</span></span> 

<span data-ttu-id="8f59a-169">Hej [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="8f59a-169">hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="8f59a-170">Den här kodningen kan algoritmer som räknar kontinuerlig viktiga funktioner, till exempel logistic regression toobe tillämpas toocategorical funktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-170">This encoding allows algorithms that expect continuous valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

    #INDEX AND ONE-HOT ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_test 
    """
    taxi_df_test_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_test_with_newFeatures.cache()
    taxi_df_test_with_newFeatures.count()

    # INDEX AND ONE-HOT ENCODING
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_test_with_newFeatures)
    encoder = OneHotEncoder(dropLast=False, inputCol="vendorIndex", outputCol="vendorVec")
    encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    stringIndexer = StringIndexer(inputCol="rate_code", outputCol="rateIndex")
    model = stringIndexer.fit(encoded1)
    indexed = model.transform(encoded1)
    encoder = OneHotEncoder(dropLast=False, inputCol="rateIndex", outputCol="rateVec")
    encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    stringIndexer = StringIndexer(inputCol="payment_type", outputCol="paymentIndex")
    model = stringIndexer.fit(encoded2)
    indexed = model.transform(encoded2)
    encoder = OneHotEncoder(dropLast=False, inputCol="paymentIndex", outputCol="paymentVec")
    encoded3 = encoder.transform(indexed)

    # INDEX AND ENCODE TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="8f59a-171">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-171">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-172">Tidsåtgång tooexecute ovanför cellen: 5.37 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-172">Time taken tooexecute above cell: 5.37 seconds</span></span>

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a><span data-ttu-id="8f59a-173">Skapa RDD objekt med funktionen matriser för indata i modeller</span><span class="sxs-lookup"><span data-stu-id="8f59a-173">Create RDD objects with feature arrays for input into models</span></span>
<span data-ttu-id="8f59a-174">Det här avsnittet innehåller kod som visar hur tooindex kategoriska textdata som ett RDD objekt och en hot koda den så att den kan vara används tootrain och testa MLlib logistic regression och trädet-baserade modeller.</span><span class="sxs-lookup"><span data-stu-id="8f59a-174">This section contains code that shows how tooindex categorical text data as an RDD object and one-hot encode it so it can be used tootrain and test MLlib logistic regression and tree-based models.</span></span> <span data-ttu-id="8f59a-175">hello indexerade data lagras i [flexibel distribuerade datauppsättningen (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objekt.</span><span class="sxs-lookup"><span data-stu-id="8f59a-175">hello indexed data is stored in [Resilient Distributed Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objects.</span></span> <span data-ttu-id="8f59a-176">Dessa är hello grundläggande abstraction i Spark.</span><span class="sxs-lookup"><span data-stu-id="8f59a-176">These are hello basic abstraction in Spark.</span></span> <span data-ttu-id="8f59a-177">En RDD-objektet representerar en ändras, partitionerad samling element kan användas på parallellt med Spark.</span><span class="sxs-lookup"><span data-stu-id="8f59a-177">An RDD object represents an immutable, partitioned collection of elements that can be operated on in parallel with Spark.</span></span>

<span data-ttu-id="8f59a-178">Den innehåller också kod som visar hur tooscale data med hello `StandardScalar` som tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD), en populär algoritm för att träna en mängd olika machine learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="8f59a-178">It also contains code that shows how tooscale data with hello `StandardScalar` provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of machine learning models.</span></span> <span data-ttu-id="8f59a-179">Hej [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) är används tooscale hello funktioner toounit varians.</span><span class="sxs-lookup"><span data-stu-id="8f59a-179">hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) is used tooscale hello features toounit variance.</span></span> <span data-ttu-id="8f59a-180">Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="8f59a-180">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> 

    # CREATE RDD OBJECTS WITH FEATURE ARRAYS FOR INPUT INTO MODELS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        return  features

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        return  features

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTESTbinary = encodedFinal.map(parseRowIndexingBinary)
    oneHotTESTbinary = encodedFinal.map(parseRowOneHotBinary)

    # FOR REGRESSION CLASSIFICATION TRAINING AND TESTING
    indexedTESTreg = encodedFinal.map(parseRowIndexingRegression)
    oneHotTESTreg = encodedFinal.map(parseRowOneHotRegression)

    # SCALING FEATURES FOR LINEARREGRESSIONWITHSGD MODEL
    scaler = StandardScaler(withMean=False, withStd=True).fit(oneHotTESTreg)
    oneHotTESTregScaled = scaler.transform(oneHotTESTreg)

    # CACHE RDDS IN MEMORY
    indexedTESTbinary.cache();
    oneHotTESTbinary.cache();
    indexedTESTreg.cache();
    oneHotTESTreg.cache();
    oneHotTESTregScaled.cache();

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="8f59a-181">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-181">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-182">Tidsåtgång tooexecute ovanför cellen: 11.72 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-182">Time taken tooexecute above cell: 11.72 seconds</span></span>

## <a name="score-with-hello-logistic-regression-model-and-save-output-tooblob"></a><span data-ttu-id="8f59a-183">Poängsätta med hello Logistic regressionsmodell och spara utdata tooblob</span><span class="sxs-lookup"><span data-stu-id="8f59a-183">Score with hello Logistic Regression Model and save output tooblob</span></span>
<span data-ttu-id="8f59a-184">hello koden i det här avsnittet visar hur tooload en Logistic regressionsmodell som har sparats i Azure blob storage och använda toopredict huruvida ett tips är betald på en taxi resa poängsätta med standard klassificering och spara och rita hello resultat tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="8f59a-184">hello code in this section shows how tooload a Logistic Regression Model that has been saved in Azure blob storage and use it toopredict whether or not a tip is paid on a taxi trip, score it with standard classification metrics, and then save and plot hello results tooblob storage.</span></span> <span data-ttu-id="8f59a-185">hello bedömas resultat lagras i RDD objekt.</span><span class="sxs-lookup"><span data-stu-id="8f59a-185">hello scored results are stored in RDD objects.</span></span> 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) tooBLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="8f59a-186">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-186">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-187">Tidsåtgång tooexecute ovanför cellen: 19.22 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-187">Time taken tooexecute above cell: 19.22 seconds</span></span>

## <a name="score-a-linear-regression-model"></a><span data-ttu-id="8f59a-188">Poängsätta en linjär regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="8f59a-188">Score a Linear Regression Model</span></span>
<span data-ttu-id="8f59a-189">Vi använde [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain en linjär regressionsmodell med hjälp av Stokastisk toning härstammar (SGD) för optimering toopredict hello mängden tips betald.</span><span class="sxs-lookup"><span data-stu-id="8f59a-189">We used [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) tootrain a linear regression model using Stochastic Gradient Descent (SGD) for optimization toopredict hello amount of tip paid.</span></span> 

<span data-ttu-id="8f59a-190">hello kod i det här avsnittet visar hur tooload en linjär regressionsmodell från Azure blob storage poängsätta använda skalade variabler och spara hello resultaten tillbaka toohello blob.</span><span class="sxs-lookup"><span data-stu-id="8f59a-190">hello code in this section shows how tooload a Linear Regression Model from Azure blob storage, score using scaled variables, and then save hello results back toohello blob.</span></span>

    #SCORE LINEAR REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #LOAD LIBRARIES
    from pyspark.mllib.regression import LinearRegressionWithSGD, LinearRegressionModel

    # LOAD MODEL AND SCORE USING ** SCALED VARIABLES **
    savedModel = LinearRegressionModel.load(sc, linearRegFileLoc)
    predictions = oneHotTESTregScaled.map(lambda features: (float(savedModel.predict(features))))

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = scoredResultDir + linearregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="8f59a-191">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-191">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-192">Tidsåtgång tooexecute ovanför cellen: 16.63 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-192">Time taken tooexecute above cell: 16.63 seconds</span></span>

## <a name="score-classification-and-regression-random-forest-models"></a><span data-ttu-id="8f59a-193">Poängsätta klassificering och regression slumpmässiga skog modeller</span><span class="sxs-lookup"><span data-stu-id="8f59a-193">Score classification and regression Random Forest Models</span></span>
<span data-ttu-id="8f59a-194">hello kod i det här avsnittet visar hur tooload hello sparas klassificering och regression slumpmässiga skog modeller som sparats i Azure blob storage poängsätta deras prestanda med standard klassificerare och regression mått och spara hello resultaten tillbaka tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="8f59a-194">hello code in this section shows how tooload hello saved classification and regression Random Forest Models saved in Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span>

<span data-ttu-id="8f59a-195">[Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="8f59a-195">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="8f59a-196">De kombinera många decision trees tooreduce hello risken för overfitting.</span><span class="sxs-lookup"><span data-stu-id="8f59a-196">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="8f59a-197">Slumpmässiga skogar kan hantera kategoriska funktioner, utöka toohello multiklass-baserad klassificering inställningen, kräver inte funktionen skalning och att kan toocapture icke-linjärt och interaktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-197">Random forests can handle categorical features, extend toohello multiclass classification setting, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="8f59a-198">Slumpmässiga skogar är en av hello mest framgångsrika maskininlärning modeller för klassificering och regression.</span><span class="sxs-lookup"><span data-stu-id="8f59a-198">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>

<span data-ttu-id="8f59a-199">[Spark.mllib](http://spark.apache.org/mllib/) stöder slumpmässiga skogar för binära och multiklass-baserad klassificering och regression som använder både kontinuerliga och kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-199">[spark.mllib](http://spark.apache.org/mllib/) supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.</span></span> 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="8f59a-200">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-200">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-201">Tidsåtgång tooexecute ovanför cellen: 31.07 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-201">Time taken tooexecute above cell: 31.07 seconds</span></span>

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a><span data-ttu-id="8f59a-202">Poängsätta klassificering och regression toning förstärkning trädet modeller</span><span class="sxs-lookup"><span data-stu-id="8f59a-202">Score classification and regression Gradient Boosting Tree Models</span></span>
<span data-ttu-id="8f59a-203">hello kod i det här avsnittet visar hur tooload klassificering och regression toning förstärkning trädet modeller från Azure blob storage poängsätta deras prestanda med standard klassificerare och regression mått och spara hello resultaten tillbaka tooblob lagring.</span><span class="sxs-lookup"><span data-stu-id="8f59a-203">hello code in this section shows how tooload classification and regression Gradient Boosting Tree Models from Azure blob storage, score their performance with standard classifier and regression measures, and then save hello results back tooblob storage.</span></span> 

<span data-ttu-id="8f59a-204">**Spark.mllib** GBTs har stöd för binär klassificering och regression som använder både kontinuerliga och kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-204">**spark.mllib** supports GBTs for binary classification and for regression, using both continuous and categorical features.</span></span> 

<span data-ttu-id="8f59a-205">[Toning förstärkning träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="8f59a-205">[Gradient Boosting Trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="8f59a-206">GBTs train beslut träd upprepade gånger toominimize en förlust-funktion.</span><span class="sxs-lookup"><span data-stu-id="8f59a-206">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="8f59a-207">GBTs kan hantera kategoriska funktioner kräver inte funktionen skalning och är kan toocapture icke-linjärt och interaktioner.</span><span class="sxs-lookup"><span data-stu-id="8f59a-207">GBTs can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="8f59a-208">De kan också användas i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="8f59a-208">They can also be used in a multiclass-classification setting.</span></span>

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    #LOAD AND SCORE hello MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK tooBLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="8f59a-209">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-209">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-210">Tidsåtgång tooexecute ovanför cellen: 14.6 sekunder</span><span class="sxs-lookup"><span data-stu-id="8f59a-210">Time taken tooexecute above cell: 14.6 seconds</span></span>

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a><span data-ttu-id="8f59a-211">Ta bort objekt från minne och Skriv ut bedömas filplatser</span><span class="sxs-lookup"><span data-stu-id="8f59a-211">Clean up objects from memory and print scored file locations</span></span>
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH tooSCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


<span data-ttu-id="8f59a-212">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="8f59a-212">**OUTPUT:**</span></span>

<span data-ttu-id="8f59a-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span><span class="sxs-lookup"><span data-stu-id="8f59a-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span></span>

<span data-ttu-id="8f59a-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span><span class="sxs-lookup"><span data-stu-id="8f59a-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span></span>

<span data-ttu-id="8f59a-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span><span class="sxs-lookup"><span data-stu-id="8f59a-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span></span>

<span data-ttu-id="8f59a-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span><span class="sxs-lookup"><span data-stu-id="8f59a-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span></span>

<span data-ttu-id="8f59a-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span><span class="sxs-lookup"><span data-stu-id="8f59a-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span></span>

<span data-ttu-id="8f59a-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span><span class="sxs-lookup"><span data-stu-id="8f59a-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span></span>

## <a name="consume-spark-models-through-a-web-interface"></a><span data-ttu-id="8f59a-219">Använda Spark modeller via ett webbgränssnitt</span><span class="sxs-lookup"><span data-stu-id="8f59a-219">Consume Spark Models through a web interface</span></span>
<span data-ttu-id="8f59a-220">Spark tillhandahåller en mekanism tooremotely skicka batchjobb eller interaktiva frågor via ett REST-gränssnitt med en komponent som kallas Livius.</span><span class="sxs-lookup"><span data-stu-id="8f59a-220">Spark provides a mechanism tooremotely submit batch jobs or interactive queries through a REST interface with a component called Livy.</span></span> <span data-ttu-id="8f59a-221">Livius är aktiverad som standard på HDInsight Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="8f59a-221">Livy is enabled by default on your HDInsight Spark cluster.</span></span> <span data-ttu-id="8f59a-222">Läs mer på Livius: [skicka Spark jobb via fjärranslutning med Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="8f59a-222">For more information on Livy, see: [Submit Spark jobs remotely using Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span></span> 

<span data-ttu-id="8f59a-223">Du kan använda Livius tooremotely skickar ett jobb som batch poäng en fil som lagras i en Azure blob och sedan skriver hello resultat tooanother blob.</span><span class="sxs-lookup"><span data-stu-id="8f59a-223">You can use Livy tooremotely submit a job that batch scores a file that is stored in an Azure blob and then writes hello results tooanother blob.</span></span> <span data-ttu-id="8f59a-224">toodo kan du överföra hello Python-skriptet från</span><span class="sxs-lookup"><span data-stu-id="8f59a-224">toodo this, you upload hello Python script from</span></span>  
<span data-ttu-id="8f59a-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob hello Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="8f59a-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) toohello blob of hello Spark cluster.</span></span> <span data-ttu-id="8f59a-226">Du kan använda ett verktyg som **Microsoft Azure Lagringsutforskaren** eller **AzCopy** toocopy hello skriptet toohello klustret blob.</span><span class="sxs-lookup"><span data-stu-id="8f59a-226">You can use a tool like **Microsoft Azure Storage Explorer** or **AzCopy** toocopy hello script toohello cluster blob.</span></span> <span data-ttu-id="8f59a-227">I vårt fall vi upp hello skript för***wasb:///example/python/ConsumeGBNYCReg.py***.</span><span class="sxs-lookup"><span data-stu-id="8f59a-227">In our case we uploaded hello script too***wasb:///example/python/ConsumeGBNYCReg.py***.</span></span>   

> [!NOTE]
> <span data-ttu-id="8f59a-228">Hej åtkomstnycklar som du behöver finns på hello portal för hello storage-konto som är associerade med hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-228">hello access keys that you need can be found on hello portal for hello storage account associated with hello Spark cluster.</span></span> 
> 
> 

<span data-ttu-id="8f59a-229">När du har överfört toothis plats körs skriptet inom hello Spark-kluster i en distribuerad kontext.</span><span class="sxs-lookup"><span data-stu-id="8f59a-229">Once uploaded toothis location, this script runs within hello Spark cluster in a distributed context.</span></span> <span data-ttu-id="8f59a-230">Den läser in hello modellen och kör förutsägelser på indatafiler utifrån hello modell.</span><span class="sxs-lookup"><span data-stu-id="8f59a-230">It loads hello model and runs predictions on input files based on hello model.</span></span>  

<span data-ttu-id="8f59a-231">Du kan anropa skriptet från en fjärrdator genom att göra en enkel HTTPS/REST-begäran på Livius.</span><span class="sxs-lookup"><span data-stu-id="8f59a-231">You can invoke this script remotely by making a simple HTTPS/REST request on Livy.</span></span>  <span data-ttu-id="8f59a-232">Här är en curl-kommando tooconstruct hello HTTP-begäran tooinvoke hello Python-skriptet från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="8f59a-232">Here is a curl command tooconstruct hello HTTP request tooinvoke hello Python script remotely.</span></span> <span data-ttu-id="8f59a-233">Ersätt CLUSTERLOGIN, CLUSTERPASSWORD, KLUSTERNAMN med hello lämpliga värden för ditt Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8f59a-233">Replace CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME with hello appropriate values for your Spark cluster.</span></span>

    # CURL COMMAND tooINVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

<span data-ttu-id="8f59a-234">Du kan använda alla språk på hello fjärrsystemet tooinvoke hello Spark jobbet via Livius genom att göra ett enkelt HTTPS-anrop med grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="8f59a-234">You can use any language on hello remote system tooinvoke hello Spark job through Livy by making a simple HTTPS call with Basic Authentication.</span></span>   

> [!NOTE]
> <span data-ttu-id="8f59a-235">Det skulle vara lämplig toouse hello Python begäranden biblioteket när du gör detta HTTP-anrop, men det är inte installerad som standard i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="8f59a-235">It would be convenient toouse hello Python Requests library when making this HTTP call, but it is not currently installed by default in Azure Functions.</span></span> <span data-ttu-id="8f59a-236">Så att äldre HTTP-biblioteken används i stället.</span><span class="sxs-lookup"><span data-stu-id="8f59a-236">So older HTTP libraries are used instead.</span></span>   
> 
> 

<span data-ttu-id="8f59a-237">Här är hello Python-kod för hello HTTP-anropet:</span><span class="sxs-lookup"><span data-stu-id="8f59a-237">Here is hello Python code for hello HTTP call:</span></span>

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF hello REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY hello PYTHON SCRIPT tooRUN ON hello SPARK CLUSTER
    # IN hello FILE PARAMETER OF hello JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


<span data-ttu-id="8f59a-238">Du kan också lägga till Python-kod för[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger ett Spark-jobbet som poäng en blob baserat på olika händelser, t.ex. en timer, skapa eller uppdatera en blob.</span><span class="sxs-lookup"><span data-stu-id="8f59a-238">You can also add this Python code too[Azure Functions](https://azure.microsoft.com/documentation/services/functions/) tootrigger a Spark job submission that scores a blob based on various events like a timer, creation, or update of a blob.</span></span> 

<span data-ttu-id="8f59a-239">Om du föredrar en kod ledigt klientupplevelsen, använda hello [Azure Logikappar](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark batchbedömningsjobbet genom att definiera en HTTP-åtgärd på hello **Logic Apps Designer** och ange dess parametrar.</span><span class="sxs-lookup"><span data-stu-id="8f59a-239">If you prefer a code free client experience, use hello [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) tooinvoke hello Spark batch scoring by defining an HTTP action on hello **Logic Apps Designer** and setting its parameters.</span></span> 

* <span data-ttu-id="8f59a-240">Skapa en ny Logikapp från Azure-portalen genom att välja **+ ny** -> **webb + mobilt** -> **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="8f59a-240">From Azure portal, create a new Logic App by selecting **+New** -> **Web + Mobile** -> **Logic App**.</span></span> 
* <span data-ttu-id="8f59a-241">toobring in hello **Logic Apps Designer**, anger hello namnet på hello Logikapp och App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="8f59a-241">toobring up hello **Logic Apps Designer**, enter hello name of hello Logic App and App Service Plan.</span></span>
* <span data-ttu-id="8f59a-242">Välj en HTTP-åtgärd och ange hello parametrar som visas i följande bild hello:</span><span class="sxs-lookup"><span data-stu-id="8f59a-242">Select an HTTP action and enter hello parameters shown in hello following figure:</span></span>

![Logic Apps Designer](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a><span data-ttu-id="8f59a-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8f59a-244">What's next?</span></span>
<span data-ttu-id="8f59a-245">**Korsvalidering och hyperparameter omfattande**: se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parametern.</span><span class="sxs-lookup"><span data-stu-id="8f59a-245">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>

