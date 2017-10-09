---
title: "aaaScript åtgärden - installera Python-paket med Jupyter på Azure HDInsight | Microsoft Docs"
description: "Stegvisa instruktioner för hur toouse skript åtgärd tooconfigure Jupyter-anteckningsböcker med HDInsight Spark-kluster toouse externa python-paket."
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
ms.openlocfilehash: 54db79e67995dee7ca00abff979f7d74ae5ab9cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-script-action-tooinstall-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a><span data-ttu-id="13a86-103">Använd skriptåtgärder tooinstall externa Python-paket för Jupyter notebooks i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-103">Use Script Action tooinstall external Python packages for Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="13a86-104">Med hjälp av cell Magiskt tal</span><span class="sxs-lookup"><span data-stu-id="13a86-104">Using cell magic</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [<span data-ttu-id="13a86-105">Med skriptåtgärder</span><span class="sxs-lookup"><span data-stu-id="13a86-105">Using Script Action</span></span>](hdinsight-apache-spark-python-package-installation.md)
>
>

<span data-ttu-id="13a86-106">Lär dig hur toouse skriptåtgärder tooconfigure ett Apache Spark-kluster i HDInsight (Linux) toouse externa community-bidragit **python** paket som inte är inkluderat out box i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="13a86-106">Learn how toouse Script Actions tooconfigure an Apache Spark cluster on HDInsight (Linux) toouse external, community-contributed **python** packages that are not included out-of-the-box in hello cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="13a86-107">Du kan också konfigurera en Jupyter-anteckningsbok med hjälp av `%%configure` magiska toouse externa paket.</span><span class="sxs-lookup"><span data-stu-id="13a86-107">You can also configure a Jupyter notebook by using `%%configure` magic toouse external packages.</span></span> <span data-ttu-id="13a86-108">Instruktioner finns i [använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span><span class="sxs-lookup"><span data-stu-id="13a86-108">For instructions, see [Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md).</span></span>
> 
> 

<span data-ttu-id="13a86-109">Du kan söka hello [paketindexet](https://pypi.python.org/pypi) hello fullständig lista över paket som är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="13a86-109">You can search hello [package index](https://pypi.python.org/pypi) for hello complete list of packages that are available.</span></span> <span data-ttu-id="13a86-110">Du kan också hämta en lista över tillgängliga paket från andra källor.</span><span class="sxs-lookup"><span data-stu-id="13a86-110">You can also get a list of available packages from other sources.</span></span> <span data-ttu-id="13a86-111">Du kan till exempel installera paket som är tillgängliga via [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) eller [conda bedömningar](https://conda-forge.github.io/feedstocks.html).</span><span class="sxs-lookup"><span data-stu-id="13a86-111">For example, you can install packages made available through [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) or [conda-forge](https://conda-forge.github.io/feedstocks.html).</span></span>

<span data-ttu-id="13a86-112">I den här artikeln får du lära dig hur tooinstall hello [TensorFlow](https://www.tensorflow.org/) paketet med hjälp av skript Actoin på klustret och använda den via hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="13a86-112">In this article, you will learn how tooinstall hello [TensorFlow](https://www.tensorflow.org/) package using Script Actoin on your cluster and use it via hello Jupyter notebook.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13a86-113">Krav</span><span class="sxs-lookup"><span data-stu-id="13a86-113">Prerequisites</span></span>
<span data-ttu-id="13a86-114">Du måste ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="13a86-114">You must have hello following:</span></span>

* <span data-ttu-id="13a86-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="13a86-115">An Azure subscription.</span></span> <span data-ttu-id="13a86-116">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="13a86-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="13a86-117">Ett Apache Spark-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="13a86-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="13a86-118">Instruktioner finns i [skapa Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="13a86-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

   > [!NOTE]
   > <span data-ttu-id="13a86-119">Om du inte redan har ett Spark-kluster i HDInsight Linux kan du köra skriptåtgärder när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="13a86-119">If you do not already have a Spark cluster on HDInsight Linux, you can run script actions during cluster creation.</span></span> <span data-ttu-id="13a86-120">Besök hello dokumentationen om [hur toouse anpassade skript åtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="13a86-120">Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>
   > 
   > 

## <a name="use-external-packages-with-jupyter-notebooks"></a><span data-ttu-id="13a86-121">Använda externa paket med Jupyter-anteckningsböcker</span><span class="sxs-lookup"><span data-stu-id="13a86-121">Use external packages with Jupyter notebooks</span></span>

1. <span data-ttu-id="13a86-122">Från hello [Azure Portal](https://portal.azure.com/), från hello startsidan på hello panelen för ditt Spark-kluster (om du har Fäst det toohello startsidan).</span><span class="sxs-lookup"><span data-stu-id="13a86-122">From hello [Azure Portal](https://portal.azure.com/), from hello startboard, click hello tile for your Spark cluster (if you pinned it toohello startboard).</span></span> <span data-ttu-id="13a86-123">Du kan också navigera tooyour kluster under **Bläddra bland alla** > **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="13a86-123">You can also navigate tooyour cluster under **Browse All** > **HDInsight Clusters**.</span></span>   

2. <span data-ttu-id="13a86-124">Hello Spark-klusterbladet, klicka på **skriptåtgärder** under **användning**.</span><span class="sxs-lookup"><span data-stu-id="13a86-124">From hello Spark cluster blade, click **Script Actions** under **Usage**.</span></span> <span data-ttu-id="13a86-125">Kör hello anpassad åtgärd som installerar TensorFlow i hello huvudnoderna och hello arbetsnoder.</span><span class="sxs-lookup"><span data-stu-id="13a86-125">Run hello custom action that installs TensorFlow in hello head nodes and hello worker nodes.</span></span> <span data-ttu-id="13a86-126">hello bash-skript kan refereras från: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh besök hello dokumentation om [hur toouse anpassade skript åtgärder](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span><span class="sxs-lookup"><span data-stu-id="13a86-126">hello bash script can be referenced from: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh Visit hello documentation on [how toouse custom script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).</span></span>

   > [!NOTE]
   > <span data-ttu-id="13a86-127">Det finns två python installationer i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="13a86-127">There are two python installations in hello cluster.</span></span> <span data-ttu-id="13a86-128">Spark använder hello Anaconda python installationen finns på `/usr/bin/anaconda/bin`.</span><span class="sxs-lookup"><span data-stu-id="13a86-128">Spark will use hello Anaconda python installation located at `/usr/bin/anaconda/bin`.</span></span> <span data-ttu-id="13a86-129">Referera till den installationen i din anpassade åtgärder via `/usr/bin/anaconda/bin/pip` och `/usr/bin/anaconda/bin/conda`.</span><span class="sxs-lookup"><span data-stu-id="13a86-129">Reference that installation in your custom actions via `/usr/bin/anaconda/bin/pip` and `/usr/bin/anaconda/bin/conda`.</span></span>
   > 
   > 

3. <span data-ttu-id="13a86-130">Öppna en PySpark Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="13a86-130">Open a PySpark Jupyter notebook</span></span>

    <span data-ttu-id="13a86-131">![Skapa en ny Jupyter-anteckningsbok](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Skapa en ny Jupyter-anteckningsbok")</span><span class="sxs-lookup"><span data-stu-id="13a86-131">![Create a new Jupyter notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Create a new Jupyter notebook")</span></span>

4. <span data-ttu-id="13a86-132">En ny anteckningsbok skapas och öppnas med hello namnet Untitled.pynb.</span><span class="sxs-lookup"><span data-stu-id="13a86-132">A new notebook is created and opened with hello name Untitled.pynb.</span></span> <span data-ttu-id="13a86-133">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn.</span><span class="sxs-lookup"><span data-stu-id="13a86-133">Click hello notebook name at hello top, and enter a friendly name.</span></span>

    <span data-ttu-id="13a86-134">![Ange ett namn för anteckningsboken hello](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "ange ett namn för anteckningsboken hello")</span><span class="sxs-lookup"><span data-stu-id="13a86-134">![Provide a name for hello notebook](./media/hdinsight-apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Provide a name for hello notebook")</span></span>

5. <span data-ttu-id="13a86-135">Du kommer nu `import tensorflow` och kör ett hello world-exempel.</span><span class="sxs-lookup"><span data-stu-id="13a86-135">You will now `import tensorflow` and run a hello world example.</span></span> 

    <span data-ttu-id="13a86-136">Koden toocopy:</span><span class="sxs-lookup"><span data-stu-id="13a86-136">Code toocopy:</span></span>

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    <span data-ttu-id="13a86-137">hello resultatet ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="13a86-137">hello result will look like this:</span></span>
    
    <span data-ttu-id="13a86-138">![TensorFlow kodkörning](./media/hdinsight-apache-spark-python-package-installation/execution.png "köra TensorFlow kod")</span><span class="sxs-lookup"><span data-stu-id="13a86-138">![TensorFlow code execution](./media/hdinsight-apache-spark-python-package-installation/execution.png "Execute TensorFlow code")</span></span>



## <span data-ttu-id="13a86-139"><a name="seealso"></a>Se även</span><span class="sxs-lookup"><span data-stu-id="13a86-139"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="13a86-140">Översikt: Apache Spark i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-140">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a><span data-ttu-id="13a86-141">Scenarier</span><span class="sxs-lookup"><span data-stu-id="13a86-141">Scenarios</span></span>
* [<span data-ttu-id="13a86-142">Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg</span><span class="sxs-lookup"><span data-stu-id="13a86-142">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="13a86-143">Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data</span><span class="sxs-lookup"><span data-stu-id="13a86-143">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="13a86-144">Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll</span><span class="sxs-lookup"><span data-stu-id="13a86-144">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="13a86-145">Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid</span><span class="sxs-lookup"><span data-stu-id="13a86-145">Spark Streaming: Use Spark in HDInsight for building real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="13a86-146">Webbplatslogganalys med Spark i HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-146">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="13a86-147">Skapa och köra program</span><span class="sxs-lookup"><span data-stu-id="13a86-147">Create and run applications</span></span>
* [<span data-ttu-id="13a86-148">Skapa ett fristående program med hjälp av Scala</span><span class="sxs-lookup"><span data-stu-id="13a86-148">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="13a86-149">Köra jobb via fjärranslutning på ett Spark-kluster med Livy</span><span class="sxs-lookup"><span data-stu-id="13a86-149">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="13a86-150">Verktyg och tillägg</span><span class="sxs-lookup"><span data-stu-id="13a86-150">Tools and extensions</span></span>
* [<span data-ttu-id="13a86-151">Använda externa paket med Jupyter notebooks i Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-151">Use external packages with Jupyter notebooks in Apache Spark clusters on HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="13a86-152">Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-program</span><span class="sxs-lookup"><span data-stu-id="13a86-152">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applications</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="13a86-153">Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="13a86-153">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="13a86-154">Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-154">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="13a86-155">Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-155">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="13a86-156">Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="13a86-156">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="13a86-157">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="13a86-157">Manage resources</span></span>
* [<span data-ttu-id="13a86-158">Hantera resurser för hello Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-158">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="13a86-159">Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight</span><span class="sxs-lookup"><span data-stu-id="13a86-159">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)
