---
title: "aaaUse anpassade Maven-paket med Jupyter i Spark på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur tooconfigure Jupyter-anteckningsböcker med HDInsight Spark-kluster toouse anpassade Maven-paket."
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
ms.openlocfilehash: ba8ac13716bc94ab082a18fe02d4a40b2f1e09e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="31bac-103">Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-103">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="31bac-104">Med hjälp av cell Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="31bac-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="31bac-105">Med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="31bac-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="31bac-106">Lär dig hur tooconfigure en Jupyter-anteckningsbok i Apache Spark-kluster i HDInsight toouse externa community-bidragit **maven** paket som inte är inkluderat out box i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="31bac-106">Learn how tooconfigure a Jupyter notebook in Apache Spark cluster on HDInsight toouse external, community-contributed **maven** packages that are not included out-of-the-box in hello cluster.</span></span> 

<span data-ttu-id="31bac-107">Du kan söka hello [Maven databasen](http://search.maven.org/) hello fullständig lista över paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="31bac-107">You can search hello [Maven repository](http://search.maven.org/) for hello complete list of packages that are available.</span></span> <span data-ttu-id="31bac-108">Du kan också hämta en lista över tillgängliga paket från andra källor.</span><span class="sxs-lookup"><span data-stu-id="31bac-108">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="31bac-109">Till exempel en fullständig lista över community-bidragit paket finns på [Spark paket](http://spark-packages.org/).</span><span class="sxs-lookup"><span data-stu-id="31bac-109">For example, a complete list of community-contributed packages is available at [Spark Packages](http://spark-packages.org/).</span></span>

<span data-ttu-id="31bac-110">I den här artikeln får du lära dig hur toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) paket med Jupyter-anteckningsbok hello.</span><span class="sxs-lookup"><span data-stu-id="31bac-110">In this article, you will learn how toouse hello [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) package with hello Jupyter notebook.</span></span>



## <a name="prerequisites"></a><span data-ttu-id="31bac-111">Krav</span><span class="sxs-lookup"><span data-stu-id="31bac-111">Prerequisites</span></span>
<span data-ttu-id="31bac-112">Du måste ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="31bac-112">You must have hello following:</span></span>

