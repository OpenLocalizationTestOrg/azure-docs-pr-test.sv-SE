---
title: "aaaSpark BI med hjälp av verktyg för visualisering av data på Azure HDInsight | Microsoft Docs"
description: "Använd verktyg för visualisering av data för analytics med hjälp av BI för Apache Spark i HDInsight-kluster"
keywords: "Apache spark bi, spark bi, spark datavisualisering Väck affärsinformation"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: ba4bfff737ce80ffca5c24f1c2c82a1447f467fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a><span data-ttu-id="608b3-104">Apache Spark BI med hjälp av verktyg för visualisering av data med Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="608b3-104">Apache Spark BI using data visualization tools with Azure HDInsight</span></span>

<span data-ttu-id="608b3-105">Lär dig hur toouse datavisualisering verktyg, till exempel Power BI och Tableau tooanalyze raw data exempeldata med Apache Spark BI på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="608b3-105">Learn how toouse data visualization tools such as Power BI and Tableau tooanalyze a raw sample data set using Apache Spark BI on HDInsight clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="608b3-106">Anslutning med BI-verktyg som beskrivs i denna artikel stöds inte på 2.1 Spark på Azure HDInsight 3,6 Preview.</span><span class="sxs-lookup"><span data-stu-id="608b3-106">Connectivity with BI tools described in this article is not supported on Spark 2.1 on Azure HDInsight 3.6 Preview.</span></span> <span data-ttu-id="608b3-107">Endast Spark version 1.6 och 2.0 (HDInsight 3.4, 3.5 respektive) stöds.</span><span class="sxs-lookup"><span data-stu-id="608b3-107">Only Spark versions 1.6 and 2.0 (HDInsight 3.4, 3.5 respectively) are supported.</span></span>
>

<span data-ttu-id="608b3-108">Den här kursen är också tillgängliga som en Jupyter-anteckningsbok på ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="608b3-108">This tutorial is also available as a Jupyter notebook on an HDInsight Spark cluster.</span></span> <span data-ttu-id="608b3-109">hello anteckningsboken upplevelse kan du köra hello Python kodavsnitt från hello anteckningsbok sig själv.</span><span class="sxs-lookup"><span data-stu-id="608b3-109">hello notebook experience lets you run hello Python snippets from hello notebook itself.</span></span> <span data-ttu-id="608b3-110">tooperform hello vägledningen i en bärbar dator, skapa ett Spark-kluster, starta en Jupyter-anteckningsbok (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), och kör sedan hello anteckningsboken **Använd BI-verktyg med Apache Spark i HDInsight.ipynb** under hello **Python**  mapp.</span><span class="sxs-lookup"><span data-stu-id="608b3-110">tooperform hello tutorial from within a notebook, create a Spark cluster, launch a Jupyter notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), and then run hello notebook **Use BI tools with Apache Spark on HDInsight.ipynb** under hello **Python** folder.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="608b3-111">Krav</span><span class="sxs-lookup"><span data-stu-id="608b3-111">Prerequisites</span></span>

* <span data-ttu-id="608b3-112">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="608b3-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="608b3-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="608b3-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>


## <span data-ttu-id="608b3-114"><a name="hivetable"></a>Förbered data för Spark datavisualisering</span><span class="sxs-lookup"><span data-stu-id="608b3-114"><a name="hivetable"></a>Prepare data for Spark data visualization</span></span>

