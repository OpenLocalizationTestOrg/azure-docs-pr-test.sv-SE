---
title: "Använda Apache Spark för att analysera data i Azure Data Lake Store | Microsoft Docs"
description: "Köra Spark jobb för att analysera data som lagras i Azure Data Lake Store"
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
ms.openlocfilehash: 66f115c4f348ccaeb8855568ba1ad50faa442173
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-hdinsight-spark-cluster-to-analyze-data-in-data-lake-store"></a><span data-ttu-id="26aef-103">Använda HDInsight Spark-klustret för att analysera data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="26aef-103">Use HDInsight Spark cluster to analyze data in Data Lake Store</span></span>

<span data-ttu-id="26aef-104">I den här kursen använder du Jupyter-anteckningsbok tillgänglig med HDInsight Spark-kluster för att köra ett jobb som läser data från ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-104">In this tutorial, you use Jupyter notebook available with HDInsight Spark clusters to run a job that reads data from a Data Lake Store account.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26aef-105">Krav</span><span class="sxs-lookup"><span data-stu-id="26aef-105">Prerequisites</span></span>

* <span data-ttu-id="26aef-106">Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-106">Azure Data Lake Store account.</span></span> <span data-ttu-id="26aef-107">Följ instruktionerna i [Kom igång med Azure Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="26aef-107">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](../data-lake-store/data-lake-store-get-started-portal.md).</span></span>