* <span data-ttu-id="31bac-113">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="31bac-113">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="31bac-114">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="31bac-114">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="31bac-115">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="31bac-115">Use external packages with Jupyter notebooks</span></span>
1. <span data-ttu-id="31bac-116">Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="31bac-116">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="31bac-117">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="31bac-117">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   
2. <span data-ttu-id="31bac-118">Hello Spark-klusterbladet, klicka på **snabblänkar**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="31bac-118">From hello Spark cluster blade, click **Quick Links**, and then from hello **Cluster Dashboard** blade, click **Jupyter Notebook**.</span></span> <span data-ttu-id="31bac-119">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="31bac-119">If prompted, enter hello admin credentials for hello cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="31bac-120">Du kan också nå hello Jupyter Notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="31bac-120">You may also reach hello Jupyter Notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="31bac-121">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="31bac-121">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. <span data-ttu-id="31bac-122">Skapa en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="31bac-122">Create a new notebook.</span></span> <span data-ttu-id="31bac-123">Klicka på **ny**, och klicka sedan på **Spark**.</span><span class="sxs-lookup"><span data-stu-id="31bac-123">Click **New**, and then click **Spark**.</span></span>
   
    <span data-ttu-id="31bac-124">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="31bac-124">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="31bac-125">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="31bac-125">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="31bac-126">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="31bac-126">Click hello notebook name at hello top, and enter a friendly name.</span></span>
   
    <span data-ttu-id="31bac-127">![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "ange ett namn för anteckningsboken hello")</span><span class="sxs-lookup"><span data-stu-id="31bac-127">![Provide a name for hello notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="31bac-128">Du kommer att använda hello `%%configure` magiskt tooconfigure hello anteckningsboken toouse ett externa paket.</span><span class="sxs-lookup"><span data-stu-id="31bac-128">You will use hello `%%configure` magic tooconfigure hello notebook toouse an external package.</span></span> <span data-ttu-id="31bac-129">Kontrollera att du anropa hello i bärbara datorer som använder externa paket, `%%configure` magiskt i hello första kodcellen.</span><span class="sxs-lookup"><span data-stu-id="31bac-129">In notebooks that use external packages, make sure you call hello `%%configure` magic in hello first code cell.</span></span> <span data-ttu-id="31bac-130">Detta säkerställer att hello kernel konfigurerade toouse hello paketet innan du startar hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="31bac-130">This ensures that hello kernel is configured toouse hello package before hello session starts.</span></span>

    >[!IMPORTANT] 
    ><span data-ttu-id="31bac-131">Om du glömmer tooconfigure hello kernel i hello första cellen kan du använda hello `%%configure` med hello `-f` parameter, men som kommer att startas om hello sessionen och alla pågår kommer att förloras.</span><span class="sxs-lookup"><span data-stu-id="31bac-131">If you forget tooconfigure hello kernel in hello first cell, you can use hello `%%configure` with hello `-f` parameter, but that will restart hello session and all progress will be lost.</span></span>

    | <span data-ttu-id="31bac-132">HDInsight-version</span><span class="sxs-lookup"><span data-stu-id="31bac-132">HDInsight version</span></span> | <span data-ttu-id="31bac-133">Kommando</span><span class="sxs-lookup"><span data-stu-id="31bac-133">Command</span></span> |
    |-------------------|---------|
    |<span data-ttu-id="31bac-134">För HDInsight 3.3 och HDInsight 3.4</span><span class="sxs-lookup"><span data-stu-id="31bac-134">For HDInsight 3.3 and HDInsight 3.4</span></span> | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | <span data-ttu-id="31bac-135">För HDInsight 3.5</span><span class="sxs-lookup"><span data-stu-id="31bac-135">For HDInsight 3.5</span></span> | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. <span data-ttu-id="31bac-136">hello fragment ovan förväntar hello maven koordinaterna för hello externa paket i Maven centrallager.</span><span class="sxs-lookup"><span data-stu-id="31bac-136">hello snippet above expects hello maven coordinates for hello external package in Maven Central Repository.</span></span> <span data-ttu-id="31bac-137">I det här kodstycket `com.databricks:spark-csv_2.10:1.4.0` är hello maven koordinaten för **spark-csv** paketet.</span><span class="sxs-lookup"><span data-stu-id="31bac-137">In this snippet, `com.databricks:spark-csv_2.10:1.4.0` is hello maven coordinate for **spark-csv** package.</span></span> <span data-ttu-id="31bac-138">Här är hur du skapar hello koordinater för ett paket.</span><span class="sxs-lookup"><span data-stu-id="31bac-138">Here's how you construct hello coordinates for a package.</span></span>
   
    <span data-ttu-id="31bac-139">a.</span><span class="sxs-lookup"><span data-stu-id="31bac-139">a.</span></span> <span data-ttu-id="31bac-140">Leta upp hello paketet i hello Maven-databasen.</span><span class="sxs-lookup"><span data-stu-id="31bac-140">Locate hello package in hello Maven Repository.</span></span> <span data-ttu-id="31bac-141">Den här självstudiekursen kommer vi att använda [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span><span class="sxs-lookup"><span data-stu-id="31bac-141">For this tutorial, we use [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).</span></span>
   
    <span data-ttu-id="31bac-142">b.</span><span class="sxs-lookup"><span data-stu-id="31bac-142">b.</span></span> <span data-ttu-id="31bac-143">Samla in hello värden för från hello databasen **GroupId**, **artefakt-ID**, och **Version**.</span><span class="sxs-lookup"><span data-stu-id="31bac-143">From hello repository, gather hello values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="31bac-144">Se till att hello-värden som du samlar in matchar ditt kluster.</span><span class="sxs-lookup"><span data-stu-id="31bac-144">Make sure that hello values you gather match your cluster.</span></span> <span data-ttu-id="31bac-145">I det här fallet använder du en Scala 2.10 och Spark 1.4.0 paketet, men du kanske måste tooselect olika versioner för hello Scala eller Spark-version i klustret.</span><span class="sxs-lookup"><span data-stu-id="31bac-145">In this case, we are using a Scala 2.10 and Spark 1.4.0 package, but you may need tooselect different versions for hello appropriate Scala or Spark version in your cluster.</span></span> <span data-ttu-id="31bac-146">Du kan hitta hello Scala version på ditt kluster genom att köra `scala.util.Properties.versionString` på hello Spark Jupyter kernel eller skicka Spark.</span><span class="sxs-lookup"><span data-stu-id="31bac-146">You can find out hello Scala version on your cluster by running `scala.util.Properties.versionString` on hello Spark Jupyter kernel or on Spark submit.</span></span> <span data-ttu-id="31bac-147">Du kan hitta hello Spark-version på ditt kluster genom att köra `sc.version` i Jupyter-anteckningsböcker.</span><span class="sxs-lookup"><span data-stu-id="31bac-147">You can find out hello Spark version on your cluster by running `sc.version` on Jupyter notebooks.</span></span>
   
    <span data-ttu-id="31bac-148">![Använda externa paket med Jupyter-anteckningsbok](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "använda externa paket med Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="31bac-148">![Use external packages with Jupyter notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Use external packages with Jupyter notebook")</span></span>
   
    <span data-ttu-id="31bac-149">c.</span><span class="sxs-lookup"><span data-stu-id="31bac-149">c.</span></span> <span data-ttu-id="31bac-150">Sammanfoga hello tre värden, avgränsade med kolon (**:**).</span><span class="sxs-lookup"><span data-stu-id="31bac-150">Concatenate hello three values, separated by a colon (**:**).</span></span>
   
        com.databricks:spark-csv_2.10:1.4.0

7. <span data-ttu-id="31bac-151">Kör hello kodcellen med hello `%%configure` Magiskt tal.</span><span class="sxs-lookup"><span data-stu-id="31bac-151">Run hello code cell with hello `%%configure` magic.</span></span> <span data-ttu-id="31bac-152">Detta konfigurerar hello underliggande Livius session toouse hello paket du angett.</span><span class="sxs-lookup"><span data-stu-id="31bac-152">This will configure hello underlying Livy session toouse hello package you provided.</span></span> <span data-ttu-id="31bac-153">I hello efterföljande celler i hello anteckningsbok använda du nu hello paketet, enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="31bac-153">In hello subsequent cells in hello notebook, you can now use hello package, as shown below.</span></span>
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. <span data-ttu-id="31bac-154">Sedan kan du köra hello kodavsnitt, som visas nedan, tooview hello data från hello dataframe du skapade i föregående steg i hello.</span><span class="sxs-lookup"><span data-stu-id="31bac-154">You can then run hello snippets, like shown below, tooview hello data from hello dataframe you created in hello previous step.</span></span>
   
        df.show()
   
        df.select("Time").count()

## <span data-ttu-id="31bac-155"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="31bac-155"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="31bac-156">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="31bac-157">Scenarier</span><span class="sxs-lookup"><span data-stu-id="31bac-157">Scenarios</span></span>
* [<span data-ttu-id="31bac-158">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="31bac-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="31bac-159">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="31bac-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="31bac-160">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="31bac-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="31bac-161">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="31bac-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="31bac-162">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="31bac-163">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="31bac-163">Create and run applications</span></span>
* [<span data-ttu-id="31bac-164">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="31bac-164">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="31bac-165">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="31bac-165">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="31bac-166">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="31bac-166">Tools and extensions</span></span>

* [<span data-ttu-id="31bac-167">Använda externa python-paket med Jupyter notebooks i Apache Spark-kluster i HDInsight Linux</span><span class="sxs-lookup"><span data-stu-id="31bac-167">Use external python packages with Jupyter notebooks in Apache Spark clusters on HDInsight Linux</span></span>](hdinsight-apache-spark-python-package-installation.md)
* [<span data-ttu-id="31bac-168">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="31bac-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="31bac-169">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="31bac-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="31bac-170">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="31bac-171">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="31bac-172">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="31bac-172">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="31bac-173">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="31bac-173">Manage resources</span></span>
* [<span data-ttu-id="31bac-174">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-174">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="31bac-175">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="31bac-175">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

