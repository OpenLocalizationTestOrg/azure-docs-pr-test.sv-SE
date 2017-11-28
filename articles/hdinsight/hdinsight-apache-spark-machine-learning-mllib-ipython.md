---
title: "aaaMachine learning exempel med MLlib Spark på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur toouse Spark MLlib toocreate en machine learning-app som analyserar en datauppsättning med hjälp av klassificering via logistic regression."
keywords: "Spark maskininlärning spark machine learning-exempel"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c0fd4baa-946d-4e03-ad2c-a03491bd90c8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 5c3b83482de5d8fba224398aaafe07fa67ec1fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-spark-mllib-toobuild-a-machine-learning-application-and-analyze-a-dataset"></a><span data-ttu-id="0c0bf-104">Använda Spark MLlib toobuild machine learning-program och analysera en datamängd</span><span class="sxs-lookup"><span data-stu-id="0c0bf-104">Use Spark MLlib toobuild a machine learning application and analyze a dataset</span></span>

<span data-ttu-id="0c0bf-105">Lär dig hur toouse Spark **MLlib** toocreate en maskininlärning toodo enkel förutsägande analys av program på en öppen datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-105">Learn how toouse Spark **MLlib** toocreate a machine learning application toodo simple predictive analysis on an open dataset.</span></span> <span data-ttu-id="0c0bf-106">Från Sparks inbyggda maskininlärning bibliotek, det här exemplet används *klassificering* via logistic regression.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-106">From Spark's built-in machine learning libraries, this example uses *classification* through logistic regression.</span></span> 

> [!TIP]
> <span data-ttu-id="0c0bf-107">Det här exemplet är också tillgängliga som en Jupyter-anteckningsbok på ett kluster med Spark (Linux) som du skapar i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-107">This example is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="0c0bf-108">hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-108">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="0c0bf-109">toofollow hello vägledningen i en bärbar dator, skapa ett Spark-klustret och starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-109">toofollow hello tutorial from within a notebook, create a Spark cluster and launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`).</span></span> <span data-ttu-id="0c0bf-110">Kör sedan hello anteckningsboken **Spark Machine Learning - förutsägbar analys på mat inspektion data med hjälp av MLlib.ipynb** under hello **Python** mapp.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-110">Then, run hello notebook **Spark Machine Learning - Predictive analysis on food inspection data using MLlib.ipynb** under hello **Python** folder.</span></span>
>
>

<span data-ttu-id="0c0bf-111">MLlib är ett Spark core-bibliotek som innehåller många användbara verktyg för machine learning aktiviteter, inklusive verktyg som är lämpliga för:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-111">MLlib is a core Spark library that provides many utilities useful for machine learning tasks, including utilities that are suitable for:</span></span>

* <span data-ttu-id="0c0bf-112">Klassificering</span><span class="sxs-lookup"><span data-stu-id="0c0bf-112">Classification</span></span>
* <span data-ttu-id="0c0bf-113">Regression</span><span class="sxs-lookup"><span data-stu-id="0c0bf-113">Regression</span></span>
* <span data-ttu-id="0c0bf-114">Klustring</span><span class="sxs-lookup"><span data-stu-id="0c0bf-114">Clustering</span></span>
* <span data-ttu-id="0c0bf-115">Avsnittet modellering</span><span class="sxs-lookup"><span data-stu-id="0c0bf-115">Topic modeling</span></span>
* <span data-ttu-id="0c0bf-116">Enda värde uppdelning (SVD) och viktigaste komponenten analys (PCA)</span><span class="sxs-lookup"><span data-stu-id="0c0bf-116">Singular value decomposition (SVD) and principal component analysis (PCA)</span></span>
* <span data-ttu-id="0c0bf-117">Hypotesen testning och beräkna exempel statistik</span><span class="sxs-lookup"><span data-stu-id="0c0bf-117">Hypothesis testing and calculating sample statistics</span></span>

## <a name="what-are-classification-and-logistic-regression"></a><span data-ttu-id="0c0bf-118">Vad är klassificering och logistic regression?</span><span class="sxs-lookup"><span data-stu-id="0c0bf-118">What are classification and logistic regression?</span></span>
<span data-ttu-id="0c0bf-119">*Klassificering*, populära machine learning-aktivitet, är hello process för inkommande data sorteras i kategorier.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-119">*Classification*, a popular machine learning task, is hello process of sorting input data into categories.</span></span> <span data-ttu-id="0c0bf-120">Det är hello jobb av en klassificering algoritmen toofigure reda på hur tooassign ”märker” tooinput data som du anger.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-120">It is hello job of a classification algorithm toofigure out how tooassign "labels" tooinput data that you provide.</span></span> <span data-ttu-id="0c0bf-121">Exempelvis kan du se en maskininlärningsalgoritmen som accepterar lager information som indata och dividerar hello börs i två kategorier: du bör säljer och lagren bör du tänka.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-121">For example, you could think of a machine learning algorithm that accepts stock information as input and divides hello stock into two categories: stocks that you should sell and stocks that you should keep.</span></span>

