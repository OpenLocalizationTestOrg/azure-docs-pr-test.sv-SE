---
title: "aaaData undersökning och modellering med Spark | Microsoft Docs"
description: "Showcases hello datagranskning och integrera affärsmodelleringsfunktioner av hello Spark MLlib toolkit på Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b989b918-5ba5-4696-b8d0-76ae510a23f4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: cf5cee4575053f5954b08ca659dfc39c53798371
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-exploration-and-modeling-with-spark"></a><span data-ttu-id="646b9-103">Datagranskning och modellering med Spark</span><span class="sxs-lookup"><span data-stu-id="646b9-103">Data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="646b9-104">Den här genomgången använder HDInsight Spark toodo datagranskning och binär klassificering och regressionen modeling uppgifter på ett urval av hello NYC Taxitransport resa och färdavgiften 2013 dataset.</span><span class="sxs-lookup"><span data-stu-id="646b9-104">This walkthrough uses HDInsight Spark toodo data exploration and binary classification and regression modeling tasks on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span>  <span data-ttu-id="646b9-105">Den vägleder dig genom stegen för hello av hello [datavetenskap Process](http://aka.ms/datascienceprocess), slutpunkt-till-slutpunkt, med hjälp av ett HDInsight Spark-kluster för bearbetning och Azure-blobbar toostore hello data och hello modeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="646b9-106">hello processen utforskar visualizes data som tillkommit från ett Azure Storage Blob och förbereder hello data toobuild förutsägelsemodeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="646b9-107">Dessa modeller är versionen med hello Spark MLlib toolkit toodo binär klassificering och regressionen modeling uppgifter.</span><span class="sxs-lookup"><span data-stu-id="646b9-107">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span>

