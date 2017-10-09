---
title: "aaaData vetenskap med hjälp av Scala och Spark på Azure | Microsoft Docs"
description: "Hur hello toouse Scala för övervakade maskininlärning uppgifter med Spark skalbara MLlib och Spark ML paket i ett Azure HDInsight Spark-kluster."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: a7c97153-583e-48fe-b301-365123db3780
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;deguhath
ms.openlocfilehash: e32ebd0b91417183fe48ee10ebc7929fd9605762
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-science-using-scala-and-spark-on-azure"></a><span data-ttu-id="719bd-103">Datavetenskap med Scala och Spark på Azure</span><span class="sxs-lookup"><span data-stu-id="719bd-103">Data Science using Scala and Spark on Azure</span></span>
<span data-ttu-id="719bd-104">Den här artikeln visar hur toouse Scala för övervakade machine learning aktiviteter med hello Spark skalbara MLlib och Spark ML-paket i ett Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-104">This article shows you how toouse Scala for supervised machine learning tasks with hello Spark scalable MLlib and Spark ML packages on an Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="719bd-105">Den vägleder dig genom hello aktiviteter som utgör hello [datavetenskap processen](http://aka.ms/datascienceprocess): datapåfyllning och undersökning, visualisering, funktionen tekniker, modellering och förbrukning av modellen.</span><span class="sxs-lookup"><span data-stu-id="719bd-105">It walks you through hello tasks that constitute hello [Data Science process](http://aka.ms/datascienceprocess): data ingestion and exploration, visualization, feature engineering, modeling, and model consumption.</span></span> <span data-ttu-id="719bd-106">hello modeller i hello artikeln omfattar logistic och linjär regression, slumpmässiga skogar och ökat toningen träd (GBTs), dessutom tootwo gemensamma övervakad machine learning uppgifter:</span><span class="sxs-lookup"><span data-stu-id="719bd-106">hello models in hello article include logistic and linear regression, random forests, and gradient-boosted trees (GBTs), in addition tootwo common supervised machine learning tasks:</span></span>

* <span data-ttu-id="719bd-107">Regression problem: förutsägelse av hello-tips ($) för en taxi resa</span><span class="sxs-lookup"><span data-stu-id="719bd-107">Regression problem: Prediction of hello tip amount ($) for a taxi trip</span></span>
* <span data-ttu-id="719bd-108">Binär klassificering: förutsägelser av tips eller inget tips (1/0) för en taxi resa</span><span class="sxs-lookup"><span data-stu-id="719bd-108">Binary classification: Prediction of tip or no tip (1/0) for a taxi trip</span></span>

<span data-ttu-id="719bd-109">hello modellera processen kräver träning och utvärdering på en datauppsättning test och relevanta noggrannhet mått.</span><span class="sxs-lookup"><span data-stu-id="719bd-109">hello modeling process requires training and evaluation on a test data set and relevant accuracy metrics.</span></span> <span data-ttu-id="719bd-110">I den här artikeln får du veta hur toostore dessa modeller i Azure Blob storage och hur tooscore och utvärdera deras förutsägbar prestanda.</span><span class="sxs-lookup"><span data-stu-id="719bd-110">In this article, you can learn how toostore these models in Azure Blob storage and how tooscore and evaluate their predictive performance.</span></span> <span data-ttu-id="719bd-111">Den här artikeln omfattar också hello mer avancerade alternativ för hur toooptimize modeller med omfattande korsvalidering och hyper-parametern.</span><span class="sxs-lookup"><span data-stu-id="719bd-111">This article also covers hello more advanced topics of how toooptimize models by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="719bd-112">hello-data som används är ett exempel på hello 2013 NYC taxi resa och avgiften datauppsättning finns på GitHub.</span><span class="sxs-lookup"><span data-stu-id="719bd-112">hello data used is a sample of hello 2013 NYC taxi trip and fare data set available on GitHub.</span></span>