<span data-ttu-id="0c0bf-122">Logistic regression är hello-algoritm som du använder för klassificering.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-122">Logistic regression is hello algorithm that you use for classification.</span></span> <span data-ttu-id="0c0bf-123">Sparks logistic regression API är användbart för *binär klassificering*, eller klassificera indata till en av två grupper.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-123">Spark's logistic regression API is useful for *binary classification*, or classifying input data into one of two groups.</span></span> <span data-ttu-id="0c0bf-124">Läs mer om logistic regressioner [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-124">For more information about logistic regressions, see [Wikipedia](https://en.wikipedia.org/wiki/Logistic_regression).</span></span>

<span data-ttu-id="0c0bf-125">Sammanfattningsvis hello sammankoppling av logistic regression leder till en *logistic funktionen* som kan vara används toopredict hello sannolikheten att en inkommande vector tillhör en grupp eller hello andra.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-125">In summary, hello process of logistic regression produces a *logistic function* that can be used toopredict hello probability that an input vector belongs in one group or hello other.</span></span>  

## <a name="predictive-analysis-example-on-food-inspection-data"></a><span data-ttu-id="0c0bf-126">Förutsägbar analys exempel på mat inspektion data</span><span class="sxs-lookup"><span data-stu-id="0c0bf-126">Predictive analysis example on food inspection data</span></span>
<span data-ttu-id="0c0bf-127">I det här exemplet du använder Spark tooperform vissa förutsägbar analys på mat inspektion data (**Food_Inspections1.csv**) som har köpts via hello [stad Chicago dataportalen](https://data.cityofchicago.org/).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-127">In this example, you use Spark tooperform some predictive analysis on food inspection data (**Food_Inspections1.csv**) that was acquired through hello [City of Chicago data portal](https://data.cityofchicago.org/).</span></span> <span data-ttu-id="0c0bf-128">Den här datauppsättningen innehåller information om mat etablering kontroller som har utförts i Chicago, inklusive information om varje anläggning, hello överträdelser hittades (eventuella) och hello resultaten av hello kontroll.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-128">This dataset contains information about food establishment inspections that were conducted in Chicago, including information about each establishment, hello violations found (if any), and hello results of hello inspection.</span></span> <span data-ttu-id="0c0bf-129">hello CSV-fil finns redan i hello storage-konto som är associerade med hello klustret på **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-129">hello CSV data file is already available in hello storage account associated with hello cluster at **/HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv**.</span></span>

<span data-ttu-id="0c0bf-130">I hello stegen nedan, utveckla en modell toosee vad som krävs för toopass eller misslyckas en mat-kontroll.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-130">In hello steps below, you develop a model toosee what it takes toopass or fail a food inspection.</span></span>

## <a name="start-building-a-spark-mmlib-machine-learning-app"></a><span data-ttu-id="0c0bf-131">Börja skapa Spark MMLib machine learning-app</span><span class="sxs-lookup"><span data-stu-id="0c0bf-131">Start building a Spark MMLib machine learning app</span></span>
1. <span data-ttu-id="0c0bf-132">Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-132">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="0c0bf-133">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-133">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
1. <span data-ttu-id="0c0bf-134">Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-134">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="0c0bf-135">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-135">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0c0bf-136">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-136">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="0c0bf-137">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-137">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
1. <span data-ttu-id="0c0bf-138">Skapa en anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-138">Create a notebook.</span></span> <span data-ttu-id="0c0bf-139">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-139">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="0c0bf-140">![Skapa en Jupyter-anteckningsbok](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="0c0bf-140">![Create a Jupyter notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-create-jupyter.png "Create a new Jupyter notebook")</span></span>
1. <span data-ttu-id="0c0bf-141">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-141">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="0c0bf-142">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-142">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="0c0bf-143">![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "ange ett namn för anteckningsboken hello")</span><span class="sxs-lookup"><span data-stu-id="0c0bf-143">![Provide a name for hello notebook](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-name-jupyter.png "Provide a name for hello notebook")</span></span>
1. <span data-ttu-id="0c0bf-144">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-144">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="0c0bf-145">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-145">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="0c0bf-146">Du kan börja skapa ditt maskininlärning program genom att importera hello-typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-146">You can start building your machine learning application by importing hello types required for this scenario.</span></span> <span data-ttu-id="0c0bf-147">toodo så markören hello i hello cell och trycka på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-147">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.ml import Pipeline
        from pyspark.ml.classification import LogisticRegression
        from pyspark.ml.feature import HashingTF, Tokenizer
        from pyspark.sql import Row
        from pyspark.sql.functions import UserDefinedFunction
        from pyspark.sql.types import *

## <a name="construct-an-input-dataframe"></a><span data-ttu-id="0c0bf-148">Skapa en inkommande dataframe</span><span class="sxs-lookup"><span data-stu-id="0c0bf-148">Construct an input dataframe</span></span>
<span data-ttu-id="0c0bf-149">Vi kan använda `sqlContext` tooperform omformningar på strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-149">We can use `sqlContext` tooperform transformations on structured data.</span></span> <span data-ttu-id="0c0bf-150">hello första uppgiften är tooload hello exempeldata ((**Food_Inspections1.csv**)) i en Spark SQL *dataframe*.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-150">hello first task is tooload hello sample data ((**Food_Inspections1.csv**)) into a Spark SQL *dataframe*.</span></span>

1. <span data-ttu-id="0c0bf-151">Eftersom hello rådata är CSV-format, måste toouse hello Spark kontexten toopull varje rad i hello-fil i minnet som Ostrukturerade text. sedan använder Python's CSV biblioteket tooparse varje rad individuellt.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-151">Because hello raw data is in a CSV format, we need toouse hello Spark context toopull every line of hello file into memory as unstructured text; then, you use Python's CSV library tooparse each line individually.</span></span>

        def csvParse(s):
            import csv
            from StringIO import StringIO
            sio = StringIO(s)
            value = csv.reader(sio).next()
            sio.close()
            return value

        inspections = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections1.csv')\
                        .map(csvParse)
1. <span data-ttu-id="0c0bf-152">Nu har vi hello CSV-fil som en RDD.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-152">We now have hello CSV file as an RDD.</span></span>  <span data-ttu-id="0c0bf-153">toounderstand hello schemat för hello, vi hämtar en rad från hello RDD.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-153">toounderstand hello schema of hello data, we retrieve one row from hello RDD.</span></span>

        inspections.take(1)

    <span data-ttu-id="0c0bf-154">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-154">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [['413707',
          'LUNA PARK INC',
          'LUNA PARK  DAY CARE',
          '2049789',
          "Children's Services Facility",
          'Risk 1 (High)',
          '3250 W FOSTER AVE ',
          'CHICAGO',
          'IL',
          '60625',
          '09/21/2010',
          'License-Task Force',
          'Fail',
          '24. DISH WASHING FACILITIES: PROPERLY DESIGNED, CONSTRUCTED, MAINTAINED, INSTALLED, LOCATED AND OPERATED - Comments: All dishwashing machines must be of a type that complies with all requirements of hello plumbing section of hello Municipal Code of Chicago and Rules and Regulation of hello Board of Health. OBSEVERD hello 3 COMPARTMENT SINK BACKING UP INTO hello 1ST AND 2ND COMPARTMENT WITH CLEAR WATER AND SLOWLY DRAINING OUT. INST NEED HAVE IT REPAIR. CITATION ISSUED, SERIOUS VIOLATION 7-38-030 H000062369-10 COURT DATE 10-28-10 TIME 1 P.M. ROOM 107 400 W. SURPERIOR. | 36. LIGHTING: REQUIRED MINIMUM FOOT-CANDLES OF LIGHT PROVIDED, FIXTURES SHIELDED - Comments: Shielding tooprotect against broken glass falling into food shall be provided for all artificial lighting sources in preparation, service, and display facilities. LIGHT SHIELD ARE MISSING UNDER HOOD OF  COOKING EQUIPMENT AND NEED tooREPLACE LIGHT UNDER UNIT. 4 LIGHTS ARE OUT IN hello REAR CHILDREN AREA,IN hello KINDERGARDEN CLASS ROOM. 2 LIGHT ARE OUT EAST REAR, LIGHT FRONT WEST ROOM. NEED tooREPLACE ALL LIGHT THAT ARE NOT WORKING. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned. MISSING CEILING TILES WITH STAINS IN WEST,EAST, IN FRONT AREA WEST, AND BY hello 15MOS AREA. NEED tooBE REPLACED. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair. SPLASH GUARDED ARE NEEDED BY hello EXPOSED HAND SINK IN hello KITCHEN AREA | 34. FLOORS: CONSTRUCTED PER CODE, CLEANED, GOOD REPAIR, COVING INSTALLED, DUST-LESS CLEANING METHODS USED - Comments: hello floors shall be constructed per code, be smooth and easily cleaned, and be kept clean and in good repair. INST NEED tooELEVATE ALL FOOD ITEMS 6INCH OFF hello FLOOR 6 INCH AWAY FORM WALL.  ',
          '41.97583445690982',
          '-87.7107455232781',
          '(41.97583445690982, -87.7107455232781)']]
