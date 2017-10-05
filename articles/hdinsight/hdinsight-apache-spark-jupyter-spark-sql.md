---
title: Skapa ett Apache Spark-kluster i Azure HDInsight | Microsoft Docs
description: HDInsight Spark-snabbstart om hur du skapar ett Apache Spark-kluster i HDInsight.
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
ms.openlocfilehash: ad4330a1fc7f8de154d9aaa8df3acc2ab59b9dc1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-spark-cluster-in-azure-hdinsight"></a><span data-ttu-id="d490f-104">Skapa ett Apache Spark-kluster i Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="d490f-104">Create an Apache Spark cluster in Azure HDInsight</span></span>

<span data-ttu-id="d490f-105">I den här artikeln får du lära dig hur du skapar ett Apache Spark-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d490f-105">In this article, you learn how to create an Apache Spark cluster in Azure HDInsight.</span></span> <span data-ttu-id="d490f-106">Mer information om Spark på HDInsight finns i [Översikt: Apache Spark på Azure HDInsight](hdinsight-apache-spark-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d490f-106">For information on Spark on HDInsight, see [Overview: Apache Spark on Azure HDInsight](hdinsight-apache-spark-overview.md).</span></span>

   <span data-ttu-id="d490f-107">![Snabbstartsdiagram som beskriver steg för att skapa ett Apache Spark-kluster på Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark-snabbstart med Apache Spark i HDInsight. Illustrerade steg: skapa ett kluster, kör interaktiv Spark-fråga")</span><span class="sxs-lookup"><span data-stu-id="d490f-107">![Quickstart diagram describing steps to create an Apache Spark cluster on Azure HDInsight](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-quickstart-interactive-spark-query-flow.png "Spark quickstart using Apache Spark in HDInsight. Steps illustrated: create a cluster; run Spark interactive query")</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d490f-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d490f-108">Prerequisites</span></span>

* <span data-ttu-id="d490f-109">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="d490f-109">**An Azure subscription**.</span></span> <span data-ttu-id="d490f-110">Innan du börjar följa de här självstudierna måste du ha en Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d490f-110">Before you begin this tutorial, you must have an Azure subscription.</span></span> <span data-ttu-id="d490f-111">Se [Skapa ett kostnadsfritt Azure-konto i dag](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="d490f-111">See [Create your free Azure account today](https://azure.microsoft.com/free).</span></span>

## <a name="create-hdinsight-spark-cluster"></a><span data-ttu-id="d490f-112">Skapa HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="d490f-112">Create HDInsight Spark cluster</span></span>

<span data-ttu-id="d490f-113">I det här avsnittet skapar du ett Spark-kluster i HDInsight med en [Azure Resource Manager-mall](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span><span class="sxs-lookup"><span data-stu-id="d490f-113">In this section, you create an HDInsight Spark cluster using an [Azure Resource Manager template](https://azure.microsoft.com/resources/templates/101-hdinsight-spark-linux/).</span></span> <span data-ttu-id="d490f-114">Information om andra metoder för att skapa kluster finns i [Skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="d490f-114">For other cluster creation methods, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="d490f-115">Klicka på följande bild för att öppna mallen i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="d490f-115">Click the following image to open the template in the Azure portal.</span></span>         

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-spark-linux%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-jupyter-spark-sql/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. <span data-ttu-id="d490f-116">Ange följande värden:</span><span class="sxs-lookup"><span data-stu-id="d490f-116">Enter the following values:</span></span>

    <span data-ttu-id="d490f-117">![Skapa HDInsight Spark-kluster med en Azure Resource Manager-mall](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Skapa Spark-kluster i HDInsight med en Azure Resource Manager-mall")</span><span class="sxs-lookup"><span data-stu-id="d490f-117">![Create HDInsight Spark cluster using an Azure Resource Manager template](./media/hdinsight-apache-spark-jupyter-spark-sql/create-spark-cluster-in-hdinsight-using-azure-resource-manager-template.png "Create Spark cluster in HDInsight using an Azure Resource Manager template")</span></span>

    * <span data-ttu-id="d490f-118">**Prenumeration**: Välj din Azure-prenumeration för det här klustret.</span><span class="sxs-lookup"><span data-stu-id="d490f-118">**Subscription**: Select your Azure subscription for this cluster.</span></span>
    * <span data-ttu-id="d490f-119">**Resursgrupp**: Skapa en resursgrupp eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="d490f-119">**Resource group**: Create a resource group or select an existing one.</span></span> <span data-ttu-id="d490f-120">Resursgrupp används för att hantera Azure-resurser till dina projekt.</span><span class="sxs-lookup"><span data-stu-id="d490f-120">Resource group is used to manage Azure resources for your projects.</span></span>
    * <span data-ttu-id="d490f-121">**Plats**: Välj en plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d490f-121">**Location**: Select a location for the resource group.</span></span> <span data-ttu-id="d490f-122">Mallen använder den här platsen för att skapa klustret samt standardklusterlagringen.</span><span class="sxs-lookup"><span data-stu-id="d490f-122">The template uses this location for creating the cluster as well as for the default cluster storage.</span></span>
    * <span data-ttu-id="d490f-123">**Klusternamn**: Ange ett namn på det HDInsight-kluster som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="d490f-123">**ClusterName**: Enter a name for the HDInsight cluster that you want to create.</span></span>
    * <span data-ttu-id="d490f-124">**Spark-version**: Välj **2.0** som den version som ska installeras i klustret.</span><span class="sxs-lookup"><span data-stu-id="d490f-124">**Spark version**: Select **2.0** as the version that you want to install on the cluster.</span></span>
    * <span data-ttu-id="d490f-125">**Klustrets inloggningsnamn och lösenord**: Inloggningsnamnet är som standard ”admin”.</span><span class="sxs-lookup"><span data-stu-id="d490f-125">**Cluster login name and password**: The default login name is admin.</span></span>
    * <span data-ttu-id="d490f-126">**SSH-användarnamn och lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d490f-126">**SSH user name and password**.</span></span>

   <span data-ttu-id="d490f-127">Anteckna dessa värden.</span><span class="sxs-lookup"><span data-stu-id="d490f-127">Write down these values.</span></span>  <span data-ttu-id="d490f-128">Du behöver dem senare under kursen.</span><span class="sxs-lookup"><span data-stu-id="d490f-128">You need them later in the tutorial.</span></span>

3. <span data-ttu-id="d490f-129">Välj **Jag godkänner villkoren som anges ovan**, välj **Fäst på instrumentpanelen** och klicka sedan på **Köp**.</span><span class="sxs-lookup"><span data-stu-id="d490f-129">Select **I agree to the terms and conditions stated above**, select **Pin to dashboard**, and then click **Purchase**.</span></span> <span data-ttu-id="d490f-130">En ny panel visas med rubriken Skicka distribution för malldistribution.</span><span class="sxs-lookup"><span data-stu-id="d490f-130">You can see a new tile titled Submitting deployment for Template deployment.</span></span> <span data-ttu-id="d490f-131">Det tar cirka 20 minuter att skapa klustret.</span><span class="sxs-lookup"><span data-stu-id="d490f-131">It takes about 20 minutes to create the cluster.</span></span>

<span data-ttu-id="d490f-132">Om du stöter på ett problem med att skapa HDInsight-kluster kan det bero på att du inte har rätt behörighet för att göra det.</span><span class="sxs-lookup"><span data-stu-id="d490f-132">If you run into an issue with creating HDInsight clusters, it could be that you do not have the right permissions to do so.</span></span> <span data-ttu-id="d490f-133">Mer information finns i [åtkomstkravkontrollen](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="d490f-133">See [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="d490f-134">Den här artikeln skapar ett Spark-kluster som använder [Azure Storage-blobar som klusterlagring](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="d490f-134">This article creates a Spark cluster that uses [Azure Storage Blobs as the cluster storage](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="d490f-135">Du kan även skapa ett Spark-kluster som använder [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) som standardlagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="d490f-135">You can also create a Spark cluster that uses [Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md) as the default storage.</span></span> <span data-ttu-id="d490f-136">Instruktioner finns i [Skapa ett HDInsight-kluster med Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d490f-136">For instructions, see [Create an HDInsight cluster with Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>
>
>

## <a name="run-a-hive-query-using-spark-sql"></a><span data-ttu-id="d490f-137">Köra en Hive-fråga med Spark SQL</span><span class="sxs-lookup"><span data-stu-id="d490f-137">Run a Hive query using Spark SQL</span></span>

<span data-ttu-id="d490f-138">När du använder en Jupyter-anteckningsbok som har konfigurerats för HDInsight Spark-klustret kan det visas en förinställning `sqlContext` som du kan använda för att köra Hive-frågor med hjälp av Spark SQL.</span><span class="sxs-lookup"><span data-stu-id="d490f-138">When you use a Jupyter notebook configured for your HDInsight Spark cluster, you get a preset `sqlContext` that you can use to run Hive queries using Spark SQL.</span></span> <span data-ttu-id="d490f-139">I det här avsnittet lär du dig att starta en Jupyter-anteckningsbok och sedan köra en grundläggande Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="d490f-139">In this section, you learn how to start a Jupyter notebook and then run a basic Hive query.</span></span>

1. <span data-ttu-id="d490f-140">Öppna [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d490f-140">Open the [Azure portal](https://portal.azure.com/).</span></span>

2. <span data-ttu-id="d490f-141">Om du har valt att fästa klustret på instrumentpanelen klickar du på klusterikonen på instrumentpanelen för att öppna klusterbladet.</span><span class="sxs-lookup"><span data-stu-id="d490f-141">If you opted to pin the cluster to the dashboard, click the cluster tile from the dashboard to launch the cluster blade.</span></span>

    <span data-ttu-id="d490f-142">Om du inte har fäst klustret på instrumentpanelen går du till den vänstra rutan och klickar på **HDInsight-kluster**. Klicka sedan på det kluster du har skapat.</span><span class="sxs-lookup"><span data-stu-id="d490f-142">If you did not pin the cluster to the dashboard, from the left pane, click **HDInsight clusters**, and then click the cluster you created.</span></span>

3. <span data-ttu-id="d490f-143">I **Snabblänkar** klickar du på **Klusterinstrumentpaneler** och sedan på **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="d490f-143">From **Quick links**, click **Cluster dashboards**, and then click **Jupyter Notebook**.</span></span> <span data-ttu-id="d490f-144">Ange administratörsautentiseringsuppgifterna för klustret om du uppmanas att göra det.</span><span class="sxs-lookup"><span data-stu-id="d490f-144">If prompted, enter the admin credentials for the cluster.</span></span>

   <span data-ttu-id="d490f-145">![Öppna Jupyter-anteckningsboken för att köra interaktiv Spark SQL-fråga](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Öppna Jupyter-anteckningsboken för att köra interaktiv Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="d490f-145">![Open Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-open-jupyter-interactive-spark-sql-query.png "Open Jupyter notebook to run interactive Spark SQL query")</span></span>

   > [!NOTE]
   > <span data-ttu-id="d490f-146">Du kan också nå Jupyter-anteckningsboken för ditt kluster genom att öppna nedanstående URL i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="d490f-146">You may also access the Jupyter notebook for your cluster by opening the following URL in your browser.</span></span> <span data-ttu-id="d490f-147">Ersätt **CLUSTERNAME** med namnet på klustret:</span><span class="sxs-lookup"><span data-stu-id="d490f-147">Replace **CLUSTERNAME** with the name of your cluster:</span></span>
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >
3. <span data-ttu-id="d490f-148">Skapa en anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="d490f-148">Create a notebook.</span></span> <span data-ttu-id="d490f-149">Klicka på **Ny** och sedan på **PySpark**.</span><span class="sxs-lookup"><span data-stu-id="d490f-149">Click **New**, and then click **PySpark**.</span></span>

   <span data-ttu-id="d490f-150">![Skapa en Jupyter-anteckningsbok för att köra interaktiv Spark SQL-fråga](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Skapa en Jupyter-anteckningsbok för att köra interaktiv Spark SQL-fråga")</span><span class="sxs-lookup"><span data-stu-id="d490f-150">![Create a Jupyter notebook to run interactive Spark SQL query](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-create-jupyter-interactive-Spark-SQL-query.png "Create a Jupyter notebook to run interactive Spark SQL query")</span></span>

   <span data-ttu-id="d490f-151">En ny anteckningsbok skapas och öppnas med namnet Untitled(Untitled.pynb).</span><span class="sxs-lookup"><span data-stu-id="d490f-151">A new notebook is created and opened with the name Untitled(Untitled.pynb).</span></span>

4. <span data-ttu-id="d490f-152">Klicka på anteckningsbokens namn högst upp och ange ett beskrivande namn om du vill.</span><span class="sxs-lookup"><span data-stu-id="d490f-152">Click the notebook name at the top, and enter a friendly name if you want.</span></span>

    <span data-ttu-id="d490f-153">![Ange ett namn för Jupter-anteckningsboken för att köra en interaktiv Spark-fråga från](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Ange ett namn för Jupter-anteckningsboken för att köra en interaktiv Spark-fråga från")</span><span class="sxs-lookup"><span data-stu-id="d490f-153">![Provide a name for the Jupter notebook to run interactive Spark query from](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-jupyter-notebook-name.png "Provide a name for the Jupter notebook to run interactive Spark query from")</span></span>

5.  <span data-ttu-id="d490f-154">Klistra in följande kod i en tom cell och tryck sedan på **SKIFT+RETUR** för att köra koden.</span><span class="sxs-lookup"><span data-stu-id="d490f-154">Paste the following code in an empty cell, and then press **SHIFT + ENTER** to run the code.</span></span> <span data-ttu-id="d490f-155">I följande kodexempel anger `%%sql` (kallas sql magic) att Jupyter-anteckningsboken ska använda förinställningen `sqlContext` för att köra Hive-frågan.</span><span class="sxs-lookup"><span data-stu-id="d490f-155">In the code below `%%sql` (called the sql magic) tells Jupyter notebook to use the preset `sqlContext` to run the Hive query.</span></span> <span data-ttu-id="d490f-156">Frågan hämtar de översta 10 raderna från en Hive-tabell (**hivesampletable**) som är tillgängliga som standard i alla HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d490f-156">The query retrieves the top 10 rows from a Hive table (**hivesampletable**) that is available by default on all HDInsight clusters.</span></span>

        %%sql
        SELECT * FROM hivesampletable LIMIT 10

    <span data-ttu-id="d490f-157">![Hive-fråga i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive-fråga i HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="d490f-157">![Hive query in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query.png "Hive query in HDInsight Spark")</span></span>

    <span data-ttu-id="d490f-158">Mer information om `%%sql` och förinställda kontexter finns i [Jupyter-kernlar som är tillgängliga för ett HDInsight-kluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span><span class="sxs-lookup"><span data-stu-id="d490f-158">For more information on the `%%sql` magic and the preset contexts, see [Jupyter kernels available for an HDInsight cluster](hdinsight-apache-spark-jupyter-notebook-kernels.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d490f-159">Varje gång du kör en fråga i Jupyter visar fönsterrubriken i webbläsaren statusen **(Upptagen)** tillsammans med anteckningsbokens titel.</span><span class="sxs-lookup"><span data-stu-id="d490f-159">Every time you run a query in Jupyter, your web browser window title shows a **(Busy)** status along with the notebook title.</span></span> <span data-ttu-id="d490f-160">Du ser även en fylld cirkel bredvid **PySpark**-texten i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="d490f-160">You also see a solid circle next to the **PySpark** text in the top-right corner.</span></span> <span data-ttu-id="d490f-161">När jobbet har slutförts ändras detta till en tom cirkel.</span><span class="sxs-lookup"><span data-stu-id="d490f-161">After the job is completed, it changes to a hollow circle.</span></span>
    >
    >
    
6. <span data-ttu-id="d490f-162">Skärmen bör uppdateras så att frågeresultatet visas.</span><span class="sxs-lookup"><span data-stu-id="d490f-162">The screen should refresh to show the query output.</span></span>

    <span data-ttu-id="d490f-163">![Hive-frågeresultatet i HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive-frågeresultatet i HDInsight Spark")</span><span class="sxs-lookup"><span data-stu-id="d490f-163">![Hive query output in HDInsight Spark](./media/hdinsight-apache-spark-jupyter-spark-sql/hdinsight-spark-get-started-hive-query-output.png "Hive query output in HDInsight Spark")</span></span>

7. <span data-ttu-id="d490f-164">När du har kört programmet stänger du anteckningsboken för att frigöra klusterresurserna.</span><span class="sxs-lookup"><span data-stu-id="d490f-164">Shut down the notebook to release the cluster resources after you have finished running the application.</span></span> <span data-ttu-id="d490f-165">Du gör det genom att klicka på **Stäng och stoppa** i anteckningsbokens **Fil**-meny.</span><span class="sxs-lookup"><span data-stu-id="d490f-165">To do so, from the **File** menu on the notebook, click **Close and Halt**.</span></span>

8. <span data-ttu-id="d490f-166">Om du planerar att utföra nästa steg vid ett senare tillfälle är det viktigt att du tar bort HDInsight-klustret som du skapade i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="d490f-166">If you plan to complete the next steps at a later time, make sure you delete the HDInsight cluster you created in this article.</span></span> 

    [!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-step"></a><span data-ttu-id="d490f-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d490f-167">Next step</span></span> 

<span data-ttu-id="d490f-168">I den här artikeln beskrivs hur du skapar ett HDInsight Spark-kluster och kör en grundläggande Spark SQL-fråga.</span><span class="sxs-lookup"><span data-stu-id="d490f-168">In this article you learned how to create an HDInsight Spark cluster and run a basic Spark SQL query.</span></span> <span data-ttu-id="d490f-169">Gå vidare till nästa artikel om du vill lära dig hur du använder ett HDInsight Spark-kluster för att köra interaktiva frågor på exempeldata.</span><span class="sxs-lookup"><span data-stu-id="d490f-169">Advance to the next article to learn how to use an HDInsight Spark cluster to run interactive queries on sample data.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="d490f-170">Köra interaktiva frågor på ett HDInsight Spark-kluster</span><span class="sxs-lookup"><span data-stu-id="d490f-170">Run interactive queries on an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-load-data-run-query.md)