* <span data-ttu-id="646b9-108">Hej **binär klassificering** uppgift är toopredict huruvida ett tips betalat för hello resa.</span><span class="sxs-lookup"><span data-stu-id="646b9-108">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="646b9-109">Hej **regression** uppgift är toopredict hello mängden hello tips baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-109">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="646b9-110">hello modeller som vi använder inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd:</span><span class="sxs-lookup"><span data-stu-id="646b9-110">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="646b9-111">[Linjär regression med SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) är en linjär regressionsmodell som använder en Stokastisk toning härstammar (SGD)-metod och skalning toopredict hello tips belopp betald för optimering och funktion.</span><span class="sxs-lookup"><span data-stu-id="646b9-111">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="646b9-112">[Logistic regression med LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) eller ”logit” regression är en regressionsmodell som kan användas när hello beroende variabel är kategoriska toodo dataklassificering.</span><span class="sxs-lookup"><span data-stu-id="646b9-112">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="646b9-113">LBFGS är en kvasi Karlsson optimering algoritm som uppskattar hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritm med en begränsad mängd ledigt diskutrymme på datorn och som ofta används i machine learning.</span><span class="sxs-lookup"><span data-stu-id="646b9-113">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="646b9-114">[Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="646b9-114">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="646b9-115">De kombinera många decision trees tooreduce hello risken för overfitting.</span><span class="sxs-lookup"><span data-stu-id="646b9-115">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="646b9-116">Slumpmässiga skogar används för regression och klassificering kan hantera kategoriska funktioner och kan utökas toohello multiklass-baserad klassificering inställningen.</span><span class="sxs-lookup"><span data-stu-id="646b9-116">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="646b9-117">De kräver inte funktionen skalning och är kan toocapture icke-linjärt och funktionen interaktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-117">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="646b9-118">Slumpmässiga skogar är en av hello mest framgångsrika maskininlärning modeller för klassificering och regression.</span><span class="sxs-lookup"><span data-stu-id="646b9-118">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="646b9-119">[Toningen ökat träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="646b9-119">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="646b9-120">GBTs train beslut träd upprepade gånger toominimize en förlust-funktion.</span><span class="sxs-lookup"><span data-stu-id="646b9-120">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="646b9-121">GBTs för regression och klassificering kan hantera kategoriska funktioner, kräver inte funktionen skalning och att kan toocapture icke-linjärt och interaktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-121">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="646b9-122">De kan också användas i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="646b9-122">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="646b9-123">hello modellering steg innehåller också kod visar hur tootrain, utvärdera och spara varje typ av modellen.</span><span class="sxs-lookup"><span data-stu-id="646b9-123">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="646b9-124">Python har använt toocode hello lösningen och tooshow hello relevanta områden.</span><span class="sxs-lookup"><span data-stu-id="646b9-124">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span>   

> [!NOTE]
> <span data-ttu-id="646b9-125">Även om hello Spark MLlib toolkit är utformad toowork på stora datamängder, används ett relativt litet exempel (cirka 30 Mb med 170K rader, om 0,1% hello ursprungliga NYC datauppsättningen) här i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="646b9-125">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="646b9-126">hello Övning som anges här körs effektivt (i 10 minuter) på ett HDInsight-kluster med 2 arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="646b9-126">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="646b9-127">hello kan har samma kod, med mindre ändringar vara används tooprocess större-datamängder med lämpliga ändringar för cachelagring av data i minnet och ändra hello klusterstorleken.</span><span class="sxs-lookup"><span data-stu-id="646b9-127">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="646b9-128">Krav</span><span class="sxs-lookup"><span data-stu-id="646b9-128">Prerequisites</span></span>
<span data-ttu-id="646b9-129">Du behöver ett Azure-konto och en Spark 1.6 (eller Spark 2.0) HDInsight-kluster toocomplete den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="646b9-129">You need an Azure account and a Spark 1.6 (or Spark 2.0) HDInsight cluster toocomplete this walkthrough.</span></span> <span data-ttu-id="646b9-130">Se hello [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md) anvisningar för hur toosatisfy dessa krav.</span><span class="sxs-lookup"><span data-stu-id="646b9-130">See hello [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md) for instructions on how toosatisfy these requirements.</span></span> <span data-ttu-id="646b9-131">Avsnittet innehåller också en beskrivning av hello NYC 2013 Taxi data används här och anvisningar om hur tooexecute kod från en Jupyter-anteckningsbok på hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="646b9-131">That topic also contains a description of hello NYC 2013 Taxi data used here and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster.</span></span> 

## <a name="spark-clusters-and-notebooks"></a><span data-ttu-id="646b9-132">Spark-kluster och bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="646b9-132">Spark clusters and notebooks</span></span>
<span data-ttu-id="646b9-133">Konfigurationsstegen och kod finns i den här genomgången för att använda ett HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="646b9-133">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="646b9-134">Men Jupyter-anteckningsböcker som har angetts för både HDInsight Spark 1.6 och 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="646b9-134">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="646b9-135">En beskrivning av hello anteckningsböcker och länkar toothem finns i hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för hello GitHub-lagringsplats som innehåller dessa.</span><span class="sxs-lookup"><span data-stu-id="646b9-135">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="646b9-136">Dessutom hello koden här hello länkade anteckningsböcker är generisk och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="646b9-136">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="646b9-137">Om du inte använder HDInsight Spark hello konfiguration och hantering av steg kan vara skiljer sig från vad som anges här.</span><span class="sxs-lookup"><span data-stu-id="646b9-137">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="646b9-138">För enkelhetens skull följer hello länkar toohello Jupyter-anteckningsböcker för Spark 1.6 (toobe köras i hello pySpark-kerneln av hello Jupyter-anteckningsbok server) och Spark 2.0 (toobe kör i hello pySpark3 kernel av hello Jupyter-anteckningsbok server):</span><span class="sxs-lookup"><span data-stu-id="646b9-138">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 (toobe run in hello pySpark kernel of hello Jupyter Notebook server) and  Spark 2.0 (toobe run in hello pySpark3 kernel of hello Jupyter Notebook server):</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="646b9-139">Spark 1.6 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="646b9-139">Spark 1.6 notebooks</span></span>

<span data-ttu-id="646b9-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): innehåller information om hur tooperform datagranskning modellering och bedömningen med flera olika algoritmer.</span><span class="sxs-lookup"><span data-stu-id="646b9-140">[pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Provides information on how tooperform data exploration, modeling, and scoring with several different algorithms.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="646b9-141">Spark 2.0 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="646b9-141">Spark 2.0 notebooks</span></span>
<span data-ttu-id="646b9-142">hello regression och klassificering uppgifter som implementeras med hjälp av ett 2.0 Spark-kluster finns i separata anteckningsböcker och hello klassificering anteckningsboken använder en annan datauppsättning:</span><span class="sxs-lookup"><span data-stu-id="646b9-142">hello regression and classification tasks that are implemented using a Spark 2.0 cluster are in separate notebooks and hello classification notebook uses a different data set:</span></span>

- <span data-ttu-id="646b9-143">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): den här filen innehåller information om hur tooperform datagranskning modellering och bedömningen i Spark 2.0-kluster med hello NYC Taxi resa och avgiften-datamängd beskrivs [här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="646b9-143">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span> <span data-ttu-id="646b9-144">Den här anteckningsboken kan vara en bra utgångspunkt för att snabbt utforska hello koden som vi har angetts för Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="646b9-144">This notebook may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> <span data-ttu-id="646b9-145">En mer detaljerad anteckningsbok analyserar hello NYC Taxi data, finns hello nästa anteckningsboken i den här listan.</span><span class="sxs-lookup"><span data-stu-id="646b9-145">For a more detailed notebook analyzes hello NYC Taxi data, see hello next notebook in this list.</span></span> <span data-ttu-id="646b9-146">Se hello anteckningar efter den här listan som jämför dessa datorer.</span><span class="sxs-lookup"><span data-stu-id="646b9-146">See hello notes following this list that compare these notebooks.</span></span> 
- <span data-ttu-id="646b9-147">[Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello NYC Taxi resa och avgiften datauppsättning beskrivs [ här](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span><span class="sxs-lookup"><span data-stu-id="646b9-147">[Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello NYC Taxi trip and fare data-set described [here](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).</span></span>
- <span data-ttu-id="646b9-148">[Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): den här filen visar hur tooperform data wrangling (Spark SQL och dataframe operations), utforskning modellering och bedömningen med hello välkända flygbolag i tid avvikelse DataSet från 2011 och 2012.</span><span class="sxs-lookup"><span data-stu-id="646b9-148">[Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): This file shows how tooperform data wrangling (Spark SQL and dataframe operations), exploration, modeling and scoring using hello well-known Airline On-time departure dataset from 2011 and 2012.</span></span> <span data-ttu-id="646b9-149">Vi integrerad hello flygbolag dataset med hello flygplats väder data (t.ex. vindhastigheten, temperatur, höjd etc.) tidigare toomodeling, så att dessa väder-funktioner kan ingå i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="646b9-149">We integrated hello airline dataset with hello airport weather data (e.g. windspeed, temperature, altitude etc.) prior toomodeling, so these weather features can be included in hello model.</span></span>

<!-- -->

> [!NOTE]
> <span data-ttu-id="646b9-150">hello flygbolag datamängden har lagts till toohello Spark 2.0 anteckningsböcker toobetter illustrera hello användningen av algoritmer för klassificering.</span><span class="sxs-lookup"><span data-stu-id="646b9-150">hello airline dataset was added toohello Spark 2.0 notebooks toobetter illustrate hello use of classification algorithms.</span></span> <span data-ttu-id="646b9-151">Se följande länkar för information om flygbolag i tid avvikelse dataset och väder datamängd hello:</span><span class="sxs-lookup"><span data-stu-id="646b9-151">See hello following links for information about airline on-time departure dataset and weather dataset:</span></span>

>- <span data-ttu-id="646b9-152">Flygbolag i tid avvikelse data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span><span class="sxs-lookup"><span data-stu-id="646b9-152">Airline on-time departure data: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)</span></span>

>- <span data-ttu-id="646b9-153">Flygplats väder data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span><span class="sxs-lookup"><span data-stu-id="646b9-153">Airport weather data: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)</span></span> 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
<span data-ttu-id="646b9-154">hello Spark 2.0 anteckningsböcker på hello NYC taxi och flygbolag svarta fördröjning-datauppsättningar kan ta 10 minuter eller mer toorun (beroende på hello storleken på ditt HDI-kluster).</span><span class="sxs-lookup"><span data-stu-id="646b9-154">hello Spark 2.0 notebooks on hello NYC taxi and airline flight delay data-sets can take 10 mins or more toorun (depending on hello size of your HDI cluster).</span></span> <span data-ttu-id="646b9-155">hello första anteckningsboken i hello ovanför listan visar flera olika aspekter av hello datagranskning, visualisering och ML-modell utbildning i en bärbar dator som tar mindre tid toorun med ned provtagning NYC datamängd, i vilken hello taxi och avgiften filer har redan anslutits: [ Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) anteckningsboken tar en mycket kortare tid toofinish (2-3 minuter) och kan vara en bra utgångspunkt för snabbt utforska hello koden har vi föreskrivs Spark 2.0.</span><span class="sxs-lookup"><span data-stu-id="646b9-155">hello first notebook in hello above list shows many aspects of hello data exploration, visualization and ML model training in a notebook that takes less time toorun with down-sampled NYC data set, in which hello taxi and fare files have been pre-joined: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) This notebook takes a much shorter time toofinish (2-3 mins) and may be a good starting point for quickly exploring hello code we have provided for Spark 2.0.</span></span> 

<!-- -->

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<!-- -->

> [!NOTE]
<span data-ttu-id="646b9-156">hello beskrivningar nedan är relaterade toousing Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="646b9-156">hello descriptions below are related toousing Spark 1.6.</span></span> <span data-ttu-id="646b9-157">Spark 2.0 versioner, Använd hello anteckningsböcker beskrivs och länkarna ovan.</span><span class="sxs-lookup"><span data-stu-id="646b9-157">For Spark 2.0 versions, please use hello notebooks described and linked above.</span></span> 

<!-- -->

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="646b9-158">: Lagringsplatser, bibliotek och hello förinställning Spark-kontexten</span><span class="sxs-lookup"><span data-stu-id="646b9-158">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="646b9-159">Spark är kan tooread och skriva tooAzure Lagringsblob (även kallat WASB).</span><span class="sxs-lookup"><span data-stu-id="646b9-159">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="646b9-160">Så något av dina befintliga data som lagras där kan bearbetas med Spark och hello resultatet lagras igen i WASB.</span><span class="sxs-lookup"><span data-stu-id="646b9-160">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="646b9-161">toosave modeller eller filer i WASB hello sökväg behöver toobe som angetts på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="646b9-161">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="646b9-162">hello standard behållaren kopplade toohello Spark-kluster kan refereras med en sökväg som börjar med ”: wasb: / / /”.</span><span class="sxs-lookup"><span data-stu-id="646b9-162">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="646b9-163">Andra platser refererar till ”wasb: / /”.</span><span class="sxs-lookup"><span data-stu-id="646b9-163">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="646b9-164">Ange katalogsökvägar för lagringsplatser i WASB</span><span class="sxs-lookup"><span data-stu-id="646b9-164">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="646b9-165">hello följande kodexempel anger hello platsen för hello data toobe läsa och sparas hello sökväg för hello modellen lagring directory toowhich hello modellen utdata:</span><span class="sxs-lookup"><span data-stu-id="646b9-165">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";

    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/" 


### <a name="import-libraries"></a><span data-ttu-id="646b9-166">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="646b9-166">Import libraries</span></span>
<span data-ttu-id="646b9-167">Konfigurera kräver också importera nödvändiga bibliotek.</span><span class="sxs-lookup"><span data-stu-id="646b9-167">Set up also requires importing necessary libraries.</span></span> <span data-ttu-id="646b9-168">Ange spark kontext och importera nödvändiga bibliotek med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="646b9-168">Set spark context and import necessary libraries with hello following code:</span></span>

    # IMPORT LIBRARIES
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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="646b9-169">Förinställda Spark-kontexten och PySpark användbara</span><span class="sxs-lookup"><span data-stu-id="646b9-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="646b9-170">Hej PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext.</span><span class="sxs-lookup"><span data-stu-id="646b9-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="646b9-171">Så behöver du inte tooset hello Spark- eller Hive kontexter explicit innan du börjar arbeta med hello-program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="646b9-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="646b9-172">Dessa kontexter är tillgängliga för dig som standard.</span><span class="sxs-lookup"><span data-stu-id="646b9-172">These contexts are available for you by default.</span></span> <span data-ttu-id="646b9-173">Dessa kontexter är:</span><span class="sxs-lookup"><span data-stu-id="646b9-173">These contexts are:</span></span>

* <span data-ttu-id="646b9-174">SC - Spark</span><span class="sxs-lookup"><span data-stu-id="646b9-174">sc - for Spark</span></span> 
* <span data-ttu-id="646b9-175">sqlContext - för Hive</span><span class="sxs-lookup"><span data-stu-id="646b9-175">sqlContext - for Hive</span></span>

<span data-ttu-id="646b9-176">Hej PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%.</span><span class="sxs-lookup"><span data-stu-id="646b9-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="646b9-177">Det finns två kommandon som används i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="646b9-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="646b9-178">**%% lokala** anger att hello koden i efterföljande rader är toobe körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="646b9-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="646b9-179">Koden måste vara giltig Python-kod.</span><span class="sxs-lookup"><span data-stu-id="646b9-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="646b9-180">**%% sql -o <variable name>**  kör en Hive-fråga mot hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="646b9-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="646b9-181">Om hello -o parametern överförs hello resultatet av hello fråga sparas i hello %% lokala Python kontext som en Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="646b9-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="646b9-182">För mer information om hello kärnor för Jupyter-anteckningsböcker och hello fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="646b9-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="646b9-183">Datapåfyllning från offentliga blob</span><span class="sxs-lookup"><span data-stu-id="646b9-183">Data ingestion from public blob</span></span>
<span data-ttu-id="646b9-184">hello första steget i processen för hello datavetenskap är tooingest hello data toobe analyseras från källor där är finns i din data från kartläggning av naturresurser och modellering miljö.</span><span class="sxs-lookup"><span data-stu-id="646b9-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where is resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="646b9-185">hello-miljö är Spark i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="646b9-185">hello environment is Spark in this walkthrough.</span></span> <span data-ttu-id="646b9-186">Det här avsnittet innehåller hello koden toocomplete en serie aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="646b9-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="646b9-187">mata in hello data exempel toobe modelleras</span><span class="sxs-lookup"><span data-stu-id="646b9-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="646b9-188">Läs i hello inkommande dataset (som lagras som en TSV-fil)</span><span class="sxs-lookup"><span data-stu-id="646b9-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="646b9-189">Formatera och ren hello data</span><span class="sxs-lookup"><span data-stu-id="646b9-189">format and clean hello data</span></span>
* <span data-ttu-id="646b9-190">Skapa och cachelagra objekt (RDDs eller ramar) i minnet</span><span class="sxs-lookup"><span data-stu-id="646b9-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="646b9-191">registrera den som en temp-tabell i SQL-kontext.</span><span class="sxs-lookup"><span data-stu-id="646b9-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="646b9-192">Här är hello koden för datapåfyllning.</span><span class="sxs-lookup"><span data-stu-id="646b9-192">Here is hello code for data ingestion.</span></span>

    # INGEST DATA

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF hello FILE FROM HEADER
    schema_string = taxi_train_file.first()
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

    # PARSE FIELDS AND CONVERT DATA TYPE FOR SOME FIELDS
    taxi_header = taxi_train_file.filter(lambda l: "medallion" in l)
    taxi_temp = taxi_train_file.subtract(taxi_header).map(lambda k: k.split("\t"))\
            .map(lambda p: (p[0],p[1],p[2],p[3],p[4],p[5],p[6],int(p[7]),int(p[8]),int(p[9]),int(p[10]),
                            float(p[11]),float(p[12]),p[13],p[14],p[15],p[16],p[17],p[18],float(p[19]),
                            float(p[20]),float(p[21]),float(p[22]),float(p[23]),float(p[24]),int(p[25]),int(p[26])))


    # CREATE DATA FRAME
    taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    # CREATE A CLEANED DATA-FRAME BY DROPPING SOME UN-NECESSARY COLUMNS & FILTERING FOR UNDESIRED VALUES OR OUTLIERS
    taxi_df_train_cleaned = taxi_train_df.drop('medallion').drop('hack_license').drop('store_and_fwd_flag').drop('pickup_datetime')\
        .drop('dropoff_datetime').drop('pickup_longitude').drop('pickup_latitude').drop('dropoff_latitude')\
        .drop('dropoff_longitude').drop('tip_class').drop('total_amount').drop('tolls_amount').drop('mta_tax')\
        .drop('direct_distance').drop('surcharge')\
        .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200" )


    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="646b9-193">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-193">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-194">Tidsåtgång tooexecute ovanför cellen: 51.72 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-194">Time taken tooexecute above cell: 51.72 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="646b9-195">Datagranskning & visualiseringen</span><span class="sxs-lookup"><span data-stu-id="646b9-195">Data exploration & visualization</span></span>
<span data-ttu-id="646b9-196">När hello data har trätt i Spark, är hello nästa steg i processen för hello datavetenskap toogain djupare förståelse för hello data via undersökning och visualisering.</span><span class="sxs-lookup"><span data-stu-id="646b9-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="646b9-197">I det här avsnittet undersöka vi hello taxi data med hjälp av SQL-frågor och ritytans hello target-variabler och potentiella funktioner för granskning.</span><span class="sxs-lookup"><span data-stu-id="646b9-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="646b9-198">Mer specifikt Rita vi hello frekvensen av passagerare antal i taxi resor hello frekvensen av tips belopp och hur tips varierar beroende på belopp och typen.</span><span class="sxs-lookup"><span data-stu-id="646b9-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="646b9-199">Rita ett histogram passagerare antal frekvenser i hello prov av taxi resor</span><span class="sxs-lookup"><span data-stu-id="646b9-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="646b9-200">Den här koden och efterföljande kodavsnitt använder SQL magiskt tooquery hello exempel och lokala magiskt tooplot hello data.</span><span class="sxs-lookup"><span data-stu-id="646b9-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="646b9-201">**SQL-magic (`%%sql`)** hello HDInsight PySpark-kerneln stöder enkelt infogade HiveQL frågor mot hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="646b9-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="646b9-202">hello (-o VARIABLE_NAME) argumentet kvarstår hello utdata från hello SQL-frågan som en Pandas DataFrame på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="646b9-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="646b9-203">Det innebär att den är tillgänglig i hello lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="646b9-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="646b9-204">Hej  **`%%local` magic** är används toorun kod lokalt på hello Jupyter servern som hello headnode hello HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="646b9-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="646b9-205">Normalt använder du `%%local` magiskt tillsammans med hello `%%sql` magiskt med parametern -o.</span><span class="sxs-lookup"><span data-stu-id="646b9-205">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with -o parameter.</span></span> <span data-ttu-id="646b9-206">hello -o parametern skulle bevara hello utdata från lokalt hello SQL-frågan och sedan %% lokala magic initierar hello nästa uppsättning kodfragment toorun lokalt mot hello utdata för hello SQL-frågor som sparas lokalt</span><span class="sxs-lookup"><span data-stu-id="646b9-206">hello -o parameter would persist hello output of hello SQL query locally and then %%local magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally</span></span>

<span data-ttu-id="646b9-207">hello utdata visualiseras automatiskt när du har kört hello kod.</span><span class="sxs-lookup"><span data-stu-id="646b9-207">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="646b9-208">Den här frågan hämtar hello resor av passagerare count.</span><span class="sxs-lookup"><span data-stu-id="646b9-208">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts 
    FROM taxi_train 
    WHERE passenger_count > 0 and passenger_count < 7 
    GROUP BY passenger_count 

<span data-ttu-id="646b9-209">Den här koden skapar en lokala data ram från hello frågeresultatet och visar hello data.</span><span class="sxs-lookup"><span data-stu-id="646b9-209">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="646b9-210">Hej `%%local` magic skapar lokala data-ram, `sqlResults`, som kan användas för att rita upp med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="646b9-210">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="646b9-211">Den här PySpark-magic används flera gånger i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="646b9-211">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="646b9-212">Om hello mängden data är stor, bör du prov toocreate en data-ram som får plats i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="646b9-212">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    #CREATE LOCAL DATA-FRAME AND USE FOR MATPLOTLIB PLOTTING

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="646b9-213">Här är hello kod tooplot hello resor av passagerare antal</span><span class="sxs-lookup"><span data-stu-id="646b9-213">Here is hello code tooplot hello trips by passenger counts</span></span>

    # PLOT PASSENGER NUMBER VS. TRIP COUNTS
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

<span data-ttu-id="646b9-214">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-214">**OUTPUT:**</span></span>

![Resa frekvens av passagerare antal](./media/machine-learning-data-science-spark-data-exploration-modeling/trip-freqency-by-passenger-count.png)

<span data-ttu-id="646b9-216">Du kan välja bland flera olika typer av grafik (register, cirkeldiagram, rad, område eller fältet) med hjälp av hello **typen** menyn knapparna i hello anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="646b9-216">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="646b9-217">hello-fältet ritytans visas här.</span><span class="sxs-lookup"><span data-stu-id="646b9-217">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="646b9-218">Rita histogrammet tips belopp och hur mycket tips varierar efter passagerare antal och avgiften belopp.</span><span class="sxs-lookup"><span data-stu-id="646b9-218">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="646b9-219">Använda en SQL-fråga toosample data.</span><span class="sxs-lookup"><span data-stu-id="646b9-219">Use a SQL query toosample data.</span></span>

    #PLOT HISTOGRAM OF TIP AMOUNTS AND VARIATION BY PASSENGER COUNT AND PAYMENT TYPE

    # HIVEQL QUERY AGAINST hello sqlContext
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped 
    FROM taxi_train 
    WHERE passenger_count > 0 
    AND passenger_count < 7 
    AND fare_amount > 0 
    AND fare_amount < 200 
    AND payment_type in ('CSH', 'CRD') 
    AND tip_amount > 0 
    AND tip_amount < 25


<span data-ttu-id="646b9-220">Den här koden cellen använder hello SQL-frågan toocreate tre områden hello data.</span><span class="sxs-lookup"><span data-stu-id="646b9-220">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # HISTOGRAM OF TIP AMOUNTS AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 100, -2, 20])
    plt.show()


<span data-ttu-id="646b9-221">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-221">**OUTPUT:**</span></span> 

![Fördelning av tips](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-distribution.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tips belopp med avgiften belopp](./media/machine-learning-data-science-spark-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="646b9-225">Funktionen teknik, omvandling och data förberedelse för modellering</span><span class="sxs-lookup"><span data-stu-id="646b9-225">Feature engineering, transformation and data preparation for modeling</span></span>
<span data-ttu-id="646b9-226">Det här avsnittet beskriver och hello koden för procedurer används tooprepare data för användning i ML-modell.</span><span class="sxs-lookup"><span data-stu-id="646b9-226">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="646b9-227">Den visar hur toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="646b9-227">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="646b9-228">Skapa en ny funktion med diskretisering timmar till trafik tid buckets</span><span class="sxs-lookup"><span data-stu-id="646b9-228">Create a new feature by binning hours into traffic time buckets</span></span>
* <span data-ttu-id="646b9-229">Index och koda kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="646b9-229">Index and encode categorical features</span></span>
* <span data-ttu-id="646b9-230">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="646b9-230">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="646b9-231">Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="646b9-231">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="646b9-232">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="646b9-232">Feature scaling</span></span>
* <span data-ttu-id="646b9-233">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="646b9-233">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="646b9-234">Skapa en ny funktion med diskretisering timmar till trafik tid buckets</span><span class="sxs-lookup"><span data-stu-id="646b9-234">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="646b9-235">Den här koden visar hur toocreate en ny funktion av diskretisering timmar till trafik tid tidsgrupper och sedan hur toocache hello resulterande data ram i minnet.</span><span class="sxs-lookup"><span data-stu-id="646b9-235">This code shows how toocreate a new feature by binning hours into traffic time buckets and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="646b9-236">Om flexibel distribuerade datauppsättningar (RDDs) och ramar data används flera gånger, leder cachelagring tooimproved körningstider.</span><span class="sxs-lookup"><span data-stu-id="646b9-236">Where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="646b9-237">Därför cachelagra vi RDDs och data ramar i flera steg under hello genomgången.</span><span class="sxs-lookup"><span data-stu-id="646b9-237">Accordingly, we cache RDDs and data-frames at several stages in hello walkthrough.</span></span> 

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night" 
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush" 
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train 
    """
    taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE DATA-FRAME IN MEMORY & MATERIALIZE DF IN MEMORY
    # hello .COUNT() GOES THROUGH hello ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES hello COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

<span data-ttu-id="646b9-238">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-238">**OUTPUT:**</span></span> 

<span data-ttu-id="646b9-239">126050</span><span class="sxs-lookup"><span data-stu-id="646b9-239">126050</span></span>

### <a name="index-and-encode-categorical-features-for-input-into-modeling-functions"></a><span data-ttu-id="646b9-240">Index och koda kategoriska funktioner för indata i modeling funktioner</span><span class="sxs-lookup"><span data-stu-id="646b9-240">Index and encode categorical features for input into modeling functions</span></span>
<span data-ttu-id="646b9-241">Det här avsnittet visas hur tooindex eller koda kategoriska funktioner för indata i hello modeling funktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-241">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="646b9-242">Hej modellering och förutsäga funktioner för MLlib kräver funktioner med kategoriska indata toobe indexerade eller kodade tidigare toouse.</span><span class="sxs-lookup"><span data-stu-id="646b9-242">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="646b9-243">Beroende på hello modellen måste tooindex eller koda dem på olika sätt:</span><span class="sxs-lookup"><span data-stu-id="646b9-243">Depending on hello model, you need tooindex or encode them in different ways:</span></span>  

* <span data-ttu-id="646b9-244">**Trädet-baserade modellering** kräver kategorier toobe kodad som värden (funktion med tre kategorier kan till exempel vara kodad med 0, 1, 2).</span><span class="sxs-lookup"><span data-stu-id="646b9-244">**Tree-based modeling** requires categories toobe encoded as numerical values (for example, a feature with three categories may be encoded with 0, 1, 2).</span></span> <span data-ttu-id="646b9-245">Detta tillhandahålls av Mllib's [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) funktion.</span><span class="sxs-lookup"><span data-stu-id="646b9-245">This is provided by MLlib’s [StringIndexer](http://spark.apache.org/docs/latest/ml-features.html#stringindexer) function.</span></span> <span data-ttu-id="646b9-246">Den här funktionen kodar en strängkolumn för etiketter tooa kolumnen av etikett index som sorteras efter etikett frekvenser.</span><span class="sxs-lookup"><span data-stu-id="646b9-246">This function encodes a string column of labels tooa column of label indices that are ordered by label frequencies.</span></span> <span data-ttu-id="646b9-247">Även om indexeras med numeriska värden för indata- och datahantering hello trädet-baserade algoritmer vara angivna tootreat dem på lämpligt sätt som kategorier.</span><span class="sxs-lookup"><span data-stu-id="646b9-247">Although indexed with numerical values for input and data handling, hello tree-based algorithms can be specified tootreat them appropriately as categories.</span></span> 
* <span data-ttu-id="646b9-248">**Logistic och linjär Regression modeller** kräver en hot kodning, till exempel en funktion med tre kategorier kan expanderas till tre funktionen kolumner med varje som innehåller 0 eller 1 beroende på hello kategori av en observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="646b9-248">**Logistic and Linear Regression models** require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="646b9-249">MLlib ger [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) fungerar toodo ett hot kodning.</span><span class="sxs-lookup"><span data-stu-id="646b9-249">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="646b9-250">Den här kodaren mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="646b9-250">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="646b9-251">Den här kodningen kan algoritmer som räknar numeriska värden funktioner, till exempel logistic regression toobe tillämpas toocategorical funktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-251">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="646b9-252">Här är hello kod tooindex och koda kategoriska funktioner:</span><span class="sxs-lookup"><span data-stu-id="646b9-252">Here is hello code tooindex and encode categorical features:</span></span>

    # INDEX AND ENCODE CATEGORICAL FEATURES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES    
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is hello cleaned one from above
    indexed = model.transform(taxi_df_train_with_newFeatures)
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

    # INDEX AND TRAFFIC TIME BINS
    stringIndexer = StringIndexer(inputCol="TrafficTimeBins", outputCol="TrafficTimeBinsIndex")
    model = stringIndexer.fit(encoded3)
    indexed = model.transform(encoded3)
    encoder = OneHotEncoder(dropLast=False, inputCol="TrafficTimeBinsIndex", outputCol="TrafficTimeBinsVec")
    encodedFinal = encoder.transform(indexed)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-253">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-253">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-254">Tidsåtgång tooexecute ovanför cellen: 1,28 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-254">Time taken tooexecute above cell: 1.28 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="646b9-255">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="646b9-255">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="646b9-256">Det här avsnittet innehåller kod som visar hur tooindex kategoriska text datatypen som en etikett punkt data och koda den så att det kan vara används tootrain och testa MLlib logistic regression och andra klassificering modeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-256">This section contains code that shows how tooindex categorical text data as a labeled point data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="646b9-257">Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) formaterad på ett sätt som krävs av de flesta ML algoritmer i MLlib som indata.</span><span class="sxs-lookup"><span data-stu-id="646b9-257">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="646b9-258">En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.</span><span class="sxs-lookup"><span data-stu-id="646b9-258">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>  

<span data-ttu-id="646b9-259">Det här avsnittet innehåller kod som visar hur tooindex kategoriska textdata som en [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) datatypen och koda den så att det kan vara används tootrain och testa MLlib logistic regression och andra klassificering modeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-259">This section contains code that shows how tooindex categorical text data as a [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) data type and encode it so that it can be used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="646b9-260">Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) som består av en etikett (mål/svar-variabel) och funktionen vector.</span><span class="sxs-lookup"><span data-stu-id="646b9-260">Labeled point objects are Resilient Distributed Datasets (RDD) consisting of a label (target/response variable) and feature vector.</span></span> <span data-ttu-id="646b9-261">Det här formatet krävs av många ML algoritmer i MLlib som indata.</span><span class="sxs-lookup"><span data-stu-id="646b9-261">This format is needed as input by many ML algorithms in MLlib.</span></span>

<span data-ttu-id="646b9-262">Här är hello kod tooindex och koda textfunktioner för binär klassificering.</span><span class="sxs-lookup"><span data-stu-id="646b9-262">Here is hello code tooindex and encode text features for binary classification.</span></span>

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex,
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


<span data-ttu-id="646b9-263">Här är hello kod tooencode och index kategoriska text-funktioner för linjär regressionsanalys.</span><span class="sxs-lookup"><span data-stu-id="646b9-263">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

    # FUNCTIONS FOR REGRESSION WITH TIP AMOUNT AS TARGET VARIABLE

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingRegression(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.TrafficTimeBinsIndex, 
                             line.pickup_hour, line.weekday, line.passenger_count, line.trip_time_in_secs, 
                             line.trip_distance, line.fare_amount])

        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO LINEAR REGRESSION MODELS
    def parseRowOneHotRegression(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                            line.vendorVec.toArray(), line.rateVec.toArray(), 
                                            line.paymentVec.toArray(), line.TrafficTimeBinsVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tip_amount, features)
        return  labPt


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="646b9-264">Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="646b9-264">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="646b9-265">Den här koden skapar ett slumpmässigt dataurval hello (25% används här).</span><span class="sxs-lookup"><span data-stu-id="646b9-265">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="646b9-266">Även om det inte krävs för det här exemplet på grund av toohello storlek på hello dataset, visar vi hur du kan prova här så att du vet hur toouse den för din egen problem vid behov.</span><span class="sxs-lookup"><span data-stu-id="646b9-266">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample here so you know how toouse it for your own problem when needed.</span></span> <span data-ttu-id="646b9-267">När prover är stort, kan detta spara mycket tid när utbildning modeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-267">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="646b9-268">Nästa dela vi hello exempel i en utbildning (här 75%) och en tester del (här 25%) toouse i klassificering och regressionen modeling.</span><span class="sxs-lookup"><span data-stu-id="646b9-268">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.sql.functions import rand

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS (FOR USE LATER IN AN ADVANCED TOPIC)
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary = trainData.map(parseRowIndexingBinary)
    indexedTESTbinary = testData.map(parseRowIndexingBinary)
    oneHotTRAINbinary = trainData.map(parseRowOneHotBinary)
    oneHotTESTbinary = testData.map(parseRowOneHotBinary)

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg = trainData.map(parseRowIndexingRegression)
    indexedTESTreg = testData.map(parseRowIndexingRegression)
    oneHotTRAINreg = trainData.map(parseRowOneHotRegression)
    oneHotTESTreg = testData.map(parseRowOneHotRegression)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-269">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-269">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-270">Tidsåtgång tooexecute ovanför cellen: 0,24 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-270">Time taken tooexecute above cell: 0.24 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="646b9-271">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="646b9-271">Feature scaling</span></span>
<span data-ttu-id="646b9-272">Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="646b9-272">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="646b9-273">hello koden för funktionen skalning använder hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funktioner toounit varians.</span><span class="sxs-lookup"><span data-stu-id="646b9-273">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="646b9-274">Den tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD), en populär algoritm för att träna en mängd andra maskininlärning modeller som reglerats regressioner eller support vector datorer (SVM).</span><span class="sxs-lookup"><span data-stu-id="646b9-274">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD), a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>

> [!NOTE]
> <span data-ttu-id="646b9-275">Vi har hittat hello LinearRegressionWithSGD algoritmen toobe känsliga toofeature skalning.</span><span class="sxs-lookup"><span data-stu-id="646b9-275">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>
> 
> 

<span data-ttu-id="646b9-276">Här är hello kod tooscale variabler för användning med hello reglerats linjär SGD algoritm.</span><span class="sxs-lookup"><span data-stu-id="646b9-276">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

    # FEATURE SCALING

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from pyspark.mllib.linalg import Vectors
    from pyspark.mllib.feature import StandardScaler, StandardScalerModel
    from pyspark.mllib.util import MLUtils

    # SCALE VARIABLES FOR REGULARIZED LINEAR SGD ALGORITHM
    label = oneHotTRAINreg.map(lambda x: x.label)
    features = oneHotTRAINreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTRAINregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    label = oneHotTESTreg.map(lambda x: x.label)
    features = oneHotTESTreg.map(lambda x: x.features)
    scaler = StandardScaler(withMean=False, withStd=True).fit(features)
    dataTMP = label.zip(scaler.transform(features.map(lambda x: Vectors.dense(x.toArray()))))
    oneHotTESTregScaled = dataTMP.map(lambda x: LabeledPoint(x[0], x[1]))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-277">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-277">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-278">Tidsåtgång tooexecute ovanför cellen: 13.17 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-278">Time taken tooexecute above cell: 13.17 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="646b9-279">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="646b9-279">Cache objects in memory</span></span>
<span data-ttu-id="646b9-280">hello tidsåtgång för träning och testning av ML algoritmer kan minskas med cachelagring hello indata ram objekt som används för klassificering, regression, och skalas funktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-280">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression, and scaled features.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.cache()
    indexedTESTbinary.cache()
    oneHotTRAINbinary.cache()
    oneHotTESTbinary.cache()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.cache()
    indexedTESTreg.cache()
    oneHotTRAINreg.cache()
    oneHotTESTreg.cache()

    # SCALED FEATURES
    oneHotTRAINregScaled.cache()
    oneHotTESTregScaled.cache()

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-281">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-281">**OUTPUT:**</span></span> 

<span data-ttu-id="646b9-282">Tidsåtgång tooexecute ovanför cellen: 0,15 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-282">Time taken tooexecute above cell: 0.15 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="646b9-283">Förutsäga huruvida ett tips är betald med binär klassificering modeller</span><span class="sxs-lookup"><span data-stu-id="646b9-283">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="646b9-284">Det här avsnittet visas hur användning tre modeller för hello binär klassificering uppgiften att förutsäga ett tips betalat för en taxi resa eller inte.</span><span class="sxs-lookup"><span data-stu-id="646b9-284">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="646b9-285">hello modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="646b9-285">hello models presented are:</span></span>

* <span data-ttu-id="646b9-286">Reglerats logistic regression</span><span class="sxs-lookup"><span data-stu-id="646b9-286">Regularized logistic regression</span></span> 
* <span data-ttu-id="646b9-287">Slumpmässiga Skogsmodell</span><span class="sxs-lookup"><span data-stu-id="646b9-287">Random forest model</span></span>
* <span data-ttu-id="646b9-288">Toning träd</span><span class="sxs-lookup"><span data-stu-id="646b9-288">Gradient Boosting Trees</span></span>

<span data-ttu-id="646b9-289">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="646b9-289">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="646b9-290">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="646b9-290">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="646b9-291">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="646b9-291">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="646b9-292">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="646b9-292">**Saving model** in blob for future consumption</span></span>

### <a name="classification-using-logistic-regression"></a><span data-ttu-id="646b9-293">Klassificering med logistic regression</span><span class="sxs-lookup"><span data-stu-id="646b9-293">Classification using logistic regression</span></span>
<span data-ttu-id="646b9-294">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="646b9-294">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

<span data-ttu-id="646b9-295">**Hej logistic regression träningsmodell med KA och hyperparameter omfattande**</span><span class="sxs-lookup"><span data-stu-id="646b9-295">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics


    # CREATE MODEL WITH ONE SET OF PARAMETERS
    logitModel = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, iterations=20, initialWeights=None, 
                                                   regParam=0.01, regType='l2', intercept=True, corrections=10, 
                                                   tolerance=0.0001, validateData=True, numClasses=2)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitModel.weights))
    print("Intercept: " + str(logitModel.intercept))

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="646b9-296">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-296">**OUTPUT:**</span></span> 

<span data-ttu-id="646b9-297">Koefficienter: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="646b9-297">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="646b9-298">Skärningspunkt:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="646b9-298">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="646b9-299">Tidsåtgång tooexecute ovanför cellen: 14.43 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-299">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="646b9-300">**Utvärdera hello binär klassificering modellen med standard**</span><span class="sxs-lookup"><span data-stu-id="646b9-300">**Evaluate hello binary classification model with standard metrics**</span></span>

    #EVALUATE LOGISTIC REGRESSION MODEL WITH LBFGS

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # PREDICT ON TEST DATA WITH MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitModel.predict(lp.features)), lp.label))

    # INSTANTIATE METRICS OBJECT
    metrics = BinaryClassificationMetrics(predictionAndLabels)

    # AREA UNDER PRECISION-RECALL CURVE
    print("Area under PR = %s" % metrics.areaUnderPR)

    # AREA UNDER ROC CURVE
    print("Area under ROC = %s" % metrics.areaUnderROC)
    metrics = MulticlassMetrics(predictionAndLabels)

    # OVERALL STATISTICS
    precision = metrics.precision()
    recall = metrics.recall()
    f1Score = metrics.fMeasure()
    print("Summary Stats")
    print("Precision = %s" % precision)
    print("Recall = %s" % recall)
    print("F1 Score = %s" % f1Score)


    ## SAVE MODEL WITH DATE-STAMP
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;
    logitModel.save(sc, dirfilename);

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitModel.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="646b9-301">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-301">**OUTPUT:**</span></span> 

<span data-ttu-id="646b9-302">Området under PR = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="646b9-302">Area under PR = 0.985297691373</span></span>

<span data-ttu-id="646b9-303">Området under ROC = 0.983714670256</span><span class="sxs-lookup"><span data-stu-id="646b9-303">Area under ROC = 0.983714670256</span></span>

<span data-ttu-id="646b9-304">Sammanfattande statistik</span><span class="sxs-lookup"><span data-stu-id="646b9-304">Summary Stats</span></span>

<span data-ttu-id="646b9-305">Precision = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="646b9-305">Precision = 0.984304060189</span></span>

<span data-ttu-id="646b9-306">Återkalla = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="646b9-306">Recall = 0.984304060189</span></span>

<span data-ttu-id="646b9-307">F1 Poängsätta = 0.984304060189</span><span class="sxs-lookup"><span data-stu-id="646b9-307">F1 Score = 0.984304060189</span></span>

<span data-ttu-id="646b9-308">Tidsåtgång tooexecute ovanför cellen: 57.61 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-308">Time taken tooexecute above cell: 57.61 seconds</span></span>

<span data-ttu-id="646b9-309">**Rita hello ROC-kurvan.**</span><span class="sxs-lookup"><span data-stu-id="646b9-309">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="646b9-310">Hej *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="646b9-310">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="646b9-311">*tmp_results* kan använda toodo frågor och spara resultatet i hello sqlResults data-ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="646b9-311">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="646b9-312">Här är hello kod.</span><span class="sxs-lookup"><span data-stu-id="646b9-312">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="646b9-313">Här är hello kod toomake förutsägelser och ritytans hello ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="646b9-313">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    # MAKE PREDICTIONS
    predictions_pddf = test_predictions.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVE
    plt.figure(figsize=(5,5))
    plt.plot(fpr, tpr, label='ROC curve (area = %0.2f)' % roc_auc)
    plt.plot([0, 1], [0, 1], 'k--')
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate')
    plt.ylabel('True Positive Rate')
    plt.title('ROC Curve')
    plt.legend(loc="lower right")
    plt.show()


<span data-ttu-id="646b9-314">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-314">**OUTPUT:**</span></span>

![Logistic regression ROC curve.png](./media/machine-learning-data-science-spark-data-exploration-modeling/logistic-regression-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="646b9-316">Slumpmässiga skog klassificering</span><span class="sxs-lookup"><span data-stu-id="646b9-316">Random forest classification</span></span>
<span data-ttu-id="646b9-317">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig Skogsmodell som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="646b9-317">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING RANDOM FOREST

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # TRAIN RANDOMFOREST MODEL
    rfModel = RandomForest.trainClassifier(indexedTRAINbinary, numClasses=2, 
                                           categoricalFeaturesInfo=categoricalFeaturesInfo,
                                           numTrees=25, featureSubsetStrategy="auto",
                                           impurity='gini', maxDepth=5, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRINT TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = rfModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfclassificationfilename = "RandomForestClassification_" + datestamp;
    dirfilename = modelDir + rfclassificationfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-318">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-318">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-319">Området under ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="646b9-319">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="646b9-320">Tidsåtgång tooexecute ovanför cellen: 31.09 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-320">Time taken tooexecute above cell: 31.09 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="646b9-321">Toning den träd klassificering</span><span class="sxs-lookup"><span data-stu-id="646b9-321">Gradient boosting trees classification</span></span>
<span data-ttu-id="646b9-322">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="646b9-322">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    #PREDICT WHETHER A TIP IS PAID OR NOT USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo, numIterations=5)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # AREA UNDER ROC CURVE
    metrics = BinaryClassificationMetrics(predictionAndLabels)
    print("Area under ROC = %s" % metrics.areaUnderROC)

    # PERSIST MODEL IN A BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btclassificationfilename = "GradientBoostingTreeClassification_" + datestamp;
    dirfilename = modelDir + btclassificationfilename;

    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="646b9-323">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-323">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-324">Området under ROC = 0.985297691373</span><span class="sxs-lookup"><span data-stu-id="646b9-324">Area under ROC = 0.985297691373</span></span>

<span data-ttu-id="646b9-325">Tidsåtgång tooexecute ovanför cellen: 19.76 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-325">Time taken tooexecute above cell: 19.76 seconds</span></span>

## <a name="predict-tip-amounts-for-taxi-trips-with-regression-models"></a><span data-ttu-id="646b9-326">Förutsäga tips belopp för taxi resor med regression modeller</span><span class="sxs-lookup"><span data-stu-id="646b9-326">Predict tip amounts for taxi trips with regression models</span></span>
<span data-ttu-id="646b9-327">Det här avsnittet visas hur användning tre modeller för hello regression uppgiften att förutsäga hello mängden hello tips för en taxi färd baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="646b9-327">This section shows how use three models for hello regression task of predicting hello amount of hello tip paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="646b9-328">hello modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="646b9-328">hello models presented are:</span></span>

* <span data-ttu-id="646b9-329">Reglerats linjär regression</span><span class="sxs-lookup"><span data-stu-id="646b9-329">Regularized linear regression</span></span>
* <span data-ttu-id="646b9-330">Slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="646b9-330">Random forest</span></span>
* <span data-ttu-id="646b9-331">Toning träd</span><span class="sxs-lookup"><span data-stu-id="646b9-331">Gradient Boosting Trees</span></span>

<span data-ttu-id="646b9-332">Dessa modeller beskrivs i hello introduktion.</span><span class="sxs-lookup"><span data-stu-id="646b9-332">These models were described in hello introduction.</span></span> <span data-ttu-id="646b9-333">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="646b9-333">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="646b9-334">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="646b9-334">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="646b9-335">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="646b9-335">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="646b9-336">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="646b9-336">**Saving model** in blob for future consumption</span></span>

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="646b9-337">Linjär regression med SGD</span><span class="sxs-lookup"><span data-stu-id="646b9-337">Linear regression with SGD</span></span>
<span data-ttu-id="646b9-338">hello kod i det här avsnittet visar hur skalas toouse funktioner tootrain en linjär regression som använder stokastisk toning härstammar (SGD) för optimering, och hur tooscore, utvärdera och spara hello modellen i Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="646b9-338">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="646b9-339">Det kan finnas problem med hello konvergens LinearRegressionWithSGD modeller i vår erfarenhet och parametrar måste toobe ändras/optimerad noggrant för att erhålla en giltig modell.</span><span class="sxs-lookup"><span data-stu-id="646b9-339">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="646b9-340">Skalning av variabler avsevärt hjälper med konvergens.</span><span class="sxs-lookup"><span data-stu-id="646b9-340">Scaling of variables significantly helps with convergence.</span></span> 
> 
> 

    #PREDICT TIP AMOUNTS USING LINEAR REGRESSION WITH SGD

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES tooTRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(linearModel.weights))
    print("Intercept: " + str(linearModel.intercept))

    # SCORE ON SCALED TEST DATA-SET & EVALUATE
    predictionAndLabels = oneHotTESTregScaled.map(lambda lp: (float(linearModel.predict(lp.features)), lp.label))
    testMetrics = RegressionMetrics(predictionAndLabels)

    # PRINT TEST METRICS
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL WITH DATE-STAMP IN hello DEFAULT BLOB FOR hello CLUSTER
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-341">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-341">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-342">Koefficienter: [0.00457675809917,-0.0226314167349,-0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981,-0.000987181489428,-0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995,-0.00990211159703,-0.00637410344522, 0.545083566179,-0.536756072402, 0.0105762393099,-0.0130117577055, 0.0129304737772,-0.00171065945959]</span><span class="sxs-lookup"><span data-stu-id="646b9-342">Coefficients: [0.00457675809917, -0.0226314167349, -0.0191910355236, 0.246793409578, 0.312047890459, 0.359634405999, 0.00928692253981, -0.000987181489428, -0.0888306617845, 0.0569376211553, 0.115519551711, 0.149250164995, -0.00990211159703, -0.00637410344522, 0.545083566179, -0.536756072402, 0.0105762393099, -0.0130117577055, 0.0129304737772, -0.00171065945959]</span></span>

<span data-ttu-id="646b9-343">Fånga: 0.853872718283</span><span class="sxs-lookup"><span data-stu-id="646b9-343">Intercept: 0.853872718283</span></span>

<span data-ttu-id="646b9-344">RMSE = 1.24190115863</span><span class="sxs-lookup"><span data-stu-id="646b9-344">RMSE = 1.24190115863</span></span>

<span data-ttu-id="646b9-345">R-sqr = 0.608017146081</span><span class="sxs-lookup"><span data-stu-id="646b9-345">R-sqr = 0.608017146081</span></span>

<span data-ttu-id="646b9-346">Tidsåtgång tooexecute ovanför cellen: 58,42 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-346">Time taken tooexecute above cell: 58.42 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="646b9-347">Slumpmässiga skog regression</span><span class="sxs-lookup"><span data-stu-id="646b9-347">Random Forest regression</span></span>
<span data-ttu-id="646b9-348">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig skog regression som beräknar tips belopp för hello NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="646b9-348">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts tip amount for hello NYC taxi trip data.</span></span>

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    ## UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    ## PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    rfregressionfilename = "RandomForestRegression_" + datestamp;
    dirfilename = modelDir + rfregressionfilename;

    rfModel.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-349">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-349">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-350">RMSE = 0.891209218139</span><span class="sxs-lookup"><span data-stu-id="646b9-350">RMSE = 0.891209218139</span></span>

<span data-ttu-id="646b9-351">R-sqr = 0.759661334921</span><span class="sxs-lookup"><span data-stu-id="646b9-351">R-sqr = 0.759661334921</span></span>

<span data-ttu-id="646b9-352">Tidsåtgång tooexecute ovanför cellen: 49.21 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-352">Time taken tooexecute above cell: 49.21 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="646b9-353">Toning den träd regression</span><span class="sxs-lookup"><span data-stu-id="646b9-353">Gradient boosting trees regression</span></span>
<span data-ttu-id="646b9-354">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar tips belopp för hello NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="646b9-354">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="646b9-355">** Träna och utvärdera **</span><span class="sxs-lookup"><span data-stu-id="646b9-355">**Train and evaluate **</span></span>

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    ## TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    ## EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    # TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # CONVER RESULTS tooDF AND REGISER TEMP TABLE
    test_predictions = sqlContext.createDataFrame(predictionAndLabels)
    test_predictions.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="646b9-356">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-356">**OUTPUT:**</span></span>

<span data-ttu-id="646b9-357">RMSE = 0.908473148639</span><span class="sxs-lookup"><span data-stu-id="646b9-357">RMSE = 0.908473148639</span></span>

<span data-ttu-id="646b9-358">R-sqr = 0.753835096681</span><span class="sxs-lookup"><span data-stu-id="646b9-358">R-sqr = 0.753835096681</span></span>

<span data-ttu-id="646b9-359">Tidsåtgång tooexecute ovanför cellen: 34.52 sekunder</span><span class="sxs-lookup"><span data-stu-id="646b9-359">Time taken tooexecute above cell: 34.52 seconds</span></span>

<span data-ttu-id="646b9-360">**Rita**</span><span class="sxs-lookup"><span data-stu-id="646b9-360">**Plot**</span></span>

<span data-ttu-id="646b9-361">*tmp_results* registreras som en Hive-tabell i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="646b9-361">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="646b9-362">Resultat från hello tabellen är resultatet i hello *sqlResults* data ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="646b9-362">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="646b9-363">Här är hello kod</span><span class="sxs-lookup"><span data-stu-id="646b9-363">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results

<span data-ttu-id="646b9-364">Här är hello kod tooplot hello data med hjälp av hello Jupyter-server.</span><span class="sxs-lookup"><span data-stu-id="646b9-364">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    import numpy as np

    # PLOT 
    ax = test_predictions_pddf.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(test_predictions_pddf['_1'], test_predictions_pddf['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(test_predictions_pddf['_1'], fit[0] * test_predictions_pddf['_1'] + fit[1], color='magenta')
    plt.axis([-1, 20, -1, 20])
    plt.show(ax)


<span data-ttu-id="646b9-365">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="646b9-365">**OUTPUT:**</span></span>

![Aktuella-vs-förutsade-tips-belopp](./media/machine-learning-data-science-spark-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="clean-up-objects-from-memory"></a><span data-ttu-id="646b9-367">Rensa objekt från minnet</span><span class="sxs-lookup"><span data-stu-id="646b9-367">Clean up objects from memory</span></span>
<span data-ttu-id="646b9-368">Använd `unpersist()` toodelete objekt som cachelagrats i minnet.</span><span class="sxs-lookup"><span data-stu-id="646b9-368">Use `unpersist()` toodelete objects cached in memory.</span></span>

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()

    # FOR BINARY CLASSIFICATION TRAINING AND TESTING
    indexedTRAINbinary.unpersist()
    indexedTESTbinary.unpersist()
    oneHotTRAINbinary.unpersist()
    oneHotTESTbinary.unpersist()

    # FOR REGRESSION TRAINING AND TESTING
    indexedTRAINreg.unpersist()
    indexedTESTreg.unpersist()
    oneHotTRAINreg.unpersist()
    oneHotTESTreg.unpersist()

    # SCALED FEATURES
    oneHotTRAINregScaled.unpersist()
    oneHotTESTregScaled.unpersist()


## <a name="record-storage-locations-of-hello-models-for-consumption-and-scoring"></a><span data-ttu-id="646b9-369">Posten lagringsplatser för hello modeller för användnings- och poängsättning</span><span class="sxs-lookup"><span data-stu-id="646b9-369">Record storage locations of hello models for consumption and scoring</span></span>
<span data-ttu-id="646b9-370">tooconsume och poäng en oberoende datauppsättning beskrivs i hello [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md) avsnittet, behöver du toocopy och klistra in filnamn som innehåller hello sparade modeller som skapas här i hello Förbrukning Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="646b9-370">tooconsume and score an independent dataset described in hello [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) topic, you need toocopy and paste these file names containing hello saved models created here into hello Consumption Jupyter notebook.</span></span> <span data-ttu-id="646b9-371">Här är hello kod tooprint ut hello sökvägar toomodel filer du behöver det.</span><span class="sxs-lookup"><span data-stu-id="646b9-371">Here is hello code tooprint out hello paths toomodel files you need there.</span></span>

    # MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="646b9-372">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="646b9-372">**OUTPUT**</span></span>

<span data-ttu-id="646b9-373">logisticRegFileLoc = modelDir + ”LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568”</span><span class="sxs-lookup"><span data-stu-id="646b9-373">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0317_03_23.516568"</span></span>

<span data-ttu-id="646b9-374">linearRegFileLoc = modelDir + ”LinearRegressionWithSGD_2016-05-0317_05_21.577773”</span><span class="sxs-lookup"><span data-stu-id="646b9-374">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0317_05_21.577773"</span></span>

<span data-ttu-id="646b9-375">randomForestClassificationFileLoc = modelDir + ”RandomForestClassification_2016-05-0317_04_11.950206”</span><span class="sxs-lookup"><span data-stu-id="646b9-375">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0317_04_11.950206"</span></span>

<span data-ttu-id="646b9-376">randomForestRegFileLoc = modelDir + ”RandomForestRegression_2016-05-0317_06_08.723736”</span><span class="sxs-lookup"><span data-stu-id="646b9-376">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0317_06_08.723736"</span></span>

<span data-ttu-id="646b9-377">BoostedTreeClassificationFileLoc = modelDir + ”GradientBoostingTreeClassification_2016-05-0317_04_36.346583”</span><span class="sxs-lookup"><span data-stu-id="646b9-377">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0317_04_36.346583"</span></span>

<span data-ttu-id="646b9-378">BoostedTreeRegressionFileLoc = modelDir + ”GradientBoostingTreeRegression_2016-05-0317_06_51.737282”</span><span class="sxs-lookup"><span data-stu-id="646b9-378">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0317_06_51.737282"</span></span>

## <a name="whats-next"></a><span data-ttu-id="646b9-379">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="646b9-379">What's next?</span></span>
<span data-ttu-id="646b9-380">Nu när du har skapat regression och klassificering modeller med hello Spark MlLib, är du redo toolearn hur tooscore och utvärdera dessa modeller.</span><span class="sxs-lookup"><span data-stu-id="646b9-380">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span> <span data-ttu-id="646b9-381">hello avancerade datagranskning och modellering anteckningsboken dyker ned djupare i inklusive korsvalidering, hyper-parametern omfattande och modellerar utvärdering.</span><span class="sxs-lookup"><span data-stu-id="646b9-381">hello advanced data exploration and modeling notebook dives deeper into including cross-validation, hyper-parameter sweeping, and model evaluation.</span></span> 

<span data-ttu-id="646b9-382">**Modellen förbrukning:** toolearn hur tooscore och utvärdera hello klassificering och regression modeller som skapats i det här avsnittet, finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="646b9-382">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

<span data-ttu-id="646b9-383">**Korsvalidering och hyperparameter omfattande**: se [avancerade datagranskning och modellering med Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) på hur modeller kan vara tränas med omfattande korsvalidering och hyper-parameter</span><span class="sxs-lookup"><span data-stu-id="646b9-383">**Cross-validation and hyperparameter sweeping**: See [Advanced data exploration and modeling with Spark](machine-learning-data-science-spark-advanced-data-exploration-modeling.md) on how models can be trained using cross-validation and hyper-parameter sweeping</span></span>