1. <span data-ttu-id="0c0bf-155">hello ger föregående utdata oss en uppfattning om hello schemat för hello indatafilen.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-155">hello preceding output gives us an idea of hello schema of hello input file.</span></span> <span data-ttu-id="0c0bf-156">Den omfattar hello namnet på varje etablering, hello typ av etablering, hello adress, hello data för hello kontroller och hello plats, bland annat.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-156">It includes hello name of every establishment, hello type of establishment, hello address, hello data of hello inspections, and hello location, among other things.</span></span> <span data-ttu-id="0c0bf-157">Välj vi några kolumner som är användbara för våra förutsägbar analys och gruppera hello resultat som en dataframe som vi sedan använda toocreate en tillfällig tabell.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-157">Let's select a few columns that are useful for our predictive analysis and group hello results as a dataframe, which we then use toocreate a temporary table.</span></span>

        schema = StructType([
        StructField("id", IntegerType(), False),
        StructField("name", StringType(), False),
        StructField("results", StringType(), False),
        StructField("violations", StringType(), True)])

        df = sqlContext.createDataFrame(inspections.map(lambda l: (int(l[0]), l[1], l[12], l[13])) , schema)
        df.registerTempTable('CountResults')
1. <span data-ttu-id="0c0bf-158">Nu har vi en *dataframe*, `df` som vi kan utföra vår analys.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-158">We now have a *dataframe*, `df` on which we can perform our analysis.</span></span> <span data-ttu-id="0c0bf-159">Vi har också en tillfällig tabell anropet **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-159">We also have a temporary table call **CountResults**.</span></span> <span data-ttu-id="0c0bf-160">Innehåller fyra kolumner av intresse för hello dataframe: **id**, **namn**, **resultat**, och **överträdelser**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-160">We've included four columns of interest in hello dataframe: **id**, **name**, **results**, and **violations**.</span></span>

    <span data-ttu-id="0c0bf-161">Det är dags ett litet antal hello data:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-161">Let's get a small sample of hello data:</span></span>

        df.show(5)

    <span data-ttu-id="0c0bf-162">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-162">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +------+--------------------+-------+--------------------+
        |    id|                name|results|          violations|
        +------+--------------------+-------+--------------------+
        |413707|       LUNA PARK INC|   Fail|24. DISH WASHING ...|
        |391234|       CAFE SELMARIE|   Fail|2. FACILITIES too...|
        |413751|          MANCHU WOK|   Pass|33. FOOD AND NON-...|
        |413708|BENCHMARK HOSPITA...|   Pass|                    |
        |413722|           JJ BURGER|   Pass|                    |
        +------+--------------------+-------+--------------------+

