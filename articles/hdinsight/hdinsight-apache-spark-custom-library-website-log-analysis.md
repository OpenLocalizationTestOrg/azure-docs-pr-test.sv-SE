---
title: aaaAnalyze webbplatsloggar med Python-bibliotek i Spark - Azure | Microsoft Docs
description: "Den här anteckningsboken visar hur tooanalyze logga data med hjälp av en anpassad bibliotek med Spark på Azure HDInsight."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c61c70f-fe7f-4f0f-a4ab-0cccee5668c9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 29e4308b2a359aee6d69494a98307d4da07f7909
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-a-custom-python-library-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="2b6db-103">Analysera webbplatsloggar med hjälp av en anpassad Python-bibliotek med Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-103">Analyze website logs using a custom Python library with Spark cluster on HDInsight</span></span>

<span data-ttu-id="2b6db-104">Den här anteckningsboken visar hur tooanalyze logga data med hjälp av en anpassad bibliotek med Spark i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b6db-104">This notebook demonstrates how tooanalyze log data using a custom library with Spark on HDInsight.</span></span> <span data-ttu-id="2b6db-105">hello anpassade bibliotek som vi använder är en Python-bibliotek kallas **iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-105">hello custom library we use is a Python library called **iislogparser.py**.</span></span>

> [!TIP]
> <span data-ttu-id="2b6db-106">Den här kursen är också tillgängliga som en Jupyter-anteckningsbok på ett kluster med Spark (Linux) som du skapar i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b6db-106">This tutorial is also available as a Jupyter notebook on a Spark (Linux) cluster that you create in HDInsight.</span></span> <span data-ttu-id="2b6db-107">hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv.</span><span class="sxs-lookup"><span data-stu-id="2b6db-107">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="2b6db-108">tooperform hello vägledningen i en bärbar dator, skapa ett Spark-kluster, starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), och kör sedan hello anteckningsboken **analysera loggar med Spark med hjälp av en anpassad library.ipynb** under hello  **PySpark** mapp.</span><span class="sxs-lookup"><span data-stu-id="2b6db-108">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Analyze logs with Spark using a custom library.ipynb** under hello **PySpark** folder.</span></span>
>
>

<span data-ttu-id="2b6db-109">**Krav:**</span><span class="sxs-lookup"><span data-stu-id="2b6db-109">**Prerequisites:**</span></span>

<span data-ttu-id="2b6db-110">Du måste ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-110">You must have hello following:</span></span>

