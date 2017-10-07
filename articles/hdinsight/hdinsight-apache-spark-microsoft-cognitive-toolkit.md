---
title: "aaaMicrosoft kognitiva Toolkit med Azure HDInsight Spark för djup learning | Microsoft Docs"
description: "Information om hur en tränad Microsoft kognitiva Toolkit djup learning modellen kan vara tillämpade tooa dataset med hello Spark Python API i ett Azure HDInsight Spark-kluster."
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
ms.openlocfilehash: c296d4697f16d4ef6a958fdb55289807d745ea40
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-microsoft-cognitive-toolkit-deep-learning-model-with-azure-hdinsight-spark-cluster"></a><span data-ttu-id="0f16e-103">Använd Microsoft kognitiva Toolkit djup Lär modell med Azure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0f16e-103">Use Microsoft Cognitive Toolkit deep learning model with Azure HDInsight Spark cluster</span></span>

<span data-ttu-id="0f16e-104">Du hello följande steg i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0f16e-104">In this article, you do hello following steps.</span></span>

1. <span data-ttu-id="0f16e-105">Köra ett anpassat skript tooinstall Microsoft kognitiva Toolkit på ett Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0f16e-105">Run a custom script tooinstall Microsoft Cognitive Toolkit on an Azure HDInsight Spark cluster.</span></span>

