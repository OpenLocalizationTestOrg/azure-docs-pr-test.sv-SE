---
title: Operationalisera Spark-inbyggda machine learning-modeller | Microsoft Docs
description: "Så att läsa in och resultatet lagras i Azure Blob Storage (WASB) med Python learning-modeller."
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
ms.openlocfilehash: 00fec675bed0137473f7e3c5ddfe9c3c0e8344c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="operationalize-spark-built-machine-learning-models"></a><span data-ttu-id="7c61a-103">Operationalisera Spark-inbyggda machine learning-modeller</span><span class="sxs-lookup"><span data-stu-id="7c61a-103">Operationalize Spark-built machine learning models</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="7c61a-104">Det här avsnittet visar hur du operationalisera en sparad maskininlärning modell (ML) som använder Python på HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c61a-104">This topic shows how to operationalize a saved machine learning model (ML) using Python on HDInsight Spark clusters.</span></span> <span data-ttu-id="7c61a-105">Det beskriver hur du läser in machine learning-modeller som har skapats med hjälp av Spark MLlib och lagras i Azure Blob Storage (WASB) och hur du poängsätta dem med datauppsättningar som också har lagrats i WASB.</span><span class="sxs-lookup"><span data-stu-id="7c61a-105">It describes how to load machine learning models that have been built using Spark MLlib and stored in Azure Blob Storage (WASB), and how to score them with datasets that have also been stored in WASB.</span></span> <span data-ttu-id="7c61a-106">Den visar hur du Förbearbeta indata, transformera funktioner med hjälp av indexering och koda funktioner i MLlib toolkit och hur du skapar ett märkt point data-objekt som kan användas som indata för bedömningen med ML-modell.</span><span class="sxs-lookup"><span data-stu-id="7c61a-106">It shows how to pre-process the input data, transform features using the indexing and encoding functions in the MLlib toolkit, and how to create a labeled point data object that can be used as input for scoring with the ML models.</span></span> <span data-ttu-id="7c61a-107">Modellerna som används för resultatfunktioner innehåller linjär Regression, Logistic Regression, slumpmässiga skog modeller, och toning förstärkning trädet.</span><span class="sxs-lookup"><span data-stu-id="7c61a-107">The models used for scoring include Linear Regression, Logistic Regression, Random Forest Models, and Gradient Boosting Tree Models.</span></span>

## <a name="spark-clusters-and-jupyter-notebooks"></a><span data-ttu-id="7c61a-108">Spark-kluster och Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="7c61a-108">Spark clusters and Jupyter notebooks</span></span>
<span data-ttu-id="7c61a-109">Konfigurationsstegen och koden för att operationalisera ML-modell finns i den här genomgången för att använda ett kluster i HDInsight Spark 1.6 samt ett 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c61a-109">Setup steps and the code to operationalize an ML model are provided in this walkthrough for using an HDInsight Spark 1.6 cluster as well as a Spark 2.0 cluster.</span></span> <span data-ttu-id="7c61a-110">Koden för dessa procedurer finns också i Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="7c61a-110">The code for these procedures is also provided in Jupyter notebooks.</span></span>

