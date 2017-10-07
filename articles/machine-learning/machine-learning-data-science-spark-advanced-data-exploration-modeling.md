---
title: aaaAdvanced datagranskning och modellering med Spark | Microsoft Docs
description: "Använda HDInsight Spark toodo datagranskning och träna binär klassificering och regression modeller med korsvalidering och hyperparameter optimering."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: f90d9a80-4eaf-437b-a914-23514390cd60
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: 055c342857fd732633cec9810de69cee61db973d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="0a908-103">Avancerad datagranskning och modellering med Spark</span><span class="sxs-lookup"><span data-stu-id="0a908-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="0a908-104">Den här genomgången använder HDInsight Spark toodo data från kartläggning av naturresurser och train binär klassificering och regression modeller med korsvalidering och hyperparameter optimering på ett urval av hello NYC Taxitransport resa och färdavgiften 2013 dataset.</span><span class="sxs-lookup"><span data-stu-id="0a908-104">This walkthrough uses HDInsight Spark toodo data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of hello NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="0a908-105">Den vägleder dig genom stegen för hello av hello [datavetenskap Process](http://aka.ms/datascienceprocess), slutpunkt-till-slutpunkt, med hjälp av ett HDInsight Spark-kluster för bearbetning och Azure-blobbar toostore hello data och hello modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-105">It walks you through hello steps of hello [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs toostore hello data and hello models.</span></span> <span data-ttu-id="0a908-106">hello processen utforskar visualizes data som tillkommit från ett Azure Storage Blob och förbereder hello data toobuild förutsägelsemodeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-106">hello process explores and visualizes data brought in from an Azure Storage Blob and then prepares hello data toobuild predictive models.</span></span> <span data-ttu-id="0a908-107">Python har använt toocode hello lösningen och tooshow hello relevanta områden.</span><span class="sxs-lookup"><span data-stu-id="0a908-107">Python has been used toocode hello solution and tooshow hello relevant plots.</span></span> <span data-ttu-id="0a908-108">Dessa modeller är versionen med hello Spark MLlib toolkit toodo binär klassificering och regressionen modeling uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0a908-108">These models are build using hello Spark MLlib toolkit toodo binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="0a908-109">Hej **binär klassificering** uppgift är toopredict huruvida ett tips betalat för hello resa.</span><span class="sxs-lookup"><span data-stu-id="0a908-109">hello **binary classification** task is toopredict whether or not a tip is paid for hello trip.</span></span> 
* <span data-ttu-id="0a908-110">Hej **regression** uppgift är toopredict hello mängden hello tips baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-110">hello **regression** task is toopredict hello amount of hello tip based on other tip features.</span></span> 

<span data-ttu-id="0a908-111">hello modellering steg innehåller också kod visar hur tootrain, utvärdera och spara varje typ av modellen.</span><span class="sxs-lookup"><span data-stu-id="0a908-111">hello modeling steps also contain code showing how tootrain, evaluate, and save each type of model.</span></span> <span data-ttu-id="0a908-112">hello avsnittet beskrivs några av hello samma bakgrund som hello [datagranskning och modellering med Spark](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a908-112">hello topic covers some of hello same ground as hello [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="0a908-113">Men det är mer ”avancerad” den också använder korsvalidering med hyperparameter omfattande tootrain optimalt korrekt klassificering och regression modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping tootrain optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="0a908-114">**Korsvalidering (KA)** är en teknik som utvärderar hur väl en modell med en känd uppsättning data generalisering toopredicting hello funktioner för datauppsättningar som den inte har tränats.</span><span class="sxs-lookup"><span data-stu-id="0a908-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes toopredicting hello features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="0a908-115">En vanlig tillämpning används här är toodivide en datamängd i K vikningar och träna hello modell i ett resursallokering sätt på alla utom en av hello vikningar.</span><span class="sxs-lookup"><span data-stu-id="0a908-115">A common implementation used here is toodivide a dataset into K folds and then train hello model in a round-robin fashion on all but one of hello folds.</span></span> <span data-ttu-id="0a908-116">används för att utvärdera hello möjligheten för hello modellen tooprediction korrekt när testas mot hello oberoende datauppsättning i den här vikning används inte tootrain hello modellen.</span><span class="sxs-lookup"><span data-stu-id="0a908-116">hello ability of hello model tooprediction accurately when tested against hello independent dataset in this fold not used tootrain hello model is assessed.</span></span>

<span data-ttu-id="0a908-117">**Optimering av Hyperparameter** hello problem med att välja en uppsättning justeringsmodeller för en inlärningsalgoritm normalt med hello målet av optimering av ett mått på hello algoritmen prestanda på en fristående uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="0a908-117">**Hyperparameter optimization** is hello problem of choosing a set of hyperparameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="0a908-118">**Justeringsmodeller** är värden som anges utanför hello modellen utbildning proceduren.</span><span class="sxs-lookup"><span data-stu-id="0a908-118">**Hyperparameters** are values that must be specified outside of hello model training procedure.</span></span> <span data-ttu-id="0a908-119">Antaganden om dessa värden kan påverka hello flexibilitet och korrektheten i hello modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-119">Assumptions about these values can impact hello flexibility and accuracy of hello models.</span></span> <span data-ttu-id="0a908-120">Beslutsträd har justeringsmodeller, till exempel som hello önskad djup och många blad i hello-trädet.</span><span class="sxs-lookup"><span data-stu-id="0a908-120">Decision trees have hyperparameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="0a908-121">Support Vector datorer (SVMs) kräver en term som felklassificering påföljder.</span><span class="sxs-lookup"><span data-stu-id="0a908-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="0a908-122">Ett vanligt sätt tooperform hyperparameter optimering används här är en rutnätet sökning eller en **parametern Svep**.</span><span class="sxs-lookup"><span data-stu-id="0a908-122">A common way tooperform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="0a908-123">Det här består av utför en uttömmande sökning via hello värden en viss delmängd hello hyperparameter utrymme för en inlärningsalgoritm.</span><span class="sxs-lookup"><span data-stu-id="0a908-123">This consists of performing an exhaustive search through hello values a specified subset of hello hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="0a908-124">Mellan verifiering kan tillhandahålla en prestanda mått toosort ut hello bästa möjliga resultat som produceras av hello rutnätet Sök algoritm.</span><span class="sxs-lookup"><span data-stu-id="0a908-124">Cross validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="0a908-125">KA användas med hyperparameter omfattande hjälper gränsen problem angående overfitting en tootraining modelldata så att hello modellen behåller hello kapacitet tooapply toohello allmän uppsättning data från vilken hello träningsdata har extraherats.</span><span class="sxs-lookup"><span data-stu-id="0a908-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model tootraining data so that hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

<span data-ttu-id="0a908-126">hello modeller som vi använder inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd:</span><span class="sxs-lookup"><span data-stu-id="0a908-126">hello models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="0a908-127">[Linjär regression med SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) är en linjär regressionsmodell som använder en Stokastisk toning härstammar (SGD)-metod och skalning toopredict hello tips belopp betald för optimering och funktion.</span><span class="sxs-lookup"><span data-stu-id="0a908-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling toopredict hello tip amounts paid.</span></span> 
* <span data-ttu-id="0a908-128">[Logistic regression med LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) eller ”logit” regression är en regressionsmodell som kan användas när hello beroende variabel är kategoriska toodo dataklassificering.</span><span class="sxs-lookup"><span data-stu-id="0a908-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when hello dependent variable is categorical toodo data classification.</span></span> <span data-ttu-id="0a908-129">LBFGS är en kvasi Karlsson optimering algoritm som uppskattar hello Broyden – Fletcher – Goldfarb – Shanno (BFGS) algoritm med en begränsad mängd ledigt diskutrymme på datorn och som ofta används i machine learning.</span><span class="sxs-lookup"><span data-stu-id="0a908-129">LBFGS is a quasi-Newton optimization algorithm that approximates hello Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="0a908-130">[Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="0a908-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="0a908-131">De kombinera många decision trees tooreduce hello risken för overfitting.</span><span class="sxs-lookup"><span data-stu-id="0a908-131">They combine many decision trees tooreduce hello risk of overfitting.</span></span> <span data-ttu-id="0a908-132">Slumpmässiga skogar används för regression och klassificering kan hantera kategoriska funktioner och kan utökas toohello multiklass-baserad klassificering inställningen.</span><span class="sxs-lookup"><span data-stu-id="0a908-132">Random forests are used for regression and classification and can handle categorical features and can be extended toohello multiclass classification setting.</span></span> <span data-ttu-id="0a908-133">De kräver inte funktionen skalning och är kan toocapture icke-linjärt och funktionen interaktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-133">They do not require feature scaling and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="0a908-134">Slumpmässiga skogar är en av hello mest framgångsrika maskininlärning modeller för klassificering och regression.</span><span class="sxs-lookup"><span data-stu-id="0a908-134">Random forests are one of hello most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="0a908-135">[Toningen ökat träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="0a908-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="0a908-136">GBTs train beslut träd upprepade gånger toominimize en förlust-funktion.</span><span class="sxs-lookup"><span data-stu-id="0a908-136">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="0a908-137">GBTs för regression och klassificering kan hantera kategoriska funktioner, kräver inte funktionen skalning och att kan toocapture icke-linjärt och interaktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able toocapture non-linearities and feature interactions.</span></span> <span data-ttu-id="0a908-138">De kan också användas i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="0a908-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="0a908-139">Modeling exempel med hjälp av KA och Hyperparameter Svep visas för hello binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="0a908-139">Modeling examples using CV and Hyperparameter sweep are shown for hello binary classification problem.</span></span> <span data-ttu-id="0a908-140">Enklare exempel (utan parametern Svep) visas i hello huvudartikeln för regression uppgifter.</span><span class="sxs-lookup"><span data-stu-id="0a908-140">Simpler examples (without parameter sweeps) are presented in hello main topic for regression tasks.</span></span> <span data-ttu-id="0a908-141">Men i hello tillägg visas också verifiering med elastisk net för linjär regression och KA med parametern Svep för slumpmässiga skog regression.</span><span class="sxs-lookup"><span data-stu-id="0a908-141">But in hello appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="0a908-142">Hej **elastisk net** är en reglerats regression metod för att passa linjär regression modeller som linjärt du kombinerar hello L1 och L2 mått för som böter av hello [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) och [upphöjning](https://en.wikipedia.org/wiki/Tikhonov_regularization) metoder.</span><span class="sxs-lookup"><span data-stu-id="0a908-142">hello **elastic net** is a regularized regression method for fitting linear regression models that linearly combines hello L1 and L2 metrics as penalties of hello [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="0a908-143">Även om hello Spark MLlib toolkit är utformad toowork på stora datamängder, används ett relativt litet exempel (cirka 30 Mb med 170K rader, om 0,1% hello ursprungliga NYC datauppsättningen) här i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="0a908-143">Although hello Spark MLlib toolkit is designed toowork on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of hello original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="0a908-144">hello Övning som anges här körs effektivt (i 10 minuter) på ett HDInsight-kluster med 2 arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="0a908-144">hello exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="0a908-145">hello kan har samma kod, med mindre ändringar vara används tooprocess större-datamängder med lämpliga ändringar för cachelagring av data i minnet och ändra hello klusterstorleken.</span><span class="sxs-lookup"><span data-stu-id="0a908-145">hello same code, with minor modifications, can be used tooprocess larger data-sets, with appropriate modifications for caching data in memory and changing hello cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="0a908-146">Konfigurera: Spark-kluster och bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="0a908-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="0a908-147">Konfigurationsstegen och kod finns i den här genomgången för att använda ett HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="0a908-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="0a908-148">Men Jupyter-anteckningsböcker som har angetts för både HDInsight Spark 1.6 och 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0a908-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="0a908-149">En beskrivning av hello anteckningsböcker och länkar toothem finns i hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för hello GitHub-lagringsplats som innehåller dessa.</span><span class="sxs-lookup"><span data-stu-id="0a908-149">A description of hello notebooks and links toothem are provided in hello [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for hello GitHub repository containing them.</span></span> <span data-ttu-id="0a908-150">Dessutom hello koden här hello länkade anteckningsböcker är generisk och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0a908-150">Moreover, hello code here and in hello linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="0a908-151">Om du inte använder HDInsight Spark hello konfiguration och hantering av steg kan vara skiljer sig från vad som anges här.</span><span class="sxs-lookup"><span data-stu-id="0a908-151">If you are not using HDInsight Spark, hello cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="0a908-152">För enkelhetens skull följer hello länkar toohello Jupyter-anteckningsböcker för Spark 1.6 och 2.0 toobe köras i hello pyspark-kerneln av hello Jupyter-anteckningsbok server:</span><span class="sxs-lookup"><span data-stu-id="0a908-152">For convenience, here are hello links toohello Jupyter notebooks for Spark 1.6 and 2.0 toobe run in hello pyspark kernel of hello Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="0a908-153">Spark 1.6 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="0a908-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="0a908-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): innehåller avsnitt i anteckningsboken #1 och modellen utveckling med hjälp av hyperparameter justering och korsvalidering.</span><span class="sxs-lookup"><span data-stu-id="0a908-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="0a908-155">Spark 2.0 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="0a908-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="0a908-156">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): den här filen innehåller information om hur tooperform datagranskning modellering och bedömningen i 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0a908-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how tooperform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-hello-preset-spark-context"></a><span data-ttu-id="0a908-157">: Lagringsplatser, bibliotek och hello förinställning Spark-kontexten</span><span class="sxs-lookup"><span data-stu-id="0a908-157">Setup: storage locations, libraries, and hello preset Spark context</span></span>
<span data-ttu-id="0a908-158">Spark är kan tooread och skriva tooAzure Lagringsblob (även kallat WASB).</span><span class="sxs-lookup"><span data-stu-id="0a908-158">Spark is able tooread and write tooAzure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="0a908-159">Så något av dina befintliga data som lagras där kan bearbetas med Spark och hello resultatet lagras igen i WASB.</span><span class="sxs-lookup"><span data-stu-id="0a908-159">So any of your existing data stored there can be processed using Spark and hello results stored again in WASB.</span></span>

<span data-ttu-id="0a908-160">toosave modeller eller filer i WASB hello sökväg behöver toobe som angetts på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="0a908-160">toosave models or files in WASB, hello path needs toobe specified properly.</span></span> <span data-ttu-id="0a908-161">hello standard behållaren kopplade toohello Spark-kluster kan refereras med en sökväg som börjar med ”: wasb: / / /”.</span><span class="sxs-lookup"><span data-stu-id="0a908-161">hello default container attached toohello Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="0a908-162">Andra platser refererar till ”wasb: / /”.</span><span class="sxs-lookup"><span data-stu-id="0a908-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="0a908-163">Ange katalogsökvägar för lagringsplatser i WASB</span><span class="sxs-lookup"><span data-stu-id="0a908-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="0a908-164">hello följande kodexempel anger hello platsen för hello data toobe läsa och sparas hello sökväg för hello modellen lagring directory toowhich hello modellen utdata:</span><span class="sxs-lookup"><span data-stu-id="0a908-164">hello following code sample specifies hello location of hello data toobe read and hello path for hello model storage directory toowhich hello model output is saved:</span></span>

    # SET PATHS tooFILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET hello MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="0a908-165">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-165">**OUTPUT**</span></span>

<span data-ttu-id="0a908-166">datetime.datetime (2016, 4, 18, 17, 36, 27, 832799)</span><span class="sxs-lookup"><span data-stu-id="0a908-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="0a908-167">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="0a908-167">Import libraries</span></span>
<span data-ttu-id="0a908-168">Importera nödvändiga bibliotek med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="0a908-168">Import necessary libraries with hello following code:</span></span>

    # LOAD PYSPARK LIBRARIES
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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="0a908-169">Förinställda Spark-kontexten och PySpark användbara</span><span class="sxs-lookup"><span data-stu-id="0a908-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="0a908-170">Hej PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext.</span><span class="sxs-lookup"><span data-stu-id="0a908-170">hello PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="0a908-171">Så behöver du inte tooset hello Spark- eller Hive kontexter explicit innan du börjar arbeta med hello-program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="0a908-171">So you do not need tooset hello Spark or Hive contexts explicitly before you start working with hello application you are developing.</span></span> <span data-ttu-id="0a908-172">Dessa kontexter är tillgängliga för dig som standard.</span><span class="sxs-lookup"><span data-stu-id="0a908-172">These contexts are available for you by default.</span></span> <span data-ttu-id="0a908-173">Dessa kontexter är:</span><span class="sxs-lookup"><span data-stu-id="0a908-173">These contexts are:</span></span>

* <span data-ttu-id="0a908-174">SC - Spark</span><span class="sxs-lookup"><span data-stu-id="0a908-174">sc - for Spark</span></span> 
* <span data-ttu-id="0a908-175">sqlContext - för Hive</span><span class="sxs-lookup"><span data-stu-id="0a908-175">sqlContext - for Hive</span></span>

<span data-ttu-id="0a908-176">Hej PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%.</span><span class="sxs-lookup"><span data-stu-id="0a908-176">hello PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="0a908-177">Det finns två kommandon som används i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="0a908-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="0a908-178">**%% lokala** anger att hello koden i efterföljande rader är toobe körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="0a908-178">**%%local** Specifies that hello code in subsequent lines is toobe executed locally.</span></span> <span data-ttu-id="0a908-179">Koden måste vara giltig Python-kod.</span><span class="sxs-lookup"><span data-stu-id="0a908-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="0a908-180">**%% sql -o <variable name>**  kör en Hive-fråga mot hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="0a908-180">**%%sql -o <variable name>** Executes a Hive query against hello sqlContext.</span></span> <span data-ttu-id="0a908-181">Om hello -o parametern överförs hello resultatet av hello fråga sparas i hello %% lokala Python kontext som en Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="0a908-181">If hello -o parameter is passed, hello result of hello query is persisted in hello %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="0a908-182">För mer information om hello kärnor för Jupyter-anteckningsböcker och hello fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="0a908-182">For more information on hello kernels for Jupyter notebooks and hello predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="0a908-183">Datapåfyllning från offentliga blob:</span><span class="sxs-lookup"><span data-stu-id="0a908-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="0a908-184">hello är första steget i processen för hello datavetenskap tooingest hello data toobe analyseras från källor som var den finns i din data från kartläggning av naturresurser och modellering miljö.</span><span class="sxs-lookup"><span data-stu-id="0a908-184">hello first step in hello data science process is tooingest hello data toobe analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="0a908-185">Den här miljön är Spark i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="0a908-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="0a908-186">Det här avsnittet innehåller hello koden toocomplete en serie aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="0a908-186">This section contains hello code toocomplete a series of tasks:</span></span>

* <span data-ttu-id="0a908-187">mata in hello data exempel toobe modelleras</span><span class="sxs-lookup"><span data-stu-id="0a908-187">ingest hello data sample toobe modeled</span></span>
* <span data-ttu-id="0a908-188">Läs i hello inkommande dataset (som lagras som en TSV-fil)</span><span class="sxs-lookup"><span data-stu-id="0a908-188">read in hello input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="0a908-189">Formatera och ren hello data</span><span class="sxs-lookup"><span data-stu-id="0a908-189">format and clean hello data</span></span>
* <span data-ttu-id="0a908-190">Skapa och cachelagra objekt (RDDs eller ramar) i minnet</span><span class="sxs-lookup"><span data-stu-id="0a908-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="0a908-191">registrera den som en temp-tabell i SQL-kontext.</span><span class="sxs-lookup"><span data-stu-id="0a908-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="0a908-192">Här är hello koden för datapåfyllning.</span><span class="sxs-lookup"><span data-stu-id="0a908-192">Here is hello code for data ingestion.</span></span>

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

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES hello DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK tooRUN hello CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-193">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-193">**OUTPUT**</span></span>

<span data-ttu-id="0a908-194">Tidsåtgång tooexecute ovanför cellen: 276.62 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-194">Time taken tooexecute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="0a908-195">Datagranskning & visualiseringen</span><span class="sxs-lookup"><span data-stu-id="0a908-195">Data exploration & visualization</span></span>
<span data-ttu-id="0a908-196">När hello data har trätt i Spark, är hello nästa steg i processen för hello datavetenskap toogain djupare förståelse för hello data via undersökning och visualisering.</span><span class="sxs-lookup"><span data-stu-id="0a908-196">Once hello data has been brought into Spark, hello next step in hello data science process is toogain deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="0a908-197">I det här avsnittet undersöka vi hello taxi data med hjälp av SQL-frågor och ritytans hello target-variabler och potentiella funktioner för granskning.</span><span class="sxs-lookup"><span data-stu-id="0a908-197">In this section, we examine hello taxi data using SQL queries and plot hello target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="0a908-198">Mer specifikt Rita vi hello frekvensen av passagerare antal i taxi resor hello frekvensen av tips belopp och hur tips varierar beroende på belopp och typen.</span><span class="sxs-lookup"><span data-stu-id="0a908-198">Specifically, we plot hello frequency of passenger counts in taxi trips, hello frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-hello-sample-of-taxi-trips"></a><span data-ttu-id="0a908-199">Rita ett histogram passagerare antal frekvenser i hello prov av taxi resor</span><span class="sxs-lookup"><span data-stu-id="0a908-199">Plot a histogram of passenger count frequencies in hello sample of taxi trips</span></span>
<span data-ttu-id="0a908-200">Den här koden och efterföljande kodavsnitt använder SQL magiskt tooquery hello exempel och lokala magiskt tooplot hello data.</span><span class="sxs-lookup"><span data-stu-id="0a908-200">This code and subsequent snippets use SQL magic tooquery hello sample and local magic tooplot hello data.</span></span>

* <span data-ttu-id="0a908-201">**SQL-magic (`%%sql`)** hello HDInsight PySpark-kerneln stöder enkelt infogade HiveQL frågor mot hello sqlContext.</span><span class="sxs-lookup"><span data-stu-id="0a908-201">**SQL magic (`%%sql`)** hello HDInsight PySpark kernel supports easy inline HiveQL queries against hello sqlContext.</span></span> <span data-ttu-id="0a908-202">hello (-o VARIABLE_NAME) argumentet kvarstår hello utdata från hello SQL-frågan som en Pandas DataFrame på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="0a908-202">hello (-o VARIABLE_NAME) argument persists hello output of hello SQL query as a Pandas DataFrame on hello Jupyter server.</span></span> <span data-ttu-id="0a908-203">Det innebär att den är tillgänglig i hello lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="0a908-203">This means it is available in hello local mode.</span></span>
* <span data-ttu-id="0a908-204">Hej  **`%%local` magic** är används toorun kod lokalt på hello Jupyter servern som hello headnode hello HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="0a908-204">hello **`%%local` magic** is used toorun code locally on hello Jupyter server, which is hello headnode of hello HDInsight cluster.</span></span> <span data-ttu-id="0a908-205">Normalt använder du `%%local` magiskt efter hello `%%sql -o` magic är används toorun en fråga.</span><span class="sxs-lookup"><span data-stu-id="0a908-205">Typically, you use `%%local` magic after hello `%%sql -o` magic is used toorun a query.</span></span> <span data-ttu-id="0a908-206">hello -o parametern skulle sparas hello utdata från lokalt hello SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="0a908-206">hello -o parameter would persist hello output of hello SQL query locally.</span></span> <span data-ttu-id="0a908-207">Sedan hello `%%local` magiskt utlösare hello nästa uppsättning kod kodavsnitt toorun lokalt mot hello utdata för hello SQL-frågor som har sparats lokalt.</span><span class="sxs-lookup"><span data-stu-id="0a908-207">Then hello `%%local` magic triggers hello next set of code snippets toorun locally against hello output of hello SQL queries that has been persisted locally.</span></span> <span data-ttu-id="0a908-208">hello utdata visualiseras automatiskt när du har kört hello kod.</span><span class="sxs-lookup"><span data-stu-id="0a908-208">hello output is automatically visualized after you run hello code.</span></span>

<span data-ttu-id="0a908-209">Den här frågan hämtar hello resor av passagerare count.</span><span class="sxs-lookup"><span data-stu-id="0a908-209">This query retrieves hello trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="0a908-210">Den här koden skapar en lokala data ram från hello frågeresultatet och visar hello data.</span><span class="sxs-lookup"><span data-stu-id="0a908-210">This code creates a local data-frame from hello query output and plots hello data.</span></span> <span data-ttu-id="0a908-211">Hej `%%local` magic skapar lokala data-ram, `sqlResults`, som kan användas för att rita upp med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="0a908-211">hello `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="0a908-212">Den här PySpark-magic används flera gånger i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="0a908-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="0a908-213">Om hello mängden data är stor, bör du prov toocreate en data-ram som får plats i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="0a908-213">If hello amount of data is large, you should sample toocreate a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES. 
    # CLICK ON hello TYPE OF PLOT tooBE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="0a908-214">Här är hello kod tooplot hello resor av passagerare antal</span><span class="sxs-lookup"><span data-stu-id="0a908-214">Here is hello code tooplot hello trips by passenger counts</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT PASSENGER NUMBER VS TRIP COUNTS
    x_labels = sqlResults['passenger_count'].values
    fig = sqlResults[['trip_counts']].plot(kind='bar', facecolor='lightblue')
    fig.set_xticklabels(x_labels)
    fig.set_title('Counts of trips by passenger count')
    fig.set_xlabel('Passenger count in trips')
    fig.set_ylabel('Trip counts')
    plt.show()

<span data-ttu-id="0a908-215">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-215">**OUTPUT**</span></span>

![Frekvensen för resor av passagerare antal](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="0a908-217">Du kan välja bland flera olika typer av grafik (register, cirkeldiagram, rad, område eller fältet) med hjälp av hello **typen** menyn knapparna i hello anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="0a908-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using hello **Type** menu buttons in hello notebook.</span></span> <span data-ttu-id="0a908-218">hello-fältet ritytans visas här.</span><span class="sxs-lookup"><span data-stu-id="0a908-218">hello Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="0a908-219">Rita histogrammet tips belopp och hur mycket tips varierar efter passagerare antal och avgiften belopp.</span><span class="sxs-lookup"><span data-stu-id="0a908-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="0a908-220">Använd en SQL-fråga toosample...</span><span class="sxs-lookup"><span data-stu-id="0a908-220">Use a SQL query toosample data..</span></span>

    # SQL SQUERY
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


<span data-ttu-id="0a908-221">Den här koden cellen använder hello SQL-frågan toocreate tre områden hello data.</span><span class="sxs-lookup"><span data-stu-id="0a908-221">This code cell uses hello SQL query toocreate three plots hello data.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline

    # TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = resultsPDDF[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # TIP BY PASSENGER COUNT
    ax2 = resultsPDDF.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount ($) by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # TIP AMOUNT BY FARE AMOUNT, POINTS ARE SCALED BY PASSENGER COUNT
    ax = resultsPDDF.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(resultsPDDF.passenger_count))
    ax.set_title('Tip amount by Fare amount ($)')
    ax.set_xlabel('Fare Amount')
    ax.set_ylabel('Tip Amount')
    plt.axis([-2, 120, -2, 30])
    plt.show()


<span data-ttu-id="0a908-222">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="0a908-222">**OUTPUT:**</span></span> 

![Fördelning av tips](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tips belopp av avgiften belopp](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="0a908-226">Funktionen tekniker, omvandling och data förberedelse för modellering</span><span class="sxs-lookup"><span data-stu-id="0a908-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="0a908-227">Det här avsnittet beskriver och hello koden för procedurer används tooprepare data för användning i ML-modell.</span><span class="sxs-lookup"><span data-stu-id="0a908-227">This section describes and provides hello code for procedures used tooprepare data for use in ML modeling.</span></span> <span data-ttu-id="0a908-228">Den visar hur toodo hello följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="0a908-228">It shows how toodo hello following tasks:</span></span>

* <span data-ttu-id="0a908-229">Skapa en ny funktion genom partitionering timmar till trafik tid lagerplatser</span><span class="sxs-lookup"><span data-stu-id="0a908-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="0a908-230">Index och på hot koda kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="0a908-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="0a908-231">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="0a908-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="0a908-232">Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="0a908-232">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
* <span data-ttu-id="0a908-233">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="0a908-233">Feature scaling</span></span>
* <span data-ttu-id="0a908-234">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="0a908-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="0a908-235">Skapa en ny funktion genom partitionering trafik gånger till lagerplatser</span><span class="sxs-lookup"><span data-stu-id="0a908-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="0a908-236">Den här koden visar hur toocreate en ny funktion av partitionering trafik tid till lagerplatser och sedan hur toocache hello resulterande data ram i minnet.</span><span class="sxs-lookup"><span data-stu-id="0a908-236">This code shows how toocreate a new feature by partitioning traffic times into bins and then how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="0a908-237">Cachelagring leder tooimproved körningstid där flexibel distribuerade datauppsättningar (RDDs) och ramar data används flera gånger.</span><span class="sxs-lookup"><span data-stu-id="0a908-237">Caching leads tooimproved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="0a908-238">Så cachelagra vi RDDs och data ramar i flera steg i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="0a908-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

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

<span data-ttu-id="0a908-239">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-239">**OUTPUT**</span></span>

<span data-ttu-id="0a908-240">126050</span><span class="sxs-lookup"><span data-stu-id="0a908-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="0a908-241">Index och en hot koda kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="0a908-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="0a908-242">Det här avsnittet visas hur tooindex eller koda kategoriska funktioner för indata i hello modeling funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-242">This section shows how tooindex or encode categorical features for input into hello modeling functions.</span></span> <span data-ttu-id="0a908-243">Hej modellering och förutsäga funktioner för MLlib kräver att funktioner med kategoriska indata vara indexera eller kodade tidigare toouse.</span><span class="sxs-lookup"><span data-stu-id="0a908-243">hello modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior toouse.</span></span> 

<span data-ttu-id="0a908-244">Du måste tooindex eller koda dem på olika sätt beroende på hello modellen.</span><span class="sxs-lookup"><span data-stu-id="0a908-244">Depending on hello model, you need tooindex or encode them in different ways.</span></span> <span data-ttu-id="0a908-245">Till exempel Logistic och linjär Regression modeller kräver en hot kodning, där, till exempel en funktion med tre kategorier kan expanderas till tre funktionen kolumner med varje som innehåller 0 eller 1 beroende på hello kategori av en observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="0a908-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="0a908-246">MLlib ger [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) fungerar toodo ett hot kodning.</span><span class="sxs-lookup"><span data-stu-id="0a908-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function toodo one-hot encoding.</span></span> <span data-ttu-id="0a908-247">Den här kodaren mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="0a908-247">This encoder maps a column of label indices tooa column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="0a908-248">Den här kodningen kan algoritmer som räknar numeriska värden funktioner, till exempel logistic regression toobe tillämpas toocategorical funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, toobe applied toocategorical features.</span></span>

<span data-ttu-id="0a908-249">Här är hello kod tooindex och koda kategoriska funktioner:</span><span class="sxs-lookup"><span data-stu-id="0a908-249">Here is hello code tooindex and encode categorical features:</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

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


<span data-ttu-id="0a908-250">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-250">**OUTPUT**</span></span>

<span data-ttu-id="0a908-251">Tidsåtgång tooexecute ovanför cellen: 3,14 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-251">Time taken tooexecute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="0a908-252">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="0a908-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="0a908-253">Det här avsnittet innehåller kod som visar hur tooindex kategoriska som ett märkt punkt textdatatyp och tooencode den.</span><span class="sxs-lookup"><span data-stu-id="0a908-253">This section contains code that shows how tooindex categorical text data as a labeled point data type and how tooencode it.</span></span> <span data-ttu-id="0a908-254">Detta förbereder toobe används tootrain och testa MLlib logistic regression och andra klassificering modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-254">This prepares it toobe used tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="0a908-255">Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) formaterad på ett sätt som krävs av de flesta ML algoritmer i MLlib som indata.</span><span class="sxs-lookup"><span data-stu-id="0a908-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="0a908-256">En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.</span><span class="sxs-lookup"><span data-stu-id="0a908-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="0a908-257">Här är hello kod tooindex och koda textfunktioner för binär klassificering.</span><span class="sxs-lookup"><span data-stu-id="0a908-257">Here is hello code tooindex and encode text features for binary classification.</span></span>

    # FUNCTIONS FOR BINARY CLASSIFICATION

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint
    from numpy import array

    # INDEXING CATEGORICAL TEXT FEATURES FOR INPUT INTO TREE-BASED MODELS
    def parseRowIndexingBinary(line):
        features = np.array([line.paymentIndex, line.vendorIndex, line.rateIndex, line.pickup_hour, line.weekday,
                             line.passenger_count, line.trip_time_in_secs, line.trip_distance, line.fare_amount])
        labPt = LabeledPoint(line.tipped, features)
        return  labPt

    # ONE-HOT ENCODING OF CATEGORICAL TEXT FEATURES FOR INPUT INTO LOGISTIC RERESSION MODELS
    def parseRowOneHotBinary(line):
        features = np.concatenate((np.array([line.pickup_hour, line.weekday, line.passenger_count,
                                            line.trip_time_in_secs, line.trip_distance, line.fare_amount]), 
                                   line.vendorVec.toArray(), line.rateVec.toArray(), line.paymentVec.toArray()), axis=0)
        labPt = LabeledPoint(line.tipped, features)
        return  labPt


<span data-ttu-id="0a908-258">Här är hello kod tooencode och index kategoriska text-funktioner för linjär regressionsanalys.</span><span class="sxs-lookup"><span data-stu-id="0a908-258">Here is hello code tooencode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-hello-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="0a908-259">Skapa ett slumpmässigt underordnade dataurval hello och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="0a908-259">Create a random sub-sampling of hello data and split it into training and testing sets</span></span>
<span data-ttu-id="0a908-260">Den här koden skapar ett slumpmässigt dataurval hello (25% används här).</span><span class="sxs-lookup"><span data-stu-id="0a908-260">This code creates a random sampling of hello data (25% is used here).</span></span> <span data-ttu-id="0a908-261">Även om det inte krävs för det här exemplet på grund av toohello storlek på hello dataset, visar vi hur du kan prova hello data här.</span><span class="sxs-lookup"><span data-stu-id="0a908-261">Although it is not required for this example due toohello size of hello dataset, we demonstrate how you can sample hello data here.</span></span> <span data-ttu-id="0a908-262">Vet du hur toouse den för din egen problem om det behövs.</span><span class="sxs-lookup"><span data-stu-id="0a908-262">Then you know how toouse it for your own problem if needed.</span></span> <span data-ttu-id="0a908-263">När prover är stort, kan detta spara mycket tid när utbildning modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="0a908-264">Nästa dela vi hello exempel i en utbildning (här 75%) och en tester del (här 25%) toouse i klassificering och regressionen modeling.</span><span class="sxs-lookup"><span data-stu-id="0a908-264">Next we split hello sample into a training part (75% here) and a testing part (25% here) toouse in classification and regression modeling.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    from pyspark.sql.functions import rand

    samplingFraction = 0.25;
    trainingFraction = 0.75; testingFraction = (1-trainingFraction);
    seed = 1234;
    encodedFinalSampled = encodedFinal.sample(False, samplingFraction, seed=seed)

    # SPLIT SAMPLED DATA-FRAME INTO TRAIN/TEST, WITH A RANDOM COLUMN ADDED FOR DOING CV (SHOWN LATER)
    # INCLUDE RAND COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    dfTmpRand = encodedFinalSampled.select("*", rand(0).alias("rand"));
    trainData, testData = dfTmpRand.randomSplit([trainingFraction, testingFraction], seed=seed);

    # CACHE TRAIN AND TEST DATA
    trainData.cache()
    testData.cache()

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

<span data-ttu-id="0a908-265">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-265">**OUTPUT**</span></span>

<span data-ttu-id="0a908-266">Tidsåtgång tooexecute ovanför cellen: 0.31 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-266">Time taken tooexecute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="0a908-267">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="0a908-267">Feature scaling</span></span>
<span data-ttu-id="0a908-268">Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål hello-funktionen.</span><span class="sxs-lookup"><span data-stu-id="0a908-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in hello objective function.</span></span> <span data-ttu-id="0a908-269">hello koden för funktionen skalning använder hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello funktioner toounit varians.</span><span class="sxs-lookup"><span data-stu-id="0a908-269">hello code for feature scaling uses hello [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) tooscale hello features toounit variance.</span></span> <span data-ttu-id="0a908-270">Den tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD).</span><span class="sxs-lookup"><span data-stu-id="0a908-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="0a908-271">SGD är en populär algoritm för att träna en mängd andra maskininlärning modeller som reglerats regressioner eller support vector datorer (SVM).</span><span class="sxs-lookup"><span data-stu-id="0a908-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="0a908-272">Vi har hittat hello LinearRegressionWithSGD algoritmen toobe känsliga toofeature skalning.</span><span class="sxs-lookup"><span data-stu-id="0a908-272">We have found hello LinearRegressionWithSGD algorithm toobe sensitive toofeature scaling.</span></span>   
> 
> 

<span data-ttu-id="0a908-273">Här är hello kod tooscale variabler för användning med hello reglerats linjär SGD algoritm.</span><span class="sxs-lookup"><span data-stu-id="0a908-273">Here is hello code tooscale variables for use with hello regularized linear SGD algorithm.</span></span>

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

<span data-ttu-id="0a908-274">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-274">**OUTPUT**</span></span>

<span data-ttu-id="0a908-275">Tidsåtgång tooexecute ovanför cellen: 11.67 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-275">Time taken tooexecute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="0a908-276">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="0a908-276">Cache objects in memory</span></span>
<span data-ttu-id="0a908-277">hello tidsåtgång för träning och testning av ML algoritmer kan minskas med cachelagring hello indata ram objekt används för klassificering, regression och skalas funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-277">hello time taken for training and testing of ML algorithms can be reduced by caching hello input data frame objects used for classification, regression and, scaled features.</span></span>

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

<span data-ttu-id="0a908-278">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-278">**OUTPUT**</span></span> 

<span data-ttu-id="0a908-279">Tidsåtgång tooexecute ovanför cellen: 0,13 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-279">Time taken tooexecute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="0a908-280">Förutsäga huruvida ett tips är betald med binär klassificering modeller</span><span class="sxs-lookup"><span data-stu-id="0a908-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="0a908-281">Det här avsnittet visas hur användning tre modeller för hello binär klassificering uppgiften att förutsäga ett tips betalat för en taxi resa eller inte.</span><span class="sxs-lookup"><span data-stu-id="0a908-281">This section shows how use three models for hello binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="0a908-282">hello modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="0a908-282">hello models presented are:</span></span>

* <span data-ttu-id="0a908-283">Logistic regression</span><span class="sxs-lookup"><span data-stu-id="0a908-283">Logistic regression</span></span> 
* <span data-ttu-id="0a908-284">Slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="0a908-284">Random forest</span></span>
* <span data-ttu-id="0a908-285">Toning träd</span><span class="sxs-lookup"><span data-stu-id="0a908-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="0a908-286">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="0a908-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="0a908-287">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="0a908-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="0a908-288">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="0a908-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="0a908-289">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="0a908-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="0a908-290">Visar vi hur toodo korsvalidering (KA) med parametern omfattande på två sätt:</span><span class="sxs-lookup"><span data-stu-id="0a908-290">We show how toodo cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="0a908-291">Med hjälp av **allmänna** anpassad kod som kan vara tillämpade tooany algoritmen i MLlib och tooany parametern anger i en algoritm.</span><span class="sxs-lookup"><span data-stu-id="0a908-291">Using **generic** custom code which can be applied tooany algorithm in MLlib and tooany parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="0a908-292">Med hjälp av hello **pySpark CrossValidator pipeline funktionen**.</span><span class="sxs-lookup"><span data-stu-id="0a908-292">Using hello **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="0a908-293">Observera att CrossValidator inte har några begränsningar för Spark 1.5.0:</span><span class="sxs-lookup"><span data-stu-id="0a908-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="0a908-294">Pipeline-modeller får inte vara sparade/beständiga för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="0a908-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="0a908-295">Kan inte användas för varje parameter i en modell.</span><span class="sxs-lookup"><span data-stu-id="0a908-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="0a908-296">Kan inte användas för varje MLlib-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="0a908-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-hello-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="0a908-297">Generisk mellan validering och hyperparameter omfattande används med algoritmen för hello logistic regression för binär klassificering</span><span class="sxs-lookup"><span data-stu-id="0a908-297">Generic cross validation and hyperparameter sweeping used with hello logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="0a908-298">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="0a908-298">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="0a908-299">hello-modellen har tränats med mellan validering (KA) och hyperparameter omfattande har implementerats med anpassad kod som kan vara tillämpade tooany av hello learning algoritmer i MLlib.</span><span class="sxs-lookup"><span data-stu-id="0a908-299">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied tooany of hello learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="0a908-300">hello körning av anpassade KA koden kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="0a908-300">hello execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="0a908-301">**Hej logistic regression träningsmodell med KA och hyperparameter omfattande**</span><span class="sxs-lookup"><span data-stu-id="0a908-301">**Train hello logistic regression model using CV and hyperparameter sweeping**</span></span>

    # LOGISTIC REGRESSION CLASSIFICATION WITH CV AND HYPERPARAMETER SWEEPING

    # GET ACCURACY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionWithLBFGS 
    from pyspark.mllib.evaluation import BinaryClassificationMetrics

    # CREATE PARAMETER GRID FOR LOGISTIC REGRESSION PARAMETER SWEEP
    from sklearn.grid_search import ParameterGrid
    grid = [{'regParam': [0.01, 0.1], 'iterations': [5, 10], 'regType': ["l1", "l2"], 'tolerance': [1e-3, 1e-4]}]
    paramGrid = list(ParameterGrid(grid))
    numModels = len(paramGrid)

    # SET NUM FOLDS AND NUM PARAMETER SETS tooSWEEP ON
    nFolds = 3;
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    # BEGIN CV WITH PARAMETER SWEEP
    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create LabeledPoints from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowOneHotBinary)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowOneHotBinary)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            regt = paramGrid[j]['regType']
            regp = paramGrid[j]['regParam']
            iters = paramGrid[j]['iterations']
            tol = paramGrid[j]['tolerance']
            # Train logistic regression model with hypermarameter set
            model = LogisticRegressionWithLBFGS.train(trainCVLabPt, regType=regt, iterations=iters,  
                                                      regParam=regp, tolerance = tol, intercept=True)
            predictionAndLabels = validationLabPt.map(lambda lp: (float(model.predict(lp.features)), lp.label))
            # Use ROC-AUC as accuracy metrics
            validMetrics = BinaryClassificationMetrics(predictionAndLabels)
            metric = validMetrics.areaUnderROC
            metricSum[j] += metric

    avgAcc = metricSum / nFolds;
    bestParam = paramGrid[np.argmax(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    # TRAIN ON FULL TRAIING SET USING BEST PARAMETERS FROM CV/PARAMETER SWEEP
    logitBest = LogisticRegressionWithLBFGS.train(oneHotTRAINbinary, regType=bestParam['regType'], 
                                                  iterations=bestParam['iterations'], 
                                                  regParam=bestParam['regParam'], tolerance = bestParam['tolerance'], 
                                                  intercept=True)


    # PRINT COEFFICIENTS AND INTERCEPT OF hello MODEL
    # NOTE: There are 20 coefficient terms for hello 10 features, 
    #       and hello different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-302">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-302">**OUTPUT**</span></span>

<span data-ttu-id="0a908-303">Koefficienter: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="0a908-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="0a908-304">Skärningspunkt:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="0a908-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="0a908-305">Tidsåtgång tooexecute ovanför cellen: 14.43 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-305">Time taken tooexecute above cell: 14.43 seconds</span></span>

<span data-ttu-id="0a908-306">**Utvärdera hello binär klassificering modellen med standard**</span><span class="sxs-lookup"><span data-stu-id="0a908-306">**Evaluate hello binary classification model with standard metrics**</span></span>

<span data-ttu-id="0a908-307">hello kod i det här avsnittet visar hur tooevaluate logistic regression modell mot en test-datamängd, inklusive ritning av hello ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="0a908-307">hello code in this section shows how tooevaluate a logistic regression model against a test data-set, including a plot of hello ROC curve.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    #IMPORT LIBRARIES
    from sklearn.metrics import roc_curve,auc
    from pyspark.mllib.evaluation import BinaryClassificationMetrics
    from pyspark.mllib.evaluation import MulticlassMetrics

    # PREDICT ON TEST DATA WITH BEST/FINAL MODEL
    predictionAndLabels = oneHotTESTbinary.map(lambda lp: (float(logitBest.predict(lp.features)), lp.label))

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

    # OUTPUT PROBABILITIES AND REGISTER TEMP TABLE
    logitBest.clearThreshold(); # This clears threshold for classification (0.5) and outputs probabilities
    predictionAndLabelsDF = predictionAndLabels.toDF()
    predictionAndLabelsDF.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-308">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-308">**OUTPUT**</span></span>

<span data-ttu-id="0a908-309">Området under PR = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="0a908-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="0a908-310">Området under ROC = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="0a908-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="0a908-311">Sammanfattande statistik</span><span class="sxs-lookup"><span data-stu-id="0a908-311">Summary Stats</span></span>

<span data-ttu-id="0a908-312">Precision = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="0a908-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="0a908-313">Återkalla = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="0a908-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="0a908-314">F1 Poängsätta = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="0a908-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="0a908-315">Tidsåtgång tooexecute ovanför cellen: 2.67 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-315">Time taken tooexecute above cell: 2.67 seconds</span></span>

<span data-ttu-id="0a908-316">**Rita hello ROC-kurvan.**</span><span class="sxs-lookup"><span data-stu-id="0a908-316">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="0a908-317">Hej *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="0a908-317">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="0a908-318">*tmp_results* kan använda toodo frågor och spara resultatet i hello sqlResults data-ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="0a908-318">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="0a908-319">Här är hello kod.</span><span class="sxs-lookup"><span data-stu-id="0a908-319">Here is hello code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="0a908-320">Här är hello kod toomake förutsägelser och ritytans hello ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="0a908-320">Here is hello code toomake predictions and plot hello ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES                              
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    #PREDICTIONS
    predictions_pddf = sqlResults.rename(columns={'_1': 'probability', '_2': 'label'})
    prob = predictions_pddf["probability"] 
    fpr, tpr, thresholds = roc_curve(predictions_pddf['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT ROC CURVES
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


<span data-ttu-id="0a908-321">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-321">**OUTPUT**</span></span>

![Logistic regression ROC-kurvan generisk metod](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="0a908-323">**Spara modellen i en blob för framtida användning**</span><span class="sxs-lookup"><span data-stu-id="0a908-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="0a908-324">hello kod i det här avsnittet visar hur toosave hello logistic regression modell för användning.</span><span class="sxs-lookup"><span data-stu-id="0a908-324">hello code in this section shows how toosave hello logistic regression model for consumption.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.classification import LogisticRegressionModel

    # PERSIST MODEL
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    logisticregressionfilename = "LogisticRegressionWithLBFGS_" + datestamp;
    dirfilename = modelDir + logisticregressionfilename;

    logitBest.save(sc, dirfilename);

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";


<span data-ttu-id="0a908-325">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-325">**OUTPUT**</span></span>

<span data-ttu-id="0a908-326">Tidsåtgång tooexecute ovanför cellen: 34.57 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-326">Time taken tooexecute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="0a908-327">Använda Mllib's CrossValidator pipeline-funktionen med logistic regressionsmodell (elastisk regression)</span><span class="sxs-lookup"><span data-stu-id="0a908-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="0a908-328">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="0a908-328">hello code in this section shows how tootrain, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="0a908-329">hello-modellen har tränats med mellan validering (KA) och hyperparameter omfattande implementerats med hello MLlib CrossValidator pipeline-funktionen för KA med parametern Svep.</span><span class="sxs-lookup"><span data-stu-id="0a908-329">hello model is trained using cross validation (CV) and hyperparameter sweeping implemented with hello MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="0a908-330">hello körningen av den här koden MLlib KA kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="0a908-330">hello execution of this MLlib CV code can take several minutes.</span></span>
> 
> 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.classification import LogisticRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import BinaryClassificationEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder
    from sklearn.metrics import roc_curve,auc

    # DEFINE ALGORITHM / MODEL
    lr = LogisticRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build()

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=BinaryClassificationEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA-FRAME: THIS DOES NOT RUN ON RDDs
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINbinary, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    ## PREDICT AND EVALUATE ON TEST DATA-SET

    # USE TEST DATASET FOR PREDICTION
    testDataFrame = sqlContext.createDataFrame(oneHotTESTbinary, ["features", "label"])
    test_predictions = cv_model.transform(testDataFrame)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="0a908-331">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-331">**OUTPUT**</span></span>

<span data-ttu-id="0a908-332">Tidsåtgång tooexecute ovanför cellen: 107.98 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-332">Time taken tooexecute above cell: 107.98 seconds</span></span>

<span data-ttu-id="0a908-333">**Rita hello ROC-kurvan.**</span><span class="sxs-lookup"><span data-stu-id="0a908-333">**Plot hello ROC curve.**</span></span>

<span data-ttu-id="0a908-334">Hej *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="0a908-334">hello *predictionAndLabelsDF* is registered as a table, *tmp_results*, in hello previous cell.</span></span> <span data-ttu-id="0a908-335">*tmp_results* kan använda toodo frågor och spara resultatet i hello sqlResults data-ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="0a908-335">*tmp_results* can be used toodo queries and output results into hello sqlResults data-frame for plotting.</span></span> <span data-ttu-id="0a908-336">Här är hello kod.</span><span class="sxs-lookup"><span data-stu-id="0a908-336">Here is hello code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="0a908-337">Här är hello kod tooplot hello ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="0a908-337">Here is hello code tooplot hello ROC curve.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES 
    %%local
    from sklearn.metrics import roc_curve,auc

    # ROC CURVE
    prob = [x["values"][1] for x in sqlResults["probability"]]
    fpr, tpr, thresholds = roc_curve(sqlResults['label'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    #PLOT
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


<span data-ttu-id="0a908-338">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-338">**OUTPUT**</span></span>

![Logistic regression ROC-kurvan med Mllib's CrossValidator](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="0a908-340">Slumpmässiga skog klassificering</span><span class="sxs-lookup"><span data-stu-id="0a908-340">Random forest classification</span></span>
<span data-ttu-id="0a908-341">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig skog regression som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="0a908-341">hello code in this section shows how tootrain, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

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
    ## UN-COMMENT IF YOU WANT tooPRING TREES
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


<span data-ttu-id="0a908-342">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-342">**OUTPUT**</span></span>

<span data-ttu-id="0a908-343">Området under ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="0a908-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="0a908-344">Tidsåtgång tooexecute ovanför cellen: 26.72 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-344">Time taken tooexecute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="0a908-345">Toning den träd klassificering</span><span class="sxs-lookup"><span data-stu-id="0a908-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="0a908-346">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar huruvida ett tips är betalat för en resa i hello NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="0a908-346">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in hello NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT tooPRINT TREE DETAILS
    #print('Learned classification GBT model:')
    #print(bgtModel.toDebugString())

    # PREDICT ON TEST DATA AND EVALUATE
    predictions = gbtModel.predict(indexedTESTbinary.map(lambda x: x.features))
    predictionAndLabels = indexedTESTbinary.map(lambda lp: lp.label).zip(predictions)

    # Area under ROC curve
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

<span data-ttu-id="0a908-347">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-347">**OUTPUT**</span></span>

<span data-ttu-id="0a908-348">Området under ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="0a908-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="0a908-349">Tidsåtgång tooexecute ovanför cellen: 28.13 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-349">Time taken tooexecute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="0a908-350">Förutsäga tips belopp med regression modeller (inte med KA)</span><span class="sxs-lookup"><span data-stu-id="0a908-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="0a908-351">Det här avsnittet visas hur användning tre modeller för hello regression uppgiften: förutsäga hello tips summa för taxi resa baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="0a908-351">This section shows how use three models for hello regression task: predict hello tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="0a908-352">hello modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="0a908-352">hello models presented are:</span></span>

* <span data-ttu-id="0a908-353">Reglerats linjär regression</span><span class="sxs-lookup"><span data-stu-id="0a908-353">Regularized linear regression</span></span>
* <span data-ttu-id="0a908-354">Slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="0a908-354">Random forest</span></span>
* <span data-ttu-id="0a908-355">Toning träd</span><span class="sxs-lookup"><span data-stu-id="0a908-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="0a908-356">Dessa modeller beskrivs i hello introduktion.</span><span class="sxs-lookup"><span data-stu-id="0a908-356">These models were described in hello introduction.</span></span> <span data-ttu-id="0a908-357">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="0a908-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="0a908-358">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="0a908-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="0a908-359">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="0a908-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="0a908-360">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="0a908-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="0a908-361">AZURE Obs: Korsvalidering används inte med hello tre regression modeller i det här avsnittet, eftersom det som visas i detalj för hello logistic regression modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-361">AZURE NOTE: Cross-validation is not used with hello three regression models in this section, since this was shown in detail for hello logistic regression models.</span></span> <span data-ttu-id="0a908-362">Ett exempel som visar hur toouse KA med elastisk Net för linjär regression tillhandahålls i hello tillägg av det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0a908-362">An example showing how toouse CV with Elastic Net for linear regression is provided in hello Appendix of this topic.</span></span>
> 
> <span data-ttu-id="0a908-363">AZURE Obs: I vår erfarenhet det kan finnas problem med konvergens LinearRegressionWithSGD modeller och parametrar måste toobe ändras/optimerad noggrant för att erhålla en giltig modell.</span><span class="sxs-lookup"><span data-stu-id="0a908-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="0a908-364">Skalning av variabler avsevärt hjälper med konvergens.</span><span class="sxs-lookup"><span data-stu-id="0a908-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="0a908-365">Elastisk net regression som visas i hello bilaga toothis artikeln kan också användas i stället för LinearRegressionWithSGD.</span><span class="sxs-lookup"><span data-stu-id="0a908-365">Elastic net regression, shown in hello Appendix toothis topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="0a908-366">Linjär regression med SGD</span><span class="sxs-lookup"><span data-stu-id="0a908-366">Linear regression with SGD</span></span>
<span data-ttu-id="0a908-367">hello kod i det här avsnittet visar hur skalas toouse funktioner tootrain en linjär regression som använder stokastisk toning härstammar (SGD) för optimering, och hur tooscore, utvärdera och spara hello modellen i Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="0a908-367">hello code in this section shows how toouse scaled features tootrain a linear regression that uses stochastic gradient descent (SGD) for optimization, and how tooscore, evaluate, and save hello model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="0a908-368">Det kan finnas problem med hello konvergens LinearRegressionWithSGD modeller i vår erfarenhet och parametrar måste toobe ändras/optimerad noggrant för att erhålla en giltig modell.</span><span class="sxs-lookup"><span data-stu-id="0a908-368">In our experience, there can be issues with hello convergence of LinearRegressionWithSGD models, and parameters need toobe changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="0a908-369">Skalning av variabler avsevärt hjälper med konvergens.</span><span class="sxs-lookup"><span data-stu-id="0a908-369">Scaling of variables significantly helps with convergence.</span></span>
> 
> 

    # LINEAR REGRESSION WITH SGD 

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

    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    linearregressionfilename = "LinearRegressionWithSGD_" + datestamp;
    dirfilename = modelDir + linearregressionfilename;

    linearModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="0a908-370">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-370">**OUTPUT**</span></span>

<span data-ttu-id="0a908-371">Koefficienter: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,- 0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="0a908-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="0a908-372">Fånga: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="0a908-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="0a908-373">RMSE = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="0a908-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="0a908-374">R-sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="0a908-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="0a908-375">Tidsåtgång tooexecute ovanför cellen: 38.62 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-375">Time taken tooexecute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="0a908-376">Slumpmässiga skog regression</span><span class="sxs-lookup"><span data-stu-id="0a908-376">Random Forest regression</span></span>
<span data-ttu-id="0a908-377">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en slumpmässig Skogsmodell som beräknar tips belopp för hello NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="0a908-377">hello code in this section shows how tootrain, evaluate, and save a random forest model that predicts tip amount for hello NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="0a908-378">Korsvalidering med parametern omfattande med anpassad kod har angetts i hello tillägg.</span><span class="sxs-lookup"><span data-stu-id="0a908-378">Cross-validation with parameter sweeping using custom code is provided in hello appendix.</span></span>
> 
> 

    #PREDICT TIP AMOUNTS USING RANDOM FOREST

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics


    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=25, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=10, maxBins=32)
    # UN-COMMENT IF YOU WANT tooPRING TREES
    #print('Learned classification forest model:')
    #print(rfModel.toDebugString())

    # PREDICT AND EVALUATE ON TEST DATA-SET
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = oneHotTESTreg.map(lambda lp: lp.label).zip(predictions)

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

<span data-ttu-id="0a908-379">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-379">**OUTPUT**</span></span>

<span data-ttu-id="0a908-380">RMSE = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="0a908-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="0a908-381">R-sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="0a908-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="0a908-382">Tidsåtgång tooexecute ovanför cellen: 25.98 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-382">Time taken tooexecute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="0a908-383">Toning den träd regression</span><span class="sxs-lookup"><span data-stu-id="0a908-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="0a908-384">hello kod i det här avsnittet visar hur tootrain, utvärdera och spara en toning den träd modell som beräknar tips belopp för hello NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="0a908-384">hello code in this section shows how tootrain, evaluate, and save a gradient boosting trees model that predicts tip amount for hello NYC taxi trip data.</span></span>

<span data-ttu-id="0a908-385">** Träna och utvärdera **</span><span class="sxs-lookup"><span data-stu-id="0a908-385">**Train and evaluate **</span></span>

    #PREDICT TIP AMOUNTS USING GRADIENT BOOSTING TREES

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel
    from pyspark.mllib.util import MLUtils

    # TRAIN MODEL
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}
    gbtModel = GradientBoostedTrees.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo, 
                                                    numIterations=10, maxBins=32, maxDepth = 4, learningRate=0.1)

    # EVALUATE A TEST DATA-SET
    predictions = gbtModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES
    test_predictions= sqlContext.createDataFrame(predictionAndLabels)
    test_predictions_pddf = test_predictions.toPandas()

    # SAVE MODEL IN BLOB
    datestamp = unicode(datetime.datetime.now()).replace(' ','').replace(':','_');
    btregressionfilename = "GradientBoostingTreeRegression_" + datestamp;
    dirfilename = modelDir + btregressionfilename;
    gbtModel.save(sc, dirfilename)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-386">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-386">**OUTPUT**</span></span>

<span data-ttu-id="0a908-387">RMSE = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="0a908-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="0a908-388">R-sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="0a908-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="0a908-389">Tidsåtgång tooexecute ovanför cellen: 20,9 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-389">Time taken tooexecute above cell: 20.9 seconds</span></span>

<span data-ttu-id="0a908-390">**Rita**</span><span class="sxs-lookup"><span data-stu-id="0a908-390">**Plot**</span></span>

<span data-ttu-id="0a908-391">*tmp_results* registreras som en Hive-tabell i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="0a908-391">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="0a908-392">Resultat från hello tabellen är resultatet i hello *sqlResults* data ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="0a908-392">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="0a908-393">Här är hello kod</span><span class="sxs-lookup"><span data-stu-id="0a908-393">Here is hello code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="0a908-394">Här är hello kod tooplot hello data med hjälp av hello Jupyter-server.</span><span class="sxs-lookup"><span data-stu-id="0a908-394">Here is hello code tooplot hello data using hello Jupyter server.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import numpy as np

    # PLOT
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='_1', y='_2', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['_1'], sqlResults['_2'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    ax.plot(sqlResults['_1'], fit[0] * sqlResults['_1'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 15])
    plt.show(ax)

![Aktuella-vs-förutsade-tips-belopp](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/actual-vs-predicted-tips.png)

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="0a908-396">Bilaga: Ytterligare regression uppgifter med parametern Svep mellan verifiering</span><span class="sxs-lookup"><span data-stu-id="0a908-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="0a908-397">Den här bilagan innehåller kod som visar hur toodo KA med elastisk net för linjär regression och hur toodo KA med parametern Sopa med anpassad kod för slumpmässiga skog regression.</span><span class="sxs-lookup"><span data-stu-id="0a908-397">This appendix contains code showing how toodo CV using Elastic net for linear regression and how toodo CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="0a908-398">Mellan med hjälp av net elastisk för linjär regression</span><span class="sxs-lookup"><span data-stu-id="0a908-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="0a908-399">hello kod i det här avsnittet visar hur toodo mellan med hjälp av net elastisk för linjär regression och hur tooevaluate hello modell mot testdata.</span><span class="sxs-lookup"><span data-stu-id="0a908-399">hello code in this section shows how toodo cross validation using Elastic net for linear regression and how tooevaluate hello model against test data.</span></span>

    ###  CV USING ELASTIC NET FOR LINEAR REGRESSION

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.regression import LinearRegression
    from pyspark.ml import Pipeline
    from pyspark.ml.evaluation import RegressionEvaluator
    from pyspark.ml.tuning import CrossValidator, ParamGridBuilder

    # DEFINE ALGORITHM/MODEL
    lr = LinearRegression()

    # DEFINE GRID PARAMETERS
    paramGrid = ParamGridBuilder().addGrid(lr.regParam, (0.01, 0.1))\
                                  .addGrid(lr.maxIter, (5, 10))\
                                  .addGrid(lr.tol, (1e-4, 1e-5))\
                                  .addGrid(lr.elasticNetParam, (0.25,0.75))\
                                  .build() 

    # DEFINE PIPELINE 
    # SIMPLY hello MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT tooDATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses hello best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT tooDF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-400">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-400">**OUTPUT**</span></span>

<span data-ttu-id="0a908-401">Tidsåtgång tooexecute ovanför cellen: 161.21 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-401">Time taken tooexecute above cell: 161.21  seconds</span></span>

<span data-ttu-id="0a908-402">**Utvärdera med R SQR mått**</span><span class="sxs-lookup"><span data-stu-id="0a908-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="0a908-403">*tmp_results* registreras som en Hive-tabell i hello föregående cell.</span><span class="sxs-lookup"><span data-stu-id="0a908-403">*tmp_results* is registered as a Hive table in hello previous cell.</span></span> <span data-ttu-id="0a908-404">Resultat från hello tabellen är resultatet i hello *sqlResults* data ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="0a908-404">Results from hello table are output into hello *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="0a908-405">Här är hello kod</span><span class="sxs-lookup"><span data-stu-id="0a908-405">Here is hello code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="0a908-406">Här är hello kod toocalculate R sqr.</span><span class="sxs-lookup"><span data-stu-id="0a908-406">Here is hello code toocalculate R-sqr.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="0a908-407">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-407">**OUTPUT**</span></span>

<span data-ttu-id="0a908-408">R-sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="0a908-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="0a908-409">Mellan verifiering med parametern Svep med anpassad kod för slumpmässiga skog regression</span><span class="sxs-lookup"><span data-stu-id="0a908-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="0a908-410">hello kod i det här avsnittet visar hur toodo mellan verifiering med parametern Svep med anpassad kod för slumpmässiga skog regression och hur tooevaluate hello modell mot testdata.</span><span class="sxs-lookup"><span data-stu-id="0a908-410">hello code in this section shows how toodo cross validation with parameter sweep using custom code for random forest regression and how tooevaluate hello model against test data.</span></span>

    # RECORD START TIME
    timestart= datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    # GET ACCURARY FOR HYPERPARAMETERS BASED ON CROSS-VALIDATION IN TRAINING DATA-SET
    from pyspark.mllib.tree import RandomForest, RandomForestModel
    from pyspark.mllib.util import MLUtils
    from pyspark.mllib.evaluation import RegressionMetrics
    from sklearn.grid_search import ParameterGrid

    ## CREATE PARAMETER GRID
    grid = [{'maxDepth': [5,10], 'numTrees': [25,50]}]
    paramGrid = list(ParameterGrid(grid))

    ## SPECIFY LEVELS OF CATEGORICAL VARIBLES
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    # SPECIFY NUMFOLDS AND ARRAY tooHOLD METRICS
    nFolds = 3;
    numModels = len(paramGrid)
    h = 1.0 / nFolds;
    metricSum = np.zeros(numModels);

    for i in range(nFolds):
        # Create training and x-validation sets
        validateLB = i * h
        validateUB = (i + 1) * h
        condition = (trainData["rand"] >= validateLB) & (trainData["rand"] < validateUB)
        validation = trainData.filter(condition)
        # Create labeled points from data-frames
        if i > 0:
            trainCVLabPt.unpersist()
            validationLabPt.unpersist()
        trainCV = trainData.filter(~condition)
        trainCVLabPt = trainCV.map(parseRowIndexingRegression)
        trainCVLabPt.cache()
        validationLabPt = validation.map(parseRowIndexingRegression)
        validationLabPt.cache()
        # For parameter sets compute metrics from x-validation
        for j in range(numModels):
            maxD = paramGrid[j]['maxDepth']
            numT = paramGrid[j]['numTrees']
            # Train logistic regression model with hypermarameter set
            rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=numT, featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=maxD, maxBins=32)
            predictions = rfModel.predict(validationLabPt.map(lambda x: x.features))
            predictionAndLabels = validationLabPt.map(lambda lp: lp.label).zip(predictions)
            # Use ROC-AUC as accuracy metrics
            validMetrics = RegressionMetrics(predictionAndLabels)
            metric = validMetrics.rootMeanSquaredError
            metricSum[j] += metric

    avgAcc = metricSum/nFolds;
    bestParam = paramGrid[np.argmin(avgAcc)];

    # UNPERSIST OBJECTS
    trainCVLabPt.unpersist()
    validationLabPt.unpersist()

    ## TRAIN FINAL MODL WIHT BEST PARAMETERS
    rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                        numTrees=bestParam['numTrees'], featureSubsetStrategy="auto",
                                        impurity='variance', maxDepth=bestParam['maxDepth'], maxBins=32)

    # EVALUATE MODEL ON TEST DATA
    predictions = rfModel.predict(indexedTESTreg.map(lambda x: x.features))
    predictionAndLabels = indexedTESTreg.map(lambda lp: lp.label).zip(predictions)

    #PRINT TEST METRICS
    testMetrics = RegressionMetrics(predictionAndLabels)
    print("RMSE = %s" % testMetrics.rootMeanSquaredError)
    print("R-sqr = %s" % testMetrics.r2)

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken tooexecute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="0a908-411">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-411">**OUTPUT**</span></span>

<span data-ttu-id="0a908-412">RMSE = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="0a908-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="0a908-413">R-sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="0a908-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="0a908-414">Tidsåtgång tooexecute ovanför cellen: 69.17 sekunder</span><span class="sxs-lookup"><span data-stu-id="0a908-414">Time taken tooexecute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="0a908-415">Rensa objekt från minne och skriva ut modellen platser</span><span class="sxs-lookup"><span data-stu-id="0a908-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="0a908-416">Använd `unpersist()` toodelete objekt som cachelagrats i minnet.</span><span class="sxs-lookup"><span data-stu-id="0a908-416">Use `unpersist()` toodelete objects cached in memory.</span></span>

    # UNPERSIST OBJECTS CACHED IN MEMORY

    # REMOVE ORIGINAL DFs
    taxi_df_train_cleaned.unpersist()
    taxi_df_train_with_newFeatures.unpersist()
    trainData.unpersist()
    trainData.unpersist()

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


<span data-ttu-id="0a908-417">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-417">**OUTPUT**</span></span>

<span data-ttu-id="0a908-418">PythonRDD [122] vid RDD på PythonRDD.scala: 43</span><span class="sxs-lookup"><span data-stu-id="0a908-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="0a908-419">** Utskriften sökvägen toomodel filer toobe som används i hello förbrukning anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="0a908-419">**Printout path toomodel files toobe used in hello consumption notebook.</span></span> <span data-ttu-id="0a908-420">** tooconsume och poäng en självständig-datauppsättning, du behöver toocopy och klistra in dessa filnamn i hello ”förbrukning bärbar dator”.</span><span class="sxs-lookup"><span data-stu-id="0a908-420">** tooconsume and score an independent data-set, you need toocopy and paste these file names in hello "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="0a908-421">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="0a908-421">**OUTPUT**</span></span>

<span data-ttu-id="0a908-422">logisticRegFileLoc = modelDir + ”LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528”</span><span class="sxs-lookup"><span data-stu-id="0a908-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="0a908-423">linearRegFileLoc = modelDir + ”LinearRegressionWithSGD_2016-05-0316_51_28.433670”</span><span class="sxs-lookup"><span data-stu-id="0a908-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="0a908-424">randomForestClassificationFileLoc = modelDir + ”RandomForestClassification_2016-05-0316_50_17.454440”</span><span class="sxs-lookup"><span data-stu-id="0a908-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="0a908-425">randomForestRegFileLoc = modelDir + ”RandomForestRegression_2016-05-0316_51_57.331730”</span><span class="sxs-lookup"><span data-stu-id="0a908-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="0a908-426">BoostedTreeClassificationFileLoc = modelDir + ”GradientBoostingTreeClassification_2016-05-0316_50_40.138809”</span><span class="sxs-lookup"><span data-stu-id="0a908-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="0a908-427">BoostedTreeRegressionFileLoc = modelDir + ”GradientBoostingTreeRegression_2016-05-0316_52_18.827237”</span><span class="sxs-lookup"><span data-stu-id="0a908-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="0a908-428">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a908-428">What's next?</span></span>
<span data-ttu-id="0a908-429">Nu när du har skapat regression och klassificering modeller med hello Spark MlLib, är du redo toolearn hur tooscore och utvärdera dessa modeller.</span><span class="sxs-lookup"><span data-stu-id="0a908-429">Now that you have created regression and classification models with hello Spark MlLib, you are ready toolearn how tooscore and evaluate these models.</span></span>

<span data-ttu-id="0a908-430">**Modellen förbrukning:** toolearn hur tooscore och utvärdera hello klassificering och regression modeller som skapats i det här avsnittet, finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="0a908-430">**Model consumption:** toolearn how tooscore and evaluate hello classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

