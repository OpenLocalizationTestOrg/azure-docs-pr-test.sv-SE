---
title: Avancerade datagranskning och modellering med Spark | Microsoft Docs
description: "Använda HDInsight Spark datagranskning och träna binär klassificering och regression modeller med korsvalidering och hyperparameter optimering."
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
ms.openlocfilehash: e6bf6bd3c905f077841ef166540337a251b91ad1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-data-exploration-and-modeling-with-spark"></a><span data-ttu-id="59358-103">Avancerad datagranskning och modellering med Spark</span><span class="sxs-lookup"><span data-stu-id="59358-103">Advanced data exploration and modeling with Spark</span></span>
[!INCLUDE [machine-learning-spark-modeling](../../includes/machine-learning-spark-modeling.md)]

<span data-ttu-id="59358-104">Den här genomgången använder HDInsight Spark datagranskning och träna binär klassificering och regression modeller med korsvalidering och hyperparameter optimering på ett urval av NYC Taxitransport resa och färdavgiften 2013 dataset.</span><span class="sxs-lookup"><span data-stu-id="59358-104">This walkthrough uses HDInsight Spark to do data exploration and train binary classification and regression models using cross-validation and hyperparameter optimization on a sample of the NYC taxi trip and fare 2013 dataset.</span></span> <span data-ttu-id="59358-105">Den vägleder dig genom stegen för den [datavetenskap Process](http://aka.ms/datascienceprocess), slutpunkt-till-slutpunkt, använder ett HDInsight Spark-kluster för bearbetning och Azure BLOB för att lagra data och modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-105">It walks you through the steps of the [Data Science Process](http://aka.ms/datascienceprocess), end-to-end, using an HDInsight Spark cluster for processing and Azure blobs to store the data and the models.</span></span> <span data-ttu-id="59358-106">Processen utforskar visualizes data som tillkommit från ett Azure Storage Blob och förbereder data för att skapa förutsägelsemodeller.</span><span class="sxs-lookup"><span data-stu-id="59358-106">The process explores and visualizes data brought in from an Azure Storage Blob and then prepares the data to build predictive models.</span></span> <span data-ttu-id="59358-107">Python har använts kod lösningen och för att visa relevanta områden.</span><span class="sxs-lookup"><span data-stu-id="59358-107">Python has been used to code the solution and to show the relevant plots.</span></span> <span data-ttu-id="59358-108">Dessa modeller är versionen med Spark MLlib toolkit till binär klassificering och regressionen modeling aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="59358-108">These models are build using the Spark MLlib toolkit to do binary classification and regression modeling tasks.</span></span> 

* <span data-ttu-id="59358-109">Den **binär klassificering** uppgift är att förutsäga huruvida ett tips är betald för resan.</span><span class="sxs-lookup"><span data-stu-id="59358-109">The **binary classification** task is to predict whether or not a tip is paid for the trip.</span></span> 
* <span data-ttu-id="59358-110">Den **regression** uppgift är att förutsäga mängden tips baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-110">The **regression** task is to predict the amount of the tip based on other tip features.</span></span> 

<span data-ttu-id="59358-111">Modellering stegen innehåller också kod visar hur du träna, utvärdera och spara varje typ av modellen.</span><span class="sxs-lookup"><span data-stu-id="59358-111">The modeling steps also contain code showing how to train, evaluate, and save each type of model.</span></span> <span data-ttu-id="59358-112">I avsnittet beskrivs några av samma mark som den [datagranskning och modellering med Spark](machine-learning-data-science-spark-data-exploration-modeling.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="59358-112">The topic covers some of the same ground as the [Data exploration and modeling with Spark](machine-learning-data-science-spark-data-exploration-modeling.md) topic.</span></span> <span data-ttu-id="59358-113">Men det är mer ”avancerad” den också använder korsvalidering med hyperparameter omfattande för att träna optimalt korrekt klassificering och regression modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-113">But it is more "advanced" in that it also uses cross-validation with hyperparameter sweeping to train optimally accurate classification and regression models.</span></span> 

<span data-ttu-id="59358-114">**Korsvalidering (KA)** är en teknik som utvärderar hur väl en modell med en känd uppsättning data är en generalisering att förutsäga funktioner för datauppsättningar som den inte har tränats.</span><span class="sxs-lookup"><span data-stu-id="59358-114">**Cross-validation (CV)** is a technique that assesses how well a model trained on a known set of data generalizes to predicting the features of datasets on which it has not been trained.</span></span>  <span data-ttu-id="59358-115">En vanlig tillämpning används här är att dela upp en datamängd i K vikningar och träna modellen i ett resursallokering sätt på alla utom en av vikningar.</span><span class="sxs-lookup"><span data-stu-id="59358-115">A common implementation used here is to divide a dataset into K folds and then train the model in a round-robin fashion on all but one of the folds.</span></span> <span data-ttu-id="59358-116">Utvärdera möjligheten för modellen för förutsägelse korrekt när testas mot oberoende datauppsättning i den här vikning som inte används för att träna modellen.</span><span class="sxs-lookup"><span data-stu-id="59358-116">The ability of the model to prediction accurately when tested against the independent dataset in this fold not used to train the model is assessed.</span></span>

<span data-ttu-id="59358-117">**Optimering av Hyperparameter** är problem med att välja en uppsättning justeringsmodeller för en inlärningsalgoritm vanligtvis med målet att optimera ett mått på den algoritm prestanda på en fristående uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="59358-117">**Hyperparameter optimization** is the problem of choosing a set of hyperparameters for a learning algorithm, usually with the goal of optimizing a measure of the algorithm's performance on an independent data set.</span></span> <span data-ttu-id="59358-118">**Justeringsmodeller** är värden som anges utanför modellen utbildning proceduren.</span><span class="sxs-lookup"><span data-stu-id="59358-118">**Hyperparameters** are values that must be specified outside of the model training procedure.</span></span> <span data-ttu-id="59358-119">Antaganden om dessa värden kan påverka flexibilitet och korrektheten i modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-119">Assumptions about these values can impact the flexibility and accuracy of the models.</span></span> <span data-ttu-id="59358-120">Beslutsträd har till exempel justeringsmodeller, till exempel önskad djup och många blad i trädet.</span><span class="sxs-lookup"><span data-stu-id="59358-120">Decision trees have hyperparameters, for example, such as the desired depth and number of leaves in the tree.</span></span> <span data-ttu-id="59358-121">Support Vector datorer (SVMs) kräver en term som felklassificering påföljder.</span><span class="sxs-lookup"><span data-stu-id="59358-121">Support Vector Machines (SVMs) require setting a misclassification penalty term.</span></span> 

<span data-ttu-id="59358-122">Ett vanligt sätt att utföra hyperparameter optimering används här är en rutnätet sökning eller en **parametern Svep**.</span><span class="sxs-lookup"><span data-stu-id="59358-122">A common way to perform hyperparameter optimization used here is a grid search, or a **parameter sweep**.</span></span> <span data-ttu-id="59358-123">Det här består av utför en uttömmande sökning via värden en viss delmängd av hyperparameter utrymme för en inlärningsalgoritm.</span><span class="sxs-lookup"><span data-stu-id="59358-123">This consists of performing an exhaustive search through the values a specified subset of the hyperparameter space for a learning algorithm.</span></span> <span data-ttu-id="59358-124">Mellan verifiering kan tillhandahålla ett prestanda mått för att lösa bästa möjliga resultat som genereras av algoritmen rutnätet sökning.</span><span class="sxs-lookup"><span data-stu-id="59358-124">Cross validation can supply a performance metric to sort out the optimal results produced by the grid search algorithm.</span></span> <span data-ttu-id="59358-125">KA användas med hyperparameter omfattande hjälper gränsen problem angående overfitting en modell till utbildningsdata så att modellen behåller kapacitet att tillämpa en allmän uppsättning data från vilket utbildning data extraheras.</span><span class="sxs-lookup"><span data-stu-id="59358-125">CV used with hyperparameter sweeping helps limit problems like overfitting a model to training data so that the model retains the capacity to apply to the general set of data from which the training data was extracted.</span></span>

<span data-ttu-id="59358-126">Modeller som vi använder inkluderar logistic och linjär regression, slumpmässiga skogar och toning ökat träd:</span><span class="sxs-lookup"><span data-stu-id="59358-126">The models we use include logistic and linear regression, random forests, and gradient boosted trees:</span></span>

* <span data-ttu-id="59358-127">[Linjär regression med SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) är en linjär regressionsmodell som använder en Stokastisk toning härstammar (SGD)-metod och skalning för att förutsäga tips belopp betald för optimering och funktion.</span><span class="sxs-lookup"><span data-stu-id="59358-127">[Linear regression with SGD](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.regression.LinearRegressionWithSGD) is a linear regression model that uses a Stochastic Gradient Descent (SGD) method and for optimization and feature scaling to predict the tip amounts paid.</span></span> 
* <span data-ttu-id="59358-128">[Logistic regression med LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) eller ”logit” regression är en regressionsmodell som kan användas när den beroende variabeln är kategoriska att göra dataklassificering.</span><span class="sxs-lookup"><span data-stu-id="59358-128">[Logistic regression with LBFGS](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.classification.LogisticRegressionWithLBFGS) or "logit" regression, is a regression model that can be used when the dependent variable is categorical to do data classification.</span></span> <span data-ttu-id="59358-129">LBFGS är en kvasi Karlsson optimering algoritm som uppskattar algoritmen Broyden – Fletcher – Goldfarb – Shanno (BFGS) med en begränsad mängd ledigt diskutrymme på datorn och som ofta används i machine learning.</span><span class="sxs-lookup"><span data-stu-id="59358-129">LBFGS is a quasi-Newton optimization algorithm that approximates the Broyden–Fletcher–Goldfarb–Shanno (BFGS) algorithm using a limited amount of computer memory and that is widely used in machine learning.</span></span>
* <span data-ttu-id="59358-130">[Slumpmässiga skogar](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="59358-130">[Random forests](http://spark.apache.org/docs/latest/mllib-ensembles.html#Random-Forests) are ensembles of decision trees.</span></span>  <span data-ttu-id="59358-131">De kombinera många beslutsträd för att minska risken för overfitting.</span><span class="sxs-lookup"><span data-stu-id="59358-131">They combine many decision trees to reduce the risk of overfitting.</span></span> <span data-ttu-id="59358-132">Slumpmässiga skogar används för regression och klassificering kan hantera kategoriska funktioner och kan utökas till inställningen för multiklass-baserad klassificering.</span><span class="sxs-lookup"><span data-stu-id="59358-132">Random forests are used for regression and classification and can handle categorical features and can be extended to the multiclass classification setting.</span></span> <span data-ttu-id="59358-133">De kräver inte funktionen skalning och kan avbilda icke-linjärt och funktion interaktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-133">They do not require feature scaling and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="59358-134">Slumpmässiga skogar är en av de mest framgångsrika modeller för klassificering och regression för maskininlärning.</span><span class="sxs-lookup"><span data-stu-id="59358-134">Random forests are one of the most successful machine learning models for classification and regression.</span></span>
* <span data-ttu-id="59358-135">[Toningen ökat träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="59358-135">[Gradient boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="59358-136">GBTs träna beslutsträd upprepade gånger för att minimera dataförlust funktionen.</span><span class="sxs-lookup"><span data-stu-id="59358-136">GBTs train decision trees iteratively to minimize a loss function.</span></span> <span data-ttu-id="59358-137">GBTs för regression och klassificering kan hantera kategoriska funktioner, kräver inte funktionen skalning och kan avbilda icke-linjärt och funktion interaktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-137">GBTs are used for regression and classification and can handle categorical features, do not require feature scaling, and are able to capture non-linearities and feature interactions.</span></span> <span data-ttu-id="59358-138">De kan också användas i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="59358-138">They can also be used in a multiclass-classification setting.</span></span>

<span data-ttu-id="59358-139">Modeling exempel med hjälp av KA och Hyperparameter Svep visas för den binära klassificeringsproblem.</span><span class="sxs-lookup"><span data-stu-id="59358-139">Modeling examples using CV and Hyperparameter sweep are shown for the binary classification problem.</span></span> <span data-ttu-id="59358-140">Enklare exempel visas (utan parametern sveps) i avsnittet för regression uppgifter.</span><span class="sxs-lookup"><span data-stu-id="59358-140">Simpler examples (without parameter sweeps) are presented in the main topic for regression tasks.</span></span> <span data-ttu-id="59358-141">Men i tillägget, verifiering med elastisk net för linjär regression och KA med parametern Svep för slumpmässiga skog regression visas också.</span><span class="sxs-lookup"><span data-stu-id="59358-141">But in the appendix, validation using elastic net for linear regression and CV with parameter sweep using for random forest regression are also presented.</span></span> <span data-ttu-id="59358-142">Den **elastisk net** är en reglerats regression metod för Passning linjär regression modeller som linjärt kombinerar L1 och L2 mått som böter av den [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) och [upphöjning](https://en.wikipedia.org/wiki/Tikhonov_regularization)metoder.</span><span class="sxs-lookup"><span data-stu-id="59358-142">The **elastic net** is a regularized regression method for fitting linear regression models that linearly combines the L1 and L2 metrics as penalties of the [lasso](https://en.wikipedia.org/wiki/Lasso%20%28statistics%29) and [ridge](https://en.wikipedia.org/wiki/Tikhonov_regularization) methods.</span></span>   

> [!NOTE]
> <span data-ttu-id="59358-143">Även om Spark MLlib toolkit är utformat för att arbeta med stora datauppsättningar, används ett relativt litet exempel (cirka 30 Mb med 170K rader, om 0,1% av den ursprungliga datauppsättningen NYC) här i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="59358-143">Although the Spark MLlib toolkit is designed to work on large datasets, a relatively small sample (~30 Mb using 170K rows, about 0.1% of the original NYC dataset) is used here for convenience.</span></span> <span data-ttu-id="59358-144">Övning som anges här körs effektivt (i 10 minuter) på ett HDInsight-kluster med 2 arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="59358-144">The exercise given here runs efficiently (in about 10 minutes) on an HDInsight cluster with 2 worker nodes.</span></span> <span data-ttu-id="59358-145">Samma kod, med mindre ändringar kan användas för att bearbeta större-datamängder med lämpliga ändringar för cachelagring av data i minnet och ändrar storlek på klustret.</span><span class="sxs-lookup"><span data-stu-id="59358-145">The same code, with minor modifications, can be used to process larger data-sets, with appropriate modifications for caching data in memory and changing the cluster size.</span></span>
> 
> 

## <a name="setup-spark-clusters-and-notebooks"></a><span data-ttu-id="59358-146">Konfigurera: Spark-kluster och bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="59358-146">Setup: Spark clusters and notebooks</span></span>
<span data-ttu-id="59358-147">Konfigurationsstegen och kod finns i den här genomgången för att använda ett HDInsight Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="59358-147">Setup steps and code are provided in this walkthrough for using an HDInsight Spark 1.6.</span></span> <span data-ttu-id="59358-148">Men Jupyter-anteckningsböcker som har angetts för både HDInsight Spark 1.6 och 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="59358-148">But Jupyter notebooks are provided for both HDInsight Spark 1.6 and Spark 2.0 clusters.</span></span> <span data-ttu-id="59358-149">En beskrivning av bärbara datorer och länkar till dem finns i den [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) för GitHub-lagringsplats som innehåller dessa.</span><span class="sxs-lookup"><span data-stu-id="59358-149">A description of the notebooks and links to them are provided in the [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) for the GitHub repository containing them.</span></span> <span data-ttu-id="59358-150">Dessutom koden här länkade anteckningsböcker är generisk och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="59358-150">Moreover, the code here and in the linked notebooks is generic and should work on any Spark cluster.</span></span> <span data-ttu-id="59358-151">Om du inte använder HDInsight Spark kanske klustret installation och hantering av stegen skiljer sig från vad som anges här.</span><span class="sxs-lookup"><span data-stu-id="59358-151">If you are not using HDInsight Spark, the cluster setup and management steps may be slightly different from what is shown here.</span></span> <span data-ttu-id="59358-152">För enkelhetens skull är här länkar till Jupyter-anteckningsböcker för Spark 1.6 och 2.0 för att köras i pyspark-kerneln Jupyter Notebook-Server:</span><span class="sxs-lookup"><span data-stu-id="59358-152">For convenience, here are the links to the Jupyter notebooks for Spark 1.6 and 2.0 to be run in the pyspark kernel of the Jupyter Notebook server:</span></span>

### <a name="spark-16-notebooks"></a><span data-ttu-id="59358-153">Spark 1.6 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="59358-153">Spark 1.6 notebooks</span></span>

<span data-ttu-id="59358-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): innehåller avsnitt i anteckningsboken #1 och modellen utveckling med hjälp av hyperparameter justering och korsvalidering.</span><span class="sxs-lookup"><span data-stu-id="59358-154">[pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Includes topics in notebook #1, and model development using hyperparameter tuning and cross-validation.</span></span>

### <a name="spark-20-notebooks"></a><span data-ttu-id="59358-155">Spark 2.0 bärbara datorer</span><span class="sxs-lookup"><span data-stu-id="59358-155">Spark 2.0 notebooks</span></span>

<span data-ttu-id="59358-156">[Spark2.0-pySpark3-Machine-Learning-data-Science-Spark-Advanced-data-exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): den här filen innehåller information om hur du utför datagranskning modellering och Poängberäkningen i 2.0 Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="59358-156">[Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): This file provides information on how to perform data exploration, modeling, and scoring in Spark 2.0 clusters.</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="setup-storage-locations-libraries-and-the-preset-spark-context"></a><span data-ttu-id="59358-157">Konfigurera: lagringsplatser, bibliotek och förinställda Spark-kontexten</span><span class="sxs-lookup"><span data-stu-id="59358-157">Setup: storage locations, libraries, and the preset Spark context</span></span>
<span data-ttu-id="59358-158">Spark kan läsa och skriva till Azure Storage Blob (även kallat WASB).</span><span class="sxs-lookup"><span data-stu-id="59358-158">Spark is able to read and write to Azure Storage Blob (also known as WASB).</span></span> <span data-ttu-id="59358-159">Så någon av dina befintliga data lagras kan det bearbetas med Spark och resultatet lagras igen i WASB.</span><span class="sxs-lookup"><span data-stu-id="59358-159">So any of your existing data stored there can be processed using Spark and the results stored again in WASB.</span></span>

<span data-ttu-id="59358-160">Om du vill spara modeller eller filer i WASB måste sökvägen anges korrekt.</span><span class="sxs-lookup"><span data-stu-id="59358-160">To save models or files in WASB, the path needs to be specified properly.</span></span> <span data-ttu-id="59358-161">Standardbehållaren kopplade till Spark-klustret kan refereras med en sökväg som börjar med ”: wasb: / / /”.</span><span class="sxs-lookup"><span data-stu-id="59358-161">The default container attached to the Spark cluster can be referenced using a path beginning with: "wasb:///".</span></span> <span data-ttu-id="59358-162">Andra platser refererar till ”wasb: / /”.</span><span class="sxs-lookup"><span data-stu-id="59358-162">Other locations are referenced by “wasb://”.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-wasb"></a><span data-ttu-id="59358-163">Ange katalogsökvägar för lagringsplatser i WASB</span><span class="sxs-lookup"><span data-stu-id="59358-163">Set directory paths for storage locations in WASB</span></span>
<span data-ttu-id="59358-164">Följande kodexempel anger platsen för data som ska läsas och sökvägen för modellen Arkivkatalog som modellen sparas:</span><span class="sxs-lookup"><span data-stu-id="59358-164">The following code sample specifies the location of the data to be read and the path for the model storage directory to which the model output is saved:</span></span>

    # SET PATHS TO FILE LOCATIONS: DATA AND MODEL STORAGE

    # LOCATION OF TRAINING DATA
    taxi_train_file_loc = "wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv";


    # SET THE MODEL STORAGE DIRECTORY PATH 
    # NOTE THAT THE FINAL BACKSLASH IN THE PATH IS NEEDED.
    modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";

    # PRINT START TIME
    import datetime
    datetime.datetime.now()

<span data-ttu-id="59358-165">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-165">**OUTPUT**</span></span>

<span data-ttu-id="59358-166">datetime.datetime (2016, 4, 18, 17, 36, 27, 832799)</span><span class="sxs-lookup"><span data-stu-id="59358-166">datetime.datetime(2016, 4, 18, 17, 36, 27, 832799)</span></span>

### <a name="import-libraries"></a><span data-ttu-id="59358-167">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="59358-167">Import libraries</span></span>
<span data-ttu-id="59358-168">Importera nödvändiga bibliotek med följande kod:</span><span class="sxs-lookup"><span data-stu-id="59358-168">Import necessary libraries with the following code:</span></span>

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


### <a name="preset-spark-context-and-pyspark-magics"></a><span data-ttu-id="59358-169">Förinställda Spark-kontexten och PySpark användbara</span><span class="sxs-lookup"><span data-stu-id="59358-169">Preset Spark context and PySpark magics</span></span>
<span data-ttu-id="59358-170">PySpark-kernel som tillhandahålls med Jupyter-anteckningsböcker har en förinställd kontext.</span><span class="sxs-lookup"><span data-stu-id="59358-170">The PySpark kernels that are provided with Jupyter notebooks have a preset context.</span></span> <span data-ttu-id="59358-171">Så du behöver inte ange Spark eller Hive-kontexterna explicit innan du börjar arbeta med programmet som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="59358-171">So you do not need to set the Spark or Hive contexts explicitly before you start working with the application you are developing.</span></span> <span data-ttu-id="59358-172">Dessa kontexter är tillgängliga för dig som standard.</span><span class="sxs-lookup"><span data-stu-id="59358-172">These contexts are available for you by default.</span></span> <span data-ttu-id="59358-173">Dessa kontexter är:</span><span class="sxs-lookup"><span data-stu-id="59358-173">These contexts are:</span></span>

* <span data-ttu-id="59358-174">SC - Spark</span><span class="sxs-lookup"><span data-stu-id="59358-174">sc - for Spark</span></span> 
* <span data-ttu-id="59358-175">sqlContext - för Hive</span><span class="sxs-lookup"><span data-stu-id="59358-175">sqlContext - for Hive</span></span>

<span data-ttu-id="59358-176">PySpark-kerneln innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med %%.</span><span class="sxs-lookup"><span data-stu-id="59358-176">The PySpark kernel provides some predefined “magics”, which are special commands that you can call with %%.</span></span> <span data-ttu-id="59358-177">Det finns två kommandon som används i följande kodexempel.</span><span class="sxs-lookup"><span data-stu-id="59358-177">There are two such commands that are used in these code samples.</span></span>

* <span data-ttu-id="59358-178">**%% lokala** anger att koden i efterföljande rader är att köras lokalt.</span><span class="sxs-lookup"><span data-stu-id="59358-178">**%%local** Specifies that the code in subsequent lines is to be executed locally.</span></span> <span data-ttu-id="59358-179">Koden måste vara giltig Python-kod.</span><span class="sxs-lookup"><span data-stu-id="59358-179">Code must be valid Python code.</span></span>
* <span data-ttu-id="59358-180">**%% sql -o <variable name>**  kör en Hive-fråga mot sqlContext.</span><span class="sxs-lookup"><span data-stu-id="59358-180">**%%sql -o <variable name>** Executes a Hive query against the sqlContext.</span></span> <span data-ttu-id="59358-181">Om parametern -o skickas resultatet av frågan sparas i den %% lokala Python kontext som en Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="59358-181">If the -o parameter is passed, the result of the query is persisted in the %%local Python context as a Pandas DataFrame.</span></span>

<span data-ttu-id="59358-182">För mer information om kärnor för Jupyter-anteckningsböcker och den fördefinierade ”magics” som de tillhandahåller, se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster i HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="59358-182">For more information on the kernels for Jupyter notebooks and the predefined "magics" that they provide, see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="data-ingestion-from-public-blob"></a><span data-ttu-id="59358-183">Datapåfyllning från offentliga blob:</span><span class="sxs-lookup"><span data-stu-id="59358-183">Data ingestion from public blob:</span></span>
<span data-ttu-id="59358-184">Det första steget i vetenskap av data är att mata in data som ska analyseras från källor som var den finns i din datagranskning och modellering miljö.</span><span class="sxs-lookup"><span data-stu-id="59358-184">The first step in the data science process is to ingest the data to be analyzed from sources where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="59358-185">Den här miljön är Spark i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="59358-185">This environment is Spark in this walkthrough.</span></span> <span data-ttu-id="59358-186">Det här avsnittet innehåller koden för att genomföra ett antal aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="59358-186">This section contains the code to complete a series of tasks:</span></span>

* <span data-ttu-id="59358-187">mata in datasampel till modelleras</span><span class="sxs-lookup"><span data-stu-id="59358-187">ingest the data sample to be modeled</span></span>
* <span data-ttu-id="59358-188">Läs i inkommande datauppsättningen (som lagras som en TSV-fil)</span><span class="sxs-lookup"><span data-stu-id="59358-188">read in the input dataset (stored as a .tsv file)</span></span>
* <span data-ttu-id="59358-189">Formatera och rensa data</span><span class="sxs-lookup"><span data-stu-id="59358-189">format and clean the data</span></span>
* <span data-ttu-id="59358-190">Skapa och cachelagra objekt (RDDs eller ramar) i minnet</span><span class="sxs-lookup"><span data-stu-id="59358-190">create and cache objects (RDDs or data-frames) in memory</span></span>
* <span data-ttu-id="59358-191">registrera den som en temp-tabell i SQL-kontext.</span><span class="sxs-lookup"><span data-stu-id="59358-191">register it as a temp-table in SQL-context.</span></span>

<span data-ttu-id="59358-192">Här är koden för datapåfyllning.</span><span class="sxs-lookup"><span data-stu-id="59358-192">Here is the code for data ingestion.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # IMPORT FILE FROM PUBLIC BLOB
    taxi_train_file = sc.textFile(taxi_train_file_loc)

    # GET SCHEMA OF THE FILE FROM HEADER
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

    # CACHE & MATERIALIZE DATA-FRAME IN MEMORY. GOING THROUGH AND COUNTING NUMBER OF ROWS MATERIALIZES THE DATA-FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER DATA-FRAME AS A TEMP-TABLE IN SQL-CONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # PRINT HOW MUCH TIME IT TOOK TO RUN THE CELL
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-193">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-193">**OUTPUT**</span></span>

<span data-ttu-id="59358-194">Åtgången tid för att köra ovanför cellen: 276.62 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-194">Time taken to execute above cell: 276.62 seconds</span></span>

## <a name="data-exploration--visualization"></a><span data-ttu-id="59358-195">Datagranskning & visualiseringen</span><span class="sxs-lookup"><span data-stu-id="59358-195">Data exploration & visualization</span></span>
<span data-ttu-id="59358-196">När data har trätt i Spark, är nästa steg i processen för datavetenskap att får en djupare förståelse av data via undersökning och visualisering.</span><span class="sxs-lookup"><span data-stu-id="59358-196">Once the data has been brought into Spark, the next step in the data science process is to gain deeper understanding of the data through exploration and visualization.</span></span> <span data-ttu-id="59358-197">I det här avsnittet vi undersöka taxi data med SQL-frågor och rita target-variabler och potentiella funktioner för granskning.</span><span class="sxs-lookup"><span data-stu-id="59358-197">In this section, we examine the taxi data using SQL queries and plot the target variables and prospective features for visual inspection.</span></span> <span data-ttu-id="59358-198">Mer specifikt Rita vi frekvensen av passagerare antal i taxi resor frekvensen av tips belopp och hur tips varierar beroende på belopp och typen.</span><span class="sxs-lookup"><span data-stu-id="59358-198">Specifically, we plot the frequency of passenger counts in taxi trips, the frequency of tip amounts, and how tips vary by payment amount and type.</span></span>

### <a name="plot-a-histogram-of-passenger-count-frequencies-in-the-sample-of-taxi-trips"></a><span data-ttu-id="59358-199">Rita ett histogram passagerare antal frekvenser i exemplet på taxi resor</span><span class="sxs-lookup"><span data-stu-id="59358-199">Plot a histogram of passenger count frequencies in the sample of taxi trips</span></span>
<span data-ttu-id="59358-200">Den här koden och efterföljande kodavsnitt kan du använda SQL Magiskt tal för att fråga exemplet och lokala Magiskt tal för att rita data.</span><span class="sxs-lookup"><span data-stu-id="59358-200">This code and subsequent snippets use SQL magic to query the sample and local magic to plot the data.</span></span>

* <span data-ttu-id="59358-201">**SQL-magic (`%%sql`)** i HDInsight PySpark-kerneln stöder enkelt infogade HiveQL frågor mot sqlContext.</span><span class="sxs-lookup"><span data-stu-id="59358-201">**SQL magic (`%%sql`)** The HDInsight PySpark kernel supports easy inline HiveQL queries against the sqlContext.</span></span> <span data-ttu-id="59358-202">Den (-o VARIABLE_NAME) argumentet kvarstår utdata från SQL-frågan som en Pandas DataFrame på Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="59358-202">The (-o VARIABLE_NAME) argument persists the output of the SQL query as a Pandas DataFrame on the Jupyter server.</span></span> <span data-ttu-id="59358-203">Det innebär att den är tillgänglig i lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="59358-203">This means it is available in the local mode.</span></span>
* <span data-ttu-id="59358-204">Den  **`%%local` magic** används för att köra kod lokalt på servern Jupyter som headnode av HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="59358-204">The **`%%local` magic** is used to run code locally on the Jupyter server, which is the headnode of the HDInsight cluster.</span></span> <span data-ttu-id="59358-205">Normalt använder du `%%local` magiskt efter den `%%sql -o` magic används för att köra en fråga.</span><span class="sxs-lookup"><span data-stu-id="59358-205">Typically, you use `%%local` magic after the `%%sql -o` magic is used to run a query.</span></span> <span data-ttu-id="59358-206">Parametern -o skulle sparas utdata från SQL-frågan lokalt.</span><span class="sxs-lookup"><span data-stu-id="59358-206">The -o parameter would persist the output of the SQL query locally.</span></span> <span data-ttu-id="59358-207">Sedan `%%local` magic utlöser en uppsättning kodstycken kör lokalt mot utdata för SQL-frågor som har sparats lokalt.</span><span class="sxs-lookup"><span data-stu-id="59358-207">Then the `%%local` magic triggers the next set of code snippets to run locally against the output of the SQL queries that has been persisted locally.</span></span> <span data-ttu-id="59358-208">Utdata visualiseras automatiskt när du kör kod.</span><span class="sxs-lookup"><span data-stu-id="59358-208">The output is automatically visualized after you run the code.</span></span>

<span data-ttu-id="59358-209">Den här frågan hämtar resor av passagerare count.</span><span class="sxs-lookup"><span data-stu-id="59358-209">This query retrieves the trips by passenger count.</span></span> 

    # PLOT FREQUENCY OF PASSENGER COUNTS IN TAXI TRIPS

    # SQL QUERY
    %%sql -q -o sqlResults
    SELECT passenger_count, COUNT(*) as trip_counts FROM taxi_train WHERE passenger_count > 0 and passenger_count < 7 GROUP BY passenger_count


<span data-ttu-id="59358-210">Den här koden skapar en lokala data-ram för frågeresultatet och visar data.</span><span class="sxs-lookup"><span data-stu-id="59358-210">This code creates a local data-frame from the query output and plots the data.</span></span> <span data-ttu-id="59358-211">Den `%%local` magic skapar lokala data-ram, `sqlResults`, som kan användas för att rita upp med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="59358-211">The `%%local` magic creates a local data-frame, `sqlResults`, which can be used for plotting with matplotlib.</span></span> 

> [!NOTE]
> <span data-ttu-id="59358-212">Den här PySpark-magic används flera gånger i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="59358-212">This PySpark magic is used multiple times in this walkthrough.</span></span> <span data-ttu-id="59358-213">Om mängden data som är stor, bör du prov för att skapa en data-ram som ryms i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="59358-213">If the amount of data is large, you should sample to create a data-frame that can fit in local memory.</span></span>
> 
> 

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER
    %%local

    # USE THE JUPYTER AUTO-PLOTTING FEATURE TO CREATE INTERACTIVE FIGURES. 
    # CLICK ON THE TYPE OF PLOT TO BE GENERATED (E.G. LINE, AREA, BAR ETC.)
    sqlResults

<span data-ttu-id="59358-214">Här är koden för att rita resor med passagerare räknare</span><span class="sxs-lookup"><span data-stu-id="59358-214">Here is the code to plot the trips by passenger counts</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

<span data-ttu-id="59358-215">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-215">**OUTPUT**</span></span>

![Frekvensen för resor av passagerare antal](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/frequency-of-trips-by-passenger-count.png)

<span data-ttu-id="59358-217">Du kan välja bland flera olika typer av grafik (register, cirkeldiagram, rad, område eller fältet) med hjälp av den **typen** menyn knapparna i den bärbara datorn.</span><span class="sxs-lookup"><span data-stu-id="59358-217">You can select among several different types of visualizations (Table, Pie, Line, Area, or Bar) by using the **Type** menu buttons in the notebook.</span></span> <span data-ttu-id="59358-218">Fältet ritytans visas här.</span><span class="sxs-lookup"><span data-stu-id="59358-218">The Bar plot is shown here.</span></span>

### <a name="plot-a-histogram-of-tip-amounts-and-how-tip-amount-varies-by-passenger-count-and-fare-amounts"></a><span data-ttu-id="59358-219">Rita histogrammet tips belopp och hur mycket tips varierar efter passagerare antal och avgiften belopp.</span><span class="sxs-lookup"><span data-stu-id="59358-219">Plot a histogram of tip amounts and how tip amount varies by passenger count and fare amounts.</span></span>
<span data-ttu-id="59358-220">Använd en SQL-fråga till exempeldata...</span><span class="sxs-lookup"><span data-stu-id="59358-220">Use a SQL query to sample data..</span></span>

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


<span data-ttu-id="59358-221">Den här koden cellen använder SQL-frågan för att skapa tre områden data.</span><span class="sxs-lookup"><span data-stu-id="59358-221">This code cell uses the SQL query to create three plots the data.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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


<span data-ttu-id="59358-222">**UTDATA:**</span><span class="sxs-lookup"><span data-stu-id="59358-222">**OUTPUT:**</span></span> 

![Fördelning av tips](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-distribution.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-passenger-count.png)

![Tips belopp av avgiften belopp](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/tip-amount-by-fare-amount.png)

## <a name="feature-engineering-transformation-and-data-preparation-for-modeling"></a><span data-ttu-id="59358-226">Funktionen tekniker, omvandling och data förberedelse för modellering</span><span class="sxs-lookup"><span data-stu-id="59358-226">Feature engineering, transformation, and data preparation for modeling</span></span>
<span data-ttu-id="59358-227">Det här avsnittet beskriver och innehåller koden för procedurer som används för att förbereda data för användning i ML-modell.</span><span class="sxs-lookup"><span data-stu-id="59358-227">This section describes and provides the code for procedures used to prepare data for use in ML modeling.</span></span> <span data-ttu-id="59358-228">Den visar hur du gör följande:</span><span class="sxs-lookup"><span data-stu-id="59358-228">It shows how to do the following tasks:</span></span>

* <span data-ttu-id="59358-229">Skapa en ny funktion genom partitionering timmar till trafik tid lagerplatser</span><span class="sxs-lookup"><span data-stu-id="59358-229">Create a new feature by partitioning hours into traffic time bins</span></span>
* <span data-ttu-id="59358-230">Index och på hot koda kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="59358-230">Index and on-hot encode categorical features</span></span>
* <span data-ttu-id="59358-231">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="59358-231">Create labeled point objects for input into ML functions</span></span>
* <span data-ttu-id="59358-232">Skapa ett slumpmässigt underordnade urval av data och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="59358-232">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
* <span data-ttu-id="59358-233">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="59358-233">Feature scaling</span></span>
* <span data-ttu-id="59358-234">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="59358-234">Cache objects in memory</span></span>

### <a name="create-a-new-feature-by-partitioning-traffic-times-into-bins"></a><span data-ttu-id="59358-235">Skapa en ny funktion genom partitionering trafik gånger till lagerplatser</span><span class="sxs-lookup"><span data-stu-id="59358-235">Create a new feature by partitioning traffic times into bins</span></span>
<span data-ttu-id="59358-236">Den här koden visar hur du skapar en ny funktion med partitionering trafik gånger till lagerplatser och cachelagra dataramen resulterande i minnet.</span><span class="sxs-lookup"><span data-stu-id="59358-236">This code shows how to create a new feature by partitioning traffic times into bins and then how to cache the resulting data frame in memory.</span></span> <span data-ttu-id="59358-237">Cachelagring leder till förbättrad körningstid där flexibel distribuerade datauppsättningar (RDDs) och ramar data används flera gånger.</span><span class="sxs-lookup"><span data-stu-id="59358-237">Caching leads to improved execution time where Resilient Distributed Datasets (RDDs) and data-frames are used repeatedly.</span></span> <span data-ttu-id="59358-238">Så cachelagra vi RDDs och data ramar i flera steg i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="59358-238">So, we cache RDDs and data-frames at several stages in this walkthrough.</span></span>

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
    # THE .COUNT() GOES THROUGH THE ENTIRE DATA-FRAME,
    # MATERIALIZES IT IN MEMORY, AND GIVES THE COUNT OF ROWS.
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()

<span data-ttu-id="59358-239">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-239">**OUTPUT**</span></span>

<span data-ttu-id="59358-240">126050</span><span class="sxs-lookup"><span data-stu-id="59358-240">126050</span></span>

### <a name="index-and-one-hot-encode-categorical-features"></a><span data-ttu-id="59358-241">Index och en hot koda kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="59358-241">Index and one-hot encode categorical features</span></span>
<span data-ttu-id="59358-242">Det här avsnittet visar hur du index eller koda kategoriska funktioner för indata i modelleringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-242">This section shows how to index or encode categorical features for input into the modeling functions.</span></span> <span data-ttu-id="59358-243">Modellering och förutsäga funktioner för MLlib kräver att funktioner med kategoriska indata vara indexera eller kodade före användning.</span><span class="sxs-lookup"><span data-stu-id="59358-243">The modeling and predict functions of MLlib require that features with categorical input data be indexed or encoded prior to use.</span></span> 

<span data-ttu-id="59358-244">Beroende på modellen måste du indexera eller koda dem på olika sätt.</span><span class="sxs-lookup"><span data-stu-id="59358-244">Depending on the model, you need to index or encode them in different ways.</span></span> <span data-ttu-id="59358-245">Till exempel Logistic och linjär Regression modeller kräver en hot kodning, där, t.ex, en funktion med tre kategorier kan expanderas till tre funktionen kolumner med varje som innehåller 0 eller 1 beroende på kategorin för en observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="59358-245">For example, Logistic and Linear Regression models require one-hot encoding, where, for example, a feature with three categories can be expanded into three feature columns, with each containing 0 or 1 depending on the category of an observation.</span></span> <span data-ttu-id="59358-246">MLlib ger [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktion för att göra en hot kodning.</span><span class="sxs-lookup"><span data-stu-id="59358-246">MLlib provides [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function to do one-hot encoding.</span></span> <span data-ttu-id="59358-247">Den här kodaren mappar etikett index i en kolumn till en kolumn med binära angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="59358-247">This encoder maps a column of label indices to a column of binary vectors, with at most a single one-value.</span></span> <span data-ttu-id="59358-248">Den här kodningen kan algoritmer som räknar numeriska värden funktioner, till exempel logistic regression som ska tillämpas på kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-248">This encoding allows algorithms that expect numerical valued features, such as logistic regression, to be applied to categorical features.</span></span>

<span data-ttu-id="59358-249">Här är koden för att indexera och koda kategoriska funktioner:</span><span class="sxs-lookup"><span data-stu-id="59358-249">Here is the code to index and encode categorical features:</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.ml.feature import OneHotEncoder, StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer

    # INDEX AND ENCODE VENDOR_ID
    stringIndexer = StringIndexer(inputCol="vendor_id", outputCol="vendorIndex")
    model = stringIndexer.fit(taxi_df_train_with_newFeatures) # Input data-frame is the cleaned one from above
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-250">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-250">**OUTPUT**</span></span>

<span data-ttu-id="59358-251">Åtgången tid för att köra ovanför cellen: 3,14 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-251">Time taken to execute above cell: 3.14 seconds</span></span>

### <a name="create-labeled-point-objects-for-input-into-ml-functions"></a><span data-ttu-id="59358-252">Skapa märkt point-objekt för indata till ML-funktioner</span><span class="sxs-lookup"><span data-stu-id="59358-252">Create labeled point objects for input into ML functions</span></span>
<span data-ttu-id="59358-253">Det här avsnittet innehåller kod som visar hur du indexera kategoriska textdata med datatypen märkt punkt och koda den.</span><span class="sxs-lookup"><span data-stu-id="59358-253">This section contains code that shows how to index categorical text data as a labeled point data type and how to encode it.</span></span> <span data-ttu-id="59358-254">Detta förbereder den för att träna och testa MLlib logistic regression och andra klassificering modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-254">This prepares it to be used to train and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="59358-255">Märkt point-objekt är flexibel distribuerade datauppsättningar (RDD) formaterad på ett sätt som krävs av de flesta ML algoritmer i MLlib som indata.</span><span class="sxs-lookup"><span data-stu-id="59358-255">Labeled point objects are Resilient Distributed Datasets (RDD) formatted in a way that is needed as input data by most of ML algorithms in MLlib.</span></span> <span data-ttu-id="59358-256">En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.</span><span class="sxs-lookup"><span data-stu-id="59358-256">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="59358-257">Här är koden för att indexera och koda textfunktioner för binär klassificering.</span><span class="sxs-lookup"><span data-stu-id="59358-257">Here is the code to index and encode text features for binary classification.</span></span>

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


<span data-ttu-id="59358-258">Här är koden för att koda och index kategoriska textfunktioner för linjär regressionsanalys.</span><span class="sxs-lookup"><span data-stu-id="59358-258">Here is the code to encode and index categorical text features for linear regression analysis.</span></span>

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


### <a name="create-a-random-sub-sampling-of-the-data-and-split-it-into-training-and-testing-sets"></a><span data-ttu-id="59358-259">Skapa ett slumpmässigt underordnade urval av data och dela upp den i träning och testning uppsättningar</span><span class="sxs-lookup"><span data-stu-id="59358-259">Create a random sub-sampling of the data and split it into training and testing sets</span></span>
<span data-ttu-id="59358-260">Den här koden skapar en slumpmässig provtagning data (25% används här).</span><span class="sxs-lookup"><span data-stu-id="59358-260">This code creates a random sampling of the data (25% is used here).</span></span> <span data-ttu-id="59358-261">Även om det inte krävs för det här exemplet på grund av storleken på dataset, visar vi hur du kan samla in data här.</span><span class="sxs-lookup"><span data-stu-id="59358-261">Although it is not required for this example due to the size of the dataset, we demonstrate how you can sample the data here.</span></span> <span data-ttu-id="59358-262">Vet hur du använder för din egen problem om det behövs.</span><span class="sxs-lookup"><span data-stu-id="59358-262">Then you know how to use it for your own problem if needed.</span></span> <span data-ttu-id="59358-263">När prover är stort, kan detta spara mycket tid när utbildning modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-263">When samples are large, this can save significant time while training models.</span></span> <span data-ttu-id="59358-264">Nästa dela vi exemplet i en utbildning (här 75%) och en tester del (här 25%) ska användas i klassificering och regressionen modeling.</span><span class="sxs-lookup"><span data-stu-id="59358-264">Next we split the sample into a training part (75% here) and a testing part (25% here) to use in classification and regression modeling.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-265">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-265">**OUTPUT**</span></span>

<span data-ttu-id="59358-266">Åtgången tid för att köra ovanför cellen: 0.31 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-266">Time taken to execute above cell: 0.31 seconds</span></span>

### <a name="feature-scaling"></a><span data-ttu-id="59358-267">Funktionen skalning</span><span class="sxs-lookup"><span data-stu-id="59358-267">Feature scaling</span></span>
<span data-ttu-id="59358-268">Funktionen skalning, även kallat databasnormalisering garanterar att funktioner med brett erläggas värden är inte den angivna överdriven väg i mål-funktionen.</span><span class="sxs-lookup"><span data-stu-id="59358-268">Feature scaling, also known as data normalization, insures that features with widely disbursed values are not given excessive weigh in the objective function.</span></span> <span data-ttu-id="59358-269">Koden för funktionen skalning använder den [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) att skala funktioner enhet variansen.</span><span class="sxs-lookup"><span data-stu-id="59358-269">The code for feature scaling uses the [StandardScaler](https://spark.apache.org/docs/latest/api/python/pyspark.mllib.html#pyspark.mllib.feature.StandardScaler) to scale the features to unit variance.</span></span> <span data-ttu-id="59358-270">Den tillhandahålls av MLlib för användning i linjär regression med Stokastisk toning härstammar (SGD).</span><span class="sxs-lookup"><span data-stu-id="59358-270">It is provided by MLlib for use in linear regression with Stochastic Gradient Descent (SGD).</span></span> <span data-ttu-id="59358-271">SGD är en populär algoritm för att träna en mängd andra maskininlärning modeller som reglerats regressioner eller support vector datorer (SVM).</span><span class="sxs-lookup"><span data-stu-id="59358-271">SGD is a popular algorithm for training a wide range of other machine learning models such as regularized regressions or support vector machines (SVM).</span></span>   

> [!TIP]
> <span data-ttu-id="59358-272">Vi har hittat algoritmen LinearRegressionWithSGD vara känsliga funktion skalning.</span><span class="sxs-lookup"><span data-stu-id="59358-272">We have found the LinearRegressionWithSGD algorithm to be sensitive to feature scaling.</span></span>   
> 
> 

<span data-ttu-id="59358-273">Här är koden för att skala variabler för användning med algoritmen regularized SGD linjär.</span><span class="sxs-lookup"><span data-stu-id="59358-273">Here is the code to scale variables for use with the regularized linear SGD algorithm.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-274">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-274">**OUTPUT**</span></span>

<span data-ttu-id="59358-275">Åtgången tid för att köra ovanför cellen: 11.67 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-275">Time taken to execute above cell: 11.67 seconds</span></span>

### <a name="cache-objects-in-memory"></a><span data-ttu-id="59358-276">Cacheobjekt i minnet</span><span class="sxs-lookup"><span data-stu-id="59358-276">Cache objects in memory</span></span>
<span data-ttu-id="59358-277">Den tid det tar för träning och testning av ML algoritmer kan minskas genom cachelagring av indata-ram objekt används för klassificering, regression och skalas funktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-277">The time taken for training and testing of ML algorithms can be reduced by caching the input data frame objects used for classification, regression and, scaled features.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-278">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-278">**OUTPUT**</span></span> 

<span data-ttu-id="59358-279">Åtgången tid för att köra ovanför cellen: 0,13 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-279">Time taken to execute above cell: 0.13 seconds</span></span>

## <a name="predict-whether-or-not-a-tip-is-paid-with-binary-classification-models"></a><span data-ttu-id="59358-280">Förutsäga huruvida ett tips är betald med binär klassificering modeller</span><span class="sxs-lookup"><span data-stu-id="59358-280">Predict whether or not a tip is paid with binary classification models</span></span>
<span data-ttu-id="59358-281">Det här avsnittet visas hur användning tre modeller för binär klassificering uppgiften att förutsäga ett tips betalat för en taxi resa eller inte.</span><span class="sxs-lookup"><span data-stu-id="59358-281">This section shows how use three models for the binary classification task of predicting whether or not a tip is paid for a taxi trip.</span></span> <span data-ttu-id="59358-282">Modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="59358-282">The models presented are:</span></span>

* <span data-ttu-id="59358-283">Logistic regression</span><span class="sxs-lookup"><span data-stu-id="59358-283">Logistic regression</span></span> 
* <span data-ttu-id="59358-284">Slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="59358-284">Random forest</span></span>
* <span data-ttu-id="59358-285">Toning träd</span><span class="sxs-lookup"><span data-stu-id="59358-285">Gradient Boosting Trees</span></span>

<span data-ttu-id="59358-286">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="59358-286">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="59358-287">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="59358-287">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="59358-288">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="59358-288">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="59358-289">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="59358-289">**Saving model** in blob for future consumption</span></span>

<span data-ttu-id="59358-290">Visar vi hur du gör korsvalidering (KA) med parametern omfattande på två sätt:</span><span class="sxs-lookup"><span data-stu-id="59358-290">We show how to do cross-validation (CV) with parameter sweeping in two ways:</span></span>

1. <span data-ttu-id="59358-291">Med hjälp av **allmänna** anpassad kod som kan användas till alla algoritmer i MLlib och valfri parameter som anger i en algoritm.</span><span class="sxs-lookup"><span data-stu-id="59358-291">Using **generic** custom code which can be applied to any algorithm in MLlib and to any parameter sets in an algorithm.</span></span> 
2. <span data-ttu-id="59358-292">Med hjälp av den **pySpark CrossValidator pipeline funktionen**.</span><span class="sxs-lookup"><span data-stu-id="59358-292">Using the **pySpark CrossValidator pipeline function**.</span></span> <span data-ttu-id="59358-293">Observera att CrossValidator inte har några begränsningar för Spark 1.5.0:</span><span class="sxs-lookup"><span data-stu-id="59358-293">Note that CrossValidator has a few limitations for Spark 1.5.0:</span></span> 
   
   * <span data-ttu-id="59358-294">Pipeline-modeller får inte vara sparade/beständiga för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="59358-294">Pipeline models cannot be saved/persisted for future consumption.</span></span>
   * <span data-ttu-id="59358-295">Kan inte användas för varje parameter i en modell.</span><span class="sxs-lookup"><span data-stu-id="59358-295">Cannot be used for every parameter in a model.</span></span>
   * <span data-ttu-id="59358-296">Kan inte användas för varje MLlib-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="59358-296">Cannot be used for every MLlib algorithm.</span></span>

### <a name="generic-cross-validation-and-hyperparameter-sweeping-used-with-the-logistic-regression-algorithm-for-binary-classification"></a><span data-ttu-id="59358-297">Generisk mellan validering och hyperparameter omfattande används med algoritmen logistic regression för binär klassificering</span><span class="sxs-lookup"><span data-stu-id="59358-297">Generic cross validation and hyperparameter sweeping used with the logistic regression algorithm for binary classification</span></span>
<span data-ttu-id="59358-298">Koden i det här avsnittet visar hur du träna, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="59358-298">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="59358-299">Modellen har tränats med mellan validering (KA) och hyperparameter omfattande har implementerats med anpassad kod som kan tillämpas på någon av algoritmer för maskininlärning i MLlib.</span><span class="sxs-lookup"><span data-stu-id="59358-299">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with custom code that can be applied to any of the learning algorithms in MLlib.</span></span>   

> [!NOTE]
> <span data-ttu-id="59358-300">Körningen av den här anpassade KA koden kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="59358-300">The execution of this custom CV code can take several minutes.</span></span>
> 
> 

<span data-ttu-id="59358-301">**Träna regressionsmodellen logistic med KA och hyperparameter omfattande**</span><span class="sxs-lookup"><span data-stu-id="59358-301">**Train the logistic regression model using CV and hyperparameter sweeping**</span></span>

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

    # SET NUM FOLDS AND NUM PARAMETER SETS TO SWEEP ON
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


    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
    print("Coefficients: " + str(logitBest.weights))
    print("Intercept: " + str(logitBest.intercept))

    # PRINT ELAPSED TIME    
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-302">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-302">**OUTPUT**</span></span>

<span data-ttu-id="59358-303">Koefficienter: [0.0082065285375,-0.0223675576104,-0.0183812028036, -3.48124578069e-05,-0.00247646947233,-0.00165897881503, 0.0675394837328,-0.111823113101,-0.324609912762,-0.204549780032,-1.36499216354, 0.591088507921,-0.664263411392,-1.00439726852, 3.46567827545,-3.51025855172,-0.0471341112232,-0.043521833294, 0.000243375810385, 0.054518719222]</span><span class="sxs-lookup"><span data-stu-id="59358-303">Coefficients: [0.0082065285375, -0.0223675576104, -0.0183812028036, -3.48124578069e-05, -0.00247646947233, -0.00165897881503, 0.0675394837328, -0.111823113101, -0.324609912762, -0.204549780032, -1.36499216354, 0.591088507921, -0.664263411392, -1.00439726852, 3.46567827545, -3.51025855172, -0.0471341112232, -0.043521833294, 0.000243375810385, 0.054518719222]</span></span>

<span data-ttu-id="59358-304">Skärningspunkt:-0.0111216486893</span><span class="sxs-lookup"><span data-stu-id="59358-304">Intercept: -0.0111216486893</span></span>

<span data-ttu-id="59358-305">Åtgången tid för att köra ovanför cellen: 14.43 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-305">Time taken to execute above cell: 14.43 seconds</span></span>

<span data-ttu-id="59358-306">**Utvärdera modellen binär klassificering med standard mått**</span><span class="sxs-lookup"><span data-stu-id="59358-306">**Evaluate the binary classification model with standard metrics**</span></span>

<span data-ttu-id="59358-307">Koden i det här avsnittet visar hur du utvärdera en logistic regressionsmodell mot en test-datamängd, inklusive ritning av ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="59358-307">The code in this section shows how to evaluate a logistic regression model against a test data-set, including a plot of the ROC curve.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-308">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-308">**OUTPUT**</span></span>

<span data-ttu-id="59358-309">Området under PR = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="59358-309">Area under PR = 0.985336538462</span></span>

<span data-ttu-id="59358-310">Området under ROC = 0.983383274312</span><span class="sxs-lookup"><span data-stu-id="59358-310">Area under ROC = 0.983383274312</span></span>

<span data-ttu-id="59358-311">Sammanfattande statistik</span><span class="sxs-lookup"><span data-stu-id="59358-311">Summary Stats</span></span>

<span data-ttu-id="59358-312">Precision = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="59358-312">Precision = 0.984174341679</span></span>

<span data-ttu-id="59358-313">Återkalla = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="59358-313">Recall = 0.984174341679</span></span>

<span data-ttu-id="59358-314">F1 Poängsätta = 0.984174341679</span><span class="sxs-lookup"><span data-stu-id="59358-314">F1 Score = 0.984174341679</span></span>

<span data-ttu-id="59358-315">Åtgången tid för att köra ovanför cellen: 2.67 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-315">Time taken to execute above cell: 2.67 seconds</span></span>

<span data-ttu-id="59358-316">**Rita ROC-kurvan.**</span><span class="sxs-lookup"><span data-stu-id="59358-316">**Plot the ROC curve.**</span></span>

<span data-ttu-id="59358-317">Den *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i föregående cell.</span><span class="sxs-lookup"><span data-stu-id="59358-317">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="59358-318">*tmp_results* kan användas för att göra frågor och resultatet i sqlResults data-ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="59358-318">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="59358-319">Här är koden.</span><span class="sxs-lookup"><span data-stu-id="59358-319">Here is the code.</span></span>

    # QUERY RESULTS                              
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="59358-320">Här är koden för att göra förutsägelser och rita ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="59358-320">Here is the code to make predictions and plot the ROC-curve.</span></span>

    # MAKE PREDICTIONS AND PLOT ROC-CURVE

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES                              
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


<span data-ttu-id="59358-321">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-321">**OUTPUT**</span></span>

![Logistic regression ROC-kurvan generisk metod](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/logistic-regression-roc-curve.png)

<span data-ttu-id="59358-323">**Spara modellen i en blob för framtida användning**</span><span class="sxs-lookup"><span data-stu-id="59358-323">**Persist model in a blob for future consumption**</span></span>

<span data-ttu-id="59358-324">Koden i det här avsnittet visar hur du sparar logistic regressionsmodell för användning.</span><span class="sxs-lookup"><span data-stu-id="59358-324">The code in this section shows how to save the logistic regression model for consumption.</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";


<span data-ttu-id="59358-325">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-325">**OUTPUT**</span></span>

<span data-ttu-id="59358-326">Åtgången tid för att köra ovanför cellen: 34.57 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-326">Time taken to execute above cell: 34.57 seconds</span></span>

### <a name="use-mllibs-crossvalidator-pipeline-function-with-logistic-regression-elastic-regression-model"></a><span data-ttu-id="59358-327">Använda Mllib's CrossValidator pipeline-funktionen med logistic regressionsmodell (elastisk regression)</span><span class="sxs-lookup"><span data-stu-id="59358-327">Use MLlib's CrossValidator pipeline function with logistic regression (Elastic regression) model</span></span>
<span data-ttu-id="59358-328">Koden i det här avsnittet visar hur du träna, utvärdera och spara en logistic regressionsmodell med [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) som beräknar huruvida ett tips är betalat för en resa i NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="59358-328">The code in this section shows how to train, evaluate, and save a logistic regression model with [LBFGS](https://en.wikipedia.org/wiki/Broyden%E2%80%93Fletcher%E2%80%93Goldfarb%E2%80%93Shanno_algorithm) that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span> <span data-ttu-id="59358-329">Modellen har tränats med mellan validering (KA) och hyperparameter omfattande implementerats med funktionen MLlib CrossValidator pipeline för KA med parametern Svep.</span><span class="sxs-lookup"><span data-stu-id="59358-329">The model is trained using cross validation (CV) and hyperparameter sweeping implemented with the MLlib CrossValidator pipeline function for CV with parameter sweep.</span></span>   

> [!NOTE]
> <span data-ttu-id="59358-330">Körningen av den här koden MLlib KA kan ta flera minuter.</span><span class="sxs-lookup"><span data-stu-id="59358-330">The execution of this MLlib CV code can take several minutes.</span></span>
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

    # CONVERT TO DATA-FRAME: THIS DOES NOT RUN ON RDDs
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds";

<span data-ttu-id="59358-331">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-331">**OUTPUT**</span></span>

<span data-ttu-id="59358-332">Åtgången tid för att köra ovanför cellen: 107.98 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-332">Time taken to execute above cell: 107.98 seconds</span></span>

<span data-ttu-id="59358-333">**Rita ROC-kurvan.**</span><span class="sxs-lookup"><span data-stu-id="59358-333">**Plot the ROC curve.**</span></span>

<span data-ttu-id="59358-334">Den *predictionAndLabelsDF* registreras som en tabell, *tmp_results*, i föregående cell.</span><span class="sxs-lookup"><span data-stu-id="59358-334">The *predictionAndLabelsDF* is registered as a table, *tmp_results*, in the previous cell.</span></span> <span data-ttu-id="59358-335">*tmp_results* kan användas för att göra frågor och resultatet i sqlResults data-ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="59358-335">*tmp_results* can be used to do queries and output results into the sqlResults data-frame for plotting.</span></span> <span data-ttu-id="59358-336">Här är koden.</span><span class="sxs-lookup"><span data-stu-id="59358-336">Here is the code.</span></span>

    # QUERY RESULTS
    %%sql -q -o sqlResults
    SELECT label, prediction, probability from tmp_results

<span data-ttu-id="59358-337">Här är koden för att rita ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="59358-337">Here is the code to plot the ROC curve.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES 
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


<span data-ttu-id="59358-338">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-338">**OUTPUT**</span></span>

![Logistic regression ROC-kurvan med Mllib's CrossValidator](./media/machine-learning-data-science-spark-advanced-data-exploration-modeling/mllib-crossvalidator-roc-curve.png)

### <a name="random-forest-classification"></a><span data-ttu-id="59358-340">Slumpmässiga skog klassificering</span><span class="sxs-lookup"><span data-stu-id="59358-340">Random forest classification</span></span>
<span data-ttu-id="59358-341">Koden i det här avsnittet visar hur du träna, utvärdera och spara en slumpmässig skog regression som beräknar huruvida ett tips är betalat för en resa i NYC taxi resa och avgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="59358-341">The code in this section shows how to train, evaluate, and save a random forest regression that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

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
    ## UN-COMMENT IF YOU WANT TO PRING TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-342">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-342">**OUTPUT**</span></span>

<span data-ttu-id="59358-343">Området under ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="59358-343">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="59358-344">Åtgången tid för att köra ovanför cellen: 26.72 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-344">Time taken to execute above cell: 26.72 seconds</span></span>

### <a name="gradient-boosting-trees-classification"></a><span data-ttu-id="59358-345">Toning den träd klassificering</span><span class="sxs-lookup"><span data-stu-id="59358-345">Gradient boosting trees classification</span></span>
<span data-ttu-id="59358-346">Koden i det här avsnittet visar hur du träna, utvärdera, och spara en toning den träd modell som beräknar huruvida ett tips är betalat för en resa i NYC taxi resan och färdavgiften dataset.</span><span class="sxs-lookup"><span data-stu-id="59358-346">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts whether or not a tip is paid for a trip in the NYC taxi trip and fare dataset.</span></span>

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD PYSPARK LIBRARIES
    from pyspark.mllib.tree import GradientBoostedTrees, GradientBoostedTreesModel

    # SPECIFY NUMBER OF CATEGORIES FOR CATEGORICAL FEATURES. FEATURE #0 HAS 2 CATEGORIES, FEATURE #2 HAS 2 CATEGORIES, AND SO ON
    categoricalFeaturesInfo={0:2, 1:2, 2:6, 3:4}

    gbtModel = GradientBoostedTrees.trainClassifier(indexedTRAINbinary, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                    numIterations=10)
    ## UNCOMMENT IF YOU WANT TO PRINT TREE DETAILS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-347">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-347">**OUTPUT**</span></span>

<span data-ttu-id="59358-348">Området under ROC = 0.985336538462</span><span class="sxs-lookup"><span data-stu-id="59358-348">Area under ROC = 0.985336538462</span></span>

<span data-ttu-id="59358-349">Åtgången tid för att köra ovanför cellen: 28.13 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-349">Time taken to execute above cell: 28.13 seconds</span></span>

## <a name="predict-tip-amount-with-regression-models-not-using-cv"></a><span data-ttu-id="59358-350">Förutsäga tips belopp med regression modeller (inte med KA)</span><span class="sxs-lookup"><span data-stu-id="59358-350">Predict tip amount with regression models (not using CV)</span></span>
<span data-ttu-id="59358-351">Det här avsnittet visas hur användning tre modeller för aktiviteten regression: förutsäga tips summa för taxi resa baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="59358-351">This section shows how use three models for the regression task: predict the tip amount paid for a taxi trip based on other tip features.</span></span> <span data-ttu-id="59358-352">Modeller som visas är:</span><span class="sxs-lookup"><span data-stu-id="59358-352">The models presented are:</span></span>

* <span data-ttu-id="59358-353">Reglerats linjär regression</span><span class="sxs-lookup"><span data-stu-id="59358-353">Regularized linear regression</span></span>
* <span data-ttu-id="59358-354">Slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="59358-354">Random forest</span></span>
* <span data-ttu-id="59358-355">Toning träd</span><span class="sxs-lookup"><span data-stu-id="59358-355">Gradient Boosting Trees</span></span>

<span data-ttu-id="59358-356">Dessa modeller beskrivs i inledningen.</span><span class="sxs-lookup"><span data-stu-id="59358-356">These models were described in the introduction.</span></span> <span data-ttu-id="59358-357">Varje modell skapa avsnitt med exempelkod delas upp i steg:</span><span class="sxs-lookup"><span data-stu-id="59358-357">Each model building code section is split into steps:</span></span> 

1. <span data-ttu-id="59358-358">**Modellen utbildning** data med en enda parameter</span><span class="sxs-lookup"><span data-stu-id="59358-358">**Model training** data with one parameter set</span></span>
2. <span data-ttu-id="59358-359">**Modellen utvärdering** på en datauppsättning med test</span><span class="sxs-lookup"><span data-stu-id="59358-359">**Model evaluation** on a test data set with metrics</span></span>
3. <span data-ttu-id="59358-360">**Spara modellen** i blob för framtida användning</span><span class="sxs-lookup"><span data-stu-id="59358-360">**Saving model** in blob for future consumption</span></span>   

> <span data-ttu-id="59358-361">AZURE Obs: Korsvalidering används inte med tre regression modeller i det här avsnittet, eftersom det som visas i detalj för logistic regression modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-361">AZURE NOTE: Cross-validation is not used with the three regression models in this section, since this was shown in detail for the logistic regression models.</span></span> <span data-ttu-id="59358-362">Ett exempel som visar hur du använder KA med elastisk Net för linjär regression tillhandahålls i tillägget i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="59358-362">An example showing how to use CV with Elastic Net for linear regression is provided in the Appendix of this topic.</span></span>
> 
> <span data-ttu-id="59358-363">AZURE Obs: I vår erfarenhet det kan finnas problem med konvergens LinearRegressionWithSGD modeller och parametrar måste vara ändras/optimerad noggrant för att erhålla en giltig modell.</span><span class="sxs-lookup"><span data-stu-id="59358-363">AZURE NOTE: In our experience, there can be issues with convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="59358-364">Skalning av variabler avsevärt hjälper med konvergens.</span><span class="sxs-lookup"><span data-stu-id="59358-364">Scaling of variables significantly helps with convergence.</span></span> <span data-ttu-id="59358-365">Elastisk net regression som visas i tillägget till den här artikeln kan också användas i stället för LinearRegressionWithSGD.</span><span class="sxs-lookup"><span data-stu-id="59358-365">Elastic net regression, shown in the Appendix to this topic, can also be used instead of LinearRegressionWithSGD.</span></span>
> 
> 

### <a name="linear-regression-with-sgd"></a><span data-ttu-id="59358-366">Linjär regression med SGD</span><span class="sxs-lookup"><span data-stu-id="59358-366">Linear regression with SGD</span></span>
<span data-ttu-id="59358-367">Koden i det här avsnittet visar hur du använder skalade funktioner för att träna en linjär regression som använder stokastisk toning härstammar (SGD) för optimering, och hur du poängsätta, utvärdera och spara modellen i Azure Blob Storage (WASB).</span><span class="sxs-lookup"><span data-stu-id="59358-367">The code in this section shows how to use scaled features to train a linear regression that uses stochastic gradient descent (SGD) for optimization, and how to score, evaluate, and save the model in Azure Blob Storage (WASB).</span></span>

> [!TIP]
> <span data-ttu-id="59358-368">Det kan finnas problem med att Konvergera LinearRegressionWithSGD modeller i vår erfarenhet och parametrar måste vara ändras/optimerad noggrant för att erhålla en giltig modell.</span><span class="sxs-lookup"><span data-stu-id="59358-368">In our experience, there can be issues with the convergence of LinearRegressionWithSGD models, and parameters need to be changed/optimized carefully for obtaining a valid model.</span></span> <span data-ttu-id="59358-369">Skalning av variabler avsevärt hjälper med konvergens.</span><span class="sxs-lookup"><span data-stu-id="59358-369">Scaling of variables significantly helps with convergence.</span></span>
> 
> 

    # LINEAR REGRESSION WITH SGD 

    # RECORD START TIME
    timestart = datetime.datetime.now()

    # LOAD LIBRARIES
    from pyspark.mllib.regression import LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel
    from pyspark.mllib.evaluation import RegressionMetrics
    from scipy import stats

    # USE SCALED FEATURES TO TRAIN MODEL
    linearModel = LinearRegressionWithSGD.train(oneHotTRAINregScaled, iterations=100, step = 0.1, regType='l2', regParam=0.1, intercept = True)

    # PRINT COEFFICIENTS AND INTERCEPT OF THE MODEL
    # NOTE: There are 20 coefficient terms for the 10 features, 
    #       and the different categories for features: vendorVec (2), rateVec, paymentVec (6), TrafficTimeBinsVec (4)
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-370">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-370">**OUTPUT**</span></span>

<span data-ttu-id="59358-371">Koefficienter: [0.0141707753435,-0.0252930927087,-0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092,-0.00456498588241,-0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632,-0.00289545676449,- 0.00791124681938, 0.54396316518,-0.536293513569, 0.0119076553369,-0.0173039244582, 0.0119632796147, 0.00146764882502]</span><span class="sxs-lookup"><span data-stu-id="59358-371">Coefficients: [0.0141707753435, -0.0252930927087, -0.0231442517137, 0.247070902996, 0.312544147152, 0.360296120645, 0.0122079566092, -0.00456498588241, -0.0898228505177, 0.0714046248793, 0.102171263868, 0.100022455632, -0.00289545676449, -0.00791124681938, 0.54396316518, -0.536293513569, 0.0119076553369, -0.0173039244582, 0.0119632796147, 0.00146764882502]</span></span>

<span data-ttu-id="59358-372">Fånga: 0.854507624459</span><span class="sxs-lookup"><span data-stu-id="59358-372">Intercept: 0.854507624459</span></span>

<span data-ttu-id="59358-373">RMSE = 1.23485131376</span><span class="sxs-lookup"><span data-stu-id="59358-373">RMSE = 1.23485131376</span></span>

<span data-ttu-id="59358-374">R-sqr = 0.597963951127</span><span class="sxs-lookup"><span data-stu-id="59358-374">R-sqr = 0.597963951127</span></span>

<span data-ttu-id="59358-375">Åtgången tid för att köra ovanför cellen: 38.62 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-375">Time taken to execute above cell: 38.62 seconds</span></span>

### <a name="random-forest-regression"></a><span data-ttu-id="59358-376">Slumpmässiga skog regression</span><span class="sxs-lookup"><span data-stu-id="59358-376">Random Forest regression</span></span>
<span data-ttu-id="59358-377">Koden i det här avsnittet visar hur du träna, utvärdera och spara en slumpmässig Skogsmodell som beräknar tips belopp för NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="59358-377">The code in this section shows how to train, evaluate, and save a random forest model that predicts tip amount for the NYC taxi trip data.</span></span>   

> [!NOTE]
> <span data-ttu-id="59358-378">Korsvalidering med parametern omfattande med anpassad kod har angetts i tillägget.</span><span class="sxs-lookup"><span data-stu-id="59358-378">Cross-validation with parameter sweeping using custom code is provided in the appendix.</span></span>
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
    # UN-COMMENT IF YOU WANT TO PRING TREES
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 

<span data-ttu-id="59358-379">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-379">**OUTPUT**</span></span>

<span data-ttu-id="59358-380">RMSE = 0.931981967875</span><span class="sxs-lookup"><span data-stu-id="59358-380">RMSE = 0.931981967875</span></span>

<span data-ttu-id="59358-381">R-sqr = 0.733445485802</span><span class="sxs-lookup"><span data-stu-id="59358-381">R-sqr = 0.733445485802</span></span>

<span data-ttu-id="59358-382">Åtgången tid för att köra ovanför cellen: 25.98 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-382">Time taken to execute above cell: 25.98 seconds</span></span>

### <a name="gradient-boosting-trees-regression"></a><span data-ttu-id="59358-383">Toning den träd regression</span><span class="sxs-lookup"><span data-stu-id="59358-383">Gradient boosting trees regression</span></span>
<span data-ttu-id="59358-384">Koden i det här avsnittet visar hur du träna, utvärdera och spara en toning den träd modell som beräknar tips belopp för NYC taxi resa data.</span><span class="sxs-lookup"><span data-stu-id="59358-384">The code in this section shows how to train, evaluate, and save a gradient boosting trees model that predicts tip amount for the NYC taxi trip data.</span></span>

<span data-ttu-id="59358-385">** Träna och utvärdera **</span><span class="sxs-lookup"><span data-stu-id="59358-385">**Train and evaluate **</span></span>

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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-386">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-386">**OUTPUT**</span></span>

<span data-ttu-id="59358-387">RMSE = 0.928172197114</span><span class="sxs-lookup"><span data-stu-id="59358-387">RMSE = 0.928172197114</span></span>

<span data-ttu-id="59358-388">R-sqr = 0.732680354389</span><span class="sxs-lookup"><span data-stu-id="59358-388">R-sqr = 0.732680354389</span></span>

<span data-ttu-id="59358-389">Åtgången tid för att köra ovanför cellen: 20,9 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-389">Time taken to execute above cell: 20.9 seconds</span></span>

<span data-ttu-id="59358-390">**Rita**</span><span class="sxs-lookup"><span data-stu-id="59358-390">**Plot**</span></span>

<span data-ttu-id="59358-391">*tmp_results* registreras som en Hive-tabell i föregående cell.</span><span class="sxs-lookup"><span data-stu-id="59358-391">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="59358-392">Resultat från tabellen utdata till den *sqlResults* data ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="59358-392">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="59358-393">Här är koden</span><span class="sxs-lookup"><span data-stu-id="59358-393">Here is the code</span></span>

    # PLOT SCATTER-PLOT BETWEEN ACTUAL AND PREDICTED TIP VALUES

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT * from tmp_results


<span data-ttu-id="59358-394">Här är koden för att rita data med Jupyter-server.</span><span class="sxs-lookup"><span data-stu-id="59358-394">Here is the code to plot the data using the Jupyter server.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
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

## <a name="appendix-additional-regression-tasks-using-cross-validation-with-parameter-sweeps"></a><span data-ttu-id="59358-396">Bilaga: Ytterligare regression uppgifter med parametern Svep mellan verifiering</span><span class="sxs-lookup"><span data-stu-id="59358-396">Appendix: Additional regression tasks using cross validation with parameter sweeps</span></span>
<span data-ttu-id="59358-397">Den här bilagan innehåller kod som visar hur du gör KA med elastisk net för linjär regression och hur du gör KA med parametern Svep med anpassad kod för slumpmässiga skog regression.</span><span class="sxs-lookup"><span data-stu-id="59358-397">This appendix contains code showing how to do CV using Elastic net for linear regression and how to do CV with parameter sweep using custom code for random forest regression.</span></span>

### <a name="cross-validation-using-elastic-net-for-linear-regression"></a><span data-ttu-id="59358-398">Mellan med hjälp av net elastisk för linjär regression</span><span class="sxs-lookup"><span data-stu-id="59358-398">Cross validation using Elastic net for linear regression</span></span>
<span data-ttu-id="59358-399">Koden i det här avsnittet visar hur du mellan med hjälp av elastisk net för linjär regression och utvärdera modell mot testdata.</span><span class="sxs-lookup"><span data-stu-id="59358-399">The code in this section shows how to do cross validation using Elastic net for linear regression and how to evaluate the model against test data.</span></span>

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
    # SIMPLY THE MODEL HERE, WITHOUT TRANSFORMATIONS
    pipeline = Pipeline(stages=[lr])

    # DEFINE CV WITH PARAMETER SWEEP
    cv = CrossValidator(estimator= lr,
                        estimatorParamMaps=paramGrid,
                        evaluator=RegressionEvaluator(),
                        numFolds=3)

    # CONVERT TO DATA FRAME, AS CROSSVALIDATOR WON'T RUN ON RDDS
    trainDataFrame = sqlContext.createDataFrame(oneHotTRAINreg, ["features", "label"])

    # TRAIN WITH CROSS-VALIDATION
    cv_model = cv.fit(trainDataFrame)


    # EVALUATE MODEL ON TEST SET
    testDataFrame = sqlContext.createDataFrame(oneHotTESTreg, ["features", "label"])

    # MAKE PREDICTIONS ON TEST DOCUMENTS
    # cvModel uses the best model found (lrModel).
    predictionAndLabels = cv_model.transform(testDataFrame)

    # CONVERT TO DF AND SAVE REGISER DF AS TABLE
    predictionAndLabels.registerTempTable("tmp_results");

    # PRINT ELAPSED TIME
    timeend = datetime.datetime.now()
    timedelta = round((timeend-timestart).total_seconds(), 2) 
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-400">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-400">**OUTPUT**</span></span>

<span data-ttu-id="59358-401">Åtgången tid för att köra ovanför cellen: 161.21 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-401">Time taken to execute above cell: 161.21  seconds</span></span>

<span data-ttu-id="59358-402">**Utvärdera med R SQR mått**</span><span class="sxs-lookup"><span data-stu-id="59358-402">**Evaluate with R-SQR metric**</span></span>

<span data-ttu-id="59358-403">*tmp_results* registreras som en Hive-tabell i föregående cell.</span><span class="sxs-lookup"><span data-stu-id="59358-403">*tmp_results* is registered as a Hive table in the previous cell.</span></span> <span data-ttu-id="59358-404">Resultat från tabellen utdata till den *sqlResults* data ram för att rita upp.</span><span class="sxs-lookup"><span data-stu-id="59358-404">Results from the table are output into the *sqlResults* data-frame for plotting.</span></span> <span data-ttu-id="59358-405">Här är koden</span><span class="sxs-lookup"><span data-stu-id="59358-405">Here is the code</span></span>

    # SELECT RESULTS
    %%sql -q -o sqlResults
    SELECT label,prediction from tmp_results


<span data-ttu-id="59358-406">Här är koden för att beräkna R sqr.</span><span class="sxs-lookup"><span data-stu-id="59358-406">Here is the code to calculate R-sqr.</span></span>

    # RUN THE CODE LOCALLY ON THE JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    from scipy import stats

    #R-SQR TEST METRIC
    corstats = stats.linregress(sqlResults['label'],sqlResults['prediction'])
    r2 = (corstats[2]*corstats[2])
    print("R-sqr = %s" % r2)


<span data-ttu-id="59358-407">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-407">**OUTPUT**</span></span>

<span data-ttu-id="59358-408">R-sqr = 0.619184907088</span><span class="sxs-lookup"><span data-stu-id="59358-408">R-sqr = 0.619184907088</span></span>

### <a name="cross-validation-with-parameter-sweep-using-custom-code-for-random-forest-regression"></a><span data-ttu-id="59358-409">Mellan verifiering med parametern Svep med anpassad kod för slumpmässiga skog regression</span><span class="sxs-lookup"><span data-stu-id="59358-409">Cross validation with parameter sweep using custom code for random forest regression</span></span>
<span data-ttu-id="59358-410">Koden i det här avsnittet visar hur du mellan verifiering med parametern Svep med anpassad kod för slumpmässiga skog regression och utvärdera modell mot testdata.</span><span class="sxs-lookup"><span data-stu-id="59358-410">The code in this section shows how to do cross validation with parameter sweep using custom code for random forest regression and how to evaluate the model against test data.</span></span>

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

    # SPECIFY NUMFOLDS AND ARRAY TO HOLD METRICS
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
    print "Time taken to execute above cell: " + str(timedelta) + " seconds"; 


<span data-ttu-id="59358-411">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-411">**OUTPUT**</span></span>

<span data-ttu-id="59358-412">RMSE = 0.906972198262</span><span class="sxs-lookup"><span data-stu-id="59358-412">RMSE = 0.906972198262</span></span>

<span data-ttu-id="59358-413">R-sqr = 0.740751197012</span><span class="sxs-lookup"><span data-stu-id="59358-413">R-sqr = 0.740751197012</span></span>

<span data-ttu-id="59358-414">Åtgången tid för att köra ovanför cellen: 69.17 sekunder</span><span class="sxs-lookup"><span data-stu-id="59358-414">Time taken to execute above cell: 69.17 seconds</span></span>

### <a name="clean-up-objects-from-memory-and-print-model-locations"></a><span data-ttu-id="59358-415">Rensa objekt från minne och skriva ut modellen platser</span><span class="sxs-lookup"><span data-stu-id="59358-415">Clean up objects from memory and print model locations</span></span>
<span data-ttu-id="59358-416">Använd `unpersist()` att ta bort objekt som cachelagrats i minnet.</span><span class="sxs-lookup"><span data-stu-id="59358-416">Use `unpersist()` to delete objects cached in memory.</span></span>

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


<span data-ttu-id="59358-417">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-417">**OUTPUT**</span></span>

<span data-ttu-id="59358-418">PythonRDD [122] vid RDD på PythonRDD.scala: 43</span><span class="sxs-lookup"><span data-stu-id="59358-418">PythonRDD[122] at RDD at PythonRDD.scala: 43</span></span>

<span data-ttu-id="59358-419">** Utskriften sökvägen till modellfiler som ska användas i anteckningsboken förbrukning.</span><span class="sxs-lookup"><span data-stu-id="59358-419">**Printout path to model files to be used in the consumption notebook.</span></span> <span data-ttu-id="59358-420">** Om du vill använda och betygsätta en oberoende datamängd, måste du kopiera och klistra in dessa filnamn i ”förbrukning anteckningsbok”.</span><span class="sxs-lookup"><span data-stu-id="59358-420">** To consume and score an independent data-set, you need to copy and paste these file names in the "Consumption notebook".</span></span>

    # PRINT MODEL FILE LOCATIONS FOR CONSUMPTION
    print "logisticRegFileLoc = modelDir + \"" + logisticregressionfilename + "\"";
    print "linearRegFileLoc = modelDir + \"" + linearregressionfilename + "\"";
    print "randomForestClassificationFileLoc = modelDir + \"" + rfclassificationfilename + "\"";
    print "randomForestRegFileLoc = modelDir + \"" + rfregressionfilename + "\"";
    print "BoostedTreeClassificationFileLoc = modelDir + \"" + btclassificationfilename + "\"";
    print "BoostedTreeRegressionFileLoc = modelDir + \"" + btregressionfilename + "\"";


<span data-ttu-id="59358-421">**UTDATA**</span><span class="sxs-lookup"><span data-stu-id="59358-421">**OUTPUT**</span></span>

<span data-ttu-id="59358-422">logisticRegFileLoc = modelDir + ”LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528”</span><span class="sxs-lookup"><span data-stu-id="59358-422">logisticRegFileLoc = modelDir + "LogisticRegressionWithLBFGS_2016-05-0316_47_30.096528"</span></span>

<span data-ttu-id="59358-423">linearRegFileLoc = modelDir + ”LinearRegressionWithSGD_2016-05-0316_51_28.433670”</span><span class="sxs-lookup"><span data-stu-id="59358-423">linearRegFileLoc = modelDir + "LinearRegressionWithSGD_2016-05-0316_51_28.433670"</span></span>

<span data-ttu-id="59358-424">randomForestClassificationFileLoc = modelDir + ”RandomForestClassification_2016-05-0316_50_17.454440”</span><span class="sxs-lookup"><span data-stu-id="59358-424">randomForestClassificationFileLoc = modelDir + "RandomForestClassification_2016-05-0316_50_17.454440"</span></span>

<span data-ttu-id="59358-425">randomForestRegFileLoc = modelDir + ”RandomForestRegression_2016-05-0316_51_57.331730”</span><span class="sxs-lookup"><span data-stu-id="59358-425">randomForestRegFileLoc = modelDir + "RandomForestRegression_2016-05-0316_51_57.331730"</span></span>

<span data-ttu-id="59358-426">BoostedTreeClassificationFileLoc = modelDir + ”GradientBoostingTreeClassification_2016-05-0316_50_40.138809”</span><span class="sxs-lookup"><span data-stu-id="59358-426">BoostedTreeClassificationFileLoc = modelDir + "GradientBoostingTreeClassification_2016-05-0316_50_40.138809"</span></span>

<span data-ttu-id="59358-427">BoostedTreeRegressionFileLoc = modelDir + ”GradientBoostingTreeRegression_2016-05-0316_52_18.827237”</span><span class="sxs-lookup"><span data-stu-id="59358-427">BoostedTreeRegressionFileLoc = modelDir + "GradientBoostingTreeRegression_2016-05-0316_52_18.827237"</span></span>

## <a name="whats-next"></a><span data-ttu-id="59358-428">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59358-428">What's next?</span></span>
<span data-ttu-id="59358-429">Nu när du har skapat regression och klassificering modeller med Spark MlLib, är du redo att lära dig hur du poängsätta och utvärdera dessa modeller.</span><span class="sxs-lookup"><span data-stu-id="59358-429">Now that you have created regression and classification models with the Spark MlLib, you are ready to learn how to score and evaluate these models.</span></span>

<span data-ttu-id="59358-430">**Modellen förbrukning:** information om hur du poängsätta och utvärdera klassificering och regression modeller som skapats i det här avsnittet finns [poängsätta och utvärdera Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="59358-430">**Model consumption:** To learn how to score and evaluate the classification and regression models created in this topic, see [Score and evaluate Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md).</span></span>