<span data-ttu-id="608b3-115">I det här avsnittet ska vi använda hello [Jupyter](https://jupyter.org) bärbar dator från ett HDInsight Spark-kluster toorun jobb som bearbetar dina rådata exempeldata och spara den som en tabell.</span><span class="sxs-lookup"><span data-stu-id="608b3-115">In this section, we use hello [Jupyter](https://jupyter.org) notebook from an HDInsight Spark cluster toorun jobs that process your raw sample data and save it as a table.</span></span> <span data-ttu-id="608b3-116">hello exempeldata är en CSV-fil (hvac.csv) tillgängliga på alla kluster som standard.</span><span class="sxs-lookup"><span data-stu-id="608b3-116">hello sample data is a .csv file (hvac.csv) available on all clusters by default.</span></span> <span data-ttu-id="608b3-117">När du har sparat dina data som en tabell i nästa avsnitt om hello vi använder BI verktyg tooconnect toohello tabell och utföra datavisualiseringar.</span><span class="sxs-lookup"><span data-stu-id="608b3-117">Once your data is saved as a table, in hello next section we use BI tools tooconnect toohello table and perform data visualizations.</span></span>

> [!NOTE]
> <span data-ttu-id="608b3-118">Om du utför hello stegen i den här artikeln när du har slutfört hello instruktionerna i [köra interaktiva frågor på ett HDInsight Spark-kluster](hdinsight-apache-spark-load-data-run-query.md), kan du hoppa över tooStep 8 nedan.</span><span class="sxs-lookup"><span data-stu-id="608b3-118">If you are performing hello steps in this article after completing hello instructions in [Run interactive queries on an HDInsight Spark cluster](hdinsight-apache-spark-load-data-run-query.md), you can skip tooStep 8 below.</span></span>
>

1. <span data-ttu-id="608b3-119">Från hello [Azure-portalen](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="608b3-119">From hello [Azure portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="608b3-120">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="608b3-120">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="608b3-121">Hello Spark-klusterbladet, klicka på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="608b3-121">From hello Spark cluster blade, click **Cluster Dashboard**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="608b3-122">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="608b3-122">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="608b3-123">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="608b3-123">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="608b3-124">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="608b3-124">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="608b3-125">Skapa en anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="608b3-125">Create a notebook.</span></span> <span data-ttu-id="608b3-126">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="608b3-126">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="608b3-127">![Skapa en Jupyter-anteckningsbok för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "skapa en Jupyter-anteckningsbok för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-127">![Create a Jupyter notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Create a Jupyter notebook for Apache Spark BI")</span></span>

4. <span data-ttu-id="608b3-128">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="608b3-128">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="608b3-129">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="608b3-129">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="608b3-130">![Ange ett namn för anteckningsboken hello för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "ange ett namn för anteckningsboken hello för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-130">![Provide a name for hello notebook for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Provide a name for hello notebook for Apache Spark BI")</span></span>

5. <span data-ttu-id="608b3-131">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="608b3-131">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="608b3-132">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="608b3-132">hello Spark and Hive contexts are automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="608b3-133">Du kan starta genom att importera hello-typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="608b3-133">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="608b3-134">toodo så markören hello i hello cell och trycka på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="608b3-134">toodo so, place hello cursor in hello cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql import *

6. <span data-ttu-id="608b3-135">Läs in exempeldata i en tillfällig tabell.</span><span class="sxs-lookup"><span data-stu-id="608b3-135">Load sample data into a temporary table.</span></span> <span data-ttu-id="608b3-136">När du skapar ett Spark-kluster i HDInsight, hello exempeldatafil **hvac.csv**, är kopierade toohello associerade lagringskontot under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span><span class="sxs-lookup"><span data-stu-id="608b3-136">When you create a Spark cluster in HDInsight, hello sample data file, **hvac.csv**, is copied toohello associated storage account under **\HdiSamples\HdiSamples\SensorSampleData\hvac**.</span></span>

    <span data-ttu-id="608b3-137">Klistra in följande hello i en tom cell fragment och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="608b3-137">In an empty cell, paste hello following snippet and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="608b3-138">Den här fragment registrerar hello data i en tabell som kallas **hvac**.</span><span class="sxs-lookup"><span data-stu-id="608b3-138">This snippet registers hello data into a table called **hvac**.</span></span>

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

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

7. <span data-ttu-id="608b3-139">Kontrollera att hello-tabellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="608b3-139">Verify that hello table was successfully created.</span></span> <span data-ttu-id="608b3-140">Du kan använda hello `%%sql` magiska toorun Hive-frågor direkt.</span><span class="sxs-lookup"><span data-stu-id="608b3-140">You can use hello `%%sql` magic toorun Hive queries directly.</span></span> <span data-ttu-id="608b3-141">Mer information om hello `%%sql` magic och andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="608b3-141">For more information about hello `%%sql` magic, and other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SHOW TABLES

    <span data-ttu-id="608b3-142">Du se utdata som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="608b3-142">You see an output like shown below:</span></span>

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    <span data-ttu-id="608b3-143">Endast hello tabeller som har falskt under hello **isTemporary** kolumnen är hive-tabeller som är lagrade på hello metastore och kan nås från hello BI-verktyg.</span><span class="sxs-lookup"><span data-stu-id="608b3-143">Only hello tables that have false under hello **isTemporary** column are hive tables that are stored in hello metastore and can be accessed from hello BI tools.</span></span> <span data-ttu-id="608b3-144">I den här självstudiekursen kommer vi ansluta toohello **hvac** tabellen som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="608b3-144">In this tutorial, we connect toohello **hvac** table we created.</span></span>

8. <span data-ttu-id="608b3-145">Kontrollera hello tabellen innehåller hello avsedda data.</span><span class="sxs-lookup"><span data-stu-id="608b3-145">Verify that hello table contains hello intended data.</span></span> <span data-ttu-id="608b3-146">I en tom cell i hello anteckningsbok kopiera hello följande fragment och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="608b3-146">In an empty cell in hello notebook, copy hello following snippet and press **SHIFT + ENTER**.</span></span>

        %%sql
        SELECT * FROM hvac LIMIT 10

9. <span data-ttu-id="608b3-147">Stäng hello anteckningsboken toorelease hello resurser.</span><span class="sxs-lookup"><span data-stu-id="608b3-147">Shut down hello notebook toorelease hello resources.</span></span> <span data-ttu-id="608b3-148">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="608b3-148">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

## <span data-ttu-id="608b3-149"><a name="powerbi"></a>Använd Power BI för Spark datavisualisering</span><span class="sxs-lookup"><span data-stu-id="608b3-149"><a name="powerbi"></a>Use Power BI for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="608b3-150">Det här avsnittet gäller endast för 1.6 Spark på HDInsight 3.4 och 2.0 Spark på HDInsight 3.5.</span><span class="sxs-lookup"><span data-stu-id="608b3-150">This section is applicable only for Spark 1.6 on HDInsight 3.4 and Spark 2.0 on HDInsight 3.5.</span></span>
>
>

<span data-ttu-id="608b3-151">När du har sparat hello data som en tabell kan du använda Power BI tooconnect toohello data och visualisera den toocreate rapporter instrumentpaneler, osv.</span><span class="sxs-lookup"><span data-stu-id="608b3-151">Once you have saved hello data as a table, you can use Power BI tooconnect toohello data and visualize it toocreate reports, dashboards, etc.</span></span>

1. <span data-ttu-id="608b3-152">Kontrollera att du har åtkomst tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="608b3-152">Make sure you have access tooPower BI.</span></span> <span data-ttu-id="608b3-153">Du kan hämta en kostnadsfri förhandsversion prenumeration på Power BI från [http://www.powerbi.com/](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="608b3-153">You can get a free preview subscription of Power BI from [http://www.powerbi.com/](http://www.powerbi.com/).</span></span>

2. <span data-ttu-id="608b3-154">Logga in för[Power BI](http://www.powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="608b3-154">Sign in too[Power BI](http://www.powerbi.com/).</span></span>

3. <span data-ttu-id="608b3-155">Hello längst ned i hello vänstra rutan klickar du på **hämta Data**.</span><span class="sxs-lookup"><span data-stu-id="608b3-155">From hello bottom of hello left pane, click **Get Data**.</span></span>

4. <span data-ttu-id="608b3-156">På hello **hämta Data** sidan under **importera eller Anslut tooData**, för **databaser**, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="608b3-156">On hello **Get Data** page, under **Import or Connect tooData**, for **Databases**, click **Get**.</span></span>

    <span data-ttu-id="608b3-157">![Hämta data till Power BI för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "hämta data till Power BI för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-157">![Get data into Power BI for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Get data into Power BI for Apache Spark BI")</span></span>

5. <span data-ttu-id="608b3-158">Klicka på nästa skärm hello **Spark på Azure HDInsight** och klicka sedan på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="608b3-158">On hello next screen, click **Spark on Azure HDInsight** and then click **Connect**.</span></span> <span data-ttu-id="608b3-159">När du uppmanas att ange hello kluster-URL (`mysparkcluster.azurehdinsight.net`) och hello autentiseringsuppgifter tooconnect toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="608b3-159">When prompted, enter hello cluster URL (`mysparkcluster.azurehdinsight.net`) and hello credentials tooconnect toohello cluster.</span></span>

    <span data-ttu-id="608b3-160">![Ansluta tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "ansluta tooApache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-160">![Connect tooApache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Connect tooApache Spark BI")</span></span>

    <span data-ttu-id="608b3-161">Efter hello anslutningen har upprättats startar Power BI import av data från hello Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="608b3-161">After hello connection is established, Power BI starts importing data from hello Spark cluster on HDInsight.</span></span>

6. <span data-ttu-id="608b3-162">Powerbi importerar hello data och lägger till en **Spark** dataset under hello **datauppsättningar** rubrik.</span><span class="sxs-lookup"><span data-stu-id="608b3-162">Power BI imports hello data and adds a **Spark** dataset under hello **Datasets** heading.</span></span> <span data-ttu-id="608b3-163">Klicka på hello datauppsättning tooopen nya toovisualize hello kalkylbladsdata.</span><span class="sxs-lookup"><span data-stu-id="608b3-163">Click hello data set tooopen a new worksheet toovisualize hello data.</span></span> <span data-ttu-id="608b3-164">Du kan också spara hello kalkylbladet som en rapport.</span><span class="sxs-lookup"><span data-stu-id="608b3-164">You can also save hello worksheet as a report.</span></span> <span data-ttu-id="608b3-165">toosave ett kalkylblad från hello **filen** -menyn klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="608b3-165">toosave a worksheet, from hello **File** menu, click **Save**.</span></span>

    <span data-ttu-id="608b3-166">![Apache Spark BI-panelen på Power BI-instrumentpanel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI-panelen på Power BI-instrumentpanel")</span><span class="sxs-lookup"><span data-stu-id="608b3-166">![Apache Spark BI tile on Power BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI tile on Power BI dashboard")</span></span>
7. <span data-ttu-id="608b3-167">Observera att hello **fält** listan hello höger visar hello **hvac** tabellen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="608b3-167">Notice that hello **Fields** list on hello right lists hello **hvac** table you created earlier.</span></span> <span data-ttu-id="608b3-168">Expandera hello toosee hello fält i hello tabell, som du har definierat i anteckningsbok tidigare.</span><span class="sxs-lookup"><span data-stu-id="608b3-168">Expand hello table toosee hello fields in hello table, as you defined in notebook earlier.</span></span>

      <span data-ttu-id="608b3-169">![Visa en lista med tabeller på Apache Spark BI-instrumentpanel](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "lista tabeller på Apache Spark BI-instrumentpanel")</span><span class="sxs-lookup"><span data-stu-id="608b3-169">![List tables on Apache Spark BI dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "List tables on Apache Spark BI dashboard")</span></span>

8. <span data-ttu-id="608b3-170">Skapa en visualiseringen tooshow hello variansen mellan target temperatur- och faktiska temperatur för varje byggnad.</span><span class="sxs-lookup"><span data-stu-id="608b3-170">Build a visualization tooshow hello variance between target temperature and actual temperature for each building.</span></span> <span data-ttu-id="608b3-171">Välj toovisualize med data **ytdiagram** (visas med röd ruta).</span><span class="sxs-lookup"><span data-stu-id="608b3-171">toovisualize yoru data, select **Area Chart** (shown in red box).</span></span> <span data-ttu-id="608b3-172">toodefine hello axeln kan dra och släpp hello **BuildingID** fältet under **axel**, och **ActualTemp**/**TargetTemp** fält **värdet**.</span><span class="sxs-lookup"><span data-stu-id="608b3-172">toodefine hello axis, drag-and-drop hello **BuildingID** field under **Axis**, and **ActualTemp**/**TargetTemp** fields under **Value**.</span></span>

    <span data-ttu-id="608b3-173">![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "skapa Spark datavisualiseringar med Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-173">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Create Spark data visualizations using Apache Spark BI")</span></span>

9. <span data-ttu-id="608b3-174">Som standard visar hello visualiseringen hello summan för **ActualTemp** och **TargetTemp**.</span><span class="sxs-lookup"><span data-stu-id="608b3-174">By default hello visualization shows hello sum for **ActualTemp** and **TargetTemp**.</span></span> <span data-ttu-id="608b3-175">För båda hello fält från hello listrutan, Välj **genomsnittlig** tooget genomsnitt faktiska och mål temperaturer för båda byggnader.</span><span class="sxs-lookup"><span data-stu-id="608b3-175">For both hello fields, from hello drop-down, select **Average** tooget an average of actual and target temperatures for both buildings.</span></span>

    <span data-ttu-id="608b3-176">![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "skapa Spark datavisualiseringar med Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-176">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Create Spark data visualizations using Apache Spark BI")</span></span>

10. <span data-ttu-id="608b3-177">Visualisering av data ska vara liknande toohello något i hello skärmbilden.</span><span class="sxs-lookup"><span data-stu-id="608b3-177">Your data visualization should be similar toohello one in hello screenshot.</span></span> <span data-ttu-id="608b3-178">Flytta markören över hello visualiseringen tooget verktygstips med relevanta data.</span><span class="sxs-lookup"><span data-stu-id="608b3-178">Move your cursor over hello visualization tooget tool tips with relevant data.</span></span>

    <span data-ttu-id="608b3-179">![Skapa Spark visualisera data med Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "skapa Spark datavisualiseringar med Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-179">![Create Spark data visualizations using Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Create Spark data visualizations using Apache Spark BI")</span></span>

11. <span data-ttu-id="608b3-180">Klicka på **spara** från hello översta menyn och ange ett rapportnamn.</span><span class="sxs-lookup"><span data-stu-id="608b3-180">Click **Save** from hello top menu and provide a report name.</span></span> <span data-ttu-id="608b3-181">Du kan även fästa hello visual.</span><span class="sxs-lookup"><span data-stu-id="608b3-181">You can also pin hello visual.</span></span> <span data-ttu-id="608b3-182">När du fäster en visualisering lagras den på din instrumentpanel så att du kan spåra hello senaste värdet i korthet.</span><span class="sxs-lookup"><span data-stu-id="608b3-182">When you pin a visualization, it is stored on your dashboard so you can track hello latest value at a glance.</span></span>

   <span data-ttu-id="608b3-183">Du kan lägga till så många visualiseringar som du vill använda för hello samma datamängd och fästa dem toohello instrumentpanel för en ögonblicksbild av dina data.</span><span class="sxs-lookup"><span data-stu-id="608b3-183">You can add as many visualizations as you want for hello same dataset and pin them toohello dashboard for a snapshot of your data.</span></span> <span data-ttu-id="608b3-184">Spark-kluster i HDInsight är också ansluten tooPower BI med direct connect.</span><span class="sxs-lookup"><span data-stu-id="608b3-184">Also, Spark clusters on HDInsight are connected tooPower BI with direct connect.</span></span> <span data-ttu-id="608b3-185">Detta säkerställer att Power BI har alltid hello de flesta uppdaterade data från ditt kluster så du inte behöver tooschedule uppdateringar för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="608b3-185">This ensures that Power BI always has hello most up-to-date data from your cluster so you do not need tooschedule refreshes for hello dataset.</span></span>

## <span data-ttu-id="608b3-186"><a name="tableau"></a>Använda Tableau skrivbordet för Spark datavisualisering</span><span class="sxs-lookup"><span data-stu-id="608b3-186"><a name="tableau"></a>Use Tableau Desktop for Spark data visualization</span></span>

> [!NOTE]
> <span data-ttu-id="608b3-187">Det här avsnittet gäller endast för Spark 1.5.2 kluster som skapas i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="608b3-187">This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.</span></span>
>
>

1. <span data-ttu-id="608b3-188">Installera [Tableau Desktop](http://www.tableau.com/products/desktop) på hello dator där du kör den här kursen om Apache Spark BI.</span><span class="sxs-lookup"><span data-stu-id="608b3-188">Install [Tableau Desktop](http://www.tableau.com/products/desktop) on hello computer where you are running this Apache Spark BI tutorial.</span></span>

2. <span data-ttu-id="608b3-189">Kontrollera att datorn har även Microsoft Spark ODBC-drivrutinen ska installeras.</span><span class="sxs-lookup"><span data-stu-id="608b3-189">Make sure that computer also has Microsoft Spark ODBC driver installed.</span></span> <span data-ttu-id="608b3-190">Du kan installera hello drivrutin från [här](http://go.microsoft.com/fwlink/?LinkId=616229).</span><span class="sxs-lookup"><span data-stu-id="608b3-190">You can install hello driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).</span></span>

1. <span data-ttu-id="608b3-191">Starta Tableau skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="608b3-191">Launch Tableau Desktop.</span></span> <span data-ttu-id="608b3-192">Klicka på hello vänster hello listan över server tooconnect, **Spark SQL**.</span><span class="sxs-lookup"><span data-stu-id="608b3-192">In hello left pane, from hello list of server tooconnect to, click **Spark SQL**.</span></span> <span data-ttu-id="608b3-193">Om inte Spark SQL visas som standard i hello vänster hittar du den genom att klicka på **fler servrar**.</span><span class="sxs-lookup"><span data-stu-id="608b3-193">If Spark SQL is not listed by default in hello left pane, you can find it by click **More Servers**.</span></span>
2. <span data-ttu-id="608b3-194">Ange hello värden hello Spark SQL-anslutningen i dialogrutan som visas i skärmbilden hello och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="608b3-194">In hello Spark SQL connection dialog box, provide hello values as shown in hello screenshot, and then click **OK**.</span></span>

    <span data-ttu-id="608b3-195">![Anslut tooa kluster för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Anslut tooa kluster för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-195">![Connect tooa cluster for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect tooa cluster for Apache Spark BI")</span></span>

    <span data-ttu-id="608b3-196">Hej autentisering listrutorna **Microsoft Azure HDInsight-tjänst** som ett alternativ, endast om du har installerat hello [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="608b3-196">hello authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed hello [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on hello computer.</span></span>
3. <span data-ttu-id="608b3-197">På nästa hello-skärmen, från hello **schemat** listrutan, klickar du på hello **hitta** ikonen och klickar sedan på **standard**.</span><span class="sxs-lookup"><span data-stu-id="608b3-197">On hello next screen, from hello **Schema** drop-down, click hello **Find** icon, and then click **default**.</span></span>

    <span data-ttu-id="608b3-198">![Hitta schema för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "hitta schemat för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-198">![Find schema for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")</span></span>
4. <span data-ttu-id="608b3-199">För hello **tabell** klickar hello **hitta** ikonen igen toolist alla hello Hive tabeller som är tillgängliga i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="608b3-199">For hello **Table** field, click hello **Find** icon again toolist all hello Hive tables available in hello cluster.</span></span> <span data-ttu-id="608b3-200">Du bör se hello **hvac** tabellen som du skapat tidigare med hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="608b3-200">You should see hello **hvac** table you created earlier using hello notebook.</span></span>

    <span data-ttu-id="608b3-201">![Hitta tabellen för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "hitta tabellen för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-201">![Find table for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")</span></span>
5. <span data-ttu-id="608b3-202">Dra och släpp hello tabell toohello översta rutan på hello rätt.</span><span class="sxs-lookup"><span data-stu-id="608b3-202">Drag and drop hello table toohello top box on hello right.</span></span> <span data-ttu-id="608b3-203">Tableau importerar hello data och visar hello schemat som markerade av hello röd ruta.</span><span class="sxs-lookup"><span data-stu-id="608b3-203">Tableau imports hello data and displays hello schema as highlighted by hello red box.</span></span>

    <span data-ttu-id="608b3-204">![Lägga till tabeller tooTableau för Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "lägga till tabeller tooTableau för Apache Spark BI")</span><span class="sxs-lookup"><span data-stu-id="608b3-204">![Add tables tooTableau for Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables tooTableau for Apache Spark BI")</span></span>
6. <span data-ttu-id="608b3-205">Klicka på hello **Blad1** högst hello längst ned till vänster.</span><span class="sxs-lookup"><span data-stu-id="608b3-205">Click hello **Sheet1** tab at hello bottom left.</span></span> <span data-ttu-id="608b3-206">Gör en visualisering som visar hello genomsnittlig mål och faktiska temperaturer för alla byggnader för varje datum.</span><span class="sxs-lookup"><span data-stu-id="608b3-206">Make a visualization that shows hello average target and actual temperatures for all buildings for each date.</span></span> <span data-ttu-id="608b3-207">Dra **datum** och **byggnad ID** för**kolumner** och **faktiska Temp**/**mål Temp**för**rader**.</span><span class="sxs-lookup"><span data-stu-id="608b3-207">Drag **Date** and **Building ID** too**Columns** and **Actual Temp**/**Target Temp** too**Rows**.</span></span> <span data-ttu-id="608b3-208">Under **märken**väljer **området** toouse en området mappning för Spark datavisualisering.</span><span class="sxs-lookup"><span data-stu-id="608b3-208">Under **Marks**, select **Area** toouse an area map for Spark data visualization.</span></span>

     <span data-ttu-id="608b3-209">![Lägga till fält i Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "lägga till fält i Spark datavisualisering")</span><span class="sxs-lookup"><span data-stu-id="608b3-209">![Add fields for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")</span></span>
7. <span data-ttu-id="608b3-210">Som standard hello temperatur fält visas som aggregat.</span><span class="sxs-lookup"><span data-stu-id="608b3-210">By default, hello temperature fields are shown as aggregate.</span></span> <span data-ttu-id="608b3-211">Om du vill tooshow hello genomsnittlig temperaturer i stället kan göra du det från hello nedrullningsbara enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="608b3-211">If you want tooshow hello average temperatures instead, you can do so from hello drop-down, as shown below.</span></span>

    <span data-ttu-id="608b3-212">![Ta medelvärde för temperaturen för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "ta medelvärde för temperaturen för Spark datavisualisering")</span><span class="sxs-lookup"><span data-stu-id="608b3-212">![Take average of temperature for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")</span></span>

8. <span data-ttu-id="608b3-213">Du kan också super-införa en temperatur mappning över hello andra tooget en bättre känsla skillnaden mellan mål- och faktiska temperaturer.</span><span class="sxs-lookup"><span data-stu-id="608b3-213">You can also super-impose one temperature map over hello other tooget a better feel of difference between target and actual temperatures.</span></span> <span data-ttu-id="608b3-214">Flytta hello musen toohello hörn hello lägre området kartan tills du ser hello referensen formen markerade med en röd cirkel.</span><span class="sxs-lookup"><span data-stu-id="608b3-214">Move hello mouse toohello corner of hello lower area map till you see hello handle shape highlighted in a red circle.</span></span> <span data-ttu-id="608b3-215">Dra hello mappa toohello andra kartan på hello upp och version hello musen när du ser hello formen markerat i rött rektangel.</span><span class="sxs-lookup"><span data-stu-id="608b3-215">Drag hello map toohello other map on hello top and release hello mouse when you see hello shape highlighted in red rectangle.</span></span>

    <span data-ttu-id="608b3-216">![Sammanfoga för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge mappar för Spark datavisualisering")</span><span class="sxs-lookup"><span data-stu-id="608b3-216">![Merge maps for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")</span></span>

     <span data-ttu-id="608b3-217">Ändra din datavisualisering som visas i skärmbilden hello:</span><span class="sxs-lookup"><span data-stu-id="608b3-217">Your data visualization should change as shown in hello screenshot:</span></span>

    <span data-ttu-id="608b3-218">![Tableau utdata för Spark datavisualisering](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau utdata för Spark datavisualisering")</span><span class="sxs-lookup"><span data-stu-id="608b3-218">![Tableau output for Spark data visualization](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")</span></span>
9. <span data-ttu-id="608b3-219">Klicka på **spara** toosave hello kalkylblad.</span><span class="sxs-lookup"><span data-stu-id="608b3-219">Click **Save** toosave hello worksheet.</span></span> <span data-ttu-id="608b3-220">Du kan skapa instrumentpaneler och lägga till en eller flera blad tooit.</span><span class="sxs-lookup"><span data-stu-id="608b3-220">You can create dashboards and add one or more sheets tooit.</span></span>

## <a name="next-steps"></a><span data-ttu-id="608b3-221">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="608b3-221">Next steps</span></span>

<span data-ttu-id="608b3-222">Så länge du lärt dig hur toocreate ett kluster, skapa Spark data ramar tooquery data och komma åt dessa data från BI-verktyg.</span><span class="sxs-lookup"><span data-stu-id="608b3-222">So far you learned how toocreate a cluster, create Spark data frames tooquery data, and then access that data from BI tools.</span></span> <span data-ttu-id="608b3-223">Du kan nu se instruktioner för hur toomanage hello klusterresurser och felsöka jobb som körs i ett HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="608b3-223">You can now look at instructions on how toomanage hello cluster resources and debug jobs that are running in an HDInsight Spark cluster.</span></span>

* [<span data-ttu-id="608b3-224">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="608b3-224">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="608b3-225">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="608b3-225">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

