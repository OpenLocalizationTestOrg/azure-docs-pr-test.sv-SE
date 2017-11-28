---
title: "aaaRun interaktiva frågor i ett Azure HDInsight Spark-kluster | Microsoft Docs"
description: "HDInsight Spark quickstart på hur toocreate ett Apache Spark-kluster i HDInsight."
keywords: "spark-snabbstart,interaktiv spark,interaktiv fråga,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 3864eba50eb3828a9ecb657ded88080e1974585f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-interactive-queries-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="8db97-104">Köra interaktiva frågor på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="8db97-104">Run interactive queries on an HDInsight Spark cluster</span></span>

<span data-ttu-id="8db97-105">I den här artikeln använder du en Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågor mot Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8db97-105">In this article, you use a Jupyter notebook toorun interactive Spark SQL queries against a Spark cluster.</span></span> <span data-ttu-id="8db97-106">Jupyter-anteckningsbok är ett webbaserat program som utökar hello konsolbaserad interaktiva upplevelse toohello Web.</span><span class="sxs-lookup"><span data-stu-id="8db97-106">Jupyter notebook is a browser-based application that extends hello console-based interactive experience toohello Web.</span></span> <span data-ttu-id="8db97-107">Mer information finns i [hello Jupyter-anteckningsbok](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span><span class="sxs-lookup"><span data-stu-id="8db97-107">For more information, see [hello Jupyter notebook](http://jupyter-notebook.readthedocs.io/en/latest/notebook.html).</span></span>

