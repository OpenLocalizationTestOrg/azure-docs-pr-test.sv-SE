---
title: "Script åtgärden - installera Python-paket med Jupyter på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner om hur du använder skriptåtgärd till att konfigurera tillgängliga Jupyter-anteckningsböcker med HDInsight Spark-kluster att använda externa python-paket."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21978b71-eb53-480b-a3d1-c5d428a7eb5b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 20cf384c96d4ff4eaf064c8880ad128d521fb9bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="ec085-103">Använd skriptåtgärd till att installera externa Python-paket för Jupyter-anteckningsböcker i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-103">Use Script Action to install external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ec085-104">Med hjälp av cell Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="ec085-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="ec085-105">Med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="ec085-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="ec085-106">Lär dig använda Script Actions för att konfigurera ett Apache Spark-kluster i HDInsight (Linux) för att använda externa, community-bidragit **python** paket som inte ingår out box i klustret.</span><span class="sxs-lookup"><span data-stu-id="ec085-106">Learn how to use Script Actions to configure an Apache Spark cluster on HDInsight (Linux) to use external, community-contributed **python** packages that are not included out-of-the-box in the cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="ec085-107">Du kan också konfigurera en Jupyter-anteckningsbok med hjälp av `%%configure` Magiskt tal för att använda externa paket.</span><span class="sxs-lookup"><span data-stu-id="ec085-107">You can also configure a Jupyter notebook by using `%%configure` magic to use external packages.</span></span> <span data-ttu-id="ec085-108">Instruktioner finns i [använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="ec085-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="ec085-109">Du kan söka i [paketindexet](https://pypi.python.org/pypi) för en fullständig lista över paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="ec085-109">You can search the [package index](https://pypi.python.org/pypi) for the complete list of packages that are available.</span></span> <span data-ttu-id="ec085-110">Du kan också hämta en lista över tillgängliga paket från andra källor.</span><span class="sxs-lookup"><span data-stu-id="ec085-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="ec085-111">Du kan till exempel installera paket som är tillgängliga via [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) eller [conda bedömningar](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="ec085-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="ec085-112">I den här artikeln får du lära dig hur du installerar den [TensorFlow](https://www.tensorflow.org/) paketet med hjälp av skript Actoin på klustret och använda den via Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="ec085-112">In this article, you will learn how to install the [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via the Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ec085-113">Krav</span><span class="sxs-lookup"><span data-stu-id="ec085-113">Prerequisites</span></span>
<span data-ttu-id="ec085-114">Du måste ha följande:</span><span class="sxs-lookup"><span data-stu-id="ec085-114">You must have the following:</span></span>

* <span data-ttu-id="ec085-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ec085-115">An Azure subscription.</span></span> <span data-ttu-id="ec085-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ec085-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ec085-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec085-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="ec085-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="ec085-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="ec085-119">Om du inte redan har ett Spark-kluster i HDInsight Linux kan du köra skriptåtgärder när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="ec085-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="ec085-120">Finns i dokumentationen på [hur du använder anpassade skriptåtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="ec085-120">Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="ec085-121">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="ec085-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="ec085-122">På startsidan i [Azure-portalen](https://portal.azure.com/) klickar du på panelen för ditt Spark-kluster (om du har fäst det på startsidan).</span><span class="sxs-lookup"><span data-stu-id="ec085-122">From the [Azure Portal](https://portal.azure.com/), from the startboard, click the tile for your Spark cluster (if you pinned it to the startboard).</span></span> <span data-ttu-id="ec085-123">Du kan också navigera till ditt kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="ec085-123">You can also navigate to your cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="ec085-124">Klicka på bladet Spark-kluster **skriptåtgärder** under **användning**.</span><span class="sxs-lookup"><span data-stu-id="ec085-124">From the Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="ec085-125">Kör den anpassade åtgärden som installerar TensorFlow i huvudnoderna och arbetsnoderna.</span><span class="sxs-lookup"><span data-stu-id="ec085-125">Run the custom action that installs TensorFlow in the head nodes and the worker nodes.</span></span> <span data-ttu-id="ec085-126">Bash-skript kan refereras från: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh finns i dokumentationen på [hur du använder anpassade skriptåtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="ec085-126">The bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit the documentation on [how to use custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="ec085-127">Det finns två python installationer i klustret.</span><span class="sxs-lookup"><span data-stu-id="ec085-127">There are two python installations in the cluster.</span></span> <span data-ttu-id="ec085-128">Spark använder Anaconda python installationen finns på `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="ec085-128">Spark will use the Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="ec085-129">Referera till den installationen i din anpassade åtgärder via `/usr/bin/anaconda/bin/pip` och `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="ec085-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="ec085-130">Öppna en PySpark Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="ec085-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="ec085-131">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="ec085-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="ec085-132">En ny anteckningsbok skapas och öppnas med namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="ec085-132">A new notebook is created and opened with the name Untitled.pynb.</span></span> <span data-ttu-id="ec085-133">Klicka på anteckningsbokens namn högst upp och ange ett trevligt namn.</span><span class="sxs-lookup"><span data-stu-id="ec085-133">Click the notebook name at the top, and enter a friendly name.</span></span>

    <span data-ttu-id="ec085-134">![Ange ett namn för anteckningsboken](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Ange ett namn för anteckningsboken")</span><span class="sxs-lookup"><span data-stu-id="ec085-134">![Provide a name for the notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for the notebook")</span></span>

5. <span data-ttu-id="ec085-135">Du kommer nu `import tensorflow` och kör ett hello world-exempel.</span><span class="sxs-lookup"><span data-stu-id="ec085-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="ec085-136">Kopiera följande kod:</span><span class="sxs-lookup"><span data-stu-id="ec085-136">Code to copy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="ec085-137">Resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="ec085-137">The result will look like this:</span></span>
    
    <span data-ttu-id="ec085-138">![TensorFlow kodkörning](./media/hdinsight-apache-spark-python-package-installation/execution.png "köra TensorFlow kod")</span><span class="sxs-lookup"><span data-stu-id="ec085-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="ec085-139"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="ec085-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="ec085-140">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="ec085-141">Scenarier</span><span class="sxs-lookup"><span data-stu-id="ec085-141">Scenarios</span></span>
* [<span data-ttu-id="ec085-142">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="ec085-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="ec085-143">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="ec085-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="ec085-144">Spark med Machine Learning: Använda Spark i HDInsight för att förutsäga resultatet av en livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="ec085-144">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="ec085-145">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="ec085-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="ec085-146">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="ec085-147">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="ec085-147">Create and run applications</span></span>
* [<span data-ttu-id="ec085-148">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="ec085-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="ec085-149">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="ec085-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="ec085-150">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="ec085-150">Tools and extensions</span></span>
* [<span data-ttu-id="ec085-151">Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="ec085-152">Använda HDInsight Tools-plugin för IntelliJ IDEA till att skapa och skicka Spark Scala-appar</span><span class="sxs-lookup"><span data-stu-id="ec085-152">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="ec085-153">Använda HDInsight Tools-plugin för IntelliJ IDEA till att felsöka Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="ec085-153">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="ec085-154">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="ec085-155">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="ec085-156">Installera Jupyter på datorn och ansluta till ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="ec085-156">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="ec085-157">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="ec085-157">Manage resources</span></span>
* [<span data-ttu-id="ec085-158">Hantera resurser för Apache Spark-klustret i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-158">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="ec085-159">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ec085-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