## <a name="understand-hello-data"></a><span data-ttu-id="0c0bf-163">Förstå hello data</span><span class="sxs-lookup"><span data-stu-id="0c0bf-163">Understand hello data</span></span>
1. <span data-ttu-id="0c0bf-164">Låt oss börja tooget en uppfattning om vår datauppsättning innehåller.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-164">Let's start tooget a sense of what our dataset contains.</span></span> <span data-ttu-id="0c0bf-165">Vad är till exempel hello olika värden i hello **resultat** kolumnen?</span><span class="sxs-lookup"><span data-stu-id="0c0bf-165">For example, what are hello different values in hello **results** column?</span></span>

        df.select('results').distinct().show()

    <span data-ttu-id="0c0bf-166">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-166">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        +--------------------+
        |             results|
        +--------------------+
        |                Fail|
        |Business Not Located|
        |                Pass|
        |  Pass w/ Conditions|
        |     Out of Business|
        +--------------------+
1. <span data-ttu-id="0c0bf-167">En snabb visualisering hjälper oss orsak om hello distribution av dessa resultat.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-167">A quick visualization can help us reason about hello distribution of these outcomes.</span></span> <span data-ttu-id="0c0bf-168">Vi har redan hello data i en tillfällig tabell **CountResults**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-168">We already have hello data in a temporary table **CountResults**.</span></span> <span data-ttu-id="0c0bf-169">Du kan köra följande SQL-fråga mot hello tabell tooget hello en bättre förståelse för hur hello resultat distribueras.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-169">You can run hello following SQL query against hello table tooget a better understanding of how hello results are distributed.</span></span>

        %%sql -o countResultsdf
        SELECT results, COUNT(results) AS cnt FROM CountResults GROUP BY results

    <span data-ttu-id="0c0bf-170">Hej `%%sql` magic följt av `-o countResultsdf` säkerställer att hello utdata från frågan hello sparas lokalt på hello Jupyter-servern (vanligtvis hello headnode hello klustret).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-170">hello `%%sql` magic followed by `-o countResultsdf` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="0c0bf-171">hello utdata sparas som en [Pandas](http://pandas.pydata.org/) dataframe med hello angett namn **countResultsdf**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-171">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **countResultsdf**.</span></span>

    <span data-ttu-id="0c0bf-172">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-172">You should see an output like hello following:</span></span>

    <span data-ttu-id="0c0bf-173">![SQL-frågan](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL frågeresultatet")</span><span class="sxs-lookup"><span data-stu-id="0c0bf-173">![SQL query output](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-query-output.png "SQL query output")</span></span>

    <span data-ttu-id="0c0bf-174">Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="0c0bf-174">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
1. <span data-ttu-id="0c0bf-175">Du kan också använda Matplotlib, ett bibliotek används tooconstruct visualisering av data, toocreate ritning.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-175">You can also use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="0c0bf-176">Eftersom hello ritytans måste skapas från hello lokalt sparade **countResultsdf** dataframe, hello kodstycke måste börja med hello `%%local` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-176">Because hello plot must be created from hello locally persisted **countResultsdf** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="0c0bf-177">Detta säkerställer att hello kod körs lokalt på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-177">This ensures that hello code is run locally on hello Jupyter server.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = countResultsdf['results']
        sizes = countResultsdf['cnt']
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="0c0bf-178">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-178">You should see an output like hello following:</span></span>

    <span data-ttu-id="0c0bf-179">![Spark maskininlärning programmet utdata - cirkeldiagram med fem olika resultaten](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark maskininlärning resultatet utdata")</span><span class="sxs-lookup"><span data-stu-id="0c0bf-179">![Spark machine learning application output - pie chart with five distinct inspection results](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-1.png "Spark machine learning result output")</span></span>
1. <span data-ttu-id="0c0bf-180">Du kan se att det finns 5 distinkta resultat som kan ha en inspektion:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-180">You can see that there are 5 distinct results that an inspection can have:</span></span>

   * <span data-ttu-id="0c0bf-181">Företag som inte finns</span><span class="sxs-lookup"><span data-stu-id="0c0bf-181">Business not located</span></span>
   * <span data-ttu-id="0c0bf-182">Misslyckas</span><span class="sxs-lookup"><span data-stu-id="0c0bf-182">Fail</span></span>
   * <span data-ttu-id="0c0bf-183">Skicka</span><span class="sxs-lookup"><span data-stu-id="0c0bf-183">Pass</span></span>
   * <span data-ttu-id="0c0bf-184">PSS med villkor</span><span class="sxs-lookup"><span data-stu-id="0c0bf-184">Pss w/ conditions</span></span>
   * <span data-ttu-id="0c0bf-185">Out-of-Business</span><span class="sxs-lookup"><span data-stu-id="0c0bf-185">Out of Business</span></span>

     <span data-ttu-id="0c0bf-186">Låt oss utveckla en modell som kan gissa hello resultatet av en mat inspektion angivna hello överträdelser.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-186">Let us develop a model that can guess hello outcome of a food inspection, given hello violations.</span></span> <span data-ttu-id="0c0bf-187">Eftersom logistic regression är en klassificeringsmetod i binär, gör det klokt toogroup våra data i två kategorier: **misslyckas** och **skicka**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-187">Since logistic regression is a binary classification method, it makes sense toogroup our data into two categories: **Fail** and **Pass**.</span></span> <span data-ttu-id="0c0bf-188">En ”skicka med villkor” är fortfarande ett steg, så vi Tänk hello när vi träna modellen hello två resulterar motsvarande.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-188">A "Pass w/ Conditions" is still a Pass, so when we train hello model, we consider hello two results equivalent.</span></span> <span data-ttu-id="0c0bf-189">Data med hello andra resultat (”företag kan inte hitta” eller ”Out-of-Business”) är inte användbar så vi ta bort dem från våra träningsmängden.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-189">Data with hello other results ("Business Not Located" or "Out of Business") are not useful so we remove them from our training set.</span></span> <span data-ttu-id="0c0bf-190">Detta bör vara bra eftersom dessa två kategorier utgör en liten andel av hello resultat ändå.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-190">This should be okay since these two categories make up a very small percentage of hello results anyway.</span></span>
1. <span data-ttu-id="0c0bf-191">Låt oss gå vidare och konvertera vår befintliga dataframe (`df`) till en ny dataframe där varje inspektion representeras som ett par etikett överträdelser.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-191">Let us go ahead and convert our existing dataframe(`df`) into a new dataframe where each inspection is represented as a label-violations pair.</span></span> <span data-ttu-id="0c0bf-192">I det här fallet en etikett för `0.0` representerar ett fel, en etikett för `1.0` representerar en lyckad och en etikett för `-1.0` representerar vissa resultat utöver de två.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-192">In our case, a label of `0.0` represents a failure, a label of `1.0` represents a success, and a label of `-1.0` represents some results besides those two.</span></span> <span data-ttu-id="0c0bf-193">Vi kan filtrera dessa andra resultat ut när hello ny data ram.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-193">We filter those other results out when computing hello new data frame.</span></span>

        def labelForResults(s):
            if s == 'Fail':
                return 0.0
            elif s == 'Pass w/ Conditions' or s == 'Pass':
                return 1.0
            else:
                return -1.0
        label = UserDefinedFunction(labelForResults, DoubleType())
        labeledData = df.select(label(df.results).alias('label'), df.violations).where('label >= 0')

    <span data-ttu-id="0c0bf-194">toosee vilka hello märkta data verkar vara, hämta vi en rad.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-194">toosee what hello labeled data looks like, let's retrieve one row.</span></span>

        labeledData.take(1)

    <span data-ttu-id="0c0bf-195">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-195">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [Row(label=0.0, violations=u"41. PREMISES MAINTAINED FREE OF LITTER, UNNECESSARY ARTICLES, CLEANING  EQUIPMENT PROPERLY STORED - Comments: All parts of hello food establishment and all parts of hello property used in connection with hello operation of hello establishment shall be kept neat and clean and should not produce any offensive odors.  REMOVE MATTRESS FROM SMALL DUMPSTER. | 35. WALLS, CEILINGS, ATTACHED EQUIPMENT CONSTRUCTED PER CODE: GOOD REPAIR, SURFACES CLEAN AND DUST-LESS CLEANING METHODS - Comments: hello walls and ceilings shall be in good repair and easily cleaned.  REPAIR MISALIGNED DOORS AND DOOR NEAR ELEVATOR.  DETAIL CLEAN BLACK MOLD LIKE SUBSTANCE FROM WALLS BY BOTH DISH MACHINES.  REPAIR OR REMOVE BASEBOARD UNDER DISH MACHINE (LEFT REAR KITCHEN). SEAL ALL GAPS.  REPLACE MILK CRATES USED IN WALK IN COOLERS AND STORAGE AREAS WITH PROPER SHELVING AT LEAST 6' OFF hello FLOOR.  | 38. VENTILATION: ROOMS AND EQUIPMENT VENTED AS REQUIRED: PLUMBING: INSTALLED AND MAINTAINED - Comments: hello flow of air discharged from kitchen fans shall always be through a duct tooa point above hello roofline.  REPAIR BROKEN VENTILATION IN MEN'S AND WOMEN'S WASHROOMS NEXT tooDINING AREA. | 32. FOOD AND NON-FOOD CONTACT SURFACES PROPERLY DESIGNED, CONSTRUCTED AND MAINTAINED - Comments: All food and non-food contact equipment and utensils shall be smooth, easily cleanable, and durable, and shall be in good repair.  REPAIR DAMAGED PLUG ON LEFT SIDE OF 2 COMPARTMENT SINK.  REPAIR SELF CLOSER ON BOTTOM LEFT DOOR OF 4 DOOR PREP UNIT NEXT tooOFFICE.")]

## <a name="create-a-logistic-regression-model-from-hello-input-dataframe"></a><span data-ttu-id="0c0bf-196">Skapa en logistic regressionsmodell från hello inkommande dataframe</span><span class="sxs-lookup"><span data-stu-id="0c0bf-196">Create a logistic regression model from hello input dataframe</span></span>
<span data-ttu-id="0c0bf-197">Vår sista aktivitet är tooconvert hello märkta data till ett format som kan analyseras av logistic regression.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-197">Our final task is tooconvert hello labeled data into a format that can be analyzed by logistic regression.</span></span> <span data-ttu-id="0c0bf-198">algoritmen för hello inkommande tooa logistic regression ska vara en uppsättning *etikett-funktionen vector par*, där hello ”funktionen vector” är en vektor med siffror som representerar hello indata.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-198">hello input tooa logistic regression algorithm should be a set of *label-feature vector pairs*, where hello "feature vector" is a vector of numbers representing hello input point.</span></span> <span data-ttu-id="0c0bf-199">Vi behöver så tooconvert hello ”överträdelser” kolumn som är halvstrukturerade och innehåller många kommentarer i fritext, tooan matris av reellt tal som en dator lätt kan förstå.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-199">So, we need tooconvert hello "violations" column, which is semi-structured and contains many comments in free-text, tooan array of real numbers that a machine could easily understand.</span></span>

<span data-ttu-id="0c0bf-200">En standard machine learning-metod för behandling av naturligt språk är tooassign varje distinkta ord ”index”, och sedan skicka en vector toohello maskininlärningsalgoritmen så att varje indexvärde innehåller hello ofta ordet i hello text sträng.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-200">One standard machine learning approach for processing natural language is tooassign each distinct word an "index", and then pass a vector toohello machine learning algorithm such that each index's value contains hello relative frequency of that word in hello text string.</span></span>

<span data-ttu-id="0c0bf-201">MLlib ger ett enkelt sätt tooperform den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-201">MLlib provides an easy way tooperform this operation.</span></span> <span data-ttu-id="0c0bf-202">Först ”tokenize” varje överträdelser sträng tooget hello enskilda ord i varje sträng.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-202">First, "tokenize" each violations string tooget hello individual words in each string.</span></span> <span data-ttu-id="0c0bf-203">Använd sedan en `HashingTF` tooconvert varje uppsättning säkerhetstoken till en funktion vector som sedan kan skickade toohello logistic regression algoritmen tooconstruct en modell.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-203">Then, use a `HashingTF` tooconvert each set of tokens into a feature vector that can then be passed toohello logistic regression algorithm tooconstruct a model.</span></span> <span data-ttu-id="0c0bf-204">Vi genomför alla stegen i ordning med hjälp av en ”pipeline”.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-204">We conduct all of these steps in sequence using a "pipeline".</span></span>

    tokenizer = Tokenizer(inputCol="violations", outputCol="words")
    hashingTF = HashingTF(inputCol=tokenizer.getOutputCol(), outputCol="features")
    lr = LogisticRegression(maxIter=10, regParam=0.01)
    pipeline = Pipeline(stages=[tokenizer, hashingTF, lr])

    model = pipeline.fit(labeledData)

## <a name="evaluate-hello-model-on-a-separate-test-dataset"></a><span data-ttu-id="0c0bf-205">Utvärdera hello modell på en separat testdata</span><span class="sxs-lookup"><span data-stu-id="0c0bf-205">Evaluate hello model on a separate test dataset</span></span>
<span data-ttu-id="0c0bf-206">Vi kan använda hello modellen som vi skapade tidigare för*förutsäga* vilka hello resultaten av nya kontroller kommer att vara, baserat på hello överträdelser som observerades.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-206">We can use hello model we created earlier too*predict* what hello results of new inspections will be, based on hello violations that were observed.</span></span> <span data-ttu-id="0c0bf-207">Vi har tränat modellen för hello datauppsättningen **Food_Inspections1.csv**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-207">We trained this model on hello dataset **Food_Inspections1.csv**.</span></span> <span data-ttu-id="0c0bf-208">Låt oss använder en andra datauppsättningen **Food_Inspections2.csv**, för*utvärdera* hello styrkan hos den här modellen på nya data.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-208">Let us use a second dataset, **Food_Inspections2.csv**, too*evaluate* hello strength of this model on new data.</span></span> <span data-ttu-id="0c0bf-209">Den här andra datamängden (**Food_Inspections2.csv**) bör redan vara i hello standardbehållare för lagring som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-209">This second data set (**Food_Inspections2.csv**) should already be in hello default storage container associated with hello cluster.</span></span>

1. <span data-ttu-id="0c0bf-210">hello följande kodutdrag skapar en ny dataframe **predictionsDf** som innehåller hello förutsägelse som genereras av hello modell.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-210">hello following snippet creates a new dataframe, **predictionsDf** that contains hello prediction generated by hello model.</span></span> <span data-ttu-id="0c0bf-211">hello fragment skapas också en tillfällig tabell som kallas **förutsägelser** baserat på hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-211">hello snippet also creates a temporary table called **Predictions** based on hello dataframe.</span></span>

        testData = sc.textFile('wasb:///HdiSamples/HdiSamples/FoodInspectionData/Food_Inspections2.csv')\
                 .map(csvParse) \
                 .map(lambda l: (int(l[0]), l[1], l[12], l[13]))
        testDf = sqlContext.createDataFrame(testData, schema).where("results = 'Fail' OR results = 'Pass' OR results = 'Pass w/ Conditions'")
        predictionsDf = model.transform(testDf)
        predictionsDf.registerTempTable('Predictions')
        predictionsDf.columns

    <span data-ttu-id="0c0bf-212">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-212">You should see an output like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        ['id',
         'name',
         'results',
         'violations',
         'words',
         'features',
         'rawPrediction',
         'probability',
         'prediction']
1. <span data-ttu-id="0c0bf-213">Titta på en av hello förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-213">Look at one of hello predictions.</span></span> <span data-ttu-id="0c0bf-214">Kör följande kodutdrag:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-214">Run this snippet:</span></span>

        predictionsDf.take(1)

   <span data-ttu-id="0c0bf-215">Det finns en förutsägelse för hello första posten i hello test-datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-215">There is a prediction for hello first entry in hello test data set.</span></span>
1. <span data-ttu-id="0c0bf-216">Hej `model.transform()` metoden gäller hello samma omvandling tooany nya data med hello samma schema och kommer till en förutsägelse av hur tooclassify hello data.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-216">hello `model.transform()` method applies hello same transformation tooany new data with hello same schema, and arrive at a prediction of how tooclassify hello data.</span></span> <span data-ttu-id="0c0bf-217">Vi kan göra några enkla statistik tooget en uppfattning om hur exakt våra förutsägelser var:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-217">We can do some simple statistics tooget a sense of how accurate our predictions were:</span></span>

        numSuccesses = predictionsDf.where("""(prediction = 0 AND results = 'Fail') OR
                                              (prediction = 1 AND (results = 'Pass' OR
                                                                   results = 'Pass w/ Conditions'))""").count()
        numInspections = predictionsDf.count()

        print "There were", numInspections, "inspections and there were", numSuccesses, "successful predictions"
        print "This is a", str((float(numSuccesses) / float(numInspections)) * 100) + "%", "success rate"

    <span data-ttu-id="0c0bf-218">Det ser ut så hello följande hello utdata:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-218">hello output looks like hello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        There were 9315 inspections and there were 8087 successful predictions
        This is a 86.8169618894% success rate

    <span data-ttu-id="0c0bf-219">Med Spark logistic regression ger oss en korrekt modell för hello förhållandet mellan överträdelser beskrivningar på engelska och om ett visst företag skulle lyckat eller misslyckat en mat-kontroll.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-219">Using logistic regression with Spark gives us an accurate model of hello relationship between violations descriptions in English and whether a given business would pass or fail a food inspection.</span></span>

## <a name="create-a-visual-representation-of-hello-prediction"></a><span data-ttu-id="0c0bf-220">Skapa en bild av hello förutsägelse</span><span class="sxs-lookup"><span data-stu-id="0c0bf-220">Create a visual representation of hello prediction</span></span>
<span data-ttu-id="0c0bf-221">Vi kan nu skapa en slutlig visualiseringen toohelp oss skäl om hello resultaten av det här testet.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-221">We can now construct a final visualization toohelp us reason about hello results of this test.</span></span>

1. <span data-ttu-id="0c0bf-222">Vi börjar med att extrahera hello olika förutsägelser och resultat från hello **förutsägelser** tillfällig tabell skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-222">We start by extracting hello different predictions and results from hello **Predictions** temporary table created earlier.</span></span> <span data-ttu-id="0c0bf-223">hello följande frågor separata hello utdata som *true_positive*, *false_positive*, *true_negative*, och *false_negative*.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-223">hello following queries separate hello output as *true_positive*, *false_positive*, *true_negative*, and *false_negative*.</span></span> <span data-ttu-id="0c0bf-224">I hello frågor nedan, vi inaktivera visualisering med hjälp av `-q` och även spara hello utdata (med hjälp av `-o`) som dataframes som sedan kan användas med hello `%%local` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-224">In hello queries below, we turn off visualization by using `-q` and also save hello output (by using `-o`) as dataframes that can be then used with hello `%%local` magic.</span></span>

        %%sql -q -o true_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND results = 'Fail'

        %%sql -q -o false_positive
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 0 AND (results = 'Pass' OR results = 'Pass w/ Conditions')

        %%sql -q -o true_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND results = 'Fail'

        %%sql -q -o false_negative
        SELECT count(*) AS cnt FROM Predictions WHERE prediction = 1 AND (results = 'Pass' OR results = 'Pass w/ Conditions')
1. <span data-ttu-id="0c0bf-225">Använd slutligen hello följande fragment toogenerate hello ritytans med **Matplotlib**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-225">Finally, use hello following snippet toogenerate hello plot using **Matplotlib**.</span></span>

        %%local
        %matplotlib inline
        import matplotlib.pyplot as plt

        labels = ['True positive', 'False positive', 'True negative', 'False negative']
        sizes = [true_positive['cnt'], false_positive['cnt'], false_negative['cnt'], true_negative['cnt']]
        colors = ['turquoise', 'seagreen', 'mediumslateblue', 'palegreen', 'coral']
        plt.pie(sizes, labels=labels, autopct='%1.1f%%', colors=colors)
        plt.axis('equal')

    <span data-ttu-id="0c0bf-226">Du bör se hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="0c0bf-226">You should see hello following output:</span></span>

    <span data-ttu-id="0c0bf-227">![Spark maskininlärning programinnehåll - cirkeldiagram procentandelar av misslyckade mat kontroller. ] (./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark maskininlärning resultatet utdata")</span><span class="sxs-lookup"><span data-stu-id="0c0bf-227">![Spark machine learning application output - pie chart percentages of failed food inspections.](./media/hdinsight-apache-spark-machine-learning-mllib-ipython/spark-machine-learning-result-output-2.png "Spark machine learning result output")</span></span>

    <span data-ttu-id="0c0bf-228">I det här diagrammet refererar ”positivt” resultat toohello misslyckades mat kontroll, medan ett negativt resultat refererar tooa genomgått.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-228">In this chart, a "positive" result refers toohello failed food inspection, while a negative result refers tooa passed inspection.</span></span>

## <a name="shut-down-hello-notebook"></a><span data-ttu-id="0c0bf-229">Stäng hello anteckningsboken</span><span class="sxs-lookup"><span data-stu-id="0c0bf-229">Shut down hello notebook</span></span>
<span data-ttu-id="0c0bf-230">När du har kört programmet hello bör du stänga av hello anteckningsboken toorelease hello resurser.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-230">After you have finished running hello application, you should shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="0c0bf-231">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-231">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="0c0bf-232">Detta stängs av och stänger hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="0c0bf-232">This shuts down and closes hello notebook.</span></span>

## <span data-ttu-id="0c0bf-233"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="0c0bf-233"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="0c0bf-234">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-234">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="0c0bf-235">Scenarier</span><span class="sxs-lookup"><span data-stu-id="0c0bf-235">Scenarios</span></span>
* [<span data-ttu-id="0c0bf-236">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="0c0bf-236">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="0c0bf-237">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="0c0bf-237">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="0c0bf-238">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="0c0bf-238">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="0c0bf-239">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-239">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="0c0bf-240">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="0c0bf-240">Create and run applications</span></span>
* [<span data-ttu-id="0c0bf-241">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="0c0bf-241">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="0c0bf-242">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="0c0bf-242">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="0c0bf-243">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="0c0bf-243">Tools and extensions</span></span>
* [<span data-ttu-id="0c0bf-244">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="0c0bf-244">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="0c0bf-245">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="0c0bf-245">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="0c0bf-246">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-246">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="0c0bf-247">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-247">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="0c0bf-248">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="0c0bf-248">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="0c0bf-249">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0c0bf-249">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="0c0bf-250">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="0c0bf-250">Manage resources</span></span>
* [<span data-ttu-id="0c0bf-251">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-251">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="0c0bf-252">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c0bf-252">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
