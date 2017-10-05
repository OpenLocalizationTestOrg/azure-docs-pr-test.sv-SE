---
title: "Microsoft kognitiva Toolkit med Azure HDInsight Spark för djup learning | Microsoft Docs"
description: "Lär dig hur en tränad modell Microsoft kognitiva Toolkit djup learning kan tillämpas på en datamängd med Spark Python API i ett Azure HDInsight Spark-kluster."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: fafd738f782660b824732bab8cc3bec8405947e7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a><span data-ttu-id="7dff6-103">Använd Microsoft kognitiva Toolkit djup Lär modell med Azure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7dff6-103">Use Microsoft Cognitive Toolkit deep learning model with Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="7dff6-104">Gör följande steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7dff6-104">In this article, you do the following steps.</span></span>

1. <span data-ttu-id="7dff6-105">Köra ett anpassat skript för att installera Microsoft kognitiva Toolkit på ett Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="7dff6-105">Run a custom script to install Microsoft Cognitive Toolkit on an Azure HDInsight Spark cluster.</span></span>

2. <span data-ttu-id="7dff6-106">Överför en Jupyter-anteckningsbok till Spark-kluster för att se hur du använder en tränad modell Microsoft kognitiva Toolkit djup learning till filer i en Azure Blob Storage-konto med hjälp av den [Spark Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span><span class="sxs-lookup"><span data-stu-id="7dff6-106">Upload a Jupyter notebook to the Spark cluster to see how to apply a trained Microsoft Cognitive Toolkit deep learning model to files in an Azure Blob Storage Account using the [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7dff6-107">Krav</span><span class="sxs-lookup"><span data-stu-id="7dff6-107">Prerequisites</span></span>

* <span data-ttu-id="7dff6-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="7dff6-108">**An Azure subscription**.</span></span> <span data-ttu-id="7dff6-109">Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7dff6-109">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="7dff6-110">Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="7dff6-110">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

* <span data-ttu-id="7dff6-111">**Azure HDInsight Spark-kluster**.</span><span class="sxs-lookup"><span data-stu-id="7dff6-111">**Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="7dff6-112">Skapa ett Spark 2.0-kluster i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7dff6-112">For this article, create a Spark 2.0 cluster.</span></span> <span data-ttu-id="7dff6-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="7dff6-113">For instructions, see [Create Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="how-does-this-solution-flow"></a><span data-ttu-id="7dff6-114">Hur går den här lösningen?</span><span class="sxs-lookup"><span data-stu-id="7dff6-114">How does this solution flow?</span></span>

<span data-ttu-id="7dff6-115">Den här lösningen delas mellan den här artikeln och en Jupyter-anteckningsbok som du överför som en del av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7dff6-115">This solution is divided between this article and a Jupyter notebook that you upload as part of this tutorial.</span></span> <span data-ttu-id="7dff6-116">I den här artikeln kan du utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="7dff6-116">In this article, you complete the following steps:</span></span>

* <span data-ttu-id="7dff6-117">Kör en skriptåtgärd på ett HDInsight Spark-klustret för att installera Microsoft kognitiva Toolkit och Python-paket.</span><span class="sxs-lookup"><span data-stu-id="7dff6-117">Run a script action on an HDInsight Spark cluster to install Microsoft Cognitive Toolkit and Python packages.</span></span>
* <span data-ttu-id="7dff6-118">Överför Jupyter-anteckningsbok som kör lösningen till HDInsight Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7dff6-118">Upload the Jupyter notebook that runs the solution to the HDInsight Spark cluster.</span></span>

<span data-ttu-id="7dff6-119">De följande stegen beskrivs i Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="7dff6-119">The following remaining steps are covered in the Jupyter notebook.</span></span>

- <span data-ttu-id="7dff6-120">Läsa in exempel bilder i en distribuerad Spark Resiliant Dataset eller RDD</span><span class="sxs-lookup"><span data-stu-id="7dff6-120">Load sample images into a Spark Resiliant Distributed Dataset or RDD</span></span>
   - <span data-ttu-id="7dff6-121">Läsa in moduler och definiera förinställningar</span><span class="sxs-lookup"><span data-stu-id="7dff6-121">Load modules and define presets</span></span>
   - <span data-ttu-id="7dff6-122">Hämta dataset lokalt på Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7dff6-122">Download the dataset locally on the Spark cluster</span></span>
   - <span data-ttu-id="7dff6-123">Konvertera datauppsättningen till en RDD</span><span class="sxs-lookup"><span data-stu-id="7dff6-123">Convert the dataset into an RDD</span></span>
- <span data-ttu-id="7dff6-124">Poängsätta bilder med hjälp av en trained model kognitiva Toolkit</span><span class="sxs-lookup"><span data-stu-id="7dff6-124">Score the images using a trained Cognitive Toolkit model</span></span>
   - <span data-ttu-id="7dff6-125">Hämta den tränade modellen kognitiva Toolkit till Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7dff6-125">Download the trained Cognitive Toolkit model to the Spark cluster</span></span>
   - <span data-ttu-id="7dff6-126">Definiera funktioner som ska användas av arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="7dff6-126">Define functions to be used by worker nodes</span></span>
   - <span data-ttu-id="7dff6-127">Poängsätta avbildningar på arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="7dff6-127">Score the images on worker nodes</span></span>
   - <span data-ttu-id="7dff6-128">Utvärdera modellen noggrannhet</span><span class="sxs-lookup"><span data-stu-id="7dff6-128">Evaluate model accuracy</span></span>


## <a name="install-microsoft-cognitive-toolkit"></a><span data-ttu-id="7dff6-129">Installera Microsoft kognitiva Toolkit</span><span class="sxs-lookup"><span data-stu-id="7dff6-129">Install Microsoft Cognitive Toolkit</span></span>

<span data-ttu-id="7dff6-130">Du kan installera Microsoft kognitiva Toolkit på ett Spark-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="7dff6-130">You can install Microsoft Cognitive Toolkit on a Spark cluster using script action.</span></span> <span data-ttu-id="7dff6-131">Skriptåtgärder använder anpassade skript för att installera komponenter i klustret som inte är tillgängliga som standard.</span><span class="sxs-lookup"><span data-stu-id="7dff6-131">Script action uses custom scripts to install components on the cluster that are not available by default.</span></span> <span data-ttu-id="7dff6-132">Du kan använda anpassade skript från Azure-portalen genom att använda HDInsight .NET SDK eller med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7dff6-132">You can use the custom script from the Azure Portal, by using HDInsight .NET SDK, or by using Azure PowerShell.</span></span> <span data-ttu-id="7dff6-133">Du kan också använda skriptet för att installera verktyget antingen som en del av klustret har skapats eller när klustret är igång.</span><span class="sxs-lookup"><span data-stu-id="7dff6-133">You can also use the script to install the toolkit either as part of cluster creation, or after the cluster is up and running.</span></span> 

<span data-ttu-id="7dff6-134">Vi använder portalen för att installera verktygen, när klustret har skapats i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7dff6-134">In this article, we use the portal to install the toolkit, after the cluster has been created.</span></span> <span data-ttu-id="7dff6-135">Andra sätt att köra skriptet finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="7dff6-135">For other ways to run the custom script, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

### <a name="using-the-azure-portal"></a><span data-ttu-id="7dff6-136">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7dff6-136">Using the Azure Portal</span></span>

<span data-ttu-id="7dff6-137">Anvisningar om hur du använder Azure Portal för att köra skriptåtgärder finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span><span class="sxs-lookup"><span data-stu-id="7dff6-137">For instructions on how to use the Azure Portal to run script action, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span></span> <span data-ttu-id="7dff6-138">Kontrollera att du anger följande indata för att installera Microsoft kognitiva Toolkit.</span><span class="sxs-lookup"><span data-stu-id="7dff6-138">Make sure you provide the following inputs to install Microsoft Cognitive Toolkit.</span></span>

* <span data-ttu-id="7dff6-139">Ange ett värde för namnet på åtgärden.</span><span class="sxs-lookup"><span data-stu-id="7dff6-139">Provide a value for the script action name.</span></span>

* <span data-ttu-id="7dff6-140">För **Bash skript URI**, ange `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span><span class="sxs-lookup"><span data-stu-id="7dff6-140">For **Bash script URI**, enter `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span></span>

* <span data-ttu-id="7dff6-141">Kontrollera att du kör skriptet endast på head- och arbetsroller noder och avmarkera alla andra kryssrutor.</span><span class="sxs-lookup"><span data-stu-id="7dff6-141">Make sure you run the script only on the head and worker nodes and clear all the other checkboxes.</span></span>

* <span data-ttu-id="7dff6-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7dff6-142">Click **Create**.</span></span>

## <a name="upload-the-jupyter-notebook-to-azure-hdinsight-spark-cluster"></a><span data-ttu-id="7dff6-143">Överför Jupyter-anteckningsbok till Azure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7dff6-143">Upload the Jupyter notebook to Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="7dff6-144">Om du vill använda Microsoft kognitiva Toolkit med Azure HDInsight Spark-kluster måste du ladda Jupyter-anteckningsbok **CNTK_model_scoring_on_Spark_walkthrough.ipynb** i Azure HDInsight Spark-klustret.</span><span class="sxs-lookup"><span data-stu-id="7dff6-144">To use the Microsoft Cognitive Toolkit with the Azure HDInsight Spark cluster, you must load the Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** to the Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="7dff6-145">Den här anteckningsboken finns på GitHub på [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="7dff6-145">This notebook is available on GitHub at [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span>

1. <span data-ttu-id="7dff6-146">Klona lagringsplatsen GitHub [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="7dff6-146">Clone the GitHub repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span> <span data-ttu-id="7dff6-147">Anvisningar att klona finns [kloning av en databas](https://help.github.com/articles/cloning-a-repository/).</span><span class="sxs-lookup"><span data-stu-id="7dff6-147">For instructions to clone, see [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).</span></span>

2. <span data-ttu-id="7dff6-148">Öppna bladet Spark-kluster som du redan etablerats, klickar du på Azure-portalen **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="7dff6-148">From the Azure Portal, open the Spark cluster blade that you already provisioned, click **Cluster Dashboard**, and then click **Jupyter notebook**.</span></span>

    <span data-ttu-id="7dff6-149">Du kan också starta Jupyter-anteckningsbok genom att gå till URL: en `https://<clustername>.azurehdinsight.net/jupyter/`.</span><span class="sxs-lookup"><span data-stu-id="7dff6-149">You can also launch the Jupyter notebook by going to the URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span></span> <span data-ttu-id="7dff6-150">Ersätt \<klusternamn > med namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7dff6-150">Replace \<clustername> with the name of your HDInsight cluster.</span></span>

3. <span data-ttu-id="7dff6-151">Från Jupyter-anteckningsbok klickar du på **överför** i övre högra hörn och gå sedan till den plats där du har klonat GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="7dff6-151">From the Jupyter notebook, click **Upload** in the top-right corner and then navigate to the location where you cloned the GitHub repository.</span></span>

    <span data-ttu-id="7dff6-152">![Överför Jupyter-anteckningsbok till Azure HDInsight Spark-kluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "överför Jupyter-anteckningsbok till Azure HDInsight Spark-kluster")</span><span class="sxs-lookup"><span data-stu-id="7dff6-152">![Upload Jupyter notebook to Azure HDInsight Spark cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Upload Jupyter notebook to Azure HDInsight Spark cluster")</span></span>

4. <span data-ttu-id="7dff6-153">Klicka på **överför** igen.</span><span class="sxs-lookup"><span data-stu-id="7dff6-153">Click **Upload** again.</span></span>

5. <span data-ttu-id="7dff6-154">När den bärbara datorn har överförts klickar du på namnet på den bärbara datorn och följ sedan anvisningarna i den bärbara datorn sig själv att läsa in datauppsättningen och genomföra kursen.</span><span class="sxs-lookup"><span data-stu-id="7dff6-154">After the notebook is uploaded, click the name of the notebook and then follow the instructions in the notebook itself on how to load the data set and perform the tutorial.</span></span>

## <a name="see-also"></a><span data-ttu-id="7dff6-155">Se även</span><span class="sxs-lookup"><span data-stu-id="7dff6-155">See also</span></span>
* [<span data-ttu-id="7dff6-156">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="7dff6-157">Scenarier</span><span class="sxs-lookup"><span data-stu-id="7dff6-157">Scenarios</span></span>
* [<span data-ttu-id="7dff6-158">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="7dff6-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="7dff6-159">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="7dff6-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="7dff6-160">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="7dff6-160">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="7dff6-161">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="7dff6-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="7dff6-162">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="7dff6-163">Application Insight-telemetridataanalys i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-163">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="7dff6-164">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="7dff6-164">Create and run applications</span></span>
* [<span data-ttu-id="7dff6-165">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="7dff6-165">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="7dff6-166">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="7dff6-166">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="7dff6-167">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="7dff6-167">Tools and extensions</span></span>
* [<span data-ttu-id="7dff6-168">Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="7dff6-168">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="7dff6-169">Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="7dff6-169">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="7dff6-170">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="7dff6-171">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="7dff6-172">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="7dff6-172">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="7dff6-173">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="7dff6-173">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="7dff6-174">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="7dff6-174">Manage resources</span></span>
* [<span data-ttu-id="7dff6-175">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-175">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="7dff6-176">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7dff6-176">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