* <span data-ttu-id="2b6db-111">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2b6db-111">An Azure subscription.</span></span> <span data-ttu-id="2b6db-112">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="2b6db-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="2b6db-113">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="2b6db-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="2b6db-114">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="2b6db-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="save-raw-data-as-an-rdd"></a><span data-ttu-id="2b6db-115">Spara rådata som en RDD</span><span class="sxs-lookup"><span data-stu-id="2b6db-115">Save raw data as an RDD</span></span>
<span data-ttu-id="2b6db-116">I det här avsnittet ska vi använda hello [Jupyter](https://jupyter.org) bärbar dator som är associerad med ett Apache Spark-kluster i HDInsight toorun jobb som bearbetar dina rådata exempeldata och spara den som en Hive-tabell.</span><span class="sxs-lookup"><span data-stu-id="2b6db-116">In this section, we use hello [Jupyter](https://jupyter.org) notebook associated with an Apache Spark cluster in HDInsight toorun jobs that process your raw sample data and save it as a Hive table.</span></span> <span data-ttu-id="2b6db-117">hello exempeldata är en CSV-fil (hvac.csv) tillgängliga på alla kluster som standard.</span><span class="sxs-lookup"><span data-stu-id="2b6db-117">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span>

<span data-ttu-id="2b6db-118">När du har sparat dina data som en Hive-tabell i nästa avsnitt om hello ansluter vi toohello Hive-tabell med BI-verktyg som Power BI och Tableau.</span><span class="sxs-lookup"><span data-stu-id="2b6db-118">Once your data is saved as a Hive table, in hello next section we will connect toohello Hive table using BI tools such as Power BI and Tableau.</span></span>

1. <span data-ttu-id="2b6db-119">Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="2b6db-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="2b6db-120">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="2b6db-121">Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="2b6db-122">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="2b6db-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b6db-123">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="2b6db-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="2b6db-124">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="2b6db-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="2b6db-125">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="2b6db-125">Create a new notebook.</span></span> <span data-ttu-id="2b6db-126">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="2b6db-127">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="2b6db-127">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>
4. <span data-ttu-id="2b6db-128">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="2b6db-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="2b6db-129">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="2b6db-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="2b6db-130">![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "ange ett namn för anteckningsboken hello")</span><span class="sxs-lookup"><span data-stu-id="2b6db-130">![Provide a name for hello notebook](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-name-jupyter-notebook.png "Provide a name for hello notebook")</span></span>
5. <span data-ttu-id="2b6db-131">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="2b6db-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="2b6db-132">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="2b6db-132">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="2b6db-133">Du kan starta genom att importera hello-typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="2b6db-133">You can start by importing hello types that are required for this scenario.</span></span> <span data-ttu-id="2b6db-134">Klistra in hello följande fragment i en tom cell och tryck sedan på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-134">Paste hello following snippet in an empty cell, and then press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import Row
        from pyspark.sql.types import *


1. <span data-ttu-id="2b6db-135">Skapa en RDD via hello loggen exempeldata redan finns på hello kluster.</span><span class="sxs-lookup"><span data-stu-id="2b6db-135">Create an RDD using hello sample log data already available on hello cluster.</span></span> <span data-ttu-id="2b6db-136">Du kan komma åt hello data i hello standardkontot för lagring som associerats med hello kluster på **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-136">You can access hello data in hello default storage account associated with hello cluster at **\HdiSamples\HdiSamples\WebsiteLogSampleData\SampleLog\909f2b.log**.</span></span>

        logs = sc.textFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log')


1. <span data-ttu-id="2b6db-137">Hämta en exempellogg ange tooverify hello tidigare steget har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2b6db-137">Retrieve a sample log set tooverify that hello previous step completed successfully.</span></span>

        logs.take(5)

    <span data-ttu-id="2b6db-138">Du bör se en utdata liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-138">You should see an output similar toohello following:</span></span>

        # -----------------
        # THIS IS AN OUTPUT
        # -----------------

        [u'#Software: Microsoft Internet Information Services 8.0',
         u'#Fields: date time s-sitename cs-method cs-uri-stem cs-uri-query s-port cs-username c-ip cs(User-Agent) cs(Cookie) cs(Referer) cs-host sc-status sc-substatus sc-win32-status sc-bytes cs-bytes time-taken',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32',
         u'2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step4.png X-ARR-LOG-ID=4bea5b3d-8ac9-46c9-9b8c-ec3e9500cbea 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 72177 871 47']

## <a name="analyze-log-data-using-a-custom-python-library"></a><span data-ttu-id="2b6db-139">Analysera loggdata som använder en anpassad Python-bibliotek</span><span class="sxs-lookup"><span data-stu-id="2b6db-139">Analyze log data using a custom Python library</span></span>
1. <span data-ttu-id="2b6db-140">Hello första par raderna innehåller hello rubrikinformation i hello utdata ovan, och varje återstående rad matchar hello schemat som beskrivs i den rubriken.</span><span class="sxs-lookup"><span data-stu-id="2b6db-140">In hello output above, hello first couple lines include hello header information and each remaining line matches hello schema described in that header.</span></span> <span data-ttu-id="2b6db-141">Parsning av dessa loggar kan vara komplicerat.</span><span class="sxs-lookup"><span data-stu-id="2b6db-141">Parsing such logs could be complicated.</span></span> <span data-ttu-id="2b6db-142">Så vi använder en anpassad Python-bibliotek (**iislogparser.py**) som gör att parsning av dessa loggar som är mycket enklare.</span><span class="sxs-lookup"><span data-stu-id="2b6db-142">So, we use a custom Python library (**iislogparser.py**) that makes parsing such logs much easier.</span></span> <span data-ttu-id="2b6db-143">Det här biblioteket är som standard ingår i Spark-kluster i HDInsight på **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-143">By default, this library is included with your Spark cluster on HDInsight at **/HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py**.</span></span>

    <span data-ttu-id="2b6db-144">Det här biblioteket är dock inte i hello `PYTHONPATH` så att vi inte kan använda den med hjälp av en import-sats som `import iislogparser`.</span><span class="sxs-lookup"><span data-stu-id="2b6db-144">However, this library is not in hello `PYTHONPATH` so we cannot use it by using an import statement like `import iislogparser`.</span></span> <span data-ttu-id="2b6db-145">toouse det här biblioteket vi måste distribuera den tooall hello arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="2b6db-145">toouse this library, we must distribute it tooall hello worker nodes.</span></span> <span data-ttu-id="2b6db-146">Kör hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="2b6db-146">Run hello following snippet.</span></span>

        sc.addPyFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/iislogparser.py')


1. <span data-ttu-id="2b6db-147">`iislogparser`innehåller en funktion `parse_log_line` som returnerar `None` om en logg är en rubrikrad, och returnerar en instans av hello `LogLine` klassen om det uppstår en rad i loggfilen.</span><span class="sxs-lookup"><span data-stu-id="2b6db-147">`iislogparser` provides a function `parse_log_line` that returns `None` if a log line is a header row, and returns an instance of hello `LogLine` class if it encounters a log line.</span></span> <span data-ttu-id="2b6db-148">Använd hello `LogLine` klassen tooextract endast hello loggen rader från hello RDD:</span><span class="sxs-lookup"><span data-stu-id="2b6db-148">Use hello `LogLine` class tooextract only hello log lines from hello RDD:</span></span>

        def parse_line(l):
            import iislogparser
            return iislogparser.parse_log_line(l)
        logLines = logs.map(parse_line).filter(lambda p: p is not None).cache()
2. <span data-ttu-id="2b6db-149">Hämta ett par extraherade loggen rader tooverify som hello steg har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2b6db-149">Retrieve a couple of extracted log lines tooverify that hello step completed successfully.</span></span>

       logLines.take(2)

   <span data-ttu-id="2b6db-150">hello utdata ska vara liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-150">hello output should be similar toohello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step2.png X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 53175 871 46,
        2014-01-01 02:01:09 SAMPLEWEBSITE GET /blogposts/mvc4/step3.png X-ARR-LOG-ID=9eace870-2f49-4efd-b204-0d170da46b4a 80 - 1.54.23.196 Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36 - http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx www.sample.com 200 0 0 51237 871 32]