<span data-ttu-id="8db97-108">Den här kursen kan du använda hello **PySpark** kernel i hello Jupyter-anteckningsbok toorun en interaktiva Spark SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="8db97-108">For this tutorial, you use hello **PySpark** kernel in hello Jupyter notebook toorun an interactive Spark SQL query.</span></span> <span data-ttu-id="8db97-109">Jupyter-anteckningsböcker på HDInsight-kluster stöder också två andra kernel - **PySpark3** och **Spark**.</span><span class="sxs-lookup"><span data-stu-id="8db97-109">Jupyter notebooks on HDInsight clusters also support two other kernels - **PySpark3** and **Spark**.</span></span> <span data-ttu-id="8db97-110">Mer information om hello kärnor och hello fördelarna med att använda **PySpark**, se [Använd Jupyter-anteckningsbok kärnor med Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="8db97-110">For more information about hello kernels, and hello benefits of using **PySpark**, see [Use Jupyter notebook kernels with Apache Spark clusters in HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8db97-111">Krav</span><span class="sxs-lookup"><span data-stu-id="8db97-111">Prerequisites</span></span>

* <span data-ttu-id="8db97-112">**Ett Azure HDInsight Spark-kluster**.</span><span class="sxs-lookup"><span data-stu-id="8db97-112">**An Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="8db97-113">Instruktioner finns i [skapar ett Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="8db97-113">For instructions, see [Create an Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="create-a-jupyter-notebook-toorun-interactive-queries"></a><span data-ttu-id="8db97-114">Skapa en Jupyter-anteckningsbok toorun interaktiva frågor</span><span class="sxs-lookup"><span data-stu-id="8db97-114">Create a Jupyter notebook toorun interactive queries</span></span>

<span data-ttu-id="8db97-115">toorun frågor vi använder exempeldata är som standard tillgängligt i hello lagring som associerats med hello kluster.</span><span class="sxs-lookup"><span data-stu-id="8db97-115">toorun queries, we use sample data that is by default available in hello storage associated with hello cluster.</span></span> <span data-ttu-id="8db97-116">Du måste dock först hämta dessa data till Spark som en dataframe.</span><span class="sxs-lookup"><span data-stu-id="8db97-116">However, you must first load that data into Spark as a dataframe.</span></span> <span data-ttu-id="8db97-117">När du har hello dataframe kan köra du frågor i det med hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="8db97-117">Once you have hello dataframe, you can run queries on it using hello Jupyter notebook.</span></span> <span data-ttu-id="8db97-118">I det här avsnittet titta på hur du:</span><span class="sxs-lookup"><span data-stu-id="8db97-118">In this section, you look at how to:</span></span>

* <span data-ttu-id="8db97-119">Registrera data exempeldata som ett Spark-dataframe.</span><span class="sxs-lookup"><span data-stu-id="8db97-119">Register a sample data set as a Spark dataframe.</span></span>
* <span data-ttu-id="8db97-120">Köra frågor på hello dataframe.</span><span class="sxs-lookup"><span data-stu-id="8db97-120">Run queries on hello dataframe.</span></span>

1. <span data-ttu-id="8db97-121">Öppna hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="8db97-121">Open hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="8db97-122">Om du har valt toopin hello klusterinstrumentpanel toohello Klicka hello klustret panelen från hello instrumentpanelen toolaunch hello klusterbladet.</span><span class="sxs-lookup"><span data-stu-id="8db97-122">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="8db97-123">Om du inte fäster hello klustret toohello instrumentpanelen från hello till vänster och klicka på **HDInsight-kluster**, och klicka sedan på hello-klustret som du skapade.</span><span class="sxs-lookup"><span data-stu-id="8db97-123">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="8db97-124">I **Snabblänkar** klickar du på **Klusterinstrumentpaneler** och sedan på **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="8db97-124">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="8db97-125">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="8db97-125">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="8db97-126">![Öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="8db97-126">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-start-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="8db97-127">Du kan också komma åt hello Jupyter notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="8db97-127">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="8db97-128">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="8db97-128">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="8db97-129">Skapa en anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="8db97-129">Create a notebook.</span></span> <span data-ttu-id="8db97-130">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="8db97-130">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="8db97-131">![Skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="8db97-131">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="8db97-132">En ny anteckningsbok skapas och öppnas med hello namnet Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="8db97-132">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="8db97-133">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn om du vill.</span><span class="sxs-lookup"><span data-stu-id="8db97-133">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="8db97-134">![Ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från")</span><span class="sxs-lookup"><span data-stu-id="8db97-134">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5. <span data-ttu-id="8db97-135">Klistra in hello följande kod i en tom cell och tryck sedan på **SKIFT + RETUR** toorun hello kod.</span><span class="sxs-lookup"><span data-stu-id="8db97-135">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="8db97-136">hello kod importerar hello-typer som krävs för det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="8db97-136">hello code imports hello types required for this scenario:</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="8db97-137">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="8db97-137">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="8db97-138">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="8db97-138">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span>

    <span data-ttu-id="8db97-139">![Status för interaktiv Spark SQL-fråga](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status för interaktiv Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="8db97-139">![Status of interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-interactive-spark-query-status.png "Status of interactive Spark SQL query")</span></span>

    <span data-ttu-id="8db97-140">Varje gång du kör en interaktiv fråga i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel.</span><span class="sxs-lookup"><span data-stu-id="8db97-140">Every time you run an interactive query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="8db97-141">Du också se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="8db97-141">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="8db97-142">När hello jobbet har slutförts ändras tooa tom cirkel.</span><span class="sxs-lookup"><span data-stu-id="8db97-142">After hello job is completed, it changes tooa hollow circle.</span></span>

6. <span data-ttu-id="8db97-143">Låt oss se ut en ögonblicksbild av den innan du läser in hello data i ett Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="8db97-143">Before you load hello data into a Spark cluster, let us look a snapshot of it.</span></span> <span data-ttu-id="8db97-144">hello exempeldata som används i den här kursen är tillgänglig som en CSV-fil på alla HDInsight Spark-kluster på **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span><span class="sxs-lookup"><span data-stu-id="8db97-144">hello sample data used in this tutorial is available as a CSV file on all HDInsight Spark clusters at **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**.</span></span> <span data-ttu-id="8db97-145">hello data avbildar hello temperaturvariationer av en byggnad.</span><span class="sxs-lookup"><span data-stu-id="8db97-145">hello data captures hello temperature variations of a building.</span></span> <span data-ttu-id="8db97-146">Här följer hello första raderna i hello data.</span><span class="sxs-lookup"><span data-stu-id="8db97-146">Here are hello first few rows of hello data.</span></span>

    <span data-ttu-id="8db97-147">![Ögonblicksbild av data för interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "ögonblicksbild av data för interaktiva Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="8db97-147">![Snapshot of data for interactive Spark SQL query](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-spark-sample-data-interactive-spark-sql-query.png "Snapshot of data for interactive Spark SQL query")</span></span>

6. <span data-ttu-id="8db97-148">Skapa en dataframe och en tillfällig tabell (**hvac**) genom att köra hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="8db97-148">Create a dataframe and a temporary table (**hvac**) by running hello following code.</span></span> <span data-ttu-id="8db97-149">För den här självstudiekursen kommer skapa vi inte alla hello kolumner i hello tillfällig tabell som jämfört med toohello kolumner i CSV-hello rådata.</span><span class="sxs-lookup"><span data-stu-id="8db97-149">For this tutorial, we do not create all hello columns in hello temporary table as compared toohello columns in hello raw CSV data.</span></span> 

        # Create an RDD from sample data
        hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse hello data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))
        
        # Infer hello schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. <span data-ttu-id="8db97-150">När hello tabellen har skapats kan köra interaktiva fråga på hello data, kan du använda hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="8db97-150">Once hello table is created, run interactive query on hello data, use hello following code.</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

   <span data-ttu-id="8db97-151">Eftersom du använder en PySpark-kerneln, du kan nu direkt köra en interaktiv SQL-fråga på hello tillfällig tabell **hvac** som du skapade med hjälp av hello `%%sql` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="8db97-151">Because you are using a PySpark kernel, you can now directly run an interactive SQL query on hello temporary table **hvac** that you created by using hello `%%sql` magic.</span></span> <span data-ttu-id="8db97-152">Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="8db97-152">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

   <span data-ttu-id="8db97-153">hello följande tabular utdata visas som standard.</span><span class="sxs-lookup"><span data-stu-id="8db97-153">hello following tabular output is displayed by default.</span></span>

     <span data-ttu-id="8db97-154">![Tabellutdata från interaktivt Spark-frågeresultat](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Tabellutdata från interaktivt Spark-frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="8db97-154">![Table output of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result.png "Table output of interactive Spark query result")</span></span>

    <span data-ttu-id="8db97-155">Du kan också se hello resulterar i andra visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="8db97-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="8db97-156">Till exempel detta ett Områdesdiagram för samma utdata skulle se ut hello följande hello.</span><span class="sxs-lookup"><span data-stu-id="8db97-156">For example, an area graph for hello same output would look like hello following.</span></span>

    <span data-ttu-id="8db97-157">![Områdesdiagram över interaktivt Spark-frågeresultat](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Områdesdiagram över interaktivt Spark-frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="8db97-157">![Area graph of interactive Spark query result](./media/hdinsight-apache-spark-load-data-run-query/hdinsight-interactive-spark-query-result-area-chart.png "Area graph of interactive Spark query result")</span></span>

9. <span data-ttu-id="8db97-158">Stäng hello anteckningsboken toorelease hello klusterresurser när du har kört programmet hello.</span><span class="sxs-lookup"><span data-stu-id="8db97-158">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="8db97-159">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="8db97-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <a name="next-step"></a><span data-ttu-id="8db97-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8db97-160">Next step</span></span>

<span data-ttu-id="8db97-161">I denna artikel du lärt dig hur toorun interaktiva frågor i Spark med Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="8db97-161">In this article you learned how toorun interactive queries in Spark using Jupyter notebook.</span></span> <span data-ttu-id="8db97-162">Avancera toohello nästa artikel toosee hur hello data som du har registrerat i Spark hämtas till en BI analytics verktyg som till exempel Power BI och Tableau.</span><span class="sxs-lookup"><span data-stu-id="8db97-162">Advance toohello next article toosee how hello data you registered in Spark can be pulled into a BI analytics tool such as Power BI and Tableau.</span></span> 

> [!div class="nextstepaction"]
>[<span data-ttu-id="8db97-163">Spark BI med hjälp av verktyg för visualisering av data med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="8db97-163">Spark BI using data visualization tools with Azure HDInsight</span></span>](hdinsight-apache-spark-use-bi-tools.md)