* <span data-ttu-id="26aef-108">Azure HDInsight Spark-kluster med Data Lake Store som lagring.</span><span class="sxs-lookup"><span data-stu-id="26aef-108">Azure HDInsight Spark cluster with Data Lake Store as storage.</span></span> <span data-ttu-id="26aef-109">Följ anvisningarna på [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="26aef-109">Follow the instructions at [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

    
## <a name="prepare-the-data"></a><span data-ttu-id="26aef-110">Förbered data</span><span class="sxs-lookup"><span data-stu-id="26aef-110">Prepare the data</span></span>

> [!NOTE]
> <span data-ttu-id="26aef-111">Du behöver inte göra detta om du har skapat HDInsight-kluster med Data Lake Store som standardlagring.</span><span class="sxs-lookup"><span data-stu-id="26aef-111">You do not need to perform this step if you have created the HDInsight cluster with Data Lake Store as default storage.</span></span> <span data-ttu-id="26aef-112">Klustret gruppskapande processer lägger till exempeldata i Data Lake Store-konto som du anger när du skapar klustret.</span><span class="sxs-lookup"><span data-stu-id="26aef-112">The cluster creation processes adds some sample data in the Data Lake Store account that you specify while creating the cluster.</span></span> <span data-ttu-id="26aef-113">Gå till avsnittet [använda HDInsight Spark-kluster med Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="26aef-113">Skip to the section [Use HDInsight Spark cluster with Data Lake Store](#use-an-hdinsight-spark-cluster-with-data-lake-store).</span></span>
>
>

<span data-ttu-id="26aef-114">Om du har skapat ett HDInsight-kluster med Data Lake Store som ytterligare lagringsutrymme och Azure Storage Blob som standardlagring, bör du först kopiera över exempeldata till Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-114">If you created an HDInsight cluster with Data Lake Store as additional storage and Azure Storage Blob as default storage, you should first copy over some sample data to the Data Lake Store account.</span></span> <span data-ttu-id="26aef-115">Du kan använda exemplet data från Azure Storage Blob som är associerade med HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="26aef-115">You can use the sample data from the Azure Storage Blob associated with the HDInsight cluster.</span></span> <span data-ttu-id="26aef-116">Du kan använda den [ADLCopy verktyget](http://aka.ms/downloadadlcopy) gör.</span><span class="sxs-lookup"><span data-stu-id="26aef-116">You can use the [ADLCopy tool](http://aka.ms/downloadadlcopy) to do so.</span></span> <span data-ttu-id="26aef-117">Hämta och installera verktyget från länken.</span><span class="sxs-lookup"><span data-stu-id="26aef-117">Download and install the tool from the link.</span></span>

1. <span data-ttu-id="26aef-118">Öppna en kommandotolk och gå till den katalog där AdlCopy installeras normalt `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="26aef-118">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>

2. <span data-ttu-id="26aef-119">Kör följande kommando för att kopiera en specifik blobb från käll-behållare till ett Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="26aef-119">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="26aef-120">Kopiera den **HVAC.csv** exempeldata filen på **/HdiSamples/HdiSamples/SensorSampleData/hvac/** till Azure Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-120">Copy the **HVAC.csv** sample data file at **/HdiSamples/HdiSamples/SensorSampleData/hvac/** to the Azure Data Lake Store account.</span></span> <span data-ttu-id="26aef-121">Kodfragmentet bör se ut som:</span><span class="sxs-lookup"><span data-stu-id="26aef-121">The code snippet should look like:</span></span>

        AdlCopy /Source https://mydatastore.blob.core.windows.net/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv /dest swebhdfs://mydatalakestore.azuredatalakestore.net/hvac/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

   > [!WARNING]
   > <span data-ttu-id="26aef-122">Kontrollera att du namn och sökvägen har rätt skiftläge.</span><span class="sxs-lookup"><span data-stu-id="26aef-122">Make sure you the file and path names are in the proper case.</span></span>
   >
   >
3. <span data-ttu-id="26aef-123">Du uppmanas att ange autentiseringsuppgifter för Azure-prenumerationen som du har ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-123">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="26aef-124">Du kommer se utdata som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="26aef-124">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Copy Completed. 1 file copied.

    <span data-ttu-id="26aef-125">Datafilen (**HVAC.csv**) kopieras under en mapp **/hvac** i Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-125">The data file (**HVAC.csv**) will be copied under a folder **/hvac** in the Data Lake Store account.</span></span>

## <a name="use-an-hdinsight-spark-cluster-with-data-lake-store"></a><span data-ttu-id="26aef-126">Använda ett HDInsight Spark-kluster med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="26aef-126">Use an HDInsight Spark cluster with Data Lake Store</span></span>

1. <span data-ttu-id="26aef-127">På startsidan i [Azure-portalen](https://portal.azure.com/) klickar du på panelen för ditt Spark-kluster (om du har fäst det på startsidan).</span><span class="sxs-lookup"><span data-stu-id="26aef-127">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="26aef-128">Du kan också navigera till ditt kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="26aef-128">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>

2. <span data-ttu-id="26aef-129">Klicka på **Snabblänkar** på Spark-klusterbladet och sedan på **Jupyter Notebook** på **Klusterinstrumentpanel**-bladet.</span><span class="sxs-lookup"><span data-stu-id="26aef-129">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="26aef-130">Ange administratörsautentiseringsuppgifterna för klustret om du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="26aef-130">If prompted, enter the admin credentials for the cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="26aef-131">Du kan också nå Jupyter Notebook för ditt kluster genom att öppna nedanstående URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="26aef-131">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="26aef-132">Ersätt **CLUSTERNAME** med namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="26aef-132">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. <span data-ttu-id="26aef-133">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="26aef-133">Create a new notebook.</span></span> <span data-ttu-id="26aef-134">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="26aef-134">Click **New**, and then click **PySpark**.</span></span>

    <span data-ttu-id="26aef-135">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="26aef-135">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-create-jupyter-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="26aef-136">Du behöver inte uttryckligen skapa några kontexter eftersom du har skapat anteckningsboken med hjälp av PySpark-kerneln.</span><span class="sxs-lookup"><span data-stu-id="26aef-136">Because you created a notebook using the PySpark kernel, you do not need to create any contexts explicitly.</span></span> <span data-ttu-id="26aef-137">Spark- och Hive-kontexterna skapas automatiskt för dig när du kör den första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="26aef-137">The Spark and Hive contexts will be automatically created for you when you run the first code cell.</span></span> <span data-ttu-id="26aef-138">Du kan börja med att importera de typer som krävs för det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="26aef-138">You can start by importing the types required for this scenario.</span></span> <span data-ttu-id="26aef-139">Det gör du genom att klistra in följande kodfragment i en cell och trycka på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="26aef-139">To do so, paste the following code snippet in a cell and press **SHIFT + ENTER**.</span></span>

        from pyspark.sql.types import *

    <span data-ttu-id="26aef-140">Varje gång du kör ett jobb i Jupyter kommer fönsterrubriken i din webbläsare att visa statusen **(Upptagen)** tillsammans med anteckningsbokens titel.</span><span class="sxs-lookup"><span data-stu-id="26aef-140">Every time you run a job in Jupyter, your web browser window title will show a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="26aef-141">Du kan även se en fylld cirkel bredvid **PySpark**-texten i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="26aef-141">You will also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="26aef-142">När jobbet har slutförts ändras denna till en tom cirkel.</span><span class="sxs-lookup"><span data-stu-id="26aef-142">After the job is completed, this will change to a hollow circle.</span></span>

     <span data-ttu-id="26aef-143">![Status för ett Jupyter-anteckningsboksjobb](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status för ett Jupyter-anteckningsboksjobb")</span><span class="sxs-lookup"><span data-stu-id="26aef-143">![Status of a Jupyter notebook job](./media/hdinsight-apache-spark-use-with-data-lake-store/hdinsight-jupyter-job-status.png "Status of a Jupyter notebook job")</span></span>

5. <span data-ttu-id="26aef-144">Läs in exempeldata i en tillfällig tabell med hjälp av den **HVAC.csv** filen som du kopierade till Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="26aef-144">Load sample data into a temporary table using the **HVAC.csv** file you copied to the Data Lake Store account.</span></span> <span data-ttu-id="26aef-145">Du kan komma åt data i Data Lake Store-konto med hjälp av följande URL-mönster.</span><span class="sxs-lookup"><span data-stu-id="26aef-145">You can access the data in the Data Lake Store account using the following URL pattern.</span></span>

    * <span data-ttu-id="26aef-146">Om du har Data Lake Store som standardlagring blir HVAC.csv på sökvägen liknar följande URL:</span><span class="sxs-lookup"><span data-stu-id="26aef-146">If you have Data Lake Store as default storage, HVAC.csv will be at the path similar to the following URL:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<cluster_root>/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

        <span data-ttu-id="26aef-147">Eller, du kan också använda ett kortare format till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="26aef-147">Or, you could also use a shortened format such as the following:</span></span>

            adl:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv

    * <span data-ttu-id="26aef-148">Om du har Data Lake Store som ytterligare lagringsutrymme, blir HVAC.csv på den plats där du har kopierat, exempelvis:</span><span class="sxs-lookup"><span data-stu-id="26aef-148">If you have Data Lake Store as additional storage, HVAC.csv will be at the location where you copied it, such as:</span></span>

            adl://<data_lake_store_name>.azuredatalakestore.net/<path_to_file>

     <span data-ttu-id="26aef-149">Klistra in följande kodexempel i en tom cell ersätter **MYDATALAKESTORE** med Data Lake Store-kontonamnet och tryck på **SKIFT + RETUR**.</span><span class="sxs-lookup"><span data-stu-id="26aef-149">In an empty cell, paste the following code example, replace **MYDATALAKESTORE** with your Data Lake Store account name, and press **SHIFT + ENTER**.</span></span> <span data-ttu-id="26aef-150">Den här kodexemplet registrerar data i en tillfällig tabell som kallas **hvac**.</span><span class="sxs-lookup"><span data-stu-id="26aef-150">This code example registers the data into a temporary table called **hvac**.</span></span>

            # Load the data. The path below assumes Data Lake Store is default storage for the Spark cluster
            hvacText = sc.textFile("adl://MYDATALAKESTORE.azuredatalakestore.net/cluster/mysparkcluster/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

            # Create the schema
            hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

            # Parse the data in hvacText
            hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

            # Create a data frame
            hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

            # Register the data fram as a table to run queries against
            hvacdf.registerTempTable("hvac")

6. <span data-ttu-id="26aef-151">Eftersom du använder en PySpark-kernel kan du nu direkt köra en SQL-fråga för den tillfälliga tabellen **hvac** som du just skapade med den användbara `%%sql`-funktionen.</span><span class="sxs-lookup"><span data-stu-id="26aef-151">Because you are using a PySpark kernel, you can now directly run a SQL query on the temporary table **hvac** that you just created by using the `%%sql` magic.</span></span> <span data-ttu-id="26aef-152">Mer information om den användbara `%%sql`, samt andra mycket användbara funktioner hos PySpark-kerneln, finns i [Kernlar som är tillgängliga i Jupyter-anteckningsböcker med HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span><span class="sxs-lookup"><span data-stu-id="26aef-152">For more information about the `%%sql` magic, as well as other magics available with the PySpark kernel, see [Kernels available on Jupyter notebooks with Spark HDInsight clusters](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).</span></span>

        %%sql
        SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"

7. <span data-ttu-id="26aef-153">När jobbet har slutförts visas följande tabellutdata som standard.</span><span class="sxs-lookup"><span data-stu-id="26aef-153">Once the job is completed successfully, the following tabular output is displayed by default.</span></span>

      <span data-ttu-id="26aef-154">![Tabellutdata från frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Tabellutdata från frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="26aef-154">![Table output of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-tabular-output.png "Table output of query result")</span></span>

     <span data-ttu-id="26aef-155">Du kan också visa resultaten i andra visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="26aef-155">You can also see the results in other visualizations as well.</span></span> <span data-ttu-id="26aef-156">Ett områdesdiagram för samma utdata skulle som exempel se ut enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="26aef-156">For example, an area graph for the same output would look like the following.</span></span>

     <span data-ttu-id="26aef-157">![Områdesdiagram över frågeresultat](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Områdesdiagram över frågeresultat")</span><span class="sxs-lookup"><span data-stu-id="26aef-157">![Area graph of query result](./media/hdinsight-apache-spark-use-with-data-lake-store/jupyter-area-output.png "Area graph of query result")</span></span>

8. <span data-ttu-id="26aef-158">När du har kört appen bör du stänga ned anteckningsboken för att frigöra resurser.</span><span class="sxs-lookup"><span data-stu-id="26aef-158">After you have finished running the application, you should shutdown the notebook to release the resources.</span></span> <span data-ttu-id="26aef-159">Du gör det genom att klicka på **Stäng och stoppa** i anteckningsbokens **Fil**-meny.</span><span class="sxs-lookup"><span data-stu-id="26aef-159">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span> <span data-ttu-id="26aef-160">Då avslutas anteckningsboken och stängs ned.</span><span class="sxs-lookup"><span data-stu-id="26aef-160">This will shutdown and close the notebook.</span></span>


## <a name="next-steps"></a><span data-ttu-id="26aef-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="26aef-161">Next steps</span></span>

* [<span data-ttu-id="26aef-162">Skapa en fristående Scala program körs i Apache Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="26aef-162">Create a standalone Scala application to run on Apache Spark cluster</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="26aef-163">Använda HDInsight Tools i Azure Toolkit för IntelliJ för att skapa Spark-program för HDInsight Spark Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="26aef-163">Use HDInsight Tools in Azure Toolkit for IntelliJ to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="26aef-164">Använda HDInsight Tools i Azure Toolkit för Eclipse för att skapa Spark-program för HDInsight Spark Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="26aef-164">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications for HDInsight Spark Linux cluster</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