3. <span data-ttu-id="2b6db-151">Hej `LogLine` klass, i sin tur har vissa användbara metoder som `is_error()`, som returnerar om en loggpost har en felkod.</span><span class="sxs-lookup"><span data-stu-id="2b6db-151">hello `LogLine` class, in turn, has some useful methods, like `is_error()`, which returns whether a log entry has an error code.</span></span> <span data-ttu-id="2b6db-152">Använd den här toocompute hello antalet fel på hello extraherade loggen rader och logga sedan alla hello fel tooa annan fil.</span><span class="sxs-lookup"><span data-stu-id="2b6db-152">Use this toocompute hello number of errors in hello extracted log lines, and then log all hello errors tooa different file.</span></span>

       errors = logLines.filter(lambda p: p.is_error())
       numLines = logLines.count()
       numErrors = errors.count()
       print 'There are', numErrors, 'errors and', numLines, 'log entries'
       errors.map(lambda p: str(p)).saveAsTextFile('wasb:///HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b-2.log')

   <span data-ttu-id="2b6db-153">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-153">You should see an output like hello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       There are 30 errors and 646 log entries
4. <span data-ttu-id="2b6db-154">Du kan också använda **Matplotlib** tooconstruct en visualisering av hello data.</span><span class="sxs-lookup"><span data-stu-id="2b6db-154">You can also use **Matplotlib** tooconstruct a visualization of hello data.</span></span> <span data-ttu-id="2b6db-155">Om du vill tooisolate hello orsaken till begäranden som körs under lång tid kan kanske du vill toofind hello filer som tar hello de flesta tid tooserve i genomsnitt.</span><span class="sxs-lookup"><span data-stu-id="2b6db-155">For example, if you want tooisolate hello cause of requests that run for a long time, you might want toofind hello files that take hello most time tooserve on average.</span></span>
   <span data-ttu-id="2b6db-156">hello kodfragmentet nedan hämtar hello översta 25 resurser som tog tooserve för de flesta tid en begäran.</span><span class="sxs-lookup"><span data-stu-id="2b6db-156">hello snippet below retrieves hello top 25 resources that took most time tooserve a request.</span></span>

       def avgTimeTakenByKey(rdd):
           return rdd.combineByKey(lambda line: (line.time_taken, 1),
                                   lambda x, line: (x[0] + line.time_taken, x[1] + 1),
                                   lambda x, y: (x[0] + y[0], x[1] + y[1]))\
                     .map(lambda x: (x[0], float(x[1][0]) / float(x[1][1])))

       avgTimeTakenByKey(logLines.map(lambda p: (p.cs_uri_stem, p))).top(25, lambda x: x[1])

   <span data-ttu-id="2b6db-157">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-157">You should see an output like hello following:</span></span>

       # -----------------
       # THIS IS AN OUTPUT
       # -----------------

       [(u'/blogposts/mvc4/step13.png', 197.5),
        (u'/blogposts/mvc2/step10.jpg', 179.5),
        (u'/blogposts/extractusercontrol/step5.png', 170.0),
        (u'/blogposts/mvc4/step8.png', 159.0),
        (u'/blogposts/mvcrouting/step22.jpg', 155.0),
        (u'/blogposts/mvcrouting/step3.jpg', 152.0),
        (u'/blogposts/linqsproc1/step16.jpg', 138.75),
        (u'/blogposts/linqsproc1/step26.jpg', 137.33333333333334),
        (u'/blogposts/vs2008javascript/step10.jpg', 127.0),
        (u'/blogposts/nested/step2.jpg', 126.0),
        (u'/blogposts/adminpack/step1.png', 124.0),
        (u'/BlogPosts/datalistpaging/step2.png', 118.0),
        (u'/blogposts/mvc4/step35.png', 117.0),
        (u'/blogposts/mvcrouting/step2.jpg', 116.5),
        (u'/blogposts/aboutme/basketball.jpg', 109.0),
        (u'/blogposts/anonymoustypes/step11.jpg', 109.0),
        (u'/blogposts/mvc4/step12.png', 106.0),
        (u'/blogposts/linq8/step0.jpg', 105.5),
        (u'/blogposts/mvc2/step18.jpg', 104.0),
        (u'/blogposts/mvc2/step11.jpg', 104.0),
        (u'/blogposts/mvcrouting/step1.jpg', 104.0),
        (u'/blogposts/extractusercontrol/step1.png', 103.0),
        (u'/blogposts/sqlvideos/sqlvideos.jpg', 102.0),
        (u'/blogposts/mvcrouting/step21.jpg', 101.0),
        (u'/blogposts/mvc4/step1.png', 98.0)]
