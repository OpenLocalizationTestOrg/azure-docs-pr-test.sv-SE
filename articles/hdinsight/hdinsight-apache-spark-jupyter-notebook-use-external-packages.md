---
title: "Använda anpassade Maven-paket med Jupyter i Spark på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner om hur du konfigurerar tillgängliga Jupyter-anteckningsböcker med HDInsight Spark-kluster att använda anpassade Maven-paket."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 0bcfe220e60e34937c667c7b416065d5f3dc8d63
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="b564e-103">Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b564e-104">Med hjälp av cell Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="b564e-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="b564e-105">Med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="b564e-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="b564e-106">Lär dig hur du konfigurerar en Jupyter-anteckningsbok i Apache Spark-kluster i HDInsight för att använda externa, community-bidragit **maven** paket som inte ingår out box i klustret.</span><span class="sxs-lookup"><span data-stu-id="b564e-106">Learn how to configure a Jupyter notebook in Apache Spark cluster on HDInsight to use external, community-contributed **maven** packages that are not included out-of-the-box in the cluster.</span></span> 

<span data-ttu-id="b564e-107">Du kan söka i [Maven databasen](http://search.maven.org/) för en fullständig lista över paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="b564e-107">You can search the [Maven repository](http://search.maven.org/) for the complete list of packages that are available.</span></span> <span data-ttu-id="b564e-108">Du kan också hämta en lista över tillgängliga paket från andra källor.</span><span class="sxs-lookup"><span data-stu-id="b564e-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="b564e-109">Till exempel en fullständig lista över community-bidragit paket finns på [Spark paket](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="b564e-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="b564e-110">I den här artikeln får du lära dig hur du använder den [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paket med Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="b564e-110">In this article, you will learn how to use the [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with the Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="b564e-111">Krav</span><span class="sxs-lookup"><span data-stu-id="b564e-111">Prerequisites</span></span>
<span data-ttu-id="b564e-112">Du måste ha följande:</span><span class="sxs-lookup"><span data-stu-id="b564e-112">You must have the following:</span></span>

* <span data-ttu-id="b564e-113">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b564e-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="b564e-114">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="b564e-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="b564e-115">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="b564e-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="b564e-116">På startsidan i [Azure-portalen](https://portal.azure.com/) klickar du på panelen för ditt Spark-kluster (om du har fäst det på startsidan).</span><span class="sxs-lookup"><span data-stu-id="b564e-116">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="b564e-117">Du kan också navigera till ditt kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="b564e-117">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="b564e-118">Klicka på **Snabblänkar** på Spark-klusterbladet och sedan på **Jupyter Notebook** på **Klusterinstrumentpanel**-bladet.</span><span class="sxs-lookup"><span data-stu-id="b564e-118">From the Spark cluster blade, click **Quick Links**, and then from the **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="b564e-119">Ange administratörsautentiseringsuppgifterna för klustret om du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="b564e-119">If prompted, enter the admin credentials for the cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="b564e-120">Du kan också nå Jupyter Notebook för ditt kluster genom att öppna nedanstående URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="b564e-120">You may also reach the Jupyter Notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="b564e-121">Ersätt **CLUSTERNAME** med namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="b564e-121">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="b564e-122">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="b564e-122">Create a new notebook.</span></span> <span data-ttu-id="b564e-123">Klicka på **ny**, och klicka sedan på **Spark**.</span><span class="sxs-lookup"><span data-stu-id="b564e-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="b564e-124">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="b564e-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="b564e-125">En ny anteckningsbok skapas och öppnas med namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="b564e-125">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="b564e-126">Klicka på anteckningsbokens namn högst upp och ange ett trevligt namn.</span><span class="sxs-lookup"><span data-stu-id="b564e-126">Click the notebook name at the top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="b564e-127">![Ange ett namn för anteckningsboken](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Ange ett namn för anteckningsboken")</span><span class="sxs-lookup"><span data-stu-id="b564e-127">![Provide a name for the notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="b564e-128">Du kommer att använda den `%%configure` Magiskt tal för att konfigurera anteckningsboken för att använda ett externa paket.</span><span class="sxs-lookup"><span data-stu-id="b564e-128">You will use the `%%configure` magic to configure the notebook to use an external package.</span></span> <span data-ttu-id="b564e-129">Kontrollera att du anropar i bärbara datorer som använder externa paket, det `%%configure` magiskt i den första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="b564e-129">In notebooks that use external packages, make sure you call the `%%configure` magic in the first code cell.</span></span> <span data-ttu-id="b564e-130">Detta säkerställer att kerneln är konfigurerad för att använda paketet innan sessionen startar.</span><span class="sxs-lookup"><span data-stu-id="b564e-130">This ensures that the kernel is configured to use the package before the session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="b564e-131">Om du glömmer att konfigurera kärnan i den första cellen kan du använda den `%%configure` med den `-f` parameter, men som kommer att starta om sessionen och alla pågår kommer att förloras.</span><span class="sxs-lookup"><span data-stu-id="b564e-131">If you forget to configure the kernel in the first cell, you can use the `%%configure` with the `-f` parameter, but that will restart the session and all progress will be lost.</span></span>

    | <span data-ttu-id="b564e-132">HDInsight-version</span><span class="sxs-lookup"><span data-stu-id="b564e-132">HDInsight version</span></span> | <span data-ttu-id="b564e-133">Kommando</span><span class="sxs-lookup"><span data-stu-id="b564e-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="b564e-134">För HDInsight 3.3 och HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="b564e-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="b564e-135">För HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="b564e-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="b564e-136">Fragment ovan förväntar maven koordinaterna för det externa paketet i Maven centrallager.</span><span class="sxs-lookup"><span data-stu-id="b564e-136">The snippet above expects the maven coordinates for the external package in Maven Central Repository.</span></span> <span data-ttu-id="b564e-137">I det här kodstycket `com.databricks:spark-csv_2.10:1.4.0` är maven-koordinaten för **spark-csv** paketet.</span><span class="sxs-lookup"><span data-stu-id="b564e-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is the maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="b564e-138">Här är hur du skapar koordinaterna för ett paket.</span><span class="sxs-lookup"><span data-stu-id="b564e-138">Here's how you construct the coordinates for a package.</span></span>
   
    <span data-ttu-id="b564e-139">a.</span><span class="sxs-lookup"><span data-stu-id="b564e-139">a.</span></span> <span data-ttu-id="b564e-140">Leta upp paketet i Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="b564e-140">Locate the package in the Maven Repository.</span></span> <span data-ttu-id="b564e-141">Den här självstudiekursen kommer vi att använda [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="b564e-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="b564e-142">b.</span><span class="sxs-lookup"><span data-stu-id="b564e-142">b.</span></span> <span data-ttu-id="b564e-143">Samla in värden för från databasen, **GroupId**, **artefakt-ID**, och **Version**.</span><span class="sxs-lookup"><span data-stu-id="b564e-143">From the repository, gather the values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="b564e-144">Kontrollera att du samlar in värdena matchar ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="b564e-144">Make sure that the values you gather match your cluster.</span></span> <span data-ttu-id="b564e-145">I det här fallet använder du en Scala 2.10 och Spark 1.4.0 paket, men du kan behöva välja olika versioner för lämplig Scala eller Spark-version i klustret.</span><span class="sxs-lookup"><span data-stu-id="b564e-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need to select different versions for the appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="b564e-146">Du kan hitta Scala-version på ditt kluster genom att köra `scala.util.Properties.versionString` på Spark Jupyter-kernel eller skicka Spark.</span><span class="sxs-lookup"><span data-stu-id="b564e-146">You can find out the Scala version on your cluster by running `scala.util.Properties.versionString` on the Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="b564e-147">Du kan hitta Spark-version på ditt kluster genom att köra `sc.version` i Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="b564e-147">You can find out the Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="b564e-148">![Använda externa paket med Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "använda externa paket med Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="b564e-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="b564e-149">c.</span><span class="sxs-lookup"><span data-stu-id="b564e-149">c.</span></span> <span data-ttu-id="b564e-150">Sammanfoga tre värden, avgränsade med kolon (**:**).</span><span class="sxs-lookup"><span data-stu-id="b564e-150">Concatenate the three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="b564e-151">Kör kodcellen med den `%%configure` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="b564e-151">Run the code cell with the `%%configure` magic.</span></span> <span data-ttu-id="b564e-152">Den underliggande Livius sessionen om du vill använda paketet som du har angett kommer att konfigurera.</span><span class="sxs-lookup"><span data-stu-id="b564e-152">This will configure the underlying Livy session to use the package you provided.</span></span> <span data-ttu-id="b564e-153">I efterföljande celler i anteckningsboken kan du nu använda paketet, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="b564e-153">In the subsequent cells in the notebook, you can now use the package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="b564e-154">Sedan kan du köra kodavsnitt som visas nedan, för att visa data från dataframe som du skapade i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="b564e-154">You can then run the snippets, like shown below, to view the data from the dataframe you created in the previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="b564e-155"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="b564e-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="b564e-156">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="b564e-157">Scenarier</span><span class="sxs-lookup"><span data-stu-id="b564e-157">Scenarios</span></span>
* [<span data-ttu-id="b564e-158">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="b564e-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="b564e-159">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="b564e-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="b564e-160">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="b564e-160">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="b564e-161">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="b564e-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="b564e-162">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="b564e-163">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="b564e-163">Create and run applications</span></span>
* [<span data-ttu-id="b564e-164">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="b564e-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="b564e-165">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="b564e-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="b564e-166">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="b564e-166">Tools and extensions</span></span>

* [<span data-ttu-id="b564e-167">Använda externa python-paket med Jupyter notebooks i Apache Spark-kluster i HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="b564e-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="b564e-168">Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="b564e-168">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="b564e-169">Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="b564e-169">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="b564e-170">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="b564e-171">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="b564e-172">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="b564e-172">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="b564e-173">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="b564e-173">Manage resources</span></span>
* [<span data-ttu-id="b564e-174">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-174">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="b564e-175">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="b564e-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

