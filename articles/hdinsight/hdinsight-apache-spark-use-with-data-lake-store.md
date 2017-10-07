---
title: aaaUse Apache Spark tooanalyze data i Azure Data Lake Store | Microsoft Docs
description: "Köra Spark jobb tooanalyze data som lagras i Azure Data Lake Store"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 1f174323-c17b-428c-903d-04f0e272784c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 3b7f628f7a8114d2ca6f3f9219ce107905f1c818
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hdinsight-spark-cluster-tooanalyze-data-in-data-lake-store"></a><span data-ttu-id="7ac91-103">Använda HDInsight Spark-kluster tooanalyze data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7ac91-103">Use HDInsight Spark cluster tooanalyze data in Data Lake Store</span></span>

<span data-ttu-id="7ac91-104">I den här kursen använder du Jupyter-anteckningsbok tillgänglig med HDInsight Spark-kluster toorun ett jobb som läser data från ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters toorun a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7ac91-105">Krav</span><span class="sxs-lookup"><span data-stu-id="7ac91-105">Prerequisites</span></span>

* <span data-ttu-id="7ac91-106">Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="7ac91-107">Följ instruktionerna för hello på [Kom igång med Azure Data Lake Store med hjälp av hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ac91-107">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="7ac91-108">Azure HDInsight Spark-kluster med Data Lake Store som lagring.</span><span class="sxs-lookup"><span data-stu-id="7ac91-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="7ac91-109">Följ instruktionerna för hello på [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7ac91-109">Follow hello instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-hello-data"></a><span data-ttu-id="7ac91-110">Förbereda hello data</span><span class="sxs-lookup"><span data-stu-id="7ac91-110">Prepare hello data</span></span>

> [!NOTE]
> <span data-ttu-id="7ac91-111">Du behöver inte tooperform det här steget om du har skapat hello HDInsight-kluster med Data Lake Store som standardlagring.</span><span class="sxs-lookup"><span data-stu-id="7ac91-111">You do not need tooperform this step if you have created hello HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="7ac91-112">hello klustret gruppskapande processer lägger till exempeldata i hello Data Lake Store-konto som du anger när du skapar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="7ac91-112">hello cluster creation processes adds some sample data in hello Data Lake Store account that you specify while creating hello cluster.</span></span> <span data-ttu-id="7ac91-113">Hoppa över toohello avsnittet [använda HDInsight Spark-kluster med Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="7ac91-113">Skip toohello section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="7ac91-114">Om du har skapat ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme och Azure Storage Blob som standardlagring, bör du först kopiera över vissa exempel data toohello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data toohello Data Lake Store account.</span></span> <span data-ttu-id="7ac91-115">Du kan använda hello exempeldata från hello Azure Storage Blob som är associerade med hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7ac91-115">You can use hello sample data from hello Azure Storage Blob associated with hello HDInsight cluster.</span></span> <span data-ttu-id="7ac91-116">Du kan använda hello [ADLCopy verktyget](http://aka.ms/downloadadlcopy) toodo så.</span><span class="sxs-lookup"><span data-stu-id="7ac91-116">You can use hello [ADLCopy tool](http://aka.ms/downloadadlcopy) toodo so.</span></span> <span data-ttu-id="7ac91-117">Hämta och installera hello verktyget från hello länk.</span><span class="sxs-lookup"><span data-stu-id="7ac91-117">Download and install hello tool from hello link.</span></span>

1. <span data-ttu-id="7ac91-118">Öppna en kommandotolk och navigera toohello directory där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="7ac91-118">Open a command prompt and navigate toohello directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="7ac91-119">Kör följande kommando toocopy hello en specifik blobb från hello källa behållaren tooa Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="7ac91-119">Run hello following command toocopy a specific blob from hello source container tooa Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="7ac91-120">Kopiera hello **HVAC.csv** exempeldata filen på **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-120">Copy hello **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** toohello Azure Data Lake Store account.</span></span> <span data-ttu-id="7ac91-121">hello kodstycke bör se ut som:</span><span class="sxs-lookup"><span data-stu-id="7ac91-121">hello code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="7ac91-122">Kontrollera att du hello fil och sökvägar i hello rätt skiftläge.</span><span class="sxs-lookup"><span data-stu-id="7ac91-122">Make sure you hello file and path names are in hello proper case.</span></span>
   >
   >
3. <span data-ttu-id="7ac91-123">Du kan ange tooenter hello autentiseringsuppgifter för hello Azure-prenumeration som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-123">You will be prompted tooenter hello credentials for hello Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="7ac91-124">En liknande toohello följande i utdata visas:</span><span class="sxs-lookup"><span data-stu-id="7ac91-124">You will see an output similar toohello following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="7ac91-125">hello-datafil (**HVAC.csv**) kopieras under en mapp **/hvac** i hello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-125">hello data file (**HVAC.csv**) will be copied under a folder **/hvac** in hello Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="7ac91-126">Använda ett HDInsight Spark-kluster med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="7ac91-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="7ac91-127">Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="7ac91-127">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="7ac91-128">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-128">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="7ac91-129">Hello Spark-klusterbladet, klicka på **snabblänkar**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-129">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="7ac91-130">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="7ac91-130">If prompted, enter hello admin credentials for hello cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7ac91-131">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="7ac91-131">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="7ac91-132">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="7ac91-132">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="7ac91-133">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="7ac91-133">Create a new notebook.</span></span> <span data-ttu-id="7ac91-134">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="7ac91-135">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="7ac91-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="7ac91-136">Eftersom du har skapat anteckningsboken med hello PySpark-kerneln, behöver du inte toocreate några kontexter explicit.</span><span class="sxs-lookup"><span data-stu-id="7ac91-136">Because you created a notebook using hello PySpark kernel, you do not need toocreate any contexts explicitly.</span></span> <span data-ttu-id="7ac91-137">hello Spark och Hive-kontexterna skapas automatiskt för dig när du kör hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="7ac91-137">hello Spark and Hive contexts will be automatically created for you when you run hello first code cell.</span></span> <span data-ttu-id="7ac91-138">Du kan starta genom att importera hello-typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="7ac91-138">You can start by importing hello types required for this scenario.</span></span> <span data-ttu-id="7ac91-139">toodo så klistra in hello följande kodfragment i en cell och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-139">toodo so, paste hello following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="7ac91-140">Varje gång du kör ett jobb i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel.</span><span class="sxs-lookup"><span data-stu-id="7ac91-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="7ac91-141">Du kan även se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="7ac91-141">You will also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="7ac91-142">När hello jobbet har slutförts ändras tooa tom cirkel.</span><span class="sxs-lookup"><span data-stu-id="7ac91-142">After hello job is completed, this will change tooa hollow circle.</span></span>

     <span data-ttu-id="7ac91-143">![Status för ett Jupyter-anteckningsboksjobb](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status för ett Jupyter-anteckningsboksjobb")</span><span class="sxs-lookup"><span data-stu-id="7ac91-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="7ac91-144">Läs in exempeldata i en tillfällig tabell med hello **HVAC.csv** filen som du kopierade toohello Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="7ac91-144">Load sample data into a temporary table using hello **HVAC.csv** file you copied toohello Data Lake Store account.</span></span> <span data-ttu-id="7ac91-145">Du kan komma åt hello data i hello Data Lake Store-konto med hjälp av hello följande URL-mönster.</span><span class="sxs-lookup"><span data-stu-id="7ac91-145">You can access hello data in hello Data Lake Store account using hello following URL pattern.</span></span>

    * <span data-ttu-id="7ac91-146">Om du har Data Lake Store som standardlagring blir HVAC.csv på hello sökväg liknande toohello följande URL:</span><span class="sxs-lookup"><span data-stu-id="7ac91-146">If you have Data Lake Store as default storage, HVAC.csv will be at hello path similar toohello following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="7ac91-147">Eller, du kan också använda ett kortare format, till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="7ac91-147">Or, you could also use a shortened format such as hello following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="7ac91-148">Om du har Data Lake Store som ytterligare lagringsutrymme, kommer att vara hello platsen där du har kopierat, t.ex HVAC.csv:</span><span class="sxs-lookup"><span data-stu-id="7ac91-148">If you have Data Lake Store as additional storage, HVAC.csv will be at hello location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="7ac91-149">I en tom cell klistra in hello följande kodexempel, ersätter **MYDATALAKESTORE** med Data Lake Store-kontonamnet och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-149">In an empty cell, paste hello following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="7ac91-150">Den här kodexemplet registrerar hello data till en tillfällig tabell som kallas **hvac**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-150">This code example registers hello data into a temporary table called **hvac**.</span></span>

            # Load hello data. hello path below assumes Data Lake Store is default storage for hello Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create hello schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse hello data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register hello data fram as a table toorun queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="7ac91-151">Eftersom du använder en PySpark-kerneln, du kan nu direkt köra en SQL-fråga på hello tillfällig tabell **hvac** som du nyss skapade med hjälp av hello `%%sql` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="7ac91-151">Because you are using a PySpark kernel, you can now directly run a SQL query on hello temporary table **hvac** that you just created by using hello `%%sql` magic.</span></span> <span data-ttu-id="7ac91-152">Mer information om hello `%%sql` magic, samt andra användbara med hello PySpark-kerneln, finns [kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="7ac91-152">For more information about hello `%%sql` magic, as well as other magics available with hello PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="7ac91-153">När hello jobbet har slutförts, visas hello följande tabular utdata som standard.</span><span class="sxs-lookup"><span data-stu-id="7ac91-153">Once hello job is completed successfully, hello following tabular output is displayed by default.</span></span>

      <span data-ttu-id="7ac91-154">![Tabellutdata från frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabellutdata från frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="7ac91-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="7ac91-155">Du kan också se hello resulterar i andra visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="7ac91-155">You can also see hello results in other visualizations as well.</span></span> <span data-ttu-id="7ac91-156">Till exempel detta ett Områdesdiagram för samma utdata skulle se ut hello följande hello.</span><span class="sxs-lookup"><span data-stu-id="7ac91-156">For example, an area graph for hello same output would look like hello following.</span></span>

     <span data-ttu-id="7ac91-157">![Områdesdiagram över frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Områdesdiagram över frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="7ac91-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="7ac91-158">När du har kört programmet hello ska avstängning hello anteckningsboken toorelease hello resurser.</span><span class="sxs-lookup"><span data-stu-id="7ac91-158">After you have finished running hello application, you should shutdown hello notebook toorelease hello resources.</span></span> <span data-ttu-id="7ac91-159">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="7ac91-159">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span> <span data-ttu-id="7ac91-160">Den här stängs och Stäng hello bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="7ac91-160">This will shutdown and close hello notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="7ac91-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ac91-161">Next steps</span></span>

* [<span data-ttu-id="7ac91-162">Skapa en fristående Scala programmet toorun på Apache Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7ac91-162">Create a standalone Scala application toorun on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="7ac91-163">Använda HDInsight Tools i Azure Toolkit för IntelliJ toocreate Spark-program för HDInsight Spark Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="7ac91-163">Use HDInsight Tools in Azure Toolkit for IntelliJ toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="7ac91-164">Använda HDInsight Tools i Azure Toolkit för Eclipse toocreate Spark-program för HDInsight Spark Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="7ac91-164">Use HDInsight Tools in Azure Toolkit for Eclipse toocreate Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