### <a name="notebook-for-spark-16"></a><span data-ttu-id="7c61a-111">Bärbar dator för Spark 1.6</span><span class="sxs-lookup"><span data-stu-id="7c61a-111">Notebook for Spark 1.6</span></span>
<span data-ttu-id="7c61a-112">Den [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter-anteckningsbok visar hur du operationalisera en sparad modell med Python på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c61a-112">The [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) Jupyter notebook shows how to operationalize a saved model using Python on HDInsight clusters.</span></span> 

### <a name="notebook-for-spark-20"></a><span data-ttu-id="7c61a-113">Bärbar dator för Spark 2.0</span><span class="sxs-lookup"><span data-stu-id="7c61a-113">Notebook for Spark 2.0</span></span>
<span data-ttu-id="7c61a-114">Om du vill ändra Jupyter-anteckningsboken för Spark 1.6 ska användas med ett kluster i HDInsight Spark 2.0 ersätter filen Python-kod med [filen](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span><span class="sxs-lookup"><span data-stu-id="7c61a-114">To modify the Jupyter notebook for Spark 1.6 to use with an HDInsight Spark 2.0 cluster, replace the Python code file with [this file](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).</span></span> <span data-ttu-id="7c61a-115">Den här koden visar hur du använder modeller som skapats i Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="7c61a-115">This code shows how to consume the models created in Spark 2.0.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="7c61a-116">Krav</span><span class="sxs-lookup"><span data-stu-id="7c61a-116">Prerequisites</span></span>

1. <span data-ttu-id="7c61a-117">Du behöver ett Azure-konto och en Spark 1.6 (eller Spark 2.0) HDInsight-klustret för att slutföra den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="7c61a-117">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster to complete this walkthrough.</span></span> <span data-ttu-id="7c61a-118">Finns det [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md) för instruktioner om hur du uppfyller dessa krav.</span><span class="sxs-lookup"><span data-stu-id="7c61a-118">See the [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how to satisfy these requirements.</span></span> <span data-ttu-id="7c61a-119">Avsnittet innehåller också en beskrivning av NYC 2013 Taxi data används här och instruktioner om hur du kör kod från en Jupyter notebook i Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7c61a-119">That topic also contains a description of the NYC 2013 Taxi data used here and instructions on how to execute code from a Jupyter notebook on the Spark cluster.</span></span> 
2. <span data-ttu-id="7c61a-120">Du måste också skapa maskininlärning modeller som ska mätas här genom att utföra den [datagranskning och modellering med Spark](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet för 1.6 Spark-kluster eller Spark 2.0-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="7c61a-120">You must also create the machine learning models to be scored here by working through the [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic for the Spark 1.6 cluster or the Spark 2.0 notebooks.</span></span> 
3. <span data-ttu-id="7c61a-121">Spark 2.0-datorer använder ytterligare data för aktiviteten klassificering välkända flygbolag i tid avvikelse datauppsättningen från 2011 och 2012.</span><span class="sxs-lookup"><span data-stu-id="7c61a-121">The Spark 2.0 notebooks use an additional data set for the classification task, the well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="7c61a-122">En beskrivning av bärbara datorer och länkar till dem finns i den [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för GitHub-lagringsplats som innehåller dessa.</span><span class="sxs-lookup"><span data-stu-id="7c61a-122">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="7c61a-123">Dessutom koden här länkade anteckningsböcker är generisk och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c61a-123">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="7c61a-124">Om du inte använder HDInsight Spark kanske klustret installation och hantering av stegen skiljer sig från vad som anges här.</span><span class="sxs-lookup"><span data-stu-id="7c61a-124">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> 

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="7c61a-125">Konfigurera: lagringsplatser, bibliotek och förinställda Spark-kontexten</span><span class="sxs-lookup"><span data-stu-id="7c61a-125">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="7c61a-126">Spark kan läsa och skriva till ett Azure Storage Blob (WASB).</span><span class="sxs-lookup"><span data-stu-id="7c61a-126">Spark is able to read and write to an Azure Storage Blob (WASB).</span></span> <span data-ttu-id="7c61a-127">Så någon av dina befintliga data lagras kan det bearbetas med Spark och resultatet lagras igen i WASB.</span><span class="sxs-lookup"><span data-stu-id="7c61a-127">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="7c61a-128">Om du vill spara modeller eller filer i WASB måste sökvägen anges korrekt.</span><span class="sxs-lookup"><span data-stu-id="7c61a-128">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="7c61a-129">Standardbehållaren kopplade till Spark-klustret kan refereras med en sökväg som börjar med: *”wasb / /”*.</span><span class="sxs-lookup"><span data-stu-id="7c61a-129">The default container attached to the Spark cluster can be referenced using a path beginning with: *"wasb//"*.</span></span> <span data-ttu-id="7c61a-130">Följande kodexempel anger platsen för data som ska läsas och sökvägen för modellen Arkivkatalog som modellen sparas.</span><span class="sxs-lookup"><span data-stu-id="7c61a-130">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved.</span></span> 

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="7c61a-131">Ange katalogsökvägar för lagringsplatser i WASB</span><span class="sxs-lookup"><span data-stu-id="7c61a-131">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="7c61a-132">Modeller sparas i ”: wasb: / / / användare/remoteuser/NYCTaxi/modeller”.</span><span class="sxs-lookup"><span data-stu-id="7c61a-132">Models are saved in: "wasb:///user/remoteuser/NYCTaxi/Models".</span></span> <span data-ttu-id="7c61a-133">Om den här sökvägen inte är korrekt, är inte läsa in modeller för resultatfunktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-133">If this path is not set properly, models are not loaded for scoring.</span></span>

<span data-ttu-id="7c61a-134">Poängsatta resultaten har sparats i ”: wasb: / / / användare/remoteuser/NYCTaxi/ScoredResults”.</span><span class="sxs-lookup"><span data-stu-id="7c61a-134">The scored results have been saved in: "wasb:///user/remoteuser/NYCTaxi/ScoredResults".</span></span> <span data-ttu-id="7c61a-135">Om sökvägen till mappen är felaktig, sparas inte resultat i den mappen.</span><span class="sxs-lookup"><span data-stu-id="7c61a-135">If the path to folder is incorrect, results are not saved in that folder.</span></span>   

> [!NOTE]
> <span data-ttu-id="7c61a-136">Sökvägar för sökvägen kan kopieras och klistras in i platshållare i den här koden från utdata från den sista cellen i den **machine-learning-data-science-spark-data-exploration-modeling.ipynb** bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="7c61a-136">The file path locations can be copied and pasted into the placeholders in this code from the output of the last cell of the **machine-learning-data-science-spark-data-exploration-modeling.ipynb** notebook.</span></span>   
> 
> 

<span data-ttu-id="7c61a-137">Här är koden för att ange katalogsökvägar:</span><span class="sxs-lookup"><span data-stu-id="7c61a-137">Here is the code to set directory paths:</span></span> 

    # LOCATION OF DATA TO BE SCORED (TEST DATA)
    taxi_test_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Test.tsv";

    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 

    # SET SCORDED RESULT DIRECTORY PATH
    # NOTE THE LAST BACKSLASH IN THIS PATH IS NEEDED
    scoredResultDir = "wasb:///user/remoteuser/NYCTaxi/ScoredResults/"; 

    # FILE LOCATIONS FOR THE MODELS TO BE SCORED
    logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-04-1817_40_35.796789"
    linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-04-1817_44_00.993832"
    randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-04-1817_42_58.899412"
    randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-04-1817_44_27.204734"
    BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-04-1817_43_16.354770"
    BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-04-1817_44_46.206262"

    # RECORD START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="7c61a-138">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-138">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-139">datetime.datetime (2016, 4, 25, 23, 56, 19, 229403)</span><span class="sxs-lookup"><span data-stu-id="7c61a-139">datetime.datetime(2016, 4, 25, 23, 56, 19, 229403)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="7c61a-140">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="7c61a-140">Import libraries</span></span>
<span data-ttu-id="7c61a-141">Ange spark kontext och importera nödvändiga bibliotek med följande kod</span><span class="sxs-lookup"><span data-stu-id="7c61a-141">Set spark context and import necessary libraries with the following code</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="7c61a-142">Förinställda Spark-kontexten och PySpark användbara</span><span class="sxs-lookup"><span data-stu-id="7c61a-142">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="7c61a-143">PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext.</span><span class="sxs-lookup"><span data-stu-id="7c61a-143">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="7c61a-144">Så du behöver inte ange Spark eller Hive-kontexterna explicit innan du börjar arbeta med programmet som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="7c61a-144">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="7c61a-145">Dessa är tillgängliga för dig som standard.</span><span class="sxs-lookup"><span data-stu-id="7c61a-145">These are available for you by default.</span></span> <span data-ttu-id="7c61a-146">Dessa kontexter är:</span><span class="sxs-lookup"><span data-stu-id="7c61a-146">These contexts are:</span></span>

* <span data-ttu-id="7c61a-147">SC - Spark</span><span class="sxs-lookup"><span data-stu-id="7c61a-147">sc - for Spark</span></span> 
* <span data-ttu-id="7c61a-148">sqlContext - för Hive</span><span class="sxs-lookup"><span data-stu-id="7c61a-148">sqlContext - for Hive</span></span>

<span data-ttu-id="7c61a-149">PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%.</span><span class="sxs-lookup"><span data-stu-id="7c61a-149">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="7c61a-150">Det finns två kommandon som används i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="7c61a-150">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="7c61a-151">**%% lokala** anges att koden i efterföljande rader körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="7c61a-151">**%%local** Specified that the code in subsequent lines is executed locally.</span></span> <span data-ttu-id="7c61a-152">Koden måste vara giltig Python-kod.</span><span class="sxs-lookup"><span data-stu-id="7c61a-152">Code must be valid Python code.</span></span>
* <span data-ttu-id="7c61a-153">**%% sql -o<variable name>**</span><span class="sxs-lookup"><span data-stu-id="7c61a-153">**%%sql -o <variable name>**</span></span> 
* <span data-ttu-id="7c61a-154">Kör en Hive-fråga mot sqlContext.</span><span class="sxs-lookup"><span data-stu-id="7c61a-154">Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="7c61a-155">Om parametern -o skickas resultatet av frågan sparas i den %% lokala Python kontext som en Pandas dataframe.</span><span class="sxs-lookup"><span data-stu-id="7c61a-155">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas dataframe.</span></span>

<span data-ttu-id="7c61a-156">För mer information om kärnor för Jupyter-anteckningsböcker och den fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="7c61a-156">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="ingest-data-and-create-a-cleaned-data-frame"></a><span data-ttu-id="7c61a-157">Mata in data och skapa en rensade data-ram</span><span class="sxs-lookup"><span data-stu-id="7c61a-157">Ingest data and create a cleaned data frame</span></span>
<span data-ttu-id="7c61a-158">Det här avsnittet innehåller koden för en serie uppgifter som krävs för att mata in data som ska mätas.</span><span class="sxs-lookup"><span data-stu-id="7c61a-158">This section contains the code for a series of tasks required to ingest the data to be scored.</span></span> <span data-ttu-id="7c61a-159">Läsa i en domänansluten 0,1% exemplet i taxi resa och avgiften filen (som lagras som en TSV-fil), format data och skapar sedan en ren data ram.</span><span class="sxs-lookup"><span data-stu-id="7c61a-159">Read in a joined 0.1% sample of the taxi trip and fare file (stored as a .tsv file), format the data, and then creates a clean data frame.</span></span>

<span data-ttu-id="7c61a-160">Taxi resa och avgiften filer kopplas baserat på det sätt som anges i den: [Team datavetenskap Process i praktiken: med HDInsight Hadoop-kluster](machine-learning-data-science-process-hive-walkthrough.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="7c61a-160">The taxi trip and fare files were joined based on the procedure provided in the: [The Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md) topic.</span></span>

    # INGEST DATA AND CREATE A CLEANED DATA FRAME

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_test_file = sc.textFile(taxi_test_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
    taxi_header = taxi_test_file.filter(lambda l: "medallion" in l)

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_temp = taxi_test_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))

    # GET SCHEMA OF THE FILE FROM HEADER
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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="7c61a-161">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-161">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-162">Åtgången tid för att köra ovanför cellen: 46.37 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-162">Time taken to execute above cell: 46.37 seconds</span></span>

## <a name="prepare-data-for-scoring-in-spark"></a><span data-ttu-id="7c61a-163">Förbered data för Poängberäkningen i Spark</span><span class="sxs-lookup"><span data-stu-id="7c61a-163">Prepare data for scoring in Spark</span></span>
<span data-ttu-id="7c61a-164">Det här avsnittet visar hur du index, koda och skala kategoriska funktioner för att förbereda dem för användning i MLlib övervakad inlärning algoritmer för klassificering och regression.</span><span class="sxs-lookup"><span data-stu-id="7c61a-164">This section shows how to index, encode, and scale categorical features to prepare them for use in MLlib supervised learning algorithms for classification and regression.</span></span>

### <a name="feature-transformation-index-and-encode-categorical-features-for-input-into-models-for-scoring"></a><span data-ttu-id="7c61a-165">Funktionen omvandling: indexera och koda kategoriska funktioner för indata i modeller för poäng.</span><span class="sxs-lookup"><span data-stu-id="7c61a-165">Feature transformation: index and encode categorical features for input into models for scoring</span></span>
<span data-ttu-id="7c61a-166">Det här avsnittet visar hur du indexera kategoriska data med hjälp av en `StringIndexer` och koda funktioner med `OneHotEncoder` indata i modeller.</span><span class="sxs-lookup"><span data-stu-id="7c61a-166">This section shows how to index categorical data using a `StringIndexer` and encode features with `OneHotEncoder` input into the models.</span></span>

<span data-ttu-id="7c61a-167">Den [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) kodar en strängkolumn etiketter till en kolumn med etiketten index.</span><span class="sxs-lookup"><span data-stu-id="7c61a-167">The [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) encodes a string column of labels to a column of label indices.</span></span> <span data-ttu-id="7c61a-168">Indexen ordnas efter etikett frekvenser.</span><span class="sxs-lookup"><span data-stu-id="7c61a-168">The indices are ordered by label frequencies.</span></span> 

<span data-ttu-id="7c61a-169">Den [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) mappar etikett index i en kolumn till en kolumn med binära angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="7c61a-169">The [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="7c61a-170">Den här kodningen kan algoritmer som räknar kontinuerlig viktiga funktioner, till exempel logistic regression som ska tillämpas på kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-170">This encoding allows algorithms that expect continuous valued features, such as logistic regression, to be applied to categorical features.</span></span>

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
    model = stringIndexer.fit(taxi_df_test_with_newFeatures) # Input data-frame is the cleaned one from above
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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="7c61a-171">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-171">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-172">Åtgången tid för att köra ovanför cellen: 5.37 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-172">Time taken to execute above cell: 5.37 seconds</span></span>

### <a name="create-rdd-objects-with-feature-arrays-for-input-into-models"></a><span data-ttu-id="7c61a-173">Skapa RDD objekt med funktionen matriser för indata i modeller</span><span class="sxs-lookup"><span data-stu-id="7c61a-173">Create RDD objects with feature arrays for input into models</span></span>
<span data-ttu-id="7c61a-174">Det här avsnittet innehåller kod som visar hur du indexera kategoriska textdata som RDD objekt och en hot koda den så den kan användas för att träna och testa MLlib logistic regression och trädet-baserade modeller.</span><span class="sxs-lookup"><span data-stu-id="7c61a-174">This section contains code that shows how to index categorical text data as an RDD object and one-hot encode it so it can be used to train and test MLlib logistic regression and tree-based models.</span></span> <span data-ttu-id="7c61a-175">Indexerade data lagras i [flexibel distribuerade datauppsättningen (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objekt.</span><span class="sxs-lookup"><span data-stu-id="7c61a-175">The indexed data is stored in [Resilient Distributed Dataset (RDD)](http://spark.apache.org/docs/latest/api/java/org/apache/spark/rdd/RDD.html) objects.</span></span> <span data-ttu-id="7c61a-176">Det här är den grundläggande framställningen i Spark.</span><span class="sxs-lookup"><span data-stu-id="7c61a-176">These are the basic abstraction in Spark.</span></span> <span data-ttu-id="7c61a-177">En RDD-objektet representerar en ändras, partitionerad samling element kan användas på parallellt med Spark.</span><span class="sxs-lookup"><span data-stu-id="7c61a-177">An RDD object represents an immutable, partitioned collection of elements that can be operated on in parallel with Spark.</span></span>

<span data-ttu-id="7c61a-178">Den innehåller också kod som visar hur du skalar data med den `StandardScalar` som tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD), en populär algoritm för att träna en mängd olika machine learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="7c61a-178">It also contains code that shows how to scale data with the `StandardScalar` provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of machine learning models.</span></span> <span data-ttu-id="7c61a-179">Den [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) används för att skala funktioner enhet variansen.</span><span class="sxs-lookup"><span data-stu-id="7c61a-179">The [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) is used to scale the features to unit variance.</span></span> <span data-ttu-id="7c61a-180">Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål-funktionen.</span><span class="sxs-lookup"><span data-stu-id="7c61a-180">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> 

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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="7c61a-181">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-181">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-182">Åtgången tid för att köra ovanför cellen: 11.72 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-182">Time taken to execute above cell: 11.72 seconds</span></span>

## <a name="score-with-the-logistic-regression-model-and-save-output-to-blob"></a><span data-ttu-id="7c61a-183">Poängsätta med Logistic regressionsmodellen och spara utdata till blob</span><span class="sxs-lookup"><span data-stu-id="7c61a-183">Score with the Logistic Regression Model and save output to blob</span></span>
<span data-ttu-id="7c61a-184">Koden i det här avsnittet visar hur att läsa in en Logistic regressionsmodell som har sparats i Azure blob storage och använda den för att förutsäga huruvida ett tips är betald på en taxi resa, poängsätta med standard klassificering, spara och rita resultat för att blob stora ge.</span><span class="sxs-lookup"><span data-stu-id="7c61a-184">The code in this section shows how to load a Logistic Regression Model that has been saved in Azure blob storage and use it to predict whether or not a tip is paid on a taxi trip, score it with standard classification metrics, and then save and plot the results to blob storage.</span></span> <span data-ttu-id="7c61a-185">Poängsatta resultatet lagras i RDD objekt.</span><span class="sxs-lookup"><span data-stu-id="7c61a-185">The scored results are stored in RDD objects.</span></span> 

    # SCORE AND EVALUATE LOGISTIC REGRESSION MODEL

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    ## LOAD SAVED MODEL
    savedModel = LogisticRegressionModel.load(sc, logisticRegFileLoc)
    predictions = oneHotTESTbinary.map(lambda features: (float(savedModel.predict(features))))

    ## SAVE SCORED RESULTS (RDD) TO BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp + ".txt";
    dirfilename = scoredResultDir + logisticregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="7c61a-186">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-186">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-187">Åtgången tid för att köra ovanför cellen: 19.22 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-187">Time taken to execute above cell: 19.22 seconds</span></span>

## <a name="score-a-linear-regression-model"></a><span data-ttu-id="7c61a-188">Poängsätta en linjär regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="7c61a-188">Score a Linear Regression Model</span></span>
<span data-ttu-id="7c61a-189">Vi använde [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) för att träna en linjär regressionsmodell med hjälp av Stokastisk toning härstammar (SGD) för optimering för att förutsäga mängden tips betald.</span><span class="sxs-lookup"><span data-stu-id="7c61a-189">We used [LinearRegressionWithSGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) to train a linear regression model using Stochastic Gradient Descent (SGD) for optimization to predict the amount of tip paid.</span></span> 

<span data-ttu-id="7c61a-190">Koden i det här avsnittet visar hur du läsa in en linjär regressionsmodell från Azure blob storage, poängsätta använda skalade variabler och spara resultaten tillbaka till blob.</span><span class="sxs-lookup"><span data-stu-id="7c61a-190">The code in this section shows how to load a Linear Regression Model from Azure blob storage, score using scaled variables, and then save the results back to the blob.</span></span>

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

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="7c61a-191">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-191">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-192">Åtgången tid för att köra ovanför cellen: 16.63 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-192">Time taken to execute above cell: 16.63 seconds</span></span>

## <a name="score-classification-and-regression-random-forest-models"></a><span data-ttu-id="7c61a-193">Poängsätta klassificering och regression slumpmässiga skog modeller</span><span class="sxs-lookup"><span data-stu-id="7c61a-193">Score classification and regression Random Forest Models</span></span>
<span data-ttu-id="7c61a-194">Koden i det här avsnittet visar hur du läser in sparade klassificeringen och regression slumpmässiga skog modeller sparas i Azure blob storage poängsätta deras prestanda med standard klassificerare och regression mått och spara resultaten tillbaka till blob storage.</span><span class="sxs-lookup"><span data-stu-id="7c61a-194">The code in this section shows how to load the saved classification and regression Random Forest Models saved in Azure blob storage, score their performance with standard classifier and regression measures, and then save the results back to blob storage.</span></span>

<span data-ttu-id="7c61a-195">[Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="7c61a-195">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="7c61a-196">De kombinera många beslutsträd för att minska risken för overfitting.</span><span class="sxs-lookup"><span data-stu-id="7c61a-196">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="7c61a-197">Slumpmässiga skogar kan hantera kategoriska funktioner, utöka för multiklass-baserad klassificering, kräver inte funktionen skalning och kan avbilda icke-linjärt och funktion interaktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-197">Random forests can handle categorical features, extend to the multiclass classification setting, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="7c61a-198">Slumpmässiga skogar är en av de mest framgångsrika modeller för klassificering och regression för maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="7c61a-198">Random forests are one of the most successful machine learning models for classification and regression.</span></span>

<span data-ttu-id="7c61a-199">[Spark.mllib](http://spark.apache.org/mllib/) stöder slumpmässiga skogar för binära och multiklass-baserad klassificering och regression som använder både kontinuerliga och kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-199">[spark.mllib](http://spark.apache.org/mllib/) supports random forests for binary and multiclass classification and for regression, using both continuous and categorical features.</span></span> 

    # SCORE RANDOM FOREST MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES    
    from pyspark.mllib.tree import RandomForest, RandomForestModel


    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB
    savedModel = RandomForestModel.load(sc, randomForestRegFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + rfregressionfilename;
    predictions.saveAsTextFile(dirfilename)

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="7c61a-200">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-200">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-201">Åtgången tid för att köra ovanför cellen: 31.07 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-201">Time taken to execute above cell: 31.07 seconds</span></span>

## <a name="score-classification-and-regression-gradient-boosting-tree-models"></a><span data-ttu-id="7c61a-202">Poängsätta klassificering och regression toning förstärkning trädet modeller</span><span class="sxs-lookup"><span data-stu-id="7c61a-202">Score classification and regression Gradient Boosting Tree Models</span></span>
<span data-ttu-id="7c61a-203">Koden i det här avsnittet visar hur du läsa in klassificering och regression toning förstärkning trädet modeller från Azure blob storage, poängsätta deras prestanda med standard klassificerare och regression mått och spara resultaten tillbaka till blob storage.</span><span class="sxs-lookup"><span data-stu-id="7c61a-203">The code in this section shows how to load classification and regression Gradient Boosting Tree Models from Azure blob storage, score their performance with standard classifier and regression measures, and then save the results back to blob storage.</span></span> 

<span data-ttu-id="7c61a-204">**Spark.mllib** GBTs har stöd för binär klassificering och regression som använder både kontinuerliga och kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-204">**spark.mllib** supports GBTs for binary classification and for regression, using both continuous and categorical features.</span></span> 

<span data-ttu-id="7c61a-205">[Toning förstärkning träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="7c61a-205">[Gradient Boosting Trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="7c61a-206">GBTs träna beslutsträd upprepade gånger för att minimera dataförlust funktionen.</span><span class="sxs-lookup"><span data-stu-id="7c61a-206">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="7c61a-207">GBTs kan hantera kategoriska funktioner kräver inte funktionen skalning och kan avbilda icke-linjärt och funktion interaktioner.</span><span class="sxs-lookup"><span data-stu-id="7c61a-207">GBTs can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="7c61a-208">De kan också användas i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="7c61a-208">They can also be used in a multiclass-classification setting.</span></span>

    # SCORE GRADIENT BOOSTING TREE MODELS FOR CLASSIFICATION AND REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT MLLIB LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # CLASSIFICATION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    #LOAD AND SCORE THE MODEL
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeClassificationFileLoc)
    predictions = savedModel.predict(indexedTESTbinary)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btclassificationfilename;
    predictions.saveAsTextFile(dirfilename)


    # REGRESSION: LOAD SAVED MODEL, SCORE AND SAVE RESULTS BACK TO BLOB

    # LOAD AND SCORE MODEL 
    savedModel = GradientBoostedTreesModel.load(sc, BoostedTreeRegressionFileLoc)
    predictions = savedModel.predict(indexedTESTreg)

    # SAVE RESULTS
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp + ".txt";
    dirfilename = scoredResultDir + btregressionfilename;
    predictions.saveAsTextFile(dirfilename)


    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="7c61a-209">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-209">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-210">Åtgången tid för att köra ovanför cellen: 14.6 sekunder</span><span class="sxs-lookup"><span data-stu-id="7c61a-210">Time taken to execute above cell: 14.6 seconds</span></span>

## <a name="clean-up-objects-from-memory-and-print-scored-file-locations"></a><span data-ttu-id="7c61a-211">Ta bort objekt från minne och Skriv ut bedömas filplatser</span><span class="sxs-lookup"><span data-stu-id="7c61a-211">Clean up objects from memory and print scored file locations</span></span>
    # UNPERSIST OBJECTS CACHED IN MEMORY
    taxi_df_test_cleaned.unpersist()
    indexedTESTbinary.unpersist();
    oneHotTESTbinary.unpersist();
    indexedTESTreg.unpersist();
    oneHotTESTreg.unpersist();
    oneHotTESTregScaled.unpersist();


    # PRINT OUT PATH TO SCORED OUTPUT FILES
    print "logisticRegFileLoc: " + logisticregressionfilename;
    print "linearRegFileLoc: " + linearregressionfilename;
    print "randomForestClassificationFileLoc: " + rfclassificationfilename;
    print "randomForestRegFileLoc: " + rfregressionfilename;
    print "BoostedTreeClassificationFileLoc: " + btclassificationfilename;
    print "BoostedTreeRegressionFileLoc: " + btregressionfilename;


<span data-ttu-id="7c61a-212">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="7c61a-212">**OUTPUT:**</span></span>

<span data-ttu-id="7c61a-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span><span class="sxs-lookup"><span data-stu-id="7c61a-213">logisticRegFileLoc: LogisticRegressionWithLBFGS_2016-05-0317_22_38.953814.txt</span></span>

<span data-ttu-id="7c61a-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span><span class="sxs-lookup"><span data-stu-id="7c61a-214">linearRegFileLoc: LinearRegressionWithSGD_2016-05-0317_22_58.878949</span></span>

<span data-ttu-id="7c61a-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span><span class="sxs-lookup"><span data-stu-id="7c61a-215">randomForestClassificationFileLoc: RandomForestClassification_2016-05-0317_23_15.939247.txt</span></span>

<span data-ttu-id="7c61a-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span><span class="sxs-lookup"><span data-stu-id="7c61a-216">randomForestRegFileLoc: RandomForestRegression_2016-05-0317_23_31.459140.txt</span></span>

<span data-ttu-id="7c61a-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span><span class="sxs-lookup"><span data-stu-id="7c61a-217">BoostedTreeClassificationFileLoc: GradientBoostingTreeClassification_2016-05-0317_23_49.648334.txt</span></span>

<span data-ttu-id="7c61a-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span><span class="sxs-lookup"><span data-stu-id="7c61a-218">BoostedTreeRegressionFileLoc: GradientBoostingTreeRegression_2016-05-0317_23_56.860740.txt</span></span>

## <a name="consume-spark-models-through-a-web-interface"></a><span data-ttu-id="7c61a-219">Använda Spark modeller via ett webbgränssnitt</span><span class="sxs-lookup"><span data-stu-id="7c61a-219">Consume Spark Models through a web interface</span></span>
<span data-ttu-id="7c61a-220">Spark tillhandahåller en mekanism för att skicka via fjärranslutning batchjobb eller interaktiva frågor via ett REST-gränssnitt med en komponent som kallas Livius.</span><span class="sxs-lookup"><span data-stu-id="7c61a-220">Spark provides a mechanism to remotely submit batch jobs or interactive queries through a REST interface with a component called Livy.</span></span> <span data-ttu-id="7c61a-221">Livius är aktiverad som standard på HDInsight Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7c61a-221">Livy is enabled by default on your HDInsight Spark cluster.</span></span> <span data-ttu-id="7c61a-222">Läs mer på Livius: [skicka Spark jobb via fjärranslutning med Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="7c61a-222">For more information on Livy, see: [Submit Spark jobs remotely using Livy](../hdinsight/hdinsight-apache-spark-livy-rest-interface.md).</span></span> 

<span data-ttu-id="7c61a-223">Du kan använda Livius för att skicka ett jobb som batch-resultat från en fjärrdator en fil som lagras i en Azure blob och skriver sedan resultaten till en annan blob.</span><span class="sxs-lookup"><span data-stu-id="7c61a-223">You can use Livy to remotely submit a job that batch scores a file that is stored in an Azure blob and then writes the results to another blob.</span></span> <span data-ttu-id="7c61a-224">Om du vill göra detta måste överföra du Python-skriptet från</span><span class="sxs-lookup"><span data-stu-id="7c61a-224">To do this, you upload the Python script from</span></span>  
<span data-ttu-id="7c61a-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) till blob med Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7c61a-225">[GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/Spark/Python/ConsumeGBNYCReg.py) to the blob of the Spark cluster.</span></span> <span data-ttu-id="7c61a-226">Du kan använda ett verktyg som **Microsoft Azure Lagringsutforskaren** eller **AzCopy** att kopiera skriptet till kluster-blob.</span><span class="sxs-lookup"><span data-stu-id="7c61a-226">You can use a tool like **Microsoft Azure Storage Explorer** or **AzCopy** to copy the script to the cluster blob.</span></span> <span data-ttu-id="7c61a-227">I vårt fall vi överföra skriptet till ***wasb:///example/python/ConsumeGBNYCReg.py***.</span><span class="sxs-lookup"><span data-stu-id="7c61a-227">In our case we uploaded the script to ***wasb:///example/python/ConsumeGBNYCReg.py***.</span></span>   

> [!NOTE]
> <span data-ttu-id="7c61a-228">Åtkomstnycklar som du behöver finns på portalen för storage-konto som är associerade med Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7c61a-228">The access keys that you need can be found on the portal for the storage account associated with the Spark cluster.</span></span> 
> 
> 

<span data-ttu-id="7c61a-229">När du har överfört till den här platsen kör skriptet i Spark-kluster i en distribuerad kontext.</span><span class="sxs-lookup"><span data-stu-id="7c61a-229">Once uploaded to this location, this script runs within the Spark cluster in a distributed context.</span></span> <span data-ttu-id="7c61a-230">Den läser in modellen och kör förutsägelser på inkommande filer baserat på modellen.</span><span class="sxs-lookup"><span data-stu-id="7c61a-230">It loads the model and runs predictions on input files based on the model.</span></span>  

<span data-ttu-id="7c61a-231">Du kan anropa skriptet från en fjärrdator genom att göra en enkel HTTPS/REST-begäran på Livius.</span><span class="sxs-lookup"><span data-stu-id="7c61a-231">You can invoke this script remotely by making a simple HTTPS/REST request on Livy.</span></span>  <span data-ttu-id="7c61a-232">Här är ett curl-kommando för att konstruera HTTP-begäran att anropa Python-skriptet från en fjärrdator.</span><span class="sxs-lookup"><span data-stu-id="7c61a-232">Here is a curl command to construct the HTTP request to invoke the Python script remotely.</span></span> <span data-ttu-id="7c61a-233">Ersätt CLUSTERLOGIN, CLUSTERPASSWORD, KLUSTERNAMN med lämpliga värden för ditt Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="7c61a-233">Replace CLUSTERLOGIN, CLUSTERPASSWORD, CLUSTERNAME with the appropriate values for your Spark cluster.</span></span>

    # CURL COMMAND TO INVOKE PYTHON SCRIPT WITH HTTP REQUEST

    curl -k --user "CLUSTERLOGIN:CLUSTERPASSWORD" -X POST --data "{\"file\": \"wasb:///example/python/ConsumeGBNYCReg.py\"}" -H "Content-Type: application/json" https://CLUSTERNAME.azurehdinsight.net/livy/batches

<span data-ttu-id="7c61a-234">Du kan använda alla språk på fjärrsystemet för att anropa Spark-jobb via Livius genom att göra ett enkelt HTTPS-anrop med grundläggande autentisering.</span><span class="sxs-lookup"><span data-stu-id="7c61a-234">You can use any language on the remote system to invoke the Spark job through Livy by making a simple HTTPS call with Basic Authentication.</span></span>   

> [!NOTE]
> <span data-ttu-id="7c61a-235">Det är praktiskt att använda biblioteket Python begäranden för denna http-samtal, men det är inte installerad som standard i Azure Functions.</span><span class="sxs-lookup"><span data-stu-id="7c61a-235">It would be convenient to use the Python Requests library when making this HTTP call, but it is not currently installed by default in Azure Functions.</span></span> <span data-ttu-id="7c61a-236">Så att äldre HTTP-biblioteken används i stället.</span><span class="sxs-lookup"><span data-stu-id="7c61a-236">So older HTTP libraries are used instead.</span></span>   
> 
> 

<span data-ttu-id="7c61a-237">Här är Python-kod för HTTP-anropet:</span><span class="sxs-lookup"><span data-stu-id="7c61a-237">Here is the Python code for the HTTP call:</span></span>

    #MAKE AN HTTPS CALL ON LIVY. 

    import os

    # OLDER HTTP LIBRARIES USED HERE INSTEAD OF THE REQUEST LIBRARY AS THEY ARE AVAILBLE BY DEFAULT
    import httplib, urllib, base64

    # REPLACE VALUE WITH ONES FOR YOUR SPARK CLUSTER
    host = '<spark cluster name>.azurehdinsight.net:443'
    username='<username>'
    password='<password>'

    #AUTHORIZATION
    conn = httplib.HTTPSConnection(host)
    auth = base64.encodestring('%s:%s' % (username, password)).replace('\n', '')
    headers = {'Content-Type': 'application/json', 'Authorization': 'Basic %s' % auth}

    # SPECIFY THE PYTHON SCRIPT TO RUN ON THE SPARK CLUSTER
    # IN THE FILE PARAMETER OF THE JSON POST REQUEST BODY
    r=conn.request("POST", '/livy/batches', '{"file": "wasb:///example/python/ConsumeGBNYCReg.py"}', headers )
    response = conn.getresponse().read()
    print(response)
    conn.close()


<span data-ttu-id="7c61a-238">Du kan också lägga till den här Python-kod till [Azure Functions](https://azure.microsoft.com/documentation/services/functions/) att utlösa ett Spark-jobbet som poäng en blob baserat på olika händelser, t.ex. en timer, skapa eller uppdatera en blob.</span><span class="sxs-lookup"><span data-stu-id="7c61a-238">You can also add this Python code to [Azure Functions](https://azure.microsoft.com/documentation/services/functions/) to trigger a Spark job submission that scores a blob based on various events like a timer, creation, or update of a blob.</span></span> 

<span data-ttu-id="7c61a-239">Om du föredrar en kod ledigt klienten upplevelse med den [Azure Logikappar](https://azure.microsoft.com/documentation/services/app-service/logic/) att anropa Spark batchbedömningsjobbet genom att definiera en HTTP-åtgärd på den **Logic Apps Designer** och ange dess parametrar.</span><span class="sxs-lookup"><span data-stu-id="7c61a-239">If you prefer a code free client experience, use the [Azure Logic Apps](https://azure.microsoft.com/documentation/services/app-service/logic/) to invoke the Spark batch scoring by defining an HTTP action on the **Logic Apps Designer** and setting its parameters.</span></span> 

* <span data-ttu-id="7c61a-240">Skapa en ny Logikapp från Azure-portalen genom att välja **+ ny** -> **webb + mobilt** -> **Logikapp**.</span><span class="sxs-lookup"><span data-stu-id="7c61a-240">From Azure portal, create a new Logic App by selecting **+New** -> **Web + Mobile** -> **Logic App**.</span></span> 
* <span data-ttu-id="7c61a-241">Så att den **Logic Apps Designer**, ange namnet på den logik och App Service-Plan.</span><span class="sxs-lookup"><span data-stu-id="7c61a-241">To bring up the **Logic Apps Designer**, enter the name of the Logic App and App Service Plan.</span></span>
* <span data-ttu-id="7c61a-242">Välj en HTTP-åtgärd och ange de parametrar som visas i följande bild:</span><span class="sxs-lookup"><span data-stu-id="7c61a-242">Select an HTTP action and enter the parameters shown in the following figure:</span></span>

![Logic Apps Designer](./media/machine-learning-data-science-spark-model-consumption/spark-logica-app-client.png)

## <a name="whats-next"></a><span data-ttu-id="7c61a-244">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7c61a-244">What's next?</span></span>
<span data-ttu-id="7c61a-245">**Korsvalidering och hyperparameter omfattande**: se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parametern.</span><span class="sxs-lookup"><span data-stu-id="7c61a-245">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping.</span></span>