5. <span data-ttu-id="2b6db-158">Du kan också visa denna information i form av hello på ritytan.</span><span class="sxs-lookup"><span data-stu-id="2b6db-158">You can also present this information in hello form of plot.</span></span> <span data-ttu-id="2b6db-159">Som ett första steg toocreate ritning, låt oss först skapa en tillfällig tabell **AverageTime**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-159">As a first step toocreate a plot, let us first create a temporary table **AverageTime**.</span></span> <span data-ttu-id="2b6db-160">hello tabell grupper hello loggar som tid toosee om det inte finns några ovanliga svarstidsspikar vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="2b6db-160">hello table groups hello logs by time toosee if there were any unusual latency spikes at any particular time.</span></span>

       avgTimeTakenByMinute = avgTimeTakenByKey(logLines.map(lambda p: (p.datetime.minute, p))).sortByKey()
       schema = StructType([StructField('Minutes', IntegerType(), True),
                            StructField('Time', FloatType(), True)])

       avgTimeTakenByMinuteDF = sqlContext.createDataFrame(avgTimeTakenByMinute, schema)
       avgTimeTakenByMinuteDF.registerTempTable('AverageTime')
6. <span data-ttu-id="2b6db-161">Sedan kan du köra följande SQL-frågan tooget hello alla hello poster i hello **AverageTime** tabell.</span><span class="sxs-lookup"><span data-stu-id="2b6db-161">You can then run hello following SQL query tooget all hello records in hello **AverageTime** table.</span></span>

       %%sql -o averagetime
       SELECT * FROM AverageTime

   <span data-ttu-id="2b6db-162">Hej `%%sql` magic följt av `-o averagetime` säkerställer att hello utdata från frågan hello sparas lokalt på hello Jupyter-servern (vanligtvis hello headnode hello klustret).</span><span class="sxs-lookup"><span data-stu-id="2b6db-162">hello `%%sql` magic followed by `-o averagetime` ensures that hello output of hello query is persisted locally on hello Jupyter server (typically hello headnode of hello cluster).</span></span> <span data-ttu-id="2b6db-163">hello utdata sparas som en [Pandas](http://pandas.pydata.org/) dataframe med hello angett namn **averagetime**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-163">hello output is persisted as a [Pandas](http://pandas.pydata.org/) dataframe with hello specified name **averagetime**.</span></span>

   <span data-ttu-id="2b6db-164">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-164">You should see an output like hello following:</span></span>

   <span data-ttu-id="2b6db-165">![SQL-frågan](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL frågeresultatet")</span><span class="sxs-lookup"><span data-stu-id="2b6db-165">![SQL query output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-jupyter-sql-qyery-output.png "SQL query output")</span></span>

   <span data-ttu-id="2b6db-166">Mer information om hello `%%sql` magiskt, se [parametrar stöds med hello %% sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="2b6db-166">For more information about hello `%%sql` magic, see [Parameters supported with hello %%sql magic](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>
7. <span data-ttu-id="2b6db-167">Du kan nu använda Matplotlib, ett bibliotek används tooconstruct visualisering av data, toocreate ritning.</span><span class="sxs-lookup"><span data-stu-id="2b6db-167">You can now use Matplotlib, a library used tooconstruct visualization of data, toocreate a plot.</span></span> <span data-ttu-id="2b6db-168">Eftersom hello ritytans måste skapas från hello lokalt sparade **averagetime** dataframe, hello kodstycke måste börja med hello `%%local` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="2b6db-168">Because hello plot must be created from hello locally persisted **averagetime** dataframe, hello code snippet must begin with hello `%%local` magic.</span></span> <span data-ttu-id="2b6db-169">Detta säkerställer att hello kod körs lokalt på hello Jupyter-servern.</span><span class="sxs-lookup"><span data-stu-id="2b6db-169">This ensures that hello code is run locally on hello Jupyter server.</span></span>

       %%local
       %matplotlib inline
       import matplotlib.pyplot as plt

       plt.plot(averagetime['Minutes'], averagetime['Time'], marker='o', linestyle='--')
       plt.xlabel('Time (min)')
       plt.ylabel('Average time taken for request (ms)')

   <span data-ttu-id="2b6db-170">Du bör se utdata som liknar hello följande:</span><span class="sxs-lookup"><span data-stu-id="2b6db-170">You should see an output like hello following:</span></span>

   <span data-ttu-id="2b6db-171">![Matplotlib utdata](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib utdata")</span><span class="sxs-lookup"><span data-stu-id="2b6db-171">![Matplotlib output](./media/hdinsight-apache-spark-custom-library-website-log-analysis/hdinsight-apache-spark-web-log-analysis-plot.png "Matplotlib output")</span></span>
8. <span data-ttu-id="2b6db-172">När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser.</span><span class="sxs-lookup"><span data-stu-id="2b6db-172">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="2b6db-173">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="2b6db-173">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="2b6db-174">Den här stängs och Stäng hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="2b6db-174">This will shutdown and close hello notebook.</span></span>

## <span data-ttu-id="2b6db-175"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="2b6db-175"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="2b6db-176">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-176">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="2b6db-177">Scenarier</span><span class="sxs-lookup"><span data-stu-id="2b6db-177">Scenarios</span></span>
* [<span data-ttu-id="2b6db-178">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="2b6db-178">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="2b6db-179">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="2b6db-179">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="2b6db-180">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="2b6db-180">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="2b6db-181">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="2b6db-181">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="2b6db-182">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="2b6db-182">Create and run applications</span></span>
* [<span data-ttu-id="2b6db-183">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="2b6db-183">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="2b6db-184">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="2b6db-184">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="2b6db-185">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="2b6db-185">Tools and extensions</span></span>
* [<span data-ttu-id="2b6db-186">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="2b6db-186">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="2b6db-187">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="2b6db-187">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="2b6db-188">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-188">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="2b6db-189">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-189">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="2b6db-190">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="2b6db-190">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="2b6db-191">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="2b6db-191">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="2b6db-192">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="2b6db-192">Manage resources</span></span>
* [<span data-ttu-id="2b6db-193">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-193">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="2b6db-194">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="2b6db-194">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