2. <span data-ttu-id="0f16e-106">Överför en Jupyter-anteckningsbok toohello Spark-kluster toosee hur tooapply en tränad Microsoft kognitiva Toolkit djup learning modellen toofiles i ett Azure Blob Storage-konto med hjälp av hello [Spark Python-API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span><span class="sxs-lookup"><span data-stu-id="0f16e-106">Upload a Jupyter notebook toohello Spark cluster toosee how tooapply a trained Microsoft Cognitive Toolkit deep learning model toofiles in an Azure Blob Storage Account using hello [Spark Python API (PySpark)](https://spark.apache.org/docs/0.9.0/python-programming-guide.html)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f16e-107">Krav</span><span class="sxs-lookup"><span data-stu-id="0f16e-107">Prerequisites</span></span>

* <span data-ttu-id="0f16e-108">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="0f16e-108">**An Azure subscription**.</span></span> <span data-ttu-id="0f16e-109">Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0f16e-109">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="0f16e-110">Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="0f16e-110">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

* <span data-ttu-id="0f16e-111">**Azure HDInsight Spark-kluster**.</span><span class="sxs-lookup"><span data-stu-id="0f16e-111">**Azure HDInsight Spark cluster**.</span></span> <span data-ttu-id="0f16e-112">Skapa ett Spark 2.0-kluster i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0f16e-112">For this article, create a Spark 2.0 cluster.</span></span> <span data-ttu-id="0f16e-113">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="0f16e-113">For instructions, see [Create Apache Spark cluster in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="how-does-this-solution-flow"></a><span data-ttu-id="0f16e-114">Hur går den här lösningen?</span><span class="sxs-lookup"><span data-stu-id="0f16e-114">How does this solution flow?</span></span>

<span data-ttu-id="0f16e-115">Den här lösningen delas mellan den här artikeln och en Jupyter-anteckningsbok som du överför som en del av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="0f16e-115">This solution is divided between this article and a Jupyter notebook that you upload as part of this tutorial.</span></span> <span data-ttu-id="0f16e-116">Du har slutfört hello följa stegen i den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="0f16e-116">In this article, you complete hello following steps:</span></span>

* <span data-ttu-id="0f16e-117">Kör en skriptåtgärd på ett HDInsight Spark-kluster tooinstall Microsoft kognitiva Toolkit och Python-paket.</span><span class="sxs-lookup"><span data-stu-id="0f16e-117">Run a script action on an HDInsight Spark cluster tooinstall Microsoft Cognitive Toolkit and Python packages.</span></span>
* <span data-ttu-id="0f16e-118">Överför hello Jupyter-anteckningsbok som kör hello lösning toohello HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0f16e-118">Upload hello Jupyter notebook that runs hello solution toohello HDInsight Spark cluster.</span></span>

<span data-ttu-id="0f16e-119">hello beskrivs följande stegen i hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="0f16e-119">hello following remaining steps are covered in hello Jupyter notebook.</span></span>

- <span data-ttu-id="0f16e-120">Läsa in exempel bilder i en distribuerad Spark Resiliant Dataset eller RDD</span><span class="sxs-lookup"><span data-stu-id="0f16e-120">Load sample images into a Spark Resiliant Distributed Dataset or RDD</span></span>
   - <span data-ttu-id="0f16e-121">Läsa in moduler och definiera förinställningar</span><span class="sxs-lookup"><span data-stu-id="0f16e-121">Load modules and define presets</span></span>
   - <span data-ttu-id="0f16e-122">Hämta hello dataset lokalt på hello Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0f16e-122">Download hello dataset locally on hello Spark cluster</span></span>
   - <span data-ttu-id="0f16e-123">Konvertera hello dataset till en RDD</span><span class="sxs-lookup"><span data-stu-id="0f16e-123">Convert hello dataset into an RDD</span></span>
- <span data-ttu-id="0f16e-124">Poäng hello bilder med hjälp av en trained model kognitiva Toolkit</span><span class="sxs-lookup"><span data-stu-id="0f16e-124">Score hello images using a trained Cognitive Toolkit model</span></span>
   - <span data-ttu-id="0f16e-125">Hämta hello tränats kognitiva Toolkit modellen toohello Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0f16e-125">Download hello trained Cognitive Toolkit model toohello Spark cluster</span></span>
   - <span data-ttu-id="0f16e-126">Definiera funktioner toobe som används av arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="0f16e-126">Define functions toobe used by worker nodes</span></span>
   - <span data-ttu-id="0f16e-127">Poäng hello bilder på arbetsnoder</span><span class="sxs-lookup"><span data-stu-id="0f16e-127">Score hello images on worker nodes</span></span>
   - <span data-ttu-id="0f16e-128">Utvärdera modellen noggrannhet</span><span class="sxs-lookup"><span data-stu-id="0f16e-128">Evaluate model accuracy</span></span>


## <a name="install-microsoft-cognitive-toolkit"></a><span data-ttu-id="0f16e-129">Installera Microsoft kognitiva Toolkit</span><span class="sxs-lookup"><span data-stu-id="0f16e-129">Install Microsoft Cognitive Toolkit</span></span>

<span data-ttu-id="0f16e-130">Du kan installera Microsoft kognitiva Toolkit på ett Spark-kluster med skriptåtgärder.</span><span class="sxs-lookup"><span data-stu-id="0f16e-130">You can install Microsoft Cognitive Toolkit on a Spark cluster using script action.</span></span> <span data-ttu-id="0f16e-131">Skriptåtgärder använder anpassade skript tooinstall komponenter på hello-kluster som inte är tillgängliga som standard.</span><span class="sxs-lookup"><span data-stu-id="0f16e-131">Script action uses custom scripts tooinstall components on hello cluster that are not available by default.</span></span> <span data-ttu-id="0f16e-132">Du kan använda hello anpassat skript från hello Azure-portalen genom att använda HDInsight .NET SDK eller med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0f16e-132">You can use hello custom script from hello Azure Portal, by using HDInsight .NET SDK, or by using Azure PowerShell.</span></span> <span data-ttu-id="0f16e-133">Du kan också använda hello skriptet tooinstall hello toolkit antingen som en del av klustret har skapats eller när hello klustret är igång.</span><span class="sxs-lookup"><span data-stu-id="0f16e-133">You can also use hello script tooinstall hello toolkit either as part of cluster creation, or after hello cluster is up and running.</span></span> 

<span data-ttu-id="0f16e-134">I den här artikeln använder vi hello portal tooinstall hello toolkit efter hello klustret har skapats.</span><span class="sxs-lookup"><span data-stu-id="0f16e-134">In this article, we use hello portal tooinstall hello toolkit, after hello cluster has been created.</span></span> <span data-ttu-id="0f16e-135">Andra sätt toorun hello anpassade skript, se [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0f16e-135">For other ways toorun hello custom script, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md).</span></span>

### <a name="using-hello-azure-portal"></a><span data-ttu-id="0f16e-136">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0f16e-136">Using hello Azure Portal</span></span>

<span data-ttu-id="0f16e-137">Anvisningar för hur toouse hello Azure Portal toorun skript åtgärd finns [anpassa HDInsight-kluster med skriptåtgärder](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span><span class="sxs-lookup"><span data-stu-id="0f16e-137">For instructions on how toouse hello Azure Portal toorun script action, see [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation).</span></span> <span data-ttu-id="0f16e-138">Kontrollera att du anger hello följande indata tooinstall Microsoft kognitiva Toolkit.</span><span class="sxs-lookup"><span data-stu-id="0f16e-138">Make sure you provide hello following inputs tooinstall Microsoft Cognitive Toolkit.</span></span>

* <span data-ttu-id="0f16e-139">Ange ett värde för hello Skriptnamn för åtgärden.</span><span class="sxs-lookup"><span data-stu-id="0f16e-139">Provide a value for hello script action name.</span></span>

* <span data-ttu-id="0f16e-140">För **Bash skript URI**, ange `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span><span class="sxs-lookup"><span data-stu-id="0f16e-140">For **Bash script URI**, enter `https://raw.githubusercontent.com/Azure-Samples/hdinsight-pyspark-cntk-integration/master/cntk-install.sh`.</span></span>

* <span data-ttu-id="0f16e-141">Kontrollera att du kör skriptet hello endast på hello head- och arbetsroller noder och avmarkera alla hello andra kryssrutorna.</span><span class="sxs-lookup"><span data-stu-id="0f16e-141">Make sure you run hello script only on hello head and worker nodes and clear all hello other checkboxes.</span></span>

* <span data-ttu-id="0f16e-142">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0f16e-142">Click **Create**.</span></span>

## <a name="upload-hello-jupyter-notebook-tooazure-hdinsight-spark-cluster"></a><span data-ttu-id="0f16e-143">Överför hello Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0f16e-143">Upload hello Jupyter notebook tooAzure HDInsight Spark cluster</span></span>

<span data-ttu-id="0f16e-144">toouse hello Microsoft kognitiva Toolkit med hello Azure HDInsight Spark-kluster, måste du ladda hello Jupyter-anteckningsbok **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="0f16e-144">toouse hello Microsoft Cognitive Toolkit with hello Azure HDInsight Spark cluster, you must load hello Jupyter notebook **CNTK_model_scoring_on_Spark_walkthrough.ipynb** toohello Azure HDInsight Spark cluster.</span></span> <span data-ttu-id="0f16e-145">Den här anteckningsboken finns på GitHub på [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="0f16e-145">This notebook is available on GitHub at [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span>

1. <span data-ttu-id="0f16e-146">Klona hello GitHub-lagringsplatsen [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span><span class="sxs-lookup"><span data-stu-id="0f16e-146">Clone hello GitHub repository [https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration](https://github.com/Azure-Samples/hdinsight-pyspark-cntk-integration).</span></span> <span data-ttu-id="0f16e-147">Läs instruktionerna tooclone [kloning av en databas](https://help.github.com/articles/cloning-a-repository/).</span><span class="sxs-lookup"><span data-stu-id="0f16e-147">For instructions tooclone, see [Cloning a repository](https://help.github.com/articles/cloning-a-repository/).</span></span>

2. <span data-ttu-id="0f16e-148">Hello Azure-portalen, öppna hello Spark-klusterbladet som du redan etablerats, klickar du på **Klusterinstrumentpanel**, och klicka sedan på **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="0f16e-148">From hello Azure Portal, open hello Spark cluster blade that you already provisioned, click **Cluster Dashboard**, and then click **Jupyter notebook**.</span></span>

    <span data-ttu-id="0f16e-149">Du kan också starta hello Jupyter-anteckningsbok genom att gå toohello URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span><span class="sxs-lookup"><span data-stu-id="0f16e-149">You can also launch hello Jupyter notebook by going toohello URL `https://<clustername>.azurehdinsight.net/jupyter/`.</span></span> <span data-ttu-id="0f16e-150">Ersätt \<klusternamn > med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0f16e-150">Replace \<clustername> with hello name of your HDInsight cluster.</span></span>

3. <span data-ttu-id="0f16e-151">Hej Jupyter-anteckningsbok, klicka på **överför** i hello övre högra hörnet och gå sedan toohello plats där du har klonat hello GitHub-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="0f16e-151">From hello Jupyter notebook, click **Upload** in hello top-right corner and then navigate toohello location where you cloned hello GitHub repository.</span></span>

    <span data-ttu-id="0f16e-152">![Överför Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "överför Jupyter-anteckningsbok tooAzure HDInsight Spark-kluster")</span><span class="sxs-lookup"><span data-stu-id="0f16e-152">![Upload Jupyter notebook tooAzure HDInsight Spark cluster](./media/hdinsight-apache-spark-microsoft-cognitive-toolkit/hdinsight-microsoft-cognitive-toolkit-load-jupyter-notebook.png "Upload Jupyter notebook tooAzure HDInsight Spark cluster")</span></span>

4. <span data-ttu-id="0f16e-153">Klicka på **överför** igen.</span><span class="sxs-lookup"><span data-stu-id="0f16e-153">Click **Upload** again.</span></span>

5. <span data-ttu-id="0f16e-154">När hello anteckningsboken har överförts klickar hello namnet på hello anteckningsboken och följ sedan instruktionerna för hello i hello anteckningsbok själva på hur tooload hello datauppsättning och utföra hello kursen.</span><span class="sxs-lookup"><span data-stu-id="0f16e-154">After hello notebook is uploaded, click hello name of hello notebook and then follow hello instructions in hello notebook itself on how tooload hello data set and perform hello tutorial.</span></span>

## <a name="see-also"></a><span data-ttu-id="0f16e-155">Se även</span><span class="sxs-lookup"><span data-stu-id="0f16e-155">See also</span></span>
* [<span data-ttu-id="0f16e-156">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-156">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="0f16e-157">Scenarier</span><span class="sxs-lookup"><span data-stu-id="0f16e-157">Scenarios</span></span>
* [<span data-ttu-id="0f16e-158">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="0f16e-158">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="0f16e-159">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="0f16e-159">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="0f16e-160">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="0f16e-160">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="0f16e-161">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="0f16e-161">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="0f16e-162">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-162">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)
* [<span data-ttu-id="0f16e-163">Application Insight-telemetridataanalys i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-163">Application Insight telemetry data analysis using Spark in HDInsight</span></span>](hdinsight-spark-analyze-application-insight-logs.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="0f16e-164">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="0f16e-164">Create and run applications</span></span>
* [<span data-ttu-id="0f16e-165">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="0f16e-165">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="0f16e-166">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="0f16e-166">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="0f16e-167">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="0f16e-167">Tools and extensions</span></span>
* [<span data-ttu-id="0f16e-168">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="0f16e-168">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="0f16e-169">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="0f16e-169">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="0f16e-170">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-170">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="0f16e-171">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-171">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="0f16e-172">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="0f16e-172">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="0f16e-173">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0f16e-173">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="0f16e-174">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="0f16e-174">Manage resources</span></span>
* [<span data-ttu-id="0f16e-175">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-175">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="0f16e-176">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f16e-176">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
