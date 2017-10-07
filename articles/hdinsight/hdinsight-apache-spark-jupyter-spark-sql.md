---
title: aaaCreate ett Apache Spark-klustret i Azure HDInsight | Microsoft Docs
description: "HDInsight Spark quickstart på hur toocreate ett Apache Spark-kluster i HDInsight."
keywords: "spark-snabbstart,interaktiv spark,interaktiv fråga,hdinsight spark,azure spark"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 91f41e6a-d463-4eb4-83ef-7bbb1f4556cc
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 002f71b3cd4fb315d4a556cebc9263026515ec4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="0eaec-104">Skapa ett Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="0eaec-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="0eaec-105">I den här artikeln lär du dig hur toocreate ett Apache Spark-klustret i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0eaec-105">In this article, you learn how toocreate an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="0eaec-106">Mer information om Spark på HDInsight finns i [Översikt: Apache Spark på Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0eaec-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="0eaec-107">![Diagram för Snabbstart som beskriver stegen toocreate ett Apache Spark-kluster i Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark Snabbstart med Apache Spark i HDInsight. Illustrerade steg: skapa ett kluster, kör interaktiv Spark-fråga")</span><span class="sxs-lookup"><span data-stu-id="0eaec-107">![Quickstart diagram describing steps toocreate an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0eaec-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0eaec-108">Prerequisites</span></span>

* <span data-ttu-id="0eaec-109">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-109">**An Azure subscription**.</span></span> <span data-ttu-id="0eaec-110">Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0eaec-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="0eaec-111">Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="0eaec-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="0eaec-112">Skapa HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0eaec-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="0eaec-113">I det här avsnittet skapar du ett Spark-kluster i HDInsight med en [Azure Resource Manager-mall](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="0eaec-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="0eaec-114">Information om andra metoder för att skapa kluster finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0eaec-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="0eaec-115">Klicka på hello följande bild tooopen hello mallen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0eaec-115">Click hello following image tooopen hello template in hello Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="0eaec-116">Ange hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="0eaec-116">Enter hello following values:</span></span>

    <span data-ttu-id="0eaec-117">![Skapa HDInsight Spark-kluster med en Azure Resource Manager-mall](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Skapa Spark-kluster i HDInsight med en Azure Resource Manager-mall")</span><span class="sxs-lookup"><span data-stu-id="0eaec-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="0eaec-118">**Prenumeration**: Välj din Azure-prenumeration för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="0eaec-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="0eaec-119">**Resursgrupp**: Skapa en resursgrupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="0eaec-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="0eaec-120">Resursgruppen är används toomanage Azure-resurser för dina projekt.</span><span class="sxs-lookup"><span data-stu-id="0eaec-120">Resource group is used toomanage Azure resources for your projects.</span></span>
    * <span data-ttu-id="0eaec-121">**Plats**: Välj en plats för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0eaec-121">**Location**: Select a location for hello resource group.</span></span> <span data-ttu-id="0eaec-122">hello-mallen använder den här platsen för att skapa hello kluster även för hello standard klusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="0eaec-122">hello template uses this location for creating hello cluster as well as for hello default cluster storage.</span></span>
    * <span data-ttu-id="0eaec-123">**Klusternamn**: Ange ett namn för hello HDInsight-kluster som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="0eaec-123">**ClusterName**: Enter a name for hello HDInsight cluster that you want toocreate.</span></span>
    * <span data-ttu-id="0eaec-124">**Spark version**: Välj **2.0** som hello-version som du vill tooinstall på hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0eaec-124">**Spark version**: Select **2.0** as hello version that you want tooinstall on hello cluster.</span></span>
    * <span data-ttu-id="0eaec-125">**Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är administratör.</span><span class="sxs-lookup"><span data-stu-id="0eaec-125">**Cluster login name and password**: hello default login name is admin.</span></span>
    * <span data-ttu-id="0eaec-126">**SSH-användarnamn och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="0eaec-127">Anteckna dessa värden.</span><span class="sxs-lookup"><span data-stu-id="0eaec-127">Write down these values.</span></span>  <span data-ttu-id="0eaec-128">Du behöver dem senare i självstudiekursen hello.</span><span class="sxs-lookup"><span data-stu-id="0eaec-128">You need them later in hello tutorial.</span></span>

3. <span data-ttu-id="0eaec-129">Välj **acceptera toohello villkoren ovan**väljer **PIN-kod toodashboard**, och klicka sedan på **inköp**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-129">Select **I agree toohello terms and conditions stated above**, select **Pin toodashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="0eaec-130">En ny panel visas med rubriken Skicka distribution för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="0eaec-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="0eaec-131">Det tar cirka 20 minuter toocreate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0eaec-131">It takes about 20 minutes toocreate hello cluster.</span></span>

<span data-ttu-id="0eaec-132">Om du stöter på ett problem med att skapa HDInsight-kluster kan bero det på att du inte har hello rätt behörigheter toodo så.</span><span class="sxs-lookup"><span data-stu-id="0eaec-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have hello right permissions toodo so.</span></span> <span data-ttu-id="0eaec-133">Mer information finns i [åtkomstkravkontrollen](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="0eaec-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="0eaec-134">Den här artikeln skapar ett Spark-kluster som använder [Azure Storage-Blobbar som hello kluster lagring](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="0eaec-134">This article creates a Spark cluster that uses [Azure Storage Blobs as hello cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="0eaec-135">Du kan också skapa ett Spark-kluster som använder [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) som hello standardlagring.</span><span class="sxs-lookup"><span data-stu-id="0eaec-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as hello default storage.</span></span> <span data-ttu-id="0eaec-136">Instruktioner finns i [Skapa ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0eaec-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="0eaec-137">Köra en Hive-fråga med Spark SQL</span><span class="sxs-lookup"><span data-stu-id="0eaec-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="0eaec-138">När du använder en Jupyter-anteckningsbok som konfigurerats för HDInsight Spark-kluster kan du få en förinställning `sqlContext` som du kan använda toorun Hive-frågor med Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="0eaec-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use toorun Hive queries using Spark SQL.</span></span> <span data-ttu-id="0eaec-139">I det här avsnittet lär du dig hur toostart en Jupyter-anteckningsbok och kör sedan en grundläggande Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="0eaec-139">In this section, you learn how toostart a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="0eaec-140">Öppna hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0eaec-140">Open hello [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="0eaec-141">Om du har valt toopin hello klusterinstrumentpanel toohello Klicka hello klustret panelen från hello instrumentpanelen toolaunch hello klusterbladet.</span><span class="sxs-lookup"><span data-stu-id="0eaec-141">If you opted toopin hello cluster toohello dashboard, click hello cluster tile from hello dashboard toolaunch hello cluster blade.</span></span>

    <span data-ttu-id="0eaec-142">Om du inte fäster hello klustret toohello instrumentpanelen från hello till vänster och klicka på **HDInsight-kluster**, och klicka sedan på hello-klustret som du skapade.</span><span class="sxs-lookup"><span data-stu-id="0eaec-142">If you did not pin hello cluster toohello dashboard, from hello left pane, click **HDInsight clusters**, and then click hello cluster you created.</span></span>

3. <span data-ttu-id="0eaec-143">I **Snabblänkar** klickar du på **Klusterinstrumentpaneler** och sedan på **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="0eaec-144">Om du uppmanas ange hello administratörsautentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0eaec-144">If prompted, enter hello admin credentials for hello cluster.</span></span>

   <span data-ttu-id="0eaec-145">![Öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-frågan](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "öppna Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="0eaec-145">![Open Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook toorun interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="0eaec-146">Du kan också komma åt hello Jupyter notebook för ditt kluster genom att öppna hello följande URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="0eaec-146">You may also access hello Jupyter notebook for your cluster by opening hello following URL in your browser.</span></span> <span data-ttu-id="0eaec-147">Ersätt **KLUSTERNAMN** med hello namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="0eaec-147">Replace **CLUSTERNAME** with hello name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="0eaec-148">Skapa en anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="0eaec-148">Create a notebook.</span></span> <span data-ttu-id="0eaec-149">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="0eaec-150">![Skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "skapa en Jupyter-anteckningsbok toorun interaktiva Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="0eaec-150">![Create a Jupyter notebook toorun interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook toorun interactive Spark SQL query")</span></span>

   <span data-ttu-id="0eaec-151">En ny anteckningsbok skapas och öppnas med hello namnet Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="0eaec-151">A new notebook is created and opened with hello name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="0eaec-152">Klicka på hello anteckningsbokens namn högst hello upp och ange ett eget namn om du vill.</span><span class="sxs-lookup"><span data-stu-id="0eaec-152">Click hello notebook name at hello top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="0eaec-153">![Ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "ange ett namn för hello Jupter anteckningsboken toorun interaktiva Spark frågan från")</span><span class="sxs-lookup"><span data-stu-id="0eaec-153">![Provide a name for hello Jupter notebook toorun interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for hello Jupter notebook toorun interactive Spark query from")</span></span>

5.  <span data-ttu-id="0eaec-154">Klistra in hello följande kod i en tom cell och tryck sedan på **SKIFT + RETUR** toorun hello kod.</span><span class="sxs-lookup"><span data-stu-id="0eaec-154">Paste hello following code in an empty cell, and then press **SHIFT + ENTER** toorun hello code.</span></span> <span data-ttu-id="0eaec-155">I hello koden nedan `%%sql` (kallas hello sql magic) anger Jupyter-anteckningsbok toouse hello förinställningen `sqlContext` toorun hello Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="0eaec-155">In hello code below `%%sql` (called hello sql magic) tells Jupyter notebook toouse hello preset `sqlContext` toorun hello Hive query.</span></span> <span data-ttu-id="0eaec-156">hello fråga hämtar hello översta 10 rader från en Hive-tabell (**hivesampletable**) som är tillgängligt som standard på alla HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0eaec-156">hello query retrieves hello top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="0eaec-157">![Hive-fråga i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-fråga i HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="0eaec-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="0eaec-158">Mer information om hello `%%sql` magic och hello förinställningen kontexter, se [Jupyter kernlar som är tillgängliga för ett HDInsight-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="0eaec-158">For more information on hello `%%sql` magic and hello preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="0eaec-159">Varje gång du kör en fråga i Jupyter fönsternamn din web webbläsaren visar en **(upptagen)** status tillsammans med hello anteckningsbokens titel.</span><span class="sxs-lookup"><span data-stu-id="0eaec-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with hello notebook title.</span></span> <span data-ttu-id="0eaec-160">Du också se en fylld cirkel nästa toohello **PySpark** text i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="0eaec-160">You also see a solid circle next toohello **PySpark** text in hello top-right corner.</span></span> <span data-ttu-id="0eaec-161">När hello jobbet har slutförts ändras tooa tom cirkel.</span><span class="sxs-lookup"><span data-stu-id="0eaec-161">After hello job is completed, it changes tooa hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="0eaec-162">hello-skärmen bör uppdatera tooshow hello frågeresultatet.</span><span class="sxs-lookup"><span data-stu-id="0eaec-162">hello screen should refresh tooshow hello query output.</span></span>

    <span data-ttu-id="0eaec-163">![Hive-frågeresultatet i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-frågeresultatet i HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="0eaec-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="0eaec-164">Stäng hello anteckningsboken toorelease hello klusterresurser när du har kört programmet hello.</span><span class="sxs-lookup"><span data-stu-id="0eaec-164">Shut down hello notebook toorelease hello cluster resources after you have finished running hello application.</span></span> <span data-ttu-id="0eaec-165">toodo så från hello **filen** Klicka på menyn på hello anteckningsboken **Stäng och stoppa**.</span><span class="sxs-lookup"><span data-stu-id="0eaec-165">toodo so, from hello **File** menu on hello notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="0eaec-166">Om du planerar toocomplete hello nästa steg vid ett senare tillfälle kan du kontrollera att du tar bort hello HDInsight-kluster som du skapade i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="0eaec-166">If you plan toocomplete hello next steps at a later time, make sure you delete hello HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="0eaec-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0eaec-167">Next step</span></span> 

<span data-ttu-id="0eaec-168">I den här artikeln som du har lärt dig hur toocreate ett HDInsight Spark-kluster och kör en grundläggande Spark SQL-frågan.</span><span class="sxs-lookup"><span data-stu-id="0eaec-168">In this article you learned how toocreate an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="0eaec-169">Avancera toohello nästa artikel toolearn hur toouse ett HDInsight Spark-kluster toorun interaktiva frågor på exempeldata.</span><span class="sxs-lookup"><span data-stu-id="0eaec-169">Advance toohello next article toolearn how toouse an HDInsight Spark cluster toorun interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="0eaec-170">Köra interaktiva frågor på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="0eaec-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