<span data-ttu-id="719bd-113">[Scala](http://www.scala-lang.org/), ett språk baserat på hello Java virtual machine integrerar Språkbegrepp för objektorienterad och fungerar.</span><span class="sxs-lookup"><span data-stu-id="719bd-113">[Scala](http://www.scala-lang.org/), a language based on hello Java virtual machine, integrates object-oriented and functional language concepts.</span></span> <span data-ttu-id="719bd-114">Det är ett skalbart språk som är bra toodistributed bearbetning i hello molnet och körs på Azure Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-114">It's a scalable language that is well suited toodistributed processing in hello cloud, and runs on Azure Spark clusters.</span></span>

<span data-ttu-id="719bd-115">[Spark](http://spark.apache.org/) ett ramverk för parallell bearbetning av öppen källkod som stöder i minnet bearbetar tooboost hello prestandan för stordata analytics program.</span><span class="sxs-lookup"><span data-stu-id="719bd-115">[Spark](http://spark.apache.org/) is an open-source parallel-processing framework that supports in-memory processing tooboost hello performance of big data analytics applications.</span></span> <span data-ttu-id="719bd-116">Hej bearbetningsmotorn i Spark är byggd för hastighet, enkel användning och avancerade analyser.</span><span class="sxs-lookup"><span data-stu-id="719bd-116">hello Spark processing engine is built for speed, ease of use, and sophisticated analytics.</span></span> <span data-ttu-id="719bd-117">Sparks funktioner för beräkning för distribuerade i minnet gör det ett bra alternativ för iterativa algoritmer i machine learning och grafberäkningar.</span><span class="sxs-lookup"><span data-stu-id="719bd-117">Spark's in-memory distributed computation capabilities make it a good choice for iterative algorithms in machine learning and graph computations.</span></span> <span data-ttu-id="719bd-118">Hej [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) paketet ger en enhetlig uppsättning övergripande API: er som bygger på data ramar som kan hjälpa dig att skapa och finjustera praktiska maskininlärning pipelines.</span><span class="sxs-lookup"><span data-stu-id="719bd-118">hello [spark.ml](http://spark.apache.org/docs/latest/ml-guide.html) package provides a uniform set of high-level APIs built on top of data frames that can help you create and tune practical machine learning pipelines.</span></span> <span data-ttu-id="719bd-119">[MLlib](http://spark.apache.org/mllib/) är Sparks skalbara machine learning-biblioteket, som ger funktioner för modellering toothis distribuerad miljö.</span><span class="sxs-lookup"><span data-stu-id="719bd-119">[MLlib](http://spark.apache.org/mllib/) is Spark's scalable machine learning library, which brings modeling capabilities toothis distributed environment.</span></span>

<span data-ttu-id="719bd-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) är hello Azure-baserad erbjuds med öppen källkod Spark.</span><span class="sxs-lookup"><span data-stu-id="719bd-120">[HDInsight Spark](../hdinsight/hdinsight-apache-spark-overview.md) is hello Azure-hosted offering of open-source Spark.</span></span> <span data-ttu-id="719bd-121">Den omfattar också stöd för Jupyter Scala-anteckningsböcker på hello Spark-kluster och kan köra Spark SQL interaktiva frågor tootransform filtrera och visualisera data som lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="719bd-121">It also includes support for Jupyter Scala notebooks on hello Spark cluster, and can run Spark SQL interactive queries tootransform, filter, and visualize data stored in Azure Blob storage.</span></span> <span data-ttu-id="719bd-122">Hej Scala kodfragment i den här artikeln som tillhandahåller hello lösningar och visa hello relevanta områden toovisualize hello data körs i Jupyter-anteckningsböcker som är installerad på hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-122">hello Scala code snippets in this article that provide hello solutions and show hello relevant plots toovisualize hello data run in Jupyter notebooks installed on hello Spark clusters.</span></span> <span data-ttu-id="719bd-123">hello modellering stegen i följande avsnitt har kod som visar dig hur tootrain, utvärdera, spara och använda varje typ av modellen.</span><span class="sxs-lookup"><span data-stu-id="719bd-123">hello modeling steps in these topics have code that shows you how tootrain, evaluate, save, and consume each type of model.</span></span>

<span data-ttu-id="719bd-124">hello konfigurationsstegen och kod i den här artikeln gäller för Azure HDInsight 3.4 Spark 1.6.</span><span class="sxs-lookup"><span data-stu-id="719bd-124">hello setup steps and code in this article are for Azure HDInsight 3.4 Spark 1.6.</span></span> <span data-ttu-id="719bd-125">Dock hello koden i den här artikeln och hello [Scala Jupyter-anteckningsbok](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) är allmänna och bör fungera på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-125">However, hello code in this article and in hello [Scala Jupyter Notebook](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration%20Modeling%20and%20Scoring%20using%20Scala.ipynb) are generic and should work on any Spark cluster.</span></span> <span data-ttu-id="719bd-126">Hej konfiguration och hantering av steg kan vara skiljer sig från vad som anges i den här artikeln om du inte använder HDInsight Spark.</span><span class="sxs-lookup"><span data-stu-id="719bd-126">hello cluster setup and management steps might be slightly different from what is shown in this article if you are not using HDInsight Spark.</span></span>

> [!NOTE]
> <span data-ttu-id="719bd-127">Ett avsnitt som visar hur toouse Python snarare än Scala toocomplete aktiviteter för en datavetenskap slutpunkt till slutpunkt-processen, se [datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="719bd-127">For a topic that shows you how toouse Python rather than Scala toocomplete tasks for an end-to-end Data Science process, see [Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="719bd-128">Krav</span><span class="sxs-lookup"><span data-stu-id="719bd-128">Prerequisites</span></span>
* <span data-ttu-id="719bd-129">Du måste ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="719bd-129">You must have an Azure subscription.</span></span> <span data-ttu-id="719bd-130">Om du inte redan har en, [hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="719bd-130">If you do not already have one, [get an Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="719bd-131">Du behöver ett Azure HDInsight 3.4 Spark 1.6 klustret toocomplete hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="719bd-131">You need an Azure HDInsight 3.4 Spark 1.6 cluster toocomplete hello following procedures.</span></span> <span data-ttu-id="719bd-132">toocreate ett kluster, se hello-instruktioner i [komma igång: skapa Apache Spark på Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="719bd-132">toocreate a cluster, see hello instructions in [Get started: Create Apache Spark on Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="719bd-133">Ange typ av hello kluster och version för hello **Välj typ av kluster** menyn.</span><span class="sxs-lookup"><span data-stu-id="719bd-133">Set hello cluster type and version on hello **Select Cluster Type** menu.</span></span>

![HDInsight klusterkonfiguration för typen](./media/machine-learning-data-science-process-scala-walkthrough/spark-cluster-on-portal.png)

> [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

<span data-ttu-id="719bd-135">En beskrivning av hello NYC taxi resa data och anvisningar om hur tooexecute kod från en Jupyter-anteckningsbok på hello Spark-kluster finns i hello relevanta avsnitt i [översikt av datavetenskap med Spark på Azure HDInsight](machine-learning-data-science-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="719bd-135">For a description of hello NYC taxi trip data and instructions on how tooexecute code from a Jupyter notebook on hello Spark cluster, see hello relevant sections in [Overview of Data Science using Spark on Azure HDInsight](machine-learning-data-science-spark-overview.md).</span></span>  

## <a name="execute-scala-code-from-a-jupyter-notebook-on-hello-spark-cluster"></a><span data-ttu-id="719bd-136">Köra Scala kod från en Jupyter-anteckningsbok på hello Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="719bd-136">Execute Scala code from a Jupyter notebook on hello Spark cluster</span></span>
<span data-ttu-id="719bd-137">Du kan starta en Jupyter-anteckningsbok från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="719bd-137">You can launch a Jupyter notebook from hello Azure portal.</span></span> <span data-ttu-id="719bd-138">Hitta hello Spark-kluster på instrumentpanelen och klicka sedan på tooenter hello hanteringssidan för klustret.</span><span class="sxs-lookup"><span data-stu-id="719bd-138">Find hello Spark cluster on your dashboard, and then click it tooenter hello management page for your cluster.</span></span> <span data-ttu-id="719bd-139">Klicka sedan på **Klusterinstrumentpaneler**, och klicka sedan på **Jupyter-anteckningsbok** tooopen hello bärbar dator som är associerade med hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-139">Next, click **Cluster Dashboards**, and then click **Jupyter Notebook** tooopen hello notebook associated with hello Spark cluster.</span></span>

![Instrumentpanelen klustret och Jupyter-anteckningsböcker](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-on-portal.png)

<span data-ttu-id="719bd-141">Du kan också komma åt Jupyter-anteckningsböcker med https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="719bd-141">You also can access Jupyter notebooks at https://&lt;clustername&gt;.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="719bd-142">Ersätt *klusternamn* med hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="719bd-142">Replace *clustername* with hello name of your cluster.</span></span> <span data-ttu-id="719bd-143">Du behöver hello lösenord för din administratör konto tooaccess hello Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="719bd-143">You need hello password for your administrator account tooaccess hello Jupyter notebooks.</span></span>

![Gå tooJupyter bärbara datorer med hjälp av hello klustrets namn](./media/machine-learning-data-science-process-scala-walkthrough/spark-jupyter-notebook.png)

<span data-ttu-id="719bd-145">Välj **Scala** toosee en katalog som är några exempel på färdigförpackade bärbara datorer att använda hello PySpark-API.</span><span class="sxs-lookup"><span data-stu-id="719bd-145">Select **Scala** toosee a directory that has a few examples of prepackaged notebooks that use hello PySpark API.</span></span> <span data-ttu-id="719bd-146">Hej utforskning modellering och beräkning med Scala.ipynb anteckningsbok med hello kodexempel för den här sviten Spark avsnitt är tillgängliga på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span><span class="sxs-lookup"><span data-stu-id="719bd-146">hello Exploration Modeling and Scoring using Scala.ipynb notebook that contains hello code samples for this suite of Spark topics is available on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/Scala).</span></span>

<span data-ttu-id="719bd-147">Du kan ladda upp hello anteckningsboken direkt från GitHub toohello Jupyter-anteckningsbok server på Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-147">You can upload hello notebook directly from GitHub toohello Jupyter Notebook server on your Spark cluster.</span></span> <span data-ttu-id="719bd-148">Klicka på startsidan Jupyter hello **överför** knappen.</span><span class="sxs-lookup"><span data-stu-id="719bd-148">On your Jupyter home page, click hello **Upload** button.</span></span> <span data-ttu-id="719bd-149">Klistra in hello GitHub (rådata innehåll) URL för hello Scala anteckningsboken i hello Utforskaren och klicka sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="719bd-149">In hello file explorer, paste hello GitHub (raw content) URL of hello Scala notebook, and then click **Open**.</span></span> <span data-ttu-id="719bd-150">Hej Scala anteckningsboken finns på hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="719bd-150">hello Scala notebook is available at hello following URL:</span></span>

[<span data-ttu-id="719bd-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span><span class="sxs-lookup"><span data-stu-id="719bd-151">Exploration-Modeling-and-Scoring-using-Scala.ipynb</span></span>](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Scala/Exploration-Modeling-and-Scoring-using-Scala.ipynb)

## <a name="setup-preset-spark-and-hive-contexts-spark-magics-and-spark-libraries"></a><span data-ttu-id="719bd-152">Konfigurera: Förinställda Spark och Hive-kontexterna, Spark användbara och Spark-bibliotek</span><span class="sxs-lookup"><span data-stu-id="719bd-152">Setup: Preset Spark and Hive contexts, Spark magics, and Spark libraries</span></span>
### <a name="preset-spark-and-hive-contexts"></a><span data-ttu-id="719bd-153">Förinställningen Spark och Hive-kontexterna</span><span class="sxs-lookup"><span data-stu-id="719bd-153">Preset Spark and Hive contexts</span></span>
    # SET hello START TIME
    import java.util.Calendar
    val beginningTime = Calendar.getInstance().getTime()


<span data-ttu-id="719bd-154">hello Spark kernlar som tillhandahålls med Jupyter-anteckningsböcker har förinställda sammanhang.</span><span class="sxs-lookup"><span data-stu-id="719bd-154">hello Spark kernels that are provided with Jupyter notebooks have preset contexts.</span></span> <span data-ttu-id="719bd-155">Du behöver inte tooexplicitly set hello Spark- eller Hive kontexter innan du börjar arbeta med hello-program som du utvecklar.</span><span class="sxs-lookup"><span data-stu-id="719bd-155">You don't need tooexplicitly set hello Spark or Hive contexts before you start working with hello application you are developing.</span></span> <span data-ttu-id="719bd-156">hello förinställda kontexter är:</span><span class="sxs-lookup"><span data-stu-id="719bd-156">hello preset contexts are:</span></span>

* <span data-ttu-id="719bd-157">`sc`för SparkContext</span><span class="sxs-lookup"><span data-stu-id="719bd-157">`sc` for SparkContext</span></span>
* <span data-ttu-id="719bd-158">`sqlContext`för HiveContext</span><span class="sxs-lookup"><span data-stu-id="719bd-158">`sqlContext` for HiveContext</span></span>

### <a name="spark-magics"></a><span data-ttu-id="719bd-159">Spark användbara</span><span class="sxs-lookup"><span data-stu-id="719bd-159">Spark magics</span></span>
<span data-ttu-id="719bd-160">hello Spark kernel innehåller vissa fördefinierade ”användbara”, som är särskilda kommandon som kan anropas med `%%`.</span><span class="sxs-lookup"><span data-stu-id="719bd-160">hello Spark kernel provides some predefined “magics,” which are special commands that you can call with `%%`.</span></span> <span data-ttu-id="719bd-161">Två av dessa kommandon som används i följande kodexempel hello.</span><span class="sxs-lookup"><span data-stu-id="719bd-161">Two of these commands are used in hello following code samples.</span></span>

* <span data-ttu-id="719bd-162">`%%local`Anger att hello koden i efterföljande rader körs lokalt.</span><span class="sxs-lookup"><span data-stu-id="719bd-162">`%%local` specifies that hello code in subsequent lines will be executed locally.</span></span> <span data-ttu-id="719bd-163">hello-koden måste vara giltig Scala-kod.</span><span class="sxs-lookup"><span data-stu-id="719bd-163">hello code must be valid Scala code.</span></span>
* <span data-ttu-id="719bd-164">`%%sql -o <variable name>`Kör en Hive-fråga mot `sqlContext`.</span><span class="sxs-lookup"><span data-stu-id="719bd-164">`%%sql -o <variable name>` executes a Hive query against `sqlContext`.</span></span> <span data-ttu-id="719bd-165">Om hello `-o` -parameter har skickats, hello resultatet av hello fråga sparas i hello `%%local` Scala kontext som en Spark data ram.</span><span class="sxs-lookup"><span data-stu-id="719bd-165">If hello `-o` parameter is passed, hello result of hello query is persisted in hello `%%local` Scala context as a Spark data frame.</span></span>

<span data-ttu-id="719bd-166">För mer information om hello kärnor för Jupyter-anteckningsböcker och deras fördefinierade ”magics” som du anropar med `%%` (till exempel `%%local`), se [kernlar som är tillgängliga för Jupyter-anteckningsböcker med HDInsight Spark Linux-kluster på HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="719bd-166">For more information about hello kernels for Jupyter notebooks and their predefined "magics" that you call with `%%` (for example, `%%local`), see [Kernels available for Jupyter notebooks with HDInsight Spark Linux clusters on HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

### <a name="import-libraries"></a><span data-ttu-id="719bd-167">Importera bibliotek</span><span class="sxs-lookup"><span data-stu-id="719bd-167">Import libraries</span></span>
<span data-ttu-id="719bd-168">Importera hello Spark, MLlib och andra bibliotek som du behöver genom att använda hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="719bd-168">Import hello Spark, MLlib, and other libraries you'll need by using hello following code.</span></span>

    # IMPORT SPARK AND JAVA LIBRARIES
    import org.apache.spark.sql.SQLContext
    import org.apache.spark.sql.functions._
    import java.text.SimpleDateFormat
    import java.util.Calendar
    import sqlContext.implicits._
    import org.apache.spark.sql.Row

    # IMPORT SPARK SQL FUNCTIONS
    import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType, FloatType, DoubleType}
    import org.apache.spark.sql.functions.rand

    # IMPORT SPARK ML FUNCTIONS
    import org.apache.spark.ml.Pipeline
    import org.apache.spark.ml.feature.{StringIndexer, VectorAssembler, OneHotEncoder, VectorIndexer, Binarizer}
    import org.apache.spark.ml.tuning.{ParamGridBuilder, TrainValidationSplit, CrossValidator}
    import org.apache.spark.ml.regression.{LinearRegression, LinearRegressionModel, RandomForestRegressor, RandomForestRegressionModel, GBTRegressor, GBTRegressionModel}
    import org.apache.spark.ml.classification.{LogisticRegression, LogisticRegressionModel, RandomForestClassifier, RandomForestClassificationModel, GBTClassifier, GBTClassificationModel}
    import org.apache.spark.ml.evaluation.{BinaryClassificationEvaluator, RegressionEvaluator, MulticlassClassificationEvaluator}

    # IMPORT SPARK MLLIB FUNCTIONS
    import org.apache.spark.mllib.linalg.{Vector, Vectors}
    import org.apache.spark.mllib.util.MLUtils
    import org.apache.spark.mllib.classification.{LogisticRegressionWithLBFGS, LogisticRegressionModel}
    import org.apache.spark.mllib.regression.{LabeledPoint, LinearRegressionWithSGD, LinearRegressionModel}
    import org.apache.spark.mllib.tree.{GradientBoostedTrees, RandomForest}
    import org.apache.spark.mllib.tree.configuration.BoostingStrategy
    import org.apache.spark.mllib.tree.model.{GradientBoostedTreesModel, RandomForestModel, Predict}
    import org.apache.spark.mllib.evaluation.{BinaryClassificationMetrics, MulticlassMetrics, RegressionMetrics}

    # SPECIFY SQLCONTEXT
    val sqlContext = new SQLContext(sc)


## <a name="data-ingestion"></a><span data-ttu-id="719bd-169">Datainhämtning</span><span class="sxs-lookup"><span data-stu-id="719bd-169">Data ingestion</span></span>
<span data-ttu-id="719bd-170">hello första steget i hello datavetenskap processen är tooingest hello data som du vill tooanalyze.</span><span class="sxs-lookup"><span data-stu-id="719bd-170">hello first step in hello Data Science process is tooingest hello data that you want tooanalyze.</span></span> <span data-ttu-id="719bd-171">Du kan aktivera hello data från externa källor eller system där den finns i din data från kartläggning av naturresurser och modellering miljö.</span><span class="sxs-lookup"><span data-stu-id="719bd-171">You bring hello data from external sources or systems where it resides into your data exploration and modeling environment.</span></span> <span data-ttu-id="719bd-172">I den här artikeln är hello data du mata in en domänansluten 0,1% exempel hello taxi resa och avgiften filen (som lagras som en TSV-fil).</span><span class="sxs-lookup"><span data-stu-id="719bd-172">In this article, hello data you ingest is a joined 0.1% sample of hello taxi trip and fare file (stored as a .tsv file).</span></span> <span data-ttu-id="719bd-173">hello data från kartläggning av naturresurser och modellering miljö är Spark.</span><span class="sxs-lookup"><span data-stu-id="719bd-173">hello data exploration and modeling environment is Spark.</span></span> <span data-ttu-id="719bd-174">Det här avsnittet innehåller följande rad uppgifter hello kod toocomplete hello:</span><span class="sxs-lookup"><span data-stu-id="719bd-174">This section contains hello code toocomplete hello following series of tasks:</span></span>

1. <span data-ttu-id="719bd-175">Ange katalogsökvägar för lagring av data och modell.</span><span class="sxs-lookup"><span data-stu-id="719bd-175">Set directory paths for data and model storage.</span></span>
2. <span data-ttu-id="719bd-176">Läs i hello inkommande datamängd (som lagras som en TSV-fil).</span><span class="sxs-lookup"><span data-stu-id="719bd-176">Read in hello input data set (stored as a .tsv file).</span></span>
3. <span data-ttu-id="719bd-177">Definiera ett schema för hello data och ren hello data.</span><span class="sxs-lookup"><span data-stu-id="719bd-177">Define a schema for hello data and clean hello data.</span></span>
4. <span data-ttu-id="719bd-178">Skapa en rensade data ram och cachelagra i minnet.</span><span class="sxs-lookup"><span data-stu-id="719bd-178">Create a cleaned data frame and cache it in memory.</span></span>
5. <span data-ttu-id="719bd-179">Registrera hello data som en tillfällig tabell i SQLContext.</span><span class="sxs-lookup"><span data-stu-id="719bd-179">Register hello data as a temporary table in SQLContext.</span></span>
6. <span data-ttu-id="719bd-180">Fråga hello tabellen och importera hello resultaten till en data-ram.</span><span class="sxs-lookup"><span data-stu-id="719bd-180">Query hello table and import hello results into a data frame.</span></span>

### <a name="set-directory-paths-for-storage-locations-in-azure-blob-storage"></a><span data-ttu-id="719bd-181">Ange katalogsökvägar för lagringsplatser i Azure Blob storage</span><span class="sxs-lookup"><span data-stu-id="719bd-181">Set directory paths for storage locations in Azure Blob storage</span></span>
<span data-ttu-id="719bd-182">Spark kan läsa och skriva tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="719bd-182">Spark can read and write tooAzure Blob storage.</span></span> <span data-ttu-id="719bd-183">Du kan använda Spark tooprocess någon av dina befintliga data och lagra om hello resultat i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="719bd-183">You can use Spark tooprocess any of your existing data, and then store hello results again in Blob storage.</span></span>

<span data-ttu-id="719bd-184">toosave modeller eller filer i Blob storage, behöver du tooproperly ange hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="719bd-184">toosave models or files in Blob storage, you need tooproperly specify hello path.</span></span> <span data-ttu-id="719bd-185">Referens hello standardbehållaren kopplad toohello Spark-kluster med hjälp av en sökväg som börjar med `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="719bd-185">Reference hello default container attached toohello Spark cluster by using a path that begins with `wasb:///`.</span></span> <span data-ttu-id="719bd-186">Referera till andra platser med hjälp av `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="719bd-186">Reference other locations by using `wasb://`.</span></span>

<span data-ttu-id="719bd-187">hello anger följande kodexempel hello plats hello indata toobe läsa och hello sökvägen tooBlob lagring som är bifogade toohello Spark-kluster där hello modellen ska sparas.</span><span class="sxs-lookup"><span data-stu-id="719bd-187">hello following code sample specifies hello location of hello input data toobe read and hello path tooBlob storage that is attached toohello Spark cluster where hello model will be saved.</span></span>

    # SET PATHS tooDATA AND MODEL FILE LOCATIONS
    # INGEST DATA AND SPECIFY HEADERS FOR COLUMNS
    val taxi_train_file = sc.textFile("wasb://mllibwalkthroughs@cdspsparksamples.blob.core.windows.net/Data/NYCTaxi/JoinedTaxiTripFare.Point1Pct.Train.tsv")
    val header = taxi_train_file.first;

    # SET hello MODEL STORAGE DIRECTORY PATH
    # NOTE THAT hello FINAL BACKSLASH IN hello PATH IS REQUIRED.
    val modelDir = "wasb:///user/remoteuser/NYCTaxi/Models/";


### <a name="import-data-create-an-rdd-and-define-a-data-frame-according-toohello-schema"></a><span data-ttu-id="719bd-188">Importera data, skapa en RDD och definierar data enligt toohello schema</span><span class="sxs-lookup"><span data-stu-id="719bd-188">Import data, create an RDD, and define a data frame according toohello schema</span></span>
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello SCHEMA BASED ON hello HEADER OF hello FILE
    val sqlContext = new SQLContext(sc)
    val taxi_schema = StructType(
        Array(
            StructField("medallion", StringType, true),
            StructField("hack_license", StringType, true),
            StructField("vendor_id", StringType, true),
            StructField("rate_code", DoubleType, true),
            StructField("store_and_fwd_flag", StringType, true),
            StructField("pickup_datetime", StringType, true),
            StructField("dropoff_datetime", StringType, true),
            StructField("pickup_hour", DoubleType, true),
            StructField("pickup_week", DoubleType, true),
            StructField("weekday", DoubleType, true),
            StructField("passenger_count", DoubleType, true),
            StructField("trip_time_in_secs", DoubleType, true),
            StructField("trip_distance", DoubleType, true),
            StructField("pickup_longitude", DoubleType, true),
            StructField("pickup_latitude", DoubleType, true),
            StructField("dropoff_longitude", DoubleType, true),
            StructField("dropoff_latitude", DoubleType, true),
            StructField("direct_distance", StringType, true),
            StructField("payment_type", StringType, true),
            StructField("fare_amount", DoubleType, true),
            StructField("surcharge", DoubleType, true),
            StructField("mta_tax", DoubleType, true),
            StructField("tip_amount", DoubleType, true),
            StructField("tolls_amount", DoubleType, true),
            StructField("total_amount", DoubleType, true),
            StructField("tipped", DoubleType, true),
            StructField("tip_class", DoubleType, true)
            )
        )

    # CAST VARIABLES ACCORDING toohello SCHEMA
    val taxi_temp = (taxi_train_file.map(_.split("\t"))
                            .filter((r) => r(0) != "medallion")
                            .map(p => Row(p(0), p(1), p(2),
                                p(3).toDouble, p(4), p(5), p(6), p(7).toDouble, p(8).toDouble, p(9).toDouble, p(10).toDouble,
                                p(11).toDouble, p(12).toDouble, p(13).toDouble, p(14).toDouble, p(15).toDouble, p(16).toDouble,
                                p(17), p(18), p(19).toDouble, p(20).toDouble, p(21).toDouble, p(22).toDouble,
                                p(23).toDouble, p(24).toDouble, p(25).toDouble, p(26).toDouble)))


    # CREATE AN INITIAL DATA FRAME AND DROP COLUMNS, AND THEN CREATE A CLEANED DATA FRAME BY FILTERING FOR UNWANTED VALUES OR OUTLIERS
    val taxi_train_df = sqlContext.createDataFrame(taxi_temp, taxi_schema)

    val taxi_df_train_cleaned = (taxi_train_df.drop(taxi_train_df.col("medallion"))
            .drop(taxi_train_df.col("hack_license")).drop(taxi_train_df.col("store_and_fwd_flag"))
            .drop(taxi_train_df.col("pickup_datetime")).drop(taxi_train_df.col("dropoff_datetime"))
            .drop(taxi_train_df.col("pickup_longitude")).drop(taxi_train_df.col("pickup_latitude"))
            .drop(taxi_train_df.col("dropoff_longitude")).drop(taxi_train_df.col("dropoff_latitude"))
            .drop(taxi_train_df.col("surcharge")).drop(taxi_train_df.col("mta_tax"))
            .drop(taxi_train_df.col("direct_distance")).drop(taxi_train_df.col("tolls_amount"))
            .drop(taxi_train_df.col("total_amount")).drop(taxi_train_df.col("tip_class"))
            .filter("passenger_count > 0 and passenger_count < 8 AND payment_type in ('CSH', 'CRD') AND tip_amount >= 0 AND tip_amount < 30 AND fare_amount >= 1 AND fare_amount < 150 AND trip_distance > 0 AND trip_distance < 100 AND trip_time_in_secs > 30 AND trip_time_in_secs < 7200"));

    # CACHE AND MATERIALIZE hello CLEANED DATA FRAME IN MEMORY
    taxi_df_train_cleaned.cache()
    taxi_df_train_cleaned.count()

    # REGISTER hello DATA FRAME AS A TEMPORARY TABLE IN SQLCONTEXT
    taxi_df_train_cleaned.registerTempTable("taxi_train")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-189">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-189">**Output:**</span></span>

<span data-ttu-id="719bd-190">Tid toorun hello cell: 8 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-190">Time toorun hello cell: 8 seconds.</span></span>

### <a name="query-hello-table-and-import-results-in-a-data-frame"></a><span data-ttu-id="719bd-191">Fråga hello tabellen och importera resulterar i en data-ram</span><span class="sxs-lookup"><span data-stu-id="719bd-191">Query hello table and import results in a data frame</span></span>
<span data-ttu-id="719bd-192">Nästa hello frågetabellen för avgiften, resande och tips data. Filtrera bort skadad och öar data. och Skriv ut flera rader.</span><span class="sxs-lookup"><span data-stu-id="719bd-192">Next, query hello table for fare, passenger, and tip data; filter out corrupt and outlying data; and print several rows.</span></span>

    # QUERY hello DATA
    val sqlStatement = """
        SELECT fare_amount, passenger_count, tip_amount, tipped
        FROM taxi_train
        WHERE passenger_count > 0 AND passenger_count < 7
        AND fare_amount > 0 AND fare_amount < 200
        AND payment_type in ('CSH', 'CRD')
        AND tip_amount > 0 AND tip_amount < 25
    """
    val sqlResultsDF = sqlContext.sql(sqlStatement)

    # SHOW ONLY hello TOP THREE ROWS
    sqlResultsDF.show(3)

<span data-ttu-id="719bd-193">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-193">**Output:**</span></span>

| <span data-ttu-id="719bd-194">fare_amount</span><span class="sxs-lookup"><span data-stu-id="719bd-194">fare_amount</span></span> | <span data-ttu-id="719bd-195">passenger_count</span><span class="sxs-lookup"><span data-stu-id="719bd-195">passenger_count</span></span> | <span data-ttu-id="719bd-196">tip_amount</span><span class="sxs-lookup"><span data-stu-id="719bd-196">tip_amount</span></span> | <span data-ttu-id="719bd-197">lutad</span><span class="sxs-lookup"><span data-stu-id="719bd-197">tipped</span></span> |
| --- | --- | --- | --- |
|        <span data-ttu-id="719bd-198">13.5</span><span class="sxs-lookup"><span data-stu-id="719bd-198">13.5</span></span> |<span data-ttu-id="719bd-199">1.0</span><span class="sxs-lookup"><span data-stu-id="719bd-199">1.0</span></span> |<span data-ttu-id="719bd-200">2.9</span><span class="sxs-lookup"><span data-stu-id="719bd-200">2.9</span></span> |<span data-ttu-id="719bd-201">1.0</span><span class="sxs-lookup"><span data-stu-id="719bd-201">1.0</span></span> |
|        <span data-ttu-id="719bd-202">16.0</span><span class="sxs-lookup"><span data-stu-id="719bd-202">16.0</span></span> |<span data-ttu-id="719bd-203">2.0</span><span class="sxs-lookup"><span data-stu-id="719bd-203">2.0</span></span> |<span data-ttu-id="719bd-204">3.4</span><span class="sxs-lookup"><span data-stu-id="719bd-204">3.4</span></span> |<span data-ttu-id="719bd-205">1.0</span><span class="sxs-lookup"><span data-stu-id="719bd-205">1.0</span></span> |
|        <span data-ttu-id="719bd-206">10.5</span><span class="sxs-lookup"><span data-stu-id="719bd-206">10.5</span></span> |<span data-ttu-id="719bd-207">2.0</span><span class="sxs-lookup"><span data-stu-id="719bd-207">2.0</span></span> |<span data-ttu-id="719bd-208">1.0</span><span class="sxs-lookup"><span data-stu-id="719bd-208">1.0</span></span> |<span data-ttu-id="719bd-209">1.0</span><span class="sxs-lookup"><span data-stu-id="719bd-209">1.0</span></span> |

## <a name="data-exploration-and-visualization"></a><span data-ttu-id="719bd-210">Datagranskning och visualisering</span><span class="sxs-lookup"><span data-stu-id="719bd-210">Data exploration and visualization</span></span>
<span data-ttu-id="719bd-211">När du sätta hello data i Spark är hello nästa steg i hello datavetenskap processen toogain en djupare förståelse för hello data via undersökning och visualisering.</span><span class="sxs-lookup"><span data-stu-id="719bd-211">After you bring hello data into Spark, hello next step in hello Data Science process is toogain a deeper understanding of hello data through exploration and visualization.</span></span> <span data-ttu-id="719bd-212">I det här avsnittet kan undersöka du hello taxi data med hjälp av SQL-frågor.</span><span class="sxs-lookup"><span data-stu-id="719bd-212">In this section, you examine hello taxi data by using SQL queries.</span></span> <span data-ttu-id="719bd-213">Sedan hello hello importresultaten till en data ram tooplot target-variabler och potentiella funktioner för granskning med funktionen hello automatiskt visualisering av Jupyter.</span><span class="sxs-lookup"><span data-stu-id="719bd-213">Then, import hello results into a data frame tooplot hello target variables and prospective features for visual inspection by using hello auto-visualization feature of Jupyter.</span></span>

### <a name="use-local-and-sql-magic-tooplot-data"></a><span data-ttu-id="719bd-214">Använda lokala och SQL magiskt tooplot data</span><span class="sxs-lookup"><span data-stu-id="719bd-214">Use local and SQL magic tooplot data</span></span>
<span data-ttu-id="719bd-215">Hello utdata från alla kodstycke som du kör från en Jupyter-anteckningsbok är tillgängliga i hello kontexten för hello-session som är kvar på hello arbetarnoder som standard.</span><span class="sxs-lookup"><span data-stu-id="719bd-215">By default, hello output of any code snippet that you run from a Jupyter notebook is available within hello context of hello session that is persisted on hello worker nodes.</span></span> <span data-ttu-id="719bd-216">Om du vill toosave en resa toohello arbetarnoder för varje beräkning och om alla hello data som du behöver för din beräkning är tillgängliga lokalt på hello Jupyter-servernoden (vilket är hello huvudnod), kan du använda hello `%%local` magiska toorun hello kod fragment på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="719bd-216">If you want toosave a trip toohello worker nodes for every computation, and if all hello data that you need for your computation is available locally on hello Jupyter server node (which is hello head node), you can use hello `%%local` magic toorun hello code snippet on hello Jupyter server.</span></span>

* <span data-ttu-id="719bd-217">**SQL-magic** (`%%sql`).</span><span class="sxs-lookup"><span data-stu-id="719bd-217">**SQL magic** (`%%sql`).</span></span> <span data-ttu-id="719bd-218">Hej HDInsight Spark kernel stöder enkelt infogade HiveQL frågor mot SQLContext.</span><span class="sxs-lookup"><span data-stu-id="719bd-218">hello HDInsight Spark kernel supports easy inline HiveQL queries against SQLContext.</span></span> <span data-ttu-id="719bd-219">hello (`-o VARIABLE_NAME`) argumentet kvarstår hello utdata från hello SQL-frågan som en Pandas data ram på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="719bd-219">hello (`-o VARIABLE_NAME`) argument persists hello output of hello SQL query as a Pandas data frame on hello Jupyter server.</span></span> <span data-ttu-id="719bd-220">Det innebär att den ska vara tillgänglig i hello lokalt läge.</span><span class="sxs-lookup"><span data-stu-id="719bd-220">This means it'll be available in hello local mode.</span></span>
* <span data-ttu-id="719bd-221">`%%local`**magic**.</span><span class="sxs-lookup"><span data-stu-id="719bd-221">`%%local` **magic**.</span></span> <span data-ttu-id="719bd-222">Hej `%%local` magic körs hello kod lokalt på hello Jupyter servern som hello huvudnod hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="719bd-222">hello `%%local` magic runs hello code locally on hello Jupyter server, which is hello head node of hello HDInsight cluster.</span></span> <span data-ttu-id="719bd-223">Normalt använder du `%%local` magiskt tillsammans med hello `%%sql` magiskt med hello `-o` parameter.</span><span class="sxs-lookup"><span data-stu-id="719bd-223">Typically, you use `%%local` magic in conjunction with hello `%%sql` magic with hello `-o` parameter.</span></span> <span data-ttu-id="719bd-224">Hej `-o` parametern skulle bevara hello utdata från hello SQL-frågan lokalt, och sedan `%%local` magic initierar hello nästa uppsättning kodfragment toorun lokalt mot hello utdata för hello SQL-frågor som sparas lokalt.</span><span class="sxs-lookup"><span data-stu-id="719bd-224">hello `-o` parameter would persist hello output of hello SQL query locally, and then `%%local` magic would trigger hello next set of code snippet toorun locally against hello output of hello SQL queries that is persisted locally.</span></span>

### <a name="query-hello-data-by-using-sql"></a><span data-ttu-id="719bd-225">Fråga hello data med hjälp av SQL</span><span class="sxs-lookup"><span data-stu-id="719bd-225">Query hello data by using SQL</span></span>
<span data-ttu-id="719bd-226">Den här frågan hämtar hello taxi resor av avgiften, passagerare antal, och tips belopp.</span><span class="sxs-lookup"><span data-stu-id="719bd-226">This query retrieves hello taxi trips by fare amount, passenger count, and tip amount.</span></span>

    # RUN hello SQL QUERY
    %%sql -q -o sqlResults
    SELECT fare_amount, passenger_count, tip_amount, tipped FROM taxi_train WHERE passenger_count > 0 AND passenger_count < 7 AND fare_amount > 0 AND fare_amount < 200 AND payment_type in ('CSH', 'CRD') AND tip_amount > 0 AND tip_amount < 25

<span data-ttu-id="719bd-227">I följande kod hello, hello `%%local` magic skapas en lokala data, sqlResults.</span><span class="sxs-lookup"><span data-stu-id="719bd-227">In hello following code, hello `%%local` magic creates a local data frame, sqlResults.</span></span> <span data-ttu-id="719bd-228">Du kan använda sqlResults tooplot med hjälp av matplotlib.</span><span class="sxs-lookup"><span data-stu-id="719bd-228">You can use sqlResults tooplot by using matplotlib.</span></span>

> [!TIP]
> <span data-ttu-id="719bd-229">Lokala magic används flera gånger i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="719bd-229">Local magic is used multiple times in this article.</span></span> <span data-ttu-id="719bd-230">Om din datauppsättning är stor, ta prov toocreate en data-ram som får plats i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="719bd-230">If your data set is large, please sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

### <a name="plot-hello-data"></a><span data-ttu-id="719bd-231">Rita hello data</span><span class="sxs-lookup"><span data-stu-id="719bd-231">Plot hello data</span></span>
<span data-ttu-id="719bd-232">Du kan rita med hjälp av Python-kod när hello dataramen är i lokala kontexten som en Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="719bd-232">You can plot by using Python code after hello data frame is in local context as a Pandas data frame.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES.
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, ETC.)
    sqlResults


 <span data-ttu-id="719bd-233">hello Spark kernel visualizes automatiskt hello utdata för SQL (HiveQL)-frågor när du har kört hello kod.</span><span class="sxs-lookup"><span data-stu-id="719bd-233">hello Spark kernel automatically visualizes hello output of SQL (HiveQL) queries after you run hello code.</span></span> <span data-ttu-id="719bd-234">Du kan välja mellan flera olika typer av visualiseringar:</span><span class="sxs-lookup"><span data-stu-id="719bd-234">You can choose between several types of visualizations:</span></span>

* <span data-ttu-id="719bd-235">Tabell</span><span class="sxs-lookup"><span data-stu-id="719bd-235">Table</span></span>
* <span data-ttu-id="719bd-236">Cirkeldiagram</span><span class="sxs-lookup"><span data-stu-id="719bd-236">Pie</span></span>
* <span data-ttu-id="719bd-237">Rad</span><span class="sxs-lookup"><span data-stu-id="719bd-237">Line</span></span>
* <span data-ttu-id="719bd-238">Område</span><span class="sxs-lookup"><span data-stu-id="719bd-238">Area</span></span>
* <span data-ttu-id="719bd-239">Fältet</span><span class="sxs-lookup"><span data-stu-id="719bd-239">Bar</span></span>

<span data-ttu-id="719bd-240">Här är hello kod tooplot hello data:</span><span class="sxs-lookup"><span data-stu-id="719bd-240">Here's hello code tooplot hello data:</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    import matplotlib.pyplot as plt
    %matplotlib inline

    # PLOT TIP BY PAYMENT TYPE AND PASSENGER COUNT
    ax1 = sqlResults[['tip_amount']].plot(kind='hist', bins=25, facecolor='lightblue')
    ax1.set_title('Tip amount distribution')
    ax1.set_xlabel('Tip Amount ($)')
    ax1.set_ylabel('Counts')
    plt.suptitle('')
    plt.show()

    # PLOT TIP BY PASSENGER COUNT
    ax2 = sqlResults.boxplot(column=['tip_amount'], by=['passenger_count'])
    ax2.set_title('Tip amount by Passenger count')
    ax2.set_xlabel('Passenger count')
    ax2.set_ylabel('Tip Amount ($)')
    plt.suptitle('')
    plt.show()

    # PLOT TIP AMOUNT BY FARE AMOUNT; SCALE POINTS BY PASSENGER COUNT
    ax = sqlResults.plot(kind='scatter', x= 'fare_amount', y = 'tip_amount', c='blue', alpha = 0.10, s=5*(sqlResults.passenger_count))
    ax.set_title('Tip amount by Fare amount')
    ax.set_xlabel('Fare Amount ($)')
    ax.set_ylabel('Tip Amount ($)')
    plt.axis([-2, 80, -2, 20])
    plt.show()


<span data-ttu-id="719bd-241">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-241">**Output:**</span></span>

![Tips belopp histogram](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-histogram.png)

![Tips belopp av passagerare antal](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-passenger-count.png)

![Tips belopp med avgiften belopp](./media/machine-learning-data-science-process-scala-walkthrough/plot-tip-amount-by-fare-amount.png)

## <a name="create-features-and-transform-features-and-then-prep-data-for-input-into-modeling-functions"></a><span data-ttu-id="719bd-245">Skapa funktioner och transformera funktioner och Förbered dig för indata i modeling funktioner</span><span class="sxs-lookup"><span data-stu-id="719bd-245">Create features and transform features, and then prep data for input into modeling functions</span></span>
<span data-ttu-id="719bd-246">Trädet-baserade modelleringsfunktioner från Spark ML och MLlib har du tooprepare mål och funktioner med hjälp av en mängd olika tekniker, till exempel diskretisering, indexering, en hot kodning och vectorization.</span><span class="sxs-lookup"><span data-stu-id="719bd-246">For tree-based modeling functions from Spark ML and MLlib, you have tooprepare target and features by using a variety of techniques, such as binning, indexing, one-hot encoding, and vectorization.</span></span> <span data-ttu-id="719bd-247">Här följer hello procedurer toofollow i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="719bd-247">Here are hello procedures toofollow in this section:</span></span>

1. <span data-ttu-id="719bd-248">Skapa en ny funktion av **diskretisering** timmar till trafik tid buckets.</span><span class="sxs-lookup"><span data-stu-id="719bd-248">Create a new feature by **binning** hours into traffic time buckets.</span></span>
2. <span data-ttu-id="719bd-249">Tillämpa **indexering och en hot kodning** toocategorical funktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-249">Apply **indexing and one-hot encoding** toocategorical features.</span></span>
3. <span data-ttu-id="719bd-250">**Exempel och dela hello datauppsättning** till bråk träning och testning.</span><span class="sxs-lookup"><span data-stu-id="719bd-250">**Sample and split hello data set** into training and test fractions.</span></span>
4. <span data-ttu-id="719bd-251">**Ange variabeln utbildning och funktioner**, och sedan skapa indexerade eller ett hot kodade utbildning och testar inkommande märkt punkt flexibel distribueras datauppsättningar (RDDs) eller ramar.</span><span class="sxs-lookup"><span data-stu-id="719bd-251">**Specify training variable and features**, and then create indexed or one-hot encoded training and testing input labeled point resilient distributed datasets (RDDs) or data frames.</span></span>
5. <span data-ttu-id="719bd-252">Automatiskt **kategorisera och vectorize funktioner och mål** toouse som indata för machine learning-modeller.</span><span class="sxs-lookup"><span data-stu-id="719bd-252">Automatically **categorize and vectorize features and targets** toouse as inputs for machine learning models.</span></span>

### <a name="create-a-new-feature-by-binning-hours-into-traffic-time-buckets"></a><span data-ttu-id="719bd-253">Skapa en ny funktion med diskretisering timmar till trafik tid buckets</span><span class="sxs-lookup"><span data-stu-id="719bd-253">Create a new feature by binning hours into traffic time buckets</span></span>
<span data-ttu-id="719bd-254">Den här koden visar hur toocreate en ny funktion av diskretisering timmar till trafik tid tidsgrupper och hur toocache hello resulterande data ram i minnet.</span><span class="sxs-lookup"><span data-stu-id="719bd-254">This code shows you how toocreate a new feature by binning hours into traffic time buckets and how toocache hello resulting data frame in memory.</span></span> <span data-ttu-id="719bd-255">Om RDDs och data används flera gånger, leder cachelagring tooimproved körningstider.</span><span class="sxs-lookup"><span data-stu-id="719bd-255">Where RDDs and data frames are used repeatedly, caching leads tooimproved execution times.</span></span> <span data-ttu-id="719bd-256">Du måste därför cachelagra RDDs och data i flera steg under hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="719bd-256">Accordingly, you'll cache RDDs and data frames at several stages in hello following procedures.</span></span>

    # CREATE FOUR BUCKETS FOR TRAFFIC TIMES
    val sqlStatement = """
        SELECT *,
        CASE
         WHEN (pickup_hour <= 6 OR pickup_hour >= 20) THEN "Night"
         WHEN (pickup_hour >= 7 AND pickup_hour <= 10) THEN "AMRush"
         WHEN (pickup_hour >= 11 AND pickup_hour <= 15) THEN "Afternoon"
         WHEN (pickup_hour >= 16 AND pickup_hour <= 19) THEN "PMRush"
        END as TrafficTimeBins
        FROM taxi_train
    """
    val taxi_df_train_with_newFeatures = sqlContext.sql(sqlStatement)

    # CACHE hello DATA FRAME IN MEMORY AND MATERIALIZE hello DATA FRAME IN MEMORY
    taxi_df_train_with_newFeatures.cache()
    taxi_df_train_with_newFeatures.count()


### <a name="indexing-and-one-hot-encoding-of-categorical-features"></a><span data-ttu-id="719bd-257">Indexering och en hot kodning av kategoriska funktioner</span><span class="sxs-lookup"><span data-stu-id="719bd-257">Indexing and one-hot encoding of categorical features</span></span>
<span data-ttu-id="719bd-258">Hej modellering och förutsäga funktioner för MLlib kräver funktioner med kategoriska indata toobe indexerade eller kodade tidigare toouse.</span><span class="sxs-lookup"><span data-stu-id="719bd-258">hello modeling and predict functions of MLlib require features with categorical input data toobe indexed or encoded prior toouse.</span></span> <span data-ttu-id="719bd-259">Det här avsnittet beskrivs hur du tooindex eller koda kategoriska funktioner för indata i hello modeling funktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-259">This section shows you how tooindex or encode categorical features for input into hello modeling functions.</span></span>

<span data-ttu-id="719bd-260">Du måste tooindex eller koda modeller på olika sätt beroende på hello modellen.</span><span class="sxs-lookup"><span data-stu-id="719bd-260">You need tooindex or encode your models in different ways, depending on hello model.</span></span> <span data-ttu-id="719bd-261">Till exempel kräva logistic och linjär regression modeller ett hot kodning.</span><span class="sxs-lookup"><span data-stu-id="719bd-261">For example, logistic and linear regression models require one-hot encoding.</span></span> <span data-ttu-id="719bd-262">Till exempel kan en funktion med tre kategorier expanderas i tre kolumner i funktionen.</span><span class="sxs-lookup"><span data-stu-id="719bd-262">For example, a feature with three categories can be expanded into three feature columns.</span></span> <span data-ttu-id="719bd-263">Varje kolumn innehåller 0 eller 1 beroende på hello kategori av en observationsintervallet.</span><span class="sxs-lookup"><span data-stu-id="719bd-263">Each column would contain 0 or 1 depending on hello category of an observation.</span></span> <span data-ttu-id="719bd-264">MLlib ger hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) funktionen för en hot kodning.</span><span class="sxs-lookup"><span data-stu-id="719bd-264">MLlib provides hello [OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder) function for one-hot encoding.</span></span> <span data-ttu-id="719bd-265">Den här kodaren mappar en kolumn med etiketten index tooa kolumn i binär angreppsmetoderna med högst ett ett-värde.</span><span class="sxs-lookup"><span data-stu-id="719bd-265">This encoder maps a column of label indices tooa column of binary vectors with at most a single one-value.</span></span> <span data-ttu-id="719bd-266">Med den här kodningen vara algoritmer som numeriska värden funktioner, till exempel logistic regression förväntas tillämpade toocategorical funktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-266">With this encoding, algorithms that expect numerical valued features, such as logistic regression, can be applied toocategorical features.</span></span>

<span data-ttu-id="719bd-267">Här kan du omvandla bara fyra variabler tooshow exempel som är strängar.</span><span class="sxs-lookup"><span data-stu-id="719bd-267">Here you transform only four variables tooshow examples, which are character strings.</span></span> <span data-ttu-id="719bd-268">Du kan också indexera andra variabler, till exempel veckodag representeras av numeriska värden som kategoriska variabler.</span><span class="sxs-lookup"><span data-stu-id="719bd-268">You also can index other variables, such as weekday, represented by numerical values, as categorical variables.</span></span>

<span data-ttu-id="719bd-269">Använd för indexering, `StringIndexer()`, och för ett hot encoding använder `OneHotEncoder()` funktioner från MLlib.</span><span class="sxs-lookup"><span data-stu-id="719bd-269">For indexing, use `StringIndexer()`, and for one-hot encoding, use `OneHotEncoder()` functions from MLlib.</span></span> <span data-ttu-id="719bd-270">Här är hello kod tooindex och koda kategoriska funktioner:</span><span class="sxs-lookup"><span data-stu-id="719bd-270">Here is hello code tooindex and encode categorical features:</span></span>

    # CREATE INDEXES AND ONE-HOT ENCODED VECTORS FOR SEVERAL CATEGORICAL FEATURES

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # INDEX AND ENCODE VENDOR_ID
    val stringIndexer = new StringIndexer().setInputCol("vendor_id").setOutputCol("vendorIndex").fit(taxi_df_train_with_newFeatures)
    val indexed = stringIndexer.transform(taxi_df_train_with_newFeatures)
    val encoder = new OneHotEncoder().setInputCol("vendorIndex").setOutputCol("vendorVec")
    val encoded1 = encoder.transform(indexed)

    # INDEX AND ENCODE RATE_CODE
    val stringIndexer = new StringIndexer().setInputCol("rate_code").setOutputCol("rateIndex").fit(encoded1)
    val indexed = stringIndexer.transform(encoded1)
    val encoder = new OneHotEncoder().setInputCol("rateIndex").setOutputCol("rateVec")
    val encoded2 = encoder.transform(indexed)

    # INDEX AND ENCODE PAYMENT_TYPE
    val stringIndexer = new StringIndexer().setInputCol("payment_type").setOutputCol("paymentIndex").fit(encoded2)
    val indexed = stringIndexer.transform(encoded2)
    val encoder = new OneHotEncoder().setInputCol("paymentIndex").setOutputCol("paymentVec")
    val encoded3 = encoder.transform(indexed)

    # INDEX AND TRAFFIC TIME BINS
    val stringIndexer = new StringIndexer().setInputCol("TrafficTimeBins").setOutputCol("TrafficTimeBinsIndex").fit(encoded3)
    val indexed = stringIndexer.transform(encoded3)
    val encoder = new OneHotEncoder().setInputCol("TrafficTimeBinsIndex").setOutputCol("TrafficTimeBinsVec")
    val encodedFinal = encoder.transform(indexed)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-271">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-271">**Output:**</span></span>

<span data-ttu-id="719bd-272">Tid toorun hello cell: 4 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-272">Time toorun hello cell: 4 seconds.</span></span>

### <a name="sample-and-split-hello-data-set-into-training-and-test-fractions"></a><span data-ttu-id="719bd-273">Exempel och dela hello datauppsättning till bråk träning och testning</span><span class="sxs-lookup"><span data-stu-id="719bd-273">Sample and split hello data set into training and test fractions</span></span>
<span data-ttu-id="719bd-274">Den här koden skapar ett slumpmässigt dataurval hello (i det här exemplet 25%).</span><span class="sxs-lookup"><span data-stu-id="719bd-274">This code creates a random sampling of hello data (25%, in this example).</span></span> <span data-ttu-id="719bd-275">Även om sampling inte krävs för det här exemplet på grund av toohello storlek på hello datauppsättning, hello artikeln lär du dig hur du kan prova så att du vet hur toouse den egna problem vid behov.</span><span class="sxs-lookup"><span data-stu-id="719bd-275">Although sampling is not required for this example due toohello size of hello data set, hello article shows you how you can sample so that you know how toouse it for your own problems when needed.</span></span> <span data-ttu-id="719bd-276">När prover är stort, kan detta spara mycket tid medan du träna modeller.</span><span class="sxs-lookup"><span data-stu-id="719bd-276">When samples are large, this can save significant time while you train models.</span></span> <span data-ttu-id="719bd-277">Dela sedan hello exempel i en utbildning (75% i det här exemplet) och en testnings-del (i det här exemplet 25%) toouse i klassificering och regressionen modeling.</span><span class="sxs-lookup"><span data-stu-id="719bd-277">Next, split hello sample into a training part (75%, in this example) and a testing part (25%, in this example) toouse in classification and regression modeling.</span></span>

<span data-ttu-id="719bd-278">Lägg till ett slumptal (mellan 0 och 1) tooeach rad (i en kolumn ”SLUMP”) som kan vara används tooselect korsvalidering vikningar vid träning.</span><span class="sxs-lookup"><span data-stu-id="719bd-278">Add a random number (between 0 and 1) tooeach row (in a "rand" column) that can be used tooselect cross-validation folds during training.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # SPECIFY SAMPLING AND SPLITTING FRACTIONS
    val samplingFraction = 0.25;
    val trainingFraction = 0.75;
    val testingFraction = (1-trainingFraction);
    val seed = 1234;
    val encodedFinalSampledTmp = encodedFinal.sample(withReplacement = false, fraction = samplingFraction, seed = seed)
    val sampledDFcount = encodedFinalSampledTmp.count().toInt

    val generateRandomDouble = udf(() => {
        scala.util.Random.nextDouble
    })

    # ADD A RANDOM NUMBER FOR CROSS-VALIDATION
    val encodedFinalSampled = encodedFinalSampledTmp.withColumn("rand", generateRandomDouble());

    # SPLIT hello SAMPLED DATA FRAME INTO TRAIN AND TEST, WITH A RANDOM COLUMN ADDED FOR DOING CROSS-VALIDATION (SHOWN LATER)
    # INCLUDE A RANDOM COLUMN FOR CREATING CROSS-VALIDATION FOLDS
    val splits = encodedFinalSampled.randomSplit(Array(trainingFraction, testingFraction), seed = seed)
    val trainData = splits(0)
    val testData = splits(1)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-279">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-279">**Output:**</span></span>

<span data-ttu-id="719bd-280">Tid toorun hello cell: 2 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-280">Time toorun hello cell: 2 seconds.</span></span>

### <a name="specify-training-variable-and-features-and-then-create-indexed-or-one-hot-encoded-training-and-testing-input-labeled-point-rdds-or-data-frames"></a><span data-ttu-id="719bd-281">Ange utbildning variabeln och funktioner, och skapa indexerade eller ett hot kodade träning och testning indata med etiketten punkt RDDs eller data ramar</span><span class="sxs-lookup"><span data-stu-id="719bd-281">Specify training variable and features, and then create indexed or one-hot encoded training and testing input labeled point RDDs or data frames</span></span>
<span data-ttu-id="719bd-282">Det här avsnittet innehåller kod som visar hur tooindex kategoriska textdata som en etikett peka datatyp och koda den så att du kan använda tootrain och testa MLlib logistic regression och andra klassificering modeller.</span><span class="sxs-lookup"><span data-stu-id="719bd-282">This section contains code that shows you how tooindex categorical text data as a labeled point data type, and encode it so you can use it tootrain and test MLlib logistic regression and other classification models.</span></span> <span data-ttu-id="719bd-283">Märkt point-objekt är RDDs som är formaterad på ett sätt som krävs av de flesta maskininlärningsalgoritmer i MLlib som indata.</span><span class="sxs-lookup"><span data-stu-id="719bd-283">Labeled point objects are RDDs that are formatted in a way that is needed as input data by most of machine learning algorithms in MLlib.</span></span> <span data-ttu-id="719bd-284">En [etikett punkt](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) är en lokal vector kompakta eller utspridd, som är associerade med en etikett/svar.</span><span class="sxs-lookup"><span data-stu-id="719bd-284">A [labeled point](https://spark.apache.org/docs/latest/mllib-data-types.html#labeled-point) is a local vector, either dense or sparse, associated with a label/response.</span></span>

<span data-ttu-id="719bd-285">I den här koden anger du hello (beroende) målvariabel och hello funktioner toouse tootrain modeller.</span><span class="sxs-lookup"><span data-stu-id="719bd-285">In this code, you specify hello target (dependent) variable and hello features toouse tootrain models.</span></span> <span data-ttu-id="719bd-286">Sedan kan skapa du indexerade eller ett hot kodade träning och testning indata med etiketten punkt RDDs eller data ramar.</span><span class="sxs-lookup"><span data-stu-id="719bd-286">Then, you create indexed or one-hot encoded training and testing input labeled point RDDs or data frames.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # MAP NAMES OF FEATURES AND TARGETS FOR CLASSIFICATION AND REGRESSION PROBLEMS
    val featuresIndOneHot = List("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))
    val featuresIndIndex = List("paymentIndex", "vendorIndex", "rateIndex", "TrafficTimeBinsIndex", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount").map(encodedFinalSampled.columns.indexOf(_))

    # SPECIFY hello TARGET FOR CLASSIFICATION ('tipped') AND REGRESSION ('tip_amount') PROBLEMS
    val targetIndBinary = List("tipped").map(encodedFinalSampled.columns.indexOf(_))
    val targetIndRegression = List("tip_amount").map(encodedFinalSampled.columns.indexOf(_))

    # CREATE INDEXED LABELED POINT RDD OBJECTS
    val indexedTRAINbinary = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTbinary = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndBinary(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTRAINreg = trainData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))
    val indexedTESTreg = testData.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)))

    # CREATE INDEXED DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val indexedTRAINbinaryDF = indexedTRAINbinary.toDF()
    val indexedTESTbinaryDF = indexedTESTbinary.toDF()
    val indexedTRAINregDF = indexedTRAINreg.toDF()
    val indexedTESTregDF = indexedTESTreg.toDF()

    # CREATE ONE-HOT ENCODED (VECTORIZED) DATA FRAMES THAT YOU CAN USE tooTRAIN BY USING SPARK ML FUNCTIONS
    val assemblerOneHot = new VectorAssembler().setInputCols(Array("paymentVec", "vendorVec", "rateVec", "TrafficTimeBinsVec", "pickup_hour", "weekday", "passenger_count", "trip_time_in_secs", "trip_distance", "fare_amount")).setOutputCol("features")
    val OneHotTRAIN = assemblerOneHot.transform(trainData)
    val OneHotTEST = assemblerOneHot.transform(testData)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-287">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-287">**Output:**</span></span>

<span data-ttu-id="719bd-288">Tid toorun hello cell: 4 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-288">Time toorun hello cell: 4 seconds.</span></span>

### <a name="automatically-categorize-and-vectorize-features-and-targets-toouse-as-inputs-for-machine-learning-models"></a><span data-ttu-id="719bd-289">Kategorisera och vectorize funktioner och mål toouse som indata för machine learning-modeller automatiskt</span><span class="sxs-lookup"><span data-stu-id="719bd-289">Automatically categorize and vectorize features and targets toouse as inputs for machine learning models</span></span>
<span data-ttu-id="719bd-290">Använda Spark ML toocategorize hello mål och funktioner toouse i trädet-baserade modelleringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-290">Use Spark ML toocategorize hello target and features toouse in tree-based modeling functions.</span></span> <span data-ttu-id="719bd-291">hello koden utför två aktiviteter:</span><span class="sxs-lookup"><span data-stu-id="719bd-291">hello code completes two tasks:</span></span>

* <span data-ttu-id="719bd-292">Skapar ett binärt mål för klassificering genom att tilldela värdet 0 eller 1 tooeach datapunkt mellan 0 och 1 med hjälp av ett tröskelvärde för 0,5.</span><span class="sxs-lookup"><span data-stu-id="719bd-292">Creates a binary target for classification by assigning a value of 0 or 1 tooeach data point between 0 and 1 by using a threshold value of 0.5.</span></span>
* <span data-ttu-id="719bd-293">Automatiskt kategoriserar funktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-293">Automatically categorizes features.</span></span> <span data-ttu-id="719bd-294">Om hello antalet distinkta numeriska värden för alla funktioner som är mindre än 32, kategoriseras den funktionen.</span><span class="sxs-lookup"><span data-stu-id="719bd-294">If hello number of distinct numerical values for any feature is less than 32, that feature is categorized.</span></span>

<span data-ttu-id="719bd-295">Här är hello koden för dessa två aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="719bd-295">Here's hello code for these two tasks.</span></span>

    # CATEGORIZE FEATURES AND BINARIZE hello TARGET FOR hello BINARY CLASSIFICATION PROBLEM

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTRAINbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTRAINwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTrainwithCatFeat = indexerModel.transform(indexedTESTbinaryDF)
    val binarizer: Binarizer = new Binarizer().setInputCol("label").setOutputCol("labelBin").setThreshold(0.5)
    val indexedTESTwithCatFeatBinTarget = binarizer.transform(indexedTrainwithCatFeat)

    # CATEGORIZE FEATURES FOR hello REGRESSION PROBLEM
    # CREATE PROPERLY INDEXED AND CATEGORIZED DATA FRAMES FOR TREE-BASED MODELS

    # TRAIN DATA
    val indexer = new VectorIndexer().setInputCol("features").setOutputCol("featuresCat").setMaxCategories(32)
    val indexerModel = indexer.fit(indexedTRAINregDF)
    val indexedTRAINwithCatFeat = indexerModel.transform(indexedTRAINregDF)

    # TEST DATA
    val indexerModel = indexer.fit(indexedTESTbinaryDF)
    val indexedTESTwithCatFeat = indexerModel.transform(indexedTESTregDF)



## <a name="binary-classification-model-predict-whether-a-tip-should-be-paid"></a><span data-ttu-id="719bd-296">Binär klassificering modell: förutsäga om tipsrutor ska användas för betalning</span><span class="sxs-lookup"><span data-stu-id="719bd-296">Binary classification model: Predict whether a tip should be paid</span></span>
<span data-ttu-id="719bd-297">I det här avsnittet kan du skapa tre olika typer av binär klassificering modeller toopredict huruvida ett tips betald:</span><span class="sxs-lookup"><span data-stu-id="719bd-297">In this section, you create three types of binary classification models toopredict whether or not a tip should be paid:</span></span>

* <span data-ttu-id="719bd-298">En **logistic regressionsmodell** med hjälp av hello Spark ML `LogisticRegression()` funktion</span><span class="sxs-lookup"><span data-stu-id="719bd-298">A **logistic regression model** by using hello Spark ML `LogisticRegression()` function</span></span>
* <span data-ttu-id="719bd-299">En **slumpmässiga klassificering Skogsmodell** med hjälp av hello Spark ML `RandomForestClassifier()` funktion</span><span class="sxs-lookup"><span data-stu-id="719bd-299">A **random forest classification model** by using hello Spark ML `RandomForestClassifier()` function</span></span>
* <span data-ttu-id="719bd-300">En **toning den trädet klassificering modellen** med hjälp av hello MLlib `GradientBoostedTrees()` funktion</span><span class="sxs-lookup"><span data-stu-id="719bd-300">A **gradient boosting tree classification model** by using hello MLlib `GradientBoostedTrees()` function</span></span>

### <a name="create-a-logistic-regression-model"></a><span data-ttu-id="719bd-301">Skapa en logistic regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="719bd-301">Create a logistic regression model</span></span>
<span data-ttu-id="719bd-302">Skapa sedan en logistic regressionsmodell med hjälp av hello Spark ML `LogisticRegression()` funktion.</span><span class="sxs-lookup"><span data-stu-id="719bd-302">Next, create a logistic regression model by using hello Spark ML `LogisticRegression()` function.</span></span> <span data-ttu-id="719bd-303">Du skapar hello modell skapa koden i ett antal steg:</span><span class="sxs-lookup"><span data-stu-id="719bd-303">You create hello model building code in a series of steps:</span></span>

1. <span data-ttu-id="719bd-304">**Hej träningsmodell** data med en enda parameter.</span><span class="sxs-lookup"><span data-stu-id="719bd-304">**Train hello model** data with one parameter set.</span></span>
2. <span data-ttu-id="719bd-305">**Utvärdera hello modellen** på en datauppsättning med test.</span><span class="sxs-lookup"><span data-stu-id="719bd-305">**Evaluate hello model** on a test data set with metrics.</span></span>
3. <span data-ttu-id="719bd-306">**Spara hello modell** i Blob storage för framtida användning.</span><span class="sxs-lookup"><span data-stu-id="719bd-306">**Save hello model** in Blob storage for future consumption.</span></span>
4. <span data-ttu-id="719bd-307">**Hej poängmodell** mot testdata.</span><span class="sxs-lookup"><span data-stu-id="719bd-307">**Score hello model** against test data.</span></span>
5. <span data-ttu-id="719bd-308">**Rita hello resultat** med operativsystemet kurvor egenskap (ROC).</span><span class="sxs-lookup"><span data-stu-id="719bd-308">**Plot hello results** with receiver operating characteristic (ROC) curves.</span></span>

<span data-ttu-id="719bd-309">Här är hello koden för dessa procedurer:</span><span class="sxs-lookup"><span data-stu-id="719bd-309">Here's hello code for these procedures:</span></span>

    # CREATE A LOGISTIC REGRESSION MODEL
    val lr = new LogisticRegression().setLabelCol("tipped").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)
    val lrModel = lr.fit(OneHotTRAIN)

    # PREDICT ON hello TEST DATA SET
    val predictions = lrModel.transform(OneHotTEST)

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)

    # SAVE hello MODEL
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LogisticRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

<span data-ttu-id="719bd-310">Läsa in poängsätta och spara hello resultat.</span><span class="sxs-lookup"><span data-stu-id="719bd-310">Load, score, and save hello results.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD hello SAVED MODEL AND SCORE hello TEST DATA SET
    val savedModel = org.apache.spark.ml.classification.LogisticRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON hello TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tipped","probability","rawPrediction")
    predictions.registerTempTable("testResults")

    # SELECT `BinaryClassificationEvaluator()` tooCOMPUTE hello TEST ERROR
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("tipped").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC RESULTS
    println("ROC on test data = " + ROC)


<span data-ttu-id="719bd-311">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-311">**Output:**</span></span>

<span data-ttu-id="719bd-312">ROC på testdata = 0.9827381497557599</span><span class="sxs-lookup"><span data-stu-id="719bd-312">ROC on test data = 0.9827381497557599</span></span>

<span data-ttu-id="719bd-313">Använda Python på lokala Pandas data ramar tooplot hello ROC-kurvan.</span><span class="sxs-lookup"><span data-stu-id="719bd-313">Use Python on local Pandas data frames tooplot hello ROC curve.</span></span>

    # QUERY hello RESULTS
    %%sql -q -o sqlResults
    SELECT tipped, probability from testResults


    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    %matplotlib inline
    from sklearn.metrics import roc_curve,auc

    sqlResults['probFloat'] = sqlResults.apply(lambda row: row['probability'].values()[0][1], axis=1)
    predictions_pddf = sqlResults[["tipped","probFloat"]]

    # PREDICT hello ROC CURVE
    # predictions_pddf = sqlResults.rename(columns={'_1': 'probability', 'tipped': 'label'})
    prob = predictions_pddf["probFloat"]
    fpr, tpr, thresholds = roc_curve(predictions_pddf['tipped'], prob, pos_label=1);
    roc_auc = auc(fpr, tpr)

    # PLOT hello ROC CURVE
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


<span data-ttu-id="719bd-314">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-314">**Output:**</span></span>

![Tips eller några tips ROC-kurvan](./media/machine-learning-data-science-process-scala-walkthrough/plot-roc-curve-tip-or-not.png)

### <a name="create-a-random-forest-classification-model"></a><span data-ttu-id="719bd-316">Skapa en modell för klassificering av slumpmässiga skog</span><span class="sxs-lookup"><span data-stu-id="719bd-316">Create a random forest classification model</span></span>
<span data-ttu-id="719bd-317">Skapa sedan en klassificering för slumpmässiga Skogsmodell med hello Spark ML `RandomForestClassifier()` fungera och sedan utvärdera hello modell på testdata.</span><span class="sxs-lookup"><span data-stu-id="719bd-317">Next, create a random forest classification model by using hello Spark ML `RandomForestClassifier()` function, and then evaluate hello model on test data.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE hello RANDOM FOREST CLASSIFIER MODEL
    val rf = new RandomForestClassifier().setLabelCol("labelBin").setFeaturesCol("featuresCat").setNumTrees(10).setSeed(1234)

    # FIT hello MODEL
    val rfModel = rf.fit(indexedTRAINwithCatFeatBinTarget)
    val predictions = rfModel.transform(indexedTESTwithCatFeatBinTarget)

    # EVALUATE hello MODEL
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(predictions)
    println("F1 score on test data: " + Test_f1Score);

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # CALCULATE BINARY CLASSIFICATION EVALUATION METRICS
    val evaluator = new BinaryClassificationEvaluator().setLabelCol("label").setRawPredictionCol("probability").setMetricName("areaUnderROC")
    val ROC = evaluator.evaluate(predictions)
    println("ROC on test data = " + ROC)


<span data-ttu-id="719bd-318">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-318">**Output:**</span></span>

<span data-ttu-id="719bd-319">ROC på testdata = 0.9847103571552683</span><span class="sxs-lookup"><span data-stu-id="719bd-319">ROC on test data = 0.9847103571552683</span></span>

### <a name="create-a-gbt-classification-model"></a><span data-ttu-id="719bd-320">Skapa en modell för klassificering av GBT</span><span class="sxs-lookup"><span data-stu-id="719bd-320">Create a GBT classification model</span></span>
<span data-ttu-id="719bd-321">Skapa sedan en modell för klassificering av GBT med hjälp av Mllib's `GradientBoostedTrees()` fungera och sedan utvärdera hello modell på testdata.</span><span class="sxs-lookup"><span data-stu-id="719bd-321">Next, create a GBT classification model by using MLlib's `GradientBoostedTrees()` function, and then evaluate hello model on test data.</span></span>

    # TRAIN A GBT CLASSIFICATION MODEL BY USING MLLIB AND A LABELED POINT

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello GBT CLASSIFICATION MODEL
    val boostingStrategy = BoostingStrategy.defaultParams("Classification")
    boostingStrategy.numIterations = 20
    boostingStrategy.treeStrategy.numClasses = 2
    boostingStrategy.treeStrategy.maxDepth = 5
    boostingStrategy.treeStrategy.categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    # TRAIN hello MODEL
    val gbtModel = GradientBoostedTrees.train(indexedTRAINbinary, boostingStrategy)

    # SAVE hello MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "GBT_Classification__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    gbtModel.save(sc, filename);

    # EVALUATE hello MODEL ON TEST INSTANCES AND hello COMPUTE TEST ERROR
    val labelAndPreds = indexedTESTbinary.map { point =>
      val prediction = gbtModel.predict(point.features)
      (point.label, prediction)
    }
    val testErr = labelAndPreds.filter(r => r._1 != r._2).count.toDouble / indexedTRAINbinary.count()
    //println("Learned classification GBT model:\n" + gbtModel.toDebugString)
    println("Test Error = " + testErr)

    # USE BINARY AND MULTICLASS METRICS tooEVALUATE hello MODEL ON hello TEST DATA
    val metrics = new MulticlassMetrics(labelAndPreds)
    println(s"Precision: ${metrics.precision}")
    println(s"Recall: ${metrics.recall}")
    println(s"F1 Score: ${metrics.fMeasure}")

    val metrics = new BinaryClassificationMetrics(labelAndPreds)
    println(s"Area under PR curve: ${metrics.areaUnderPR}")
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello ROC METRIC
    println(s"Area under ROC curve: ${metrics.areaUnderROC}")


<span data-ttu-id="719bd-322">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-322">**Output:**</span></span>

<span data-ttu-id="719bd-323">Området under ROC-kurvan: 0.9846895479241554</span><span class="sxs-lookup"><span data-stu-id="719bd-323">Area under ROC curve: 0.9846895479241554</span></span>

## <a name="regression-model-predict-tip-amount"></a><span data-ttu-id="719bd-324">Regressionsmodell: förutsäga tips belopp</span><span class="sxs-lookup"><span data-stu-id="719bd-324">Regression model: Predict tip amount</span></span>
<span data-ttu-id="719bd-325">I det här avsnittet kan du skapa två typer av regression modeller toopredict hello tips:</span><span class="sxs-lookup"><span data-stu-id="719bd-325">In this section, you create two types of regression models toopredict hello tip amount:</span></span>

* <span data-ttu-id="719bd-326">En **reglerats linjär regressionsmodell** med hjälp av hello Spark ML `LinearRegression()` funktion.</span><span class="sxs-lookup"><span data-stu-id="719bd-326">A **regularized linear regression model** by using hello Spark ML `LinearRegression()` function.</span></span> <span data-ttu-id="719bd-327">Du måste spara hello modell och utvärdera hello modell på testdata.</span><span class="sxs-lookup"><span data-stu-id="719bd-327">You'll save hello model and evaluate hello model on test data.</span></span>
* <span data-ttu-id="719bd-328">En **toningen förstärkning trädet regressionsmodell** med hjälp av hello Spark ML `GBTRegressor()` funktion.</span><span class="sxs-lookup"><span data-stu-id="719bd-328">A **gradient-boosting tree regression model** by using hello Spark ML `GBTRegressor()` function.</span></span>

### <a name="create-a-regularized-linear-regression-model"></a><span data-ttu-id="719bd-329">Skapa en reglerats linjär regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="719bd-329">Create a regularized linear regression model</span></span>
    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE A REGULARIZED LINEAR REGRESSION MODEL BY USING hello SPARK ML FUNCTION AND DATA FRAMES
    val lr = new LinearRegression().setLabelCol("tip_amount").setFeaturesCol("features").setMaxIter(10).setRegParam(0.3).setElasticNetParam(0.8)

    # FIT hello MODEL BY USING DATA FRAMES
    val lrModel = lr.fit(OneHotTRAIN)
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SUMMARIZE hello MODEL OVER hello TRAINING SET AND PRINT METRICS
    val trainingSummary = lrModel.summary
    println(s"numIterations: ${trainingSummary.totalIterations}")
    println(s"objectiveHistory: ${trainingSummary.objectiveHistory.toList}")
    trainingSummary.residuals.show()
    println(s"RMSE: ${trainingSummary.rootMeanSquaredError}")
    println(s"r2: ${trainingSummary.r2}")

    # SAVE hello MODEL IN AZURE BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "LinearRegression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    lrModel.save(filename);

    # PRINT hello COEFFICIENTS
    println(s"Coefficients: ${lrModel.coefficients} Intercept: ${lrModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = lrModel.transform(OneHotTEST)

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-330">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-330">**Output:**</span></span>

<span data-ttu-id="719bd-331">Tid toorun hello cell: 13 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-331">Time toorun hello cell: 13 seconds.</span></span>

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM BLOB STORAGE AND SCORE A TEST DATA SET

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # LOAD A SAVED LINEAR REGRESSION MODEL FROM AZURE BLOB STORAGE
    val savedModel = org.apache.spark.ml.regression.LinearRegressionModel.load(filename)
    println(s"Coefficients: ${savedModel.coefficients} Intercept: ${savedModel.intercept}")

    # SCORE hello MODEL ON TEST DATA
    val predictions = savedModel.transform(OneHotTEST).select("tip_amount","prediction")
    predictions.registerTempTable("testResults")

    # EVALUATE hello MODEL ON TEST DATA
    val evaluator = new RegressionEvaluator().setLabelCol("tip_amount").setPredictionCol("prediction").setMetricName("r2")
    val r2 = evaluator.evaluate(predictions)
    println("R-sqr on test data = " + r2)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("R-sqr on test data = " + r2)


<span data-ttu-id="719bd-332">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-332">**Output:**</span></span>

<span data-ttu-id="719bd-333">R-sqr på testdata = 0.5960320470835743</span><span class="sxs-lookup"><span data-stu-id="719bd-333">R-sqr on test data = 0.5960320470835743</span></span>

<span data-ttu-id="719bd-334">Nästa fråga hello test resulterar som en data- och Använd AutoVizWidget och matplotlib toovisualize den.</span><span class="sxs-lookup"><span data-stu-id="719bd-334">Next, query hello test results as a data frame and use AutoVizWidget and matplotlib toovisualize it.</span></span>

    # RUN A SQL QUERY
    %%sql -q -o sqlResults
    select * from testResults

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER
    %%local

    # USE hello JUPYTER AUTO-PLOTTING FEATURE tooCREATE INTERACTIVE FIGURES
    # CLICK hello TYPE OF PLOT tooGENERATE (LINE, AREA, BAR, AND SO ON)
    sqlResults

<span data-ttu-id="719bd-335">hello kod skapas en lokala data från hello frågeresultatet och visar hello data.</span><span class="sxs-lookup"><span data-stu-id="719bd-335">hello code creates a local data frame from hello query output and plots hello data.</span></span> <span data-ttu-id="719bd-336">Hej `%%local` magic skapar en lokala data ram `sqlResults`, som du kan använda tooplot med matplotlib.</span><span class="sxs-lookup"><span data-stu-id="719bd-336">hello `%%local` magic creates a local data frame, `sqlResults`, which you can use tooplot with matplotlib.</span></span>

> [!NOTE]
> <span data-ttu-id="719bd-337">Den här Spark magic används flera gånger i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="719bd-337">This Spark magic is used multiple times in this article.</span></span> <span data-ttu-id="719bd-338">Om hello mängden data är stor, bör du prov toocreate en data-ram som får plats i lokalt minne.</span><span class="sxs-lookup"><span data-stu-id="719bd-338">If hello amount of data is large, you should sample toocreate a data frame that can fit in local memory.</span></span>
> 
> 

<span data-ttu-id="719bd-339">Skapa områden med Python matplotlib.</span><span class="sxs-lookup"><span data-stu-id="719bd-339">Create plots by using Python matplotlib.</span></span>

    # RUN hello CODE LOCALLY ON hello JUPYTER SERVER AND IMPORT LIBRARIES
    %%local
    sqlResults
    %matplotlib inline
    import numpy as np

    # PLOT hello RESULTS
    ax = sqlResults.plot(kind='scatter', figsize = (6,6), x='tip_amount', y='prediction', color='blue', alpha = 0.25, label='Actual vs. predicted');
    fit = np.polyfit(sqlResults['tip_amount'], sqlResults['prediction'], deg=1)
    ax.set_title('Actual vs. Predicted Tip Amounts ($)')
    ax.set_xlabel("Actual")
    ax.set_ylabel("Predicted")
    #ax.plot(sqlResults['tip_amount'], fit[0] * sqlResults['prediction'] + fit[1], color='magenta')
    plt.axis([-1, 15, -1, 8])
    plt.show(ax)

<span data-ttu-id="719bd-340">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-340">**Output:**</span></span>

![Tips belopp: faktiska kontra förväntade](./media/machine-learning-data-science-process-scala-walkthrough/plot-actual-vs-predicted-tip-amount.png)

### <a name="create-a-gbt-regression-model"></a><span data-ttu-id="719bd-342">Skapa en GBT regressionsmodell</span><span class="sxs-lookup"><span data-stu-id="719bd-342">Create a GBT regression model</span></span>
<span data-ttu-id="719bd-343">Skapa en GBT regressionsmodell med hjälp av hello Spark ML `GBTRegressor()` fungera och sedan utvärdera hello modell på testdata.</span><span class="sxs-lookup"><span data-stu-id="719bd-343">Create a GBT regression model by using hello Spark ML `GBTRegressor()` function, and then evaluate hello model on test data.</span></span>

<span data-ttu-id="719bd-344">[Ökat toningen träd](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) är ensembler av beslutsträd.</span><span class="sxs-lookup"><span data-stu-id="719bd-344">[Gradient-boosted trees](http://spark.apache.org/docs/latest/ml-classification-regression.html#gradient-boosted-trees-gbts) (GBTs) are ensembles of decision trees.</span></span> <span data-ttu-id="719bd-345">GBTs train beslut träd upprepade gånger toominimize en förlust-funktion.</span><span class="sxs-lookup"><span data-stu-id="719bd-345">GBTs train decision trees iteratively toominimize a loss function.</span></span> <span data-ttu-id="719bd-346">Du kan använda GBTs för regression och klassificering.</span><span class="sxs-lookup"><span data-stu-id="719bd-346">You can use GBTs for regression and classification.</span></span> <span data-ttu-id="719bd-347">De kan hantera kategoriska funktioner kräver inte funktionen skalning och kan avbilda nonlinearities och funktionen interaktioner.</span><span class="sxs-lookup"><span data-stu-id="719bd-347">They can handle categorical features, do not require feature scaling, and can capture nonlinearities and feature interactions.</span></span> <span data-ttu-id="719bd-348">Du kan också använda dem i en multiclass klassificering.</span><span class="sxs-lookup"><span data-stu-id="719bd-348">You also can use them in a multiclass-classification setting.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # TRAIN A GBT REGRESSION MODEL
    val gbt = new GBTRegressor().setLabelCol("label").setFeaturesCol("featuresCat").setMaxIter(10)
    val gbtModel = gbt.fit(indexedTRAINwithCatFeat)

    # MAKE PREDICTIONS
    val predictions = gbtModel.transform(indexedTESTwithCatFeat)

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(predictions)


    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    # PRINT hello RESULTS
    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="719bd-349">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-349">**Output:**</span></span>

<span data-ttu-id="719bd-350">Testa R-sqr är: 0.7655383534596654</span><span class="sxs-lookup"><span data-stu-id="719bd-350">Test R-sqr is: 0.7655383534596654</span></span>

## <a name="advanced-modeling-utilities-for-optimization"></a><span data-ttu-id="719bd-351">Avancerade modellering verktyg för optimering</span><span class="sxs-lookup"><span data-stu-id="719bd-351">Advanced modeling utilities for optimization</span></span>
<span data-ttu-id="719bd-352">I det här avsnittet använder du machine learning verktyg som utvecklare använder ofta för optimering av modellen.</span><span class="sxs-lookup"><span data-stu-id="719bd-352">In this section, you use machine learning utilities that developers frequently use for model optimization.</span></span> <span data-ttu-id="719bd-353">Mer specifikt kan du optimera machine learning-modeller tre olika sätt med hjälp av parametern omfattande och korsvalidering:</span><span class="sxs-lookup"><span data-stu-id="719bd-353">Specifically, you can optimize machine learning models three different ways by using parameter sweeping and cross-validation:</span></span>

* <span data-ttu-id="719bd-354">Dela hello data i tåg och validering uppsättningar, optimera hello modellen med hjälp av hyper-parametern omfattande på en träningsmängden och utvärdera en verifiering mängd (linjär regression)</span><span class="sxs-lookup"><span data-stu-id="719bd-354">Split hello data into train and validation sets, optimize hello model by using hyper-parameter sweeping on a training set, and evaluate on a validation set (linear regression)</span></span>
* <span data-ttu-id="719bd-355">Optimera hello modellen med hjälp av korsvalidering och hyper-parametern omfattande med funktionen Spark ML CrossValidator (binär klassificering)</span><span class="sxs-lookup"><span data-stu-id="719bd-355">Optimize hello model by using cross-validation and hyper-parameter sweeping by using Spark ML's CrossValidator function (binary classification)</span></span>
* <span data-ttu-id="719bd-356">Optimera hello modellen med hjälp av anpassad kod för korsvalidering och parametern omfattande toouse alla maskininlärning funktion och parametern uppsättning (linjär regression)</span><span class="sxs-lookup"><span data-stu-id="719bd-356">Optimize hello model by using custom cross-validation and parameter-sweeping code toouse any machine learning function and parameter set (linear regression)</span></span>

<span data-ttu-id="719bd-357">**Korsvalidering** är en teknik som utvärderar hur väl en modell med en känd uppsättning data kommer att generalisera toopredict hello funktioner för datauppsättningar som den inte har tränats.</span><span class="sxs-lookup"><span data-stu-id="719bd-357">**Cross-validation** is a technique that assesses how well a model trained on a known set of data will generalize toopredict hello features of data sets on which it has not been trained.</span></span> <span data-ttu-id="719bd-358">hello är allmän uppfattning bakom den här metoden att en modell tränas på en datauppsättning med kända data, och sedan hello korrektheten i dess förutsägelser kontrolleras mot en oberoende datamängd.</span><span class="sxs-lookup"><span data-stu-id="719bd-358">hello general idea behind this technique is that a model is trained on a data set of known data, and then hello accuracy of its predictions is tested against an independent data set.</span></span> <span data-ttu-id="719bd-359">En vanlig tillämpning är toodivide en datamängd i *k*-vikningar och träna hello modell i ett resursallokering sätt på alla utom en av hello vikningar.</span><span class="sxs-lookup"><span data-stu-id="719bd-359">A common implementation is toodivide a data set into *k*-folds, and then train hello model in a round-robin fashion on all but one of hello folds.</span></span>

<span data-ttu-id="719bd-360">**Hyper-parametern optimering** hello problem med att välja en uppsättning hyper-parametrar för en inlärningsalgoritm normalt med hello målet av optimering av ett mått på hello algoritmen prestanda på en fristående uppsättning data.</span><span class="sxs-lookup"><span data-stu-id="719bd-360">**Hyper-parameter optimization** is hello problem of choosing a set of hyper-parameters for a learning algorithm, usually with hello goal of optimizing a measure of hello algorithm's performance on an independent data set.</span></span> <span data-ttu-id="719bd-361">Hyper-parametern är ett värde som du måste ange utanför hello modellen utbildning procedur.</span><span class="sxs-lookup"><span data-stu-id="719bd-361">A hyper-parameter is a value that you must specify outside hello model training procedure.</span></span> <span data-ttu-id="719bd-362">Antaganden om hyper-parametervärden kan påverka hello flexibilitet och korrektheten i hello modellen.</span><span class="sxs-lookup"><span data-stu-id="719bd-362">Assumptions about hyper-parameter values can affect hello flexibility and accuracy of hello model.</span></span> <span data-ttu-id="719bd-363">Beslutsträd har hyper-parametrar, till exempel som hello önskad djup och många blad i hello-trädet.</span><span class="sxs-lookup"><span data-stu-id="719bd-363">Decision trees have hyper-parameters, for example, such as hello desired depth and number of leaves in hello tree.</span></span> <span data-ttu-id="719bd-364">Du måste ange en felklassificering påföljder term för en support vector machine (SVM).</span><span class="sxs-lookup"><span data-stu-id="719bd-364">You must set a misclassification penalty term for a support vector machine (SVM).</span></span>

<span data-ttu-id="719bd-365">Ett vanligt sätt tooperform hyper parametern optimering är toouse en rutnätet sökning, kallas även en **parametern Svep**.</span><span class="sxs-lookup"><span data-stu-id="719bd-365">A common way tooperform hyper-parameter optimization is toouse a grid search, also called a **parameter sweep**.</span></span> <span data-ttu-id="719bd-366">I ett rutnät sökning utförs en fullständig sökning via hello värdena för en angiven delmängd hello hyper parametern utrymme för en inlärningsalgoritm.</span><span class="sxs-lookup"><span data-stu-id="719bd-366">In a grid search, an exhaustive search is performed through hello values of a specified subset of hello hyper-parameter space for a learning algorithm.</span></span> <span data-ttu-id="719bd-367">Korsvalidering kan tillhandahålla en prestanda mått toosort ut hello bästa möjliga resultat som produceras av hello rutnätet Sök algoritm.</span><span class="sxs-lookup"><span data-stu-id="719bd-367">Cross-validation can supply a performance metric toosort out hello optimal results produced by hello grid search algorithm.</span></span> <span data-ttu-id="719bd-368">Om du använder korsvalidering hyper parametern omfattande, kan du hjälpa gränsen problem angående overfitting en tootraining modelldata.</span><span class="sxs-lookup"><span data-stu-id="719bd-368">If you use cross-validation hyper-parameter sweeping, you can help limit problems like overfitting a model tootraining data.</span></span> <span data-ttu-id="719bd-369">Det här sättet hello modellen behåller hello kapacitet tooapply toohello allmän uppsättning data från vilken hello träningsdata har extraherats.</span><span class="sxs-lookup"><span data-stu-id="719bd-369">This way, hello model retains hello capacity tooapply toohello general set of data from which hello training data was extracted.</span></span>

### <a name="optimize-a-linear-regression-model-with-hyper-parameter-sweeping"></a><span data-ttu-id="719bd-370">Optimera en linjär regressionsmodell med hyper-parametern omfattande</span><span class="sxs-lookup"><span data-stu-id="719bd-370">Optimize a linear regression model with hyper-parameter sweeping</span></span>
<span data-ttu-id="719bd-371">Därefter dela in data i tåg och validering uppsättningar, Använd hyper-parametern omfattande på en utbildning ange toooptimize hello modellen och utvärdera en verifiering mängd (linjär regression).</span><span class="sxs-lookup"><span data-stu-id="719bd-371">Next, split data into train and validation sets, use hyper-parameter sweeping on a training set toooptimize hello model, and evaluate on a validation set (linear regression).</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # RENAME `tip_amount` AS A LABEL
    val OneHotTRAINLabeled = OneHotTRAIN.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    val OneHotTESTLabeled = OneHotTEST.select("tip_amount","features").withColumnRenamed(existingName="tip_amount",newName="label")
    OneHotTRAINLabeled.cache()
    OneHotTESTLabeled.cache()

    # DEFINE hello ESTIMATOR FUNCTION: `hello LinearRegression()` FUNCTION
    val lr = new LinearRegression().setLabelCol("label").setFeaturesCol("features").setMaxIter(10)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(lr.regParam, Array(0.1, 0.01, 0.001)).addGrid(lr.fitIntercept).addGrid(lr.elasticNetParam, Array(0.1, 0.5, 0.9)).build()

    # DEFINE hello PIPELINE WITH A TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET), AND THEN hello SPECIFY ESTIMATOR, EVALUATOR, AND PARAMETER GRID
    val trainPct = 0.75
    val trainValidationSplit = new TrainValidationSplit().setEstimator(lr).setEvaluator(new RegressionEvaluator).setEstimatorParamMaps(paramGrid).setTrainRatio(trainPct)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = trainValidationSplit.fit(OneHotTRAINLabeled)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(OneHotTESTLabeled).select("label", "prediction")

    # COMPUTE TEST SET R2
    val evaluator = new RegressionEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("r2")
    val Test_R2 = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");

    println("Test R-sqr is: " + Test_R2);


<span data-ttu-id="719bd-372">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-372">**Output:**</span></span>

<span data-ttu-id="719bd-373">Testa R-sqr är: 0.6226484708501209</span><span class="sxs-lookup"><span data-stu-id="719bd-373">Test R-sqr is: 0.6226484708501209</span></span>

### <a name="optimize-hello-binary-classification-model-by-using-cross-validation-and-hyper-parameter-sweeping"></a><span data-ttu-id="719bd-374">Optimera hello binär klassificering modellen med hjälp av korsvalidering och hyper-parametern omfattande</span><span class="sxs-lookup"><span data-stu-id="719bd-374">Optimize hello binary classification model by using cross-validation and hyper-parameter sweeping</span></span>
<span data-ttu-id="719bd-375">Det här avsnittet visar hur toooptimize en binär klassificering modellen med hjälp av omfattande korsvalidering och hyper-parametern.</span><span class="sxs-lookup"><span data-stu-id="719bd-375">This section shows you how toooptimize a binary classification model by using cross-validation and hyper-parameter sweeping.</span></span> <span data-ttu-id="719bd-376">Här används hello Spark ML `CrossValidator` funktion.</span><span class="sxs-lookup"><span data-stu-id="719bd-376">This uses hello Spark ML `CrossValidator` function.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # CREATE DATA FRAMES WITH PROPERLY LABELED COLUMNS tooUSE WITH hello TRAIN AND TEST SPLIT
    val indexedTRAINwithCatFeatBinTargetRF = indexedTRAINwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    val indexedTESTwithCatFeatBinTargetRF = indexedTESTwithCatFeatBinTarget.select("labelBin","featuresCat").withColumnRenamed(existingName="labelBin",newName="label").withColumnRenamed(existingName="featuresCat",newName="features")
    indexedTRAINwithCatFeatBinTargetRF.cache()
    indexedTESTwithCatFeatBinTargetRF.cache()

    # DEFINE hello ESTIMATOR FUNCTION
    val rf = new RandomForestClassifier().setLabelCol("label").setFeaturesCol("features").setImpurity("gini").setSeed(1234).setFeatureSubsetStrategy("auto").setMaxBins(32)

    # DEFINE hello PARAMETER GRID
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(4,8)).addGrid(rf.numTrees, Array(5,10)).addGrid(rf.minInstancesPerNode, Array(100,300)).build()

    # SPECIFY hello NUMBER OF FOLDS
    val numFolds = 3

    # DEFINE hello TRAIN/TEST VALIDATION SPLIT (75% IN hello TRAINING SET)
    val CrossValidator = new CrossValidator().setEstimator(rf).setEvaluator(new BinaryClassificationEvaluator).setEstimatorParamMaps(paramGrid).setNumFolds(numFolds)

    # RUN hello TRAIN VALIDATION SPLIT AND CHOOSE hello BEST SET OF PARAMETERS
    val model = CrossValidator.fit(indexedTRAINwithCatFeatBinTargetRF)

    # MAKE PREDICTIONS ON hello TEST DATA BY USING hello MODEL WITH hello COMBINATION OF PARAMETERS THAT PERFORMS hello BEST
    val testResults = model.transform(indexedTESTwithCatFeatBinTargetRF).select("label", "prediction")

    # COMPUTE hello TEST F1 SCORE
    val evaluator = new MulticlassClassificationEvaluator().setLabelCol("label").setPredictionCol("prediction").setMetricName("f1")
    val Test_f1Score = evaluator.evaluate(testResults)

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


<span data-ttu-id="719bd-377">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-377">**Output:**</span></span>

<span data-ttu-id="719bd-378">Tid toorun hello cell: 33 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-378">Time toorun hello cell: 33 seconds.</span></span>

### <a name="optimize-hello-linear-regression-model-by-using-custom-cross-validation-and-parameter-sweeping-code"></a><span data-ttu-id="719bd-379">Optimera hello linjär regressionsmodell med hjälp av anpassade korsvalidering och parametern omfattande kod</span><span class="sxs-lookup"><span data-stu-id="719bd-379">Optimize hello linear regression model by using custom cross-validation and parameter-sweeping code</span></span>
<span data-ttu-id="719bd-380">Sedan optimera hello modellen med hjälp av anpassade kod och identifiera hello bästa modellparametrarna med hjälp av hello kriterium högsta noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="719bd-380">Next, optimize hello model by using custom code, and identify hello best model parameters by using hello criterion of highest accuracy.</span></span> <span data-ttu-id="719bd-381">Skapa sedan hello sista modellen, utvärdera hello modell på testdata och spara hello modell i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="719bd-381">Then, create hello final model, evaluate hello model on test data, and save hello model in Blob storage.</span></span> <span data-ttu-id="719bd-382">Slutligen ladda hello modell poängsätta testdata och utvärdera noggrannhet.</span><span class="sxs-lookup"><span data-stu-id="719bd-382">Finally, load hello model, score test data, and evaluate accuracy.</span></span>

    # RECORD hello START TIME
    val starttime = Calendar.getInstance().getTime()

    # DEFINE hello PARAMETER GRID AND hello NUMBER OF FOLDS
    val paramGrid = new ParamGridBuilder().addGrid(rf.maxDepth, Array(5,10)).addGrid(rf.numTrees, Array(10,25,50)).build()

    val nFolds = 3
    val numModels = paramGrid.size
    val numParamsinGrid = 2

    # SPECIFY hello NUMBER OF CATEGORIES FOR CATEGORICAL VARIABLES
    val categoricalFeaturesInfo = Map[Int, Int]((0,2),(1,2),(2,6),(3,4))

    var maxDepth = -1
    var numTrees = -1
    var param = ""
    var paramval = -1
    var validateLB = -1.0
    var validateUB = -1.0
    val h = 1.0 / nFolds;
    val RMSE  = Array.fill(numModels)(0.0)

    # CREATE K-FOLDS
    val splits = MLUtils.kFold(indexedTRAINbinary, numFolds = nFolds, seed=1234)


    # LOOP THROUGH K-FOLDS AND hello PARAMETER GRID tooGET AND IDENTIFY hello BEST PARAMETER SET BY LEVEL OF ACCURACY
    for (i <- 0 too(nFolds-1)) {
        validateLB = i * h
        validateUB = (i + 1) * h
        val validationCV = trainData.filter($"rand" >= validateLB  && $"rand" < validateUB)
        val trainCV = trainData.filter($"rand" < validateLB  || $"rand" >= validateUB)
        val validationLabPt = validationCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        val trainCVLabPt = trainCV.rdd.map(r => LabeledPoint(r.getDouble(targetIndRegression(0).toInt), Vectors.dense(featuresIndIndex.map(r.getDouble(_)).toArray)));
        validationLabPt.cache()
        trainCVLabPt.cache()

        for (nParamSets <- 0 too(numModels-1)) {
            for (nParams <- 0 too(numParamsinGrid-1)) {
                param = paramGrid(nParamSets).toSeq(nParams).param.toString.split("__")(1)
                paramval = paramGrid(nParamSets).toSeq(nParams).value.toString.toInt
                if (param == "maxDepth") {maxDepth = paramval}
                if (param == "numTrees") {numTrees = paramval}
            }
            val rfModel = RandomForest.trainRegressor(trainCVLabPt, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=numTrees, maxDepth=maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)
            val labelAndPreds = validationLabPt.map { point =>
                                                     val prediction = rfModel.predict(point.features)
                                                     ( prediction, point.label )
                                                    }
            val validMetrics = new RegressionMetrics(labelAndPreds)
            val rmse = validMetrics.rootMeanSquaredError
            RMSE(nParamSets) += rmse
        }
        validationLabPt.unpersist();
        trainCVLabPt.unpersist();
    }
    val minRMSEindex = RMSE.indexOf(RMSE.min)

    # GET hello BEST PARAMETERS FROM A CROSS-VALIDATION AND PARAMETER SWEEP
    var best_maxDepth = -1
    var best_numTrees = -1
    for (nParams <- 0 too(numParamsinGrid-1)) {
        param = paramGrid(minRMSEindex).toSeq(nParams).param.toString.split("__")(1)
        paramval = paramGrid(minRMSEindex).toSeq(nParams).value.toString.toInt
        if (param == "maxDepth") {best_maxDepth = paramval}
        if (param == "numTrees") {best_numTrees = paramval}
    }

    # CREATE hello BEST MODEL WITH hello BEST PARAMETERS AND A FULL TRAINING DATA SET
    val best_rfModel = RandomForest.trainRegressor(indexedTRAINreg, categoricalFeaturesInfo=categoricalFeaturesInfo,
                                                      numTrees=best_numTrees, maxDepth=best_maxDepth,
                                                      featureSubsetStrategy="auto",impurity="variance", maxBins=32)

    # SAVE hello BEST RANDOM FOREST MODEL IN BLOB STORAGE
    val datestamp = Calendar.getInstance().getTime().toString.replaceAll(" ", ".").replaceAll(":", "_");
    val modelName = "BestCV_RF_Regression__"
    val filename = modelDir.concat(modelName).concat(datestamp)
    best_rfModel.save(sc, filename);

    # PREDICT ON hello TRAINING SET WITH hello BEST MODEL AND THEN EVALUATE
    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = best_rfModel.predict(point.features)
                                            ( prediction, point.label )
                                           }

    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2

    # GET hello TIME tooRUN hello CELL
    val endtime = Calendar.getInstance().getTime()
    val elapsedtime =  ((endtime.getTime() - starttime.getTime())/1000).toString;
    println("Time taken toorun hello above cell: " + elapsedtime + " seconds.");


    # LOAD hello MODEL
    val savedRFModel = RandomForestModel.load(sc, filename)

    val labelAndPreds = indexedTESTreg.map { point =>
                                            val prediction = savedRFModel.predict(point.features)
                                            ( prediction, point.label )
                                           }
    # TEST hello MODEL
    val test_rmse = new RegressionMetrics(labelAndPreds).rootMeanSquaredError
    val test_rsqr = new RegressionMetrics(labelAndPreds).r2


<span data-ttu-id="719bd-383">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="719bd-383">**Output:**</span></span>

<span data-ttu-id="719bd-384">Tid toorun hello cell: 61 sekunder.</span><span class="sxs-lookup"><span data-stu-id="719bd-384">Time toorun hello cell: 61 seconds.</span></span>

## <a name="consume-spark-built-machine-learning-models-automatically-with-scala"></a><span data-ttu-id="719bd-385">Använda Spark-inbyggda machine learning-modeller automatiskt med Scala</span><span class="sxs-lookup"><span data-stu-id="719bd-385">Consume Spark-built machine learning models automatically with Scala</span></span>
<span data-ttu-id="719bd-386">En översikt över ämnen som vägleder dig genom hello aktiviteter som utgör hello datavetenskap processen i Azure finns [Team datavetenskap Process](http://aka.ms/datascienceprocess).</span><span class="sxs-lookup"><span data-stu-id="719bd-386">For an overview of topics that walk you through hello tasks that comprise hello Data Science process in Azure, see [Team Data Science Process](http://aka.ms/datascienceprocess).</span></span>

<span data-ttu-id="719bd-387">[Teamet datavetenskap Process genomgång](data-science-process-walkthroughs.md) beskriver andra slutpunkt till slutpunkt-genomgång som visar hello stegen i hello Team datavetenskap Process för specifika scenarier.</span><span class="sxs-lookup"><span data-stu-id="719bd-387">[Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) describes other end-to-end walkthroughs that demonstrate hello steps in hello Team Data Science Process for specific scenarios.</span></span> <span data-ttu-id="719bd-388">hello genomgång också illustrerar hur toocombine molnet och lokala verktyg och tjänster i ett arbetsflöde eller pipeline toocreate ett intelligent program.</span><span class="sxs-lookup"><span data-stu-id="719bd-388">hello walkthroughs also illustrate how toocombine cloud and on-premises tools and services into a workflow or pipeline toocreate an intelligent application.</span></span>

<span data-ttu-id="719bd-389">[Poängsätta Spark-inbyggda machine learning-modeller](machine-learning-data-science-spark-model-consumption.md) visar hur toouse Scala kod tooautomatically läsa in och poängsätta nya datauppsättningar med machine learning-modeller inbyggd Spark och sparas i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="719bd-389">[Score Spark-built machine learning models](machine-learning-data-science-spark-model-consumption.md) shows you how toouse Scala code tooautomatically load and score new data sets with machine learning models built in Spark and saved in Azure Blob storage.</span></span> <span data-ttu-id="719bd-390">Du kan följa hello anvisningarna det och ersätter bara hello Python-kod med Scala kod i den här artikeln för automatisk användning.</span><span class="sxs-lookup"><span data-stu-id="719bd-390">You can follow hello instructions provided there, and simply replace hello Python code with Scala code in this article for automated consumption.</span></span>

