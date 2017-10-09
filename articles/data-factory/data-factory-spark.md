---
title: "aaaInvoke Spark-program från Azure Data Factory | Microsoft Docs"
description: "Lär dig hur tooinvoke Spark-program från ett Azure data factory med hello MapReduce Activity."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: fd98931c-cab5-4d66-97cb-4c947861255c
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: spelluru
ms.openlocfilehash: f88943ece7ee3d21dedbd857609f1b2713b62741
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-spark-programs-from-azure-data-factory-pipelines"></a><span data-ttu-id="d98d9-103">Anropa Spark-program från Azure Data Factory pipelines</span><span class="sxs-lookup"><span data-stu-id="d98d9-103">Invoke Spark programs from Azure Data Factory pipelines</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="d98d9-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-104">Hive Activity</span></span>](data-factory-hive-activity.md)
> * [<span data-ttu-id="d98d9-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="d98d9-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="d98d9-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="d98d9-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="d98d9-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="d98d9-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="d98d9-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="d98d9-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="d98d9-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="d98d9-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="d98d9-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="d98d9-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="d98d9-114">Introduktion</span><span class="sxs-lookup"><span data-stu-id="d98d9-114">Introduction</span></span>
<span data-ttu-id="d98d9-115">Spark aktivitet är en av hello [data transformation aktiviteter](data-factory-data-transformation-activities.md) stöds av Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-115">Spark Activity is one of hello [data transformation activities](data-factory-data-transformation-activities.md) supported by Azure Data Factory.</span></span> <span data-ttu-id="d98d9-116">Den här aktiviteten körs hello angivna Spark-program på din Apache Spark-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="d98d9-116">This activity runs hello specified Spark program on your Apache Spark cluster in Azure HDInsight.</span></span>    

> [!IMPORTANT]
> - <span data-ttu-id="d98d9-117">Spark aktiviteten stöder inte HDInsight Spark-kluster som använder ett Azure Data Lake Store som primär lagring.</span><span class="sxs-lookup"><span data-stu-id="d98d9-117">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
> - <span data-ttu-id="d98d9-118">Spark aktiviteten stöder endast befintliga (egna) HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-118">Spark Activity supports only existing (your own) HDInsight Spark clusters.</span></span> <span data-ttu-id="d98d9-119">Det stöder inte en länkad HDInsight på begäran-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d98d9-119">It does not support an on-demand HDInsight linked service.</span></span>

## <a name="walkthrough-create-a-pipeline-with-spark-activity"></a><span data-ttu-id="d98d9-120">Genomgång: skapa en pipeline med Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="d98d9-120">Walkthrough: create a pipeline with Spark activity</span></span>
<span data-ttu-id="d98d9-121">Här följer hello typiska steg toocreate Data Factory-pipelinen med ett Spark-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-121">Here are hello typical steps toocreate a Data Factory pipeline with a Spark activity.</span></span>  

1. <span data-ttu-id="d98d9-122">Skapa en datafabrik.</span><span class="sxs-lookup"><span data-stu-id="d98d9-122">Create a data factory.</span></span>
2. <span data-ttu-id="d98d9-123">Skapa en länkad Azure Storage service toolink ditt Azure storage som är associerad med din HDInsight Spark-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-123">Create an Azure Storage linked service toolink your Azure storage that is associated with your HDInsight Spark cluster toohello data factory.</span></span>     
2. <span data-ttu-id="d98d9-124">Skapa ett Azure HDInsight länkade tjänsten toolink Apache Spark-kluster i Azure HDInsight toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-124">Create an Azure HDInsight linked service toolink your Apache Spark cluster in Azure HDInsight toohello data factory.</span></span>
3. <span data-ttu-id="d98d9-125">Skapa en datamängd som refererar toohello länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d98d9-125">Create a dataset that refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="d98d9-126">För närvarande måste du ange en datamängd för utdata för en aktivitet även om det finns inga utdata som skapas.</span><span class="sxs-lookup"><span data-stu-id="d98d9-126">Currently, you must specify an output dataset for an activity even if there is no output being produced.</span></span>  
4. <span data-ttu-id="d98d9-127">Skapa en pipeline med Spark-aktivitet som refererar toohello HDInsight länkad tjänst skapas i #2.</span><span class="sxs-lookup"><span data-stu-id="d98d9-127">Create a pipeline with Spark activity that refers toohello HDInsight linked service created in #2.</span></span> <span data-ttu-id="d98d9-128">hello-aktiviteten är konfigurerad med hello dataset som du skapade i föregående steg för hello som en datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-128">hello activity is configured with hello dataset you created in hello previous step as an output dataset.</span></span> <span data-ttu-id="d98d9-129">hello datamängd för utdata är vilka enheter hello schema (varje timme, varje dag, osv.).</span><span class="sxs-lookup"><span data-stu-id="d98d9-129">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="d98d9-130">Därför måste du ange hello utdatauppsättningen även om hello aktiviteten inte verkligen ger utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-130">Therefore, you must specify hello output dataset even though hello activity does not really produce an output.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="d98d9-131">Krav</span><span class="sxs-lookup"><span data-stu-id="d98d9-131">Prerequisites</span></span>
1. <span data-ttu-id="d98d9-132">Skapa en **allmänna Azure Storage-konto** genom att följa anvisningarna i hello genomgång: [skapa ett lagringskonto](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d98d9-132">Create a **general-purpose Azure Storage Account** by following instructions in hello walkthrough: [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>  
2. <span data-ttu-id="d98d9-133">Skapa en **Apache Spark-kluster i Azure HDInsight** genom att följa anvisningarna i kursen hello: [skapa Apache Spark-kluster i Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="d98d9-133">Create an **Apache Spark cluster in Azure HDInsight** by following instructions in hello tutorial: [Create Apache Spark cluster in Azure HDInsight](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md).</span></span> <span data-ttu-id="d98d9-134">Associera hello Azure storage-konto som du skapade i steg #1 med det här klustret.</span><span class="sxs-lookup"><span data-stu-id="d98d9-134">Associate hello Azure storage account you created in step #1 with this cluster.</span></span>  
3. <span data-ttu-id="d98d9-135">Hämta och granska hello python skriptfilen **test.py** finns på: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span><span class="sxs-lookup"><span data-stu-id="d98d9-135">Download and review hello python script file **test.py** located at: [https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py](https://adftutorialfiles.blob.core.windows.net/sparktutorial/test.py).</span></span>  
3.  <span data-ttu-id="d98d9-136">Överför **test.py** toohello **pyFiles** mapp i hello **adfspark** behållare i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="d98d9-136">Upload **test.py** toohello **pyFiles** folder in hello **adfspark** container in your Azure Blob storage.</span></span> <span data-ttu-id="d98d9-137">Skapa hello-behållaren och hello mappen om de inte finns.</span><span class="sxs-lookup"><span data-stu-id="d98d9-137">Create hello container and hello folder if they do not exist.</span></span>

### <a name="create-data-factory"></a><span data-ttu-id="d98d9-138">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="d98d9-138">Create data factory</span></span>
<span data-ttu-id="d98d9-139">Låt oss börja med att skapa hello data factory i det här steget.</span><span class="sxs-lookup"><span data-stu-id="d98d9-139">Let's start with creating hello data factory in this step.</span></span>

1. <span data-ttu-id="d98d9-140">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d98d9-140">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d98d9-141">Klicka på **ny** på hello vänstra menyn **Data + analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-141">Click **NEW** on hello left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="d98d9-142">I hello **nya datafabriken** bladet ange **SparkDF** för hello namn.</span><span class="sxs-lookup"><span data-stu-id="d98d9-142">In hello **New data factory** blade, enter **SparkDF** for hello Name.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="d98d9-143">hello hello Azure data factory måste vara **globalt unika**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-143">hello name of hello Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="d98d9-144">Om du ser hello-fel: **datafabriksnamnet ”SparkDF” är inte tillgänglig**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-144">If you see hello error: **Data factory name “SparkDF” is not available**.</span></span> <span data-ttu-id="d98d9-145">Ändra hello namn i hello data factory (till exempel yournameSparkDFdate och försök att skapa igen.</span><span class="sxs-lookup"><span data-stu-id="d98d9-145">Change hello name of hello data factory (for example, yournameSparkDFdate, and try creating again.</span></span> <span data-ttu-id="d98d9-146">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="d98d9-146">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>   
4. <span data-ttu-id="d98d9-147">Välj hello **Azure-prenumeration** där du vill att hello data factory toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="d98d9-147">Select hello **Azure subscription** where you want hello data factory toobe created.</span></span>
5. <span data-ttu-id="d98d9-148">Välj en befintlig **resursgruppen** eller skapa ett Azure-resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d98d9-148">Select an existing **resource group** or create an Azure resource group.</span></span>
6. <span data-ttu-id="d98d9-149">Välj **PIN-kod toodashboard** alternativet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-149">Select **Pin toodashboard** option.</span></span>  
6. <span data-ttu-id="d98d9-150">Klicka på **skapa** på hello **nya data factory** bladet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-150">Click **Create** on hello **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="d98d9-151">toocreate Data Factory instanser måste du vara medlem i hello [Data Factory deltagare](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) rollen på hello-prenumeration/resursgruppsnivå.</span><span class="sxs-lookup"><span data-stu-id="d98d9-151">toocreate Data Factory instances, you must be a member of hello [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at hello subscription/resource group level.</span></span>
7. <span data-ttu-id="d98d9-152">Du ser hello data factory som skapas i hello **instrumentpanelen** av hello Azure-portalen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="d98d9-152">You see hello data factory being created in hello **dashboard** of hello Azure portal as follows:</span></span>   
8. <span data-ttu-id="d98d9-153">När hello datafabriken har skapats, visas hello data factory sidan som visar hello innehållet i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-153">After hello data factory has been created successfully, you see hello data factory page, which shows you hello contents of hello data factory.</span></span> <span data-ttu-id="d98d9-154">Om hello data factory-sidan inte visas klickar du på hello panelen för din data factory på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="d98d9-154">If you do not see hello data factory page, click hello tile for your data factory on hello dashboard.</span></span>

    ![Bladet Datafabrik](./media/data-factory-spark/data-factory-blade.png)

### <a name="create-linked-services"></a><span data-ttu-id="d98d9-156">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="d98d9-156">Create linked services</span></span>
<span data-ttu-id="d98d9-157">I det här steget och skapa två länkade tjänster, en toolink din Spark-kluster tooyour data factory och hello andra toolink din Azure storage tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-157">In this step, you create two linked services, one toolink your Spark cluster tooyour data factory, and hello other toolink your Azure storage tooyour data factory.</span></span>  

#### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="d98d9-158">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="d98d9-158">Create Azure Storage linked service</span></span>
<span data-ttu-id="d98d9-159">I det här steget kan länka du din Azure Storage-konto tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-159">In this step, you link your Azure Storage account tooyour data factory.</span></span> <span data-ttu-id="d98d9-160">En datauppsättning som du skapar i ett steg senare i den här genomgången refererar toothis länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d98d9-160">A dataset you create in a step later in this walkthrough refers toothis linked service.</span></span> <span data-ttu-id="d98d9-161">Hej HDInsight länkade tjänst som du definierar i hello nästa steg refererar toothis länkad tjänst för.</span><span class="sxs-lookup"><span data-stu-id="d98d9-161">hello HDInsight linked service that you define in hello next step refers toothis linked service too.</span></span>  

1. <span data-ttu-id="d98d9-162">Klicka på **författare och distribuera** på hello **Data Factory** bladet för din data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-162">Click **Author and deploy** on hello **Data Factory** blade for your data factory.</span></span> <span data-ttu-id="d98d9-163">Du bör se hello Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="d98d9-163">You should see hello Data Factory Editor.</span></span>
2. <span data-ttu-id="d98d9-164">Klicka på **Nytt datalager** och välj **Azure-lagring**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-164">Click **New data store** and choose **Azure storage**.</span></span>

   ![Ny datalagring - Azure Storage - meny](./media/data-factory-spark/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="d98d9-166">Du bör se hello **JSON-skript** länkade tjänsten i hello redigerare för att skapa ett Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d98d9-166">You should see hello **JSON script** for creating an Azure Storage linked service in hello editor.</span></span>

   ![Länkad Azure-lagringstjänst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="d98d9-168">Ersätt **kontonamn** och **kontonyckel** med hello namn och åtkomstnyckel för Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d98d9-168">Replace **account name** and **account key** with hello name and access key of your Azure storage account.</span></span> <span data-ttu-id="d98d9-169">toolearn hur tooget lagringen åtkomst till nyckeln, se hello information om hur tooview, kopiera och generera lagring åtkomstnycklar i [hantera ditt lagringskonto](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d98d9-169">toolearn how tooget your storage access key, see hello information about how tooview, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="d98d9-170">toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="d98d9-170">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span> <span data-ttu-id="d98d9-171">När hello länkade tjänsten har distribuerats korrekt hello **utkast 1** fönstret bör försvinner och du ser **AzureStorageLinkedService** i hello trädvyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-171">After hello linked service is deployed successfully, hello **Draft-1** window should disappear and you see **AzureStorageLinkedService** in hello tree view on hello left.</span></span>

#### <a name="create-hdinsight-linked-service"></a><span data-ttu-id="d98d9-172">Skapa länkad HDInsight-tjänst</span><span class="sxs-lookup"><span data-stu-id="d98d9-172">Create HDInsight linked service</span></span>
<span data-ttu-id="d98d9-173">I det här steget skapar du Azure HDInsight länkade tjänsten toolink din HDInsight Spark-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-173">In this step, you create Azure HDInsight linked service toolink your HDInsight Spark cluster toohello data factory.</span></span> <span data-ttu-id="d98d9-174">Hej HDInsight-kluster är används toorun hello Spark-program som angetts i hello Spark-aktivitet för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-174">hello HDInsight cluster is used toorun hello Spark program specified in hello Spark activity of hello pipeline in this sample.</span></span>  

1. <span data-ttu-id="d98d9-175">Klicka på **... Flera** på hello verktygsfältet **nya beräkning**, och klicka sedan på **HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-175">Click **... More** on hello toolbar, click **New compute**, and then click **HDInsight cluster**.</span></span>

    ![Skapa länkad HDInsight-tjänst](media/data-factory-spark/new-hdinsight-linked-service.png)
2. <span data-ttu-id="d98d9-177">Kopiera och klistra in följande kodutdrag toohello hello **utkast 1** fönster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-177">Copy and paste hello following snippet toohello **Draft-1** window.</span></span> <span data-ttu-id="d98d9-178">I Redigeraren för hello JSON hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d98d9-178">In hello JSON editor, do hello following steps:</span></span>
    1. <span data-ttu-id="d98d9-179">Ange hello **URI** för hello HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-179">Specify hello **URI** for hello HDInsight Spark cluster.</span></span> <span data-ttu-id="d98d9-180">Till exempel: `https://<sparkclustername>.azurehdinsight.net/`.</span><span class="sxs-lookup"><span data-stu-id="d98d9-180">For example: `https://<sparkclustername>.azurehdinsight.net/`.</span></span>
    2. <span data-ttu-id="d98d9-181">Ange hello namn hello **användaren** vem som har åtkomst toohello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-181">Specify hello name of hello **user** who has access toohello Spark cluster.</span></span>
    3. <span data-ttu-id="d98d9-182">Ange hello **lösenord** för användaren.</span><span class="sxs-lookup"><span data-stu-id="d98d9-182">Specify hello **password** for user.</span></span>
    4. <span data-ttu-id="d98d9-183">Ange hello **Azure länkade lagringstjänsten** som är associerad med hello HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-183">Specify hello **Azure Storage linked service** that is associated with hello HDInsight Spark cluster.</span></span> <span data-ttu-id="d98d9-184">I det här exemplet är: **AzureStorageLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-184">In this example, it is: **AzureStorageLinkedService**.</span></span>

    ```json
    {
        "name": "HDInsightLinkedService",
        "properties": {
            "type": "HDInsight",
            "typeProperties": {
                "clusterUri": "https://<sparkclustername>.azurehdinsight.net/",
                "userName": "admin",
                "password": "**********",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    > [!IMPORTANT]
    > - <span data-ttu-id="d98d9-185">Spark aktiviteten stöder inte HDInsight Spark-kluster som använder ett Azure Data Lake Store som primär lagring.</span><span class="sxs-lookup"><span data-stu-id="d98d9-185">Spark Activity does not support HDInsight Spark clusters that use an Azure Data Lake Store as primary storage.</span></span>
    > - <span data-ttu-id="d98d9-186">Spark aktiviteten stöder endast befintliga (egna) HDInsight Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-186">Spark Activity supports only existing (your own) HDInsight Spark cluster.</span></span> <span data-ttu-id="d98d9-187">Det stöder inte en länkad HDInsight på begäran-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d98d9-187">It does not support an on-demand HDInsight linked service.</span></span>

    <span data-ttu-id="d98d9-188">Se [länkad HDInsight-tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) mer information om hello HDInsight länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d98d9-188">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details about hello HDInsight linked service.</span></span>
3.  <span data-ttu-id="d98d9-189">toodeploy hello länkade tjänsten klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="d98d9-189">toodeploy hello linked service, click **Deploy** on hello command bar.</span></span>  

### <a name="create-output-dataset"></a><span data-ttu-id="d98d9-190">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="d98d9-190">Create output dataset</span></span>
<span data-ttu-id="d98d9-191">hello datamängd för utdata är vilka enheter hello schema (varje timme, varje dag, osv.).</span><span class="sxs-lookup"><span data-stu-id="d98d9-191">hello output dataset is what drives hello schedule (hourly, daily, etc.).</span></span> <span data-ttu-id="d98d9-192">Därför måste du ange en datamängd för utdata för hello spark aktiviteten i hello pipeline även om hello aktivitet verkligen inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-192">Therefore, you must specify an output dataset for hello spark activity in hello pipeline even though hello activity does not really produce any output.</span></span> <span data-ttu-id="d98d9-193">Ange en inkommande datauppsättning för hello aktiviteten är valfritt.</span><span class="sxs-lookup"><span data-stu-id="d98d9-193">Specifying an input dataset for hello activity is optional.</span></span>

1. <span data-ttu-id="d98d9-194">I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet klickar du på **ny datamängd**, och välj **Azure Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-194">In hello **Data Factory Editor**, click **... More** on hello command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="d98d9-195">Kopiera och klistra in hello följande fragment toohello utkast-1-fönstret.</span><span class="sxs-lookup"><span data-stu-id="d98d9-195">Copy and paste hello following snippet toohello Draft-1 window.</span></span> <span data-ttu-id="d98d9-196">hello JSON fragment definierar en datamängd som kallas **OutputDataset**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-196">hello JSON snippet defines a dataset called **OutputDataset**.</span></span> <span data-ttu-id="d98d9-197">Dessutom kan du ange att hello resultat lagras i hello blob-behållaren som kallas **adfspark** och hello mapp som heter **pyFiles-/ utdata**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-197">In addition, you specify that hello results are stored in hello blob container called **adfspark** and hello folder called **pyFiles/output**.</span></span> <span data-ttu-id="d98d9-198">Som tidigare nämnts är den här datauppsättningen dummy dataset.</span><span class="sxs-lookup"><span data-stu-id="d98d9-198">As mentioned earlier, this dataset is a dummy dataset.</span></span> <span data-ttu-id="d98d9-199">hello Spark-program i det här exemplet skapar inte några utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-199">hello Spark program in this example does not produce any output.</span></span> <span data-ttu-id="d98d9-200">Hej **tillgänglighet** avsnittet anger att hello utdatauppsättningen skapas varje dag.</span><span class="sxs-lookup"><span data-stu-id="d98d9-200">hello **availability** section specifies that hello output dataset is produced daily.</span></span>  

    ```json
    {
        "name": "OutputDataset",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "sparkoutput.txt",
                "folderPath": "adfspark/pyFiles/output",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }
    ```
3. <span data-ttu-id="d98d9-201">toodeploy Hej dataset, klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="d98d9-201">toodeploy hello dataset, click **Deploy** on hello command bar.</span></span>


### <a name="create-pipeline"></a><span data-ttu-id="d98d9-202">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="d98d9-202">Create pipeline</span></span>
<span data-ttu-id="d98d9-203">I det här steget skapar du en pipeline med en **HDInsightSpark** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-203">In this step, you create a pipeline with a **HDInsightSpark** activity.</span></span> <span data-ttu-id="d98d9-204">Datamängd för utdata är för närvarande vilka enheter hello schema, så du måste skapa en datamängd för utdata även om hello aktiviteten inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-204">Currently, output dataset is what drives hello schedule, so you must create an output dataset even if hello activity does not produce any output.</span></span> <span data-ttu-id="d98d9-205">Om hello aktiviteten inte tar några indata, kan du hoppa över skapar hello inkommande dataset.</span><span class="sxs-lookup"><span data-stu-id="d98d9-205">If hello activity doesn't take any input, you can skip creating hello input dataset.</span></span> <span data-ttu-id="d98d9-206">Därför har inga indata datamängden angetts i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-206">Therefore, no input dataset is specified in this example.</span></span>

1. <span data-ttu-id="d98d9-207">I hello **Data Factory-redigeraren**, klickar du på **... Flera** på hello kommandofältet och klicka sedan på **ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-207">In hello **Data Factory Editor**, click **… More** on hello command bar, and then click **New pipeline**.</span></span>
2. <span data-ttu-id="d98d9-208">Ersätt hello skript i hello utkast-1-fönster med hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="d98d9-208">Replace hello script in hello Draft-1 window with hello following script:</span></span>

    ```json
    {
        "name": "SparkPipeline",
        "properties": {
            "activities": [
                {
                    "type": "HDInsightSpark",
                    "typeProperties": {
                        "rootPath": "adfspark\\pyFiles",
                        "entryFilePath": "test.py",
                        "getDebugInfo": "Always"
                    },
                    "outputs": [
                        {
                            "name": "OutputDataset"
                        }
                    ],
                    "name": "MySparkActivity",
                    "linkedServiceName": "HDInsightLinkedService"
                }
            ],
            "start": "2017-02-05T00:00:00Z",
            "end": "2017-02-06T00:00:00Z"
        }
    }
    ```
    <span data-ttu-id="d98d9-209">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="d98d9-209">Note hello following points:</span></span>
    - <span data-ttu-id="d98d9-210">Hej **typen** egenskapen för**HDInsightSpark**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-210">hello **type** property is set too**HDInsightSpark**.</span></span>
    - <span data-ttu-id="d98d9-211">Hej **rootPath** har angetts för**adfspark\\pyFiles** där adfspark är hello Azure Blob-behållare och pyFiles är bra mapp i behållaren.</span><span class="sxs-lookup"><span data-stu-id="d98d9-211">hello **rootPath** is set too**adfspark\\pyFiles** where adfspark is hello Azure Blob container and pyFiles is fine folder in that container.</span></span> <span data-ttu-id="d98d9-212">I det här exemplet är hello Azure Blob Storage hello en som är associerad med hello Spark-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-212">In this example, hello Azure Blob Storage is hello one that is associated with hello Spark cluster.</span></span> <span data-ttu-id="d98d9-213">Du kan ladda upp hello filen tooa olika Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="d98d9-213">You can upload hello file tooa different Azure Storage.</span></span> <span data-ttu-id="d98d9-214">Om du gör det, skapa en länkad Azure Storage service toolink storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-214">If you do so, create an Azure Storage linked service toolink that storage account toohello data factory.</span></span> <span data-ttu-id="d98d9-215">Ange hello namnet på hello länkade tjänst som ett värde för hello **sparkJobLinkedService** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="d98d9-215">Then, specify hello name of hello linked service as a value for hello **sparkJobLinkedService** property.</span></span> <span data-ttu-id="d98d9-216">Se [Spark Aktivitetsegenskaper](#spark-activity-properties) för ytterligare information om den här egenskapen och andra egenskaper som stöds av hello Spark-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-216">See [Spark Activity properties](#spark-activity-properties) for details about this property and other properties supported by hello Spark Activity.</span></span>  
    - <span data-ttu-id="d98d9-217">Hej **entryFilePath** anges toohello **test.py**, vilket är hello python-fil.</span><span class="sxs-lookup"><span data-stu-id="d98d9-217">hello **entryFilePath** is set toohello **test.py**, which is hello python file.</span></span>
    - <span data-ttu-id="d98d9-218">Hej **getDebugInfo** egenskapen för**alltid**, vilket innebär att hello loggfiler är alltid genereras (lyckade eller misslyckade).</span><span class="sxs-lookup"><span data-stu-id="d98d9-218">hello **getDebugInfo** property is set too**Always**, which means hello log files are always generated (success or failure).</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="d98d9-219">Vi rekommenderar att du inte anger den här egenskapen för`Always` i en produktionsmiljö om du felsöker ett problem.</span><span class="sxs-lookup"><span data-stu-id="d98d9-219">We recommend that you do not set this property too`Always` in a production environment unless you are troubleshooting an issue.</span></span>
    - <span data-ttu-id="d98d9-220">Hej **matar ut** avsnittet innehåller en datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-220">hello **outputs** section has one output dataset.</span></span> <span data-ttu-id="d98d9-221">Du måste ange en datamängd för utdata, även om hello spark-program inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="d98d9-221">You must specify an output dataset even if hello spark program does not produce any output.</span></span> <span data-ttu-id="d98d9-222">hello utdata dataset enheter hello schema för hello pipelinen (varje timme, varje dag, osv.).</span><span class="sxs-lookup"><span data-stu-id="d98d9-222">hello output dataset drives hello schedule for hello pipeline (hourly, daily, etc.).</span></span>  

        <span data-ttu-id="d98d9-223">Mer information om hello egenskaper som stöds av Spark-aktiviteten finns [Väck Aktivitetsegenskaper](#spark-activity-properties) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d98d9-223">For details about hello properties supported by Spark activity, see [Spark activity properties](#spark-activity-properties) section.</span></span>
3. <span data-ttu-id="d98d9-224">toodeploy hello pipeline, klickar du på **distribuera** i hello kommandofält.</span><span class="sxs-lookup"><span data-stu-id="d98d9-224">toodeploy hello pipeline, click **Deploy** on hello command bar.</span></span>

### <a name="monitor-pipeline"></a><span data-ttu-id="d98d9-225">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="d98d9-225">Monitor pipeline</span></span>
1. <span data-ttu-id="d98d9-226">Klicka på **X** tooclose Data Factory-redigeraren blad och toonavigate tillbaka toohello Data Factory-startsidan.</span><span class="sxs-lookup"><span data-stu-id="d98d9-226">Click **X** tooclose Data Factory Editor blades and toonavigate back toohello Data Factory home page.</span></span> <span data-ttu-id="d98d9-227">Klicka på **övervaka och hantera** toolaunch hello övervakning av program på en annan flik.</span><span class="sxs-lookup"><span data-stu-id="d98d9-227">Click **Monitor and Manage** toolaunch hello monitoring application in another tab.</span></span>

    ![Övervaka och hantera sida vid sida](media/data-factory-spark/monitor-and-manage-tile.png)
2. <span data-ttu-id="d98d9-229">Ändra hello **starttid** filtrera hello överst för**2/1/2017**, och klicka på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-229">Change hello **Start time** filter at hello top too**2/1/2017**, and click **Apply**.</span></span>
3. <span data-ttu-id="d98d9-230">Du bör se endast en aktivitetsfönstret eftersom det finns endast en dag mellan hello start (2017-02-01)- och sluttider (2017-02-02) för hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="d98d9-230">You should see only one activity window as there is only one day between hello start (2017-02-01) and end times (2017-02-02) of hello pipeline.</span></span> <span data-ttu-id="d98d9-231">Kontrollera att hello datasektorn i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d98d9-231">Confirm that hello data slice is in **ready** state.</span></span>

    ![Övervakaren hello pipeline](media/data-factory-spark/monitor-and-manage-app.png)    
4. <span data-ttu-id="d98d9-233">Välj hello **aktivitetsfönstret** toosee information om hello aktiviteter körs.</span><span class="sxs-lookup"><span data-stu-id="d98d9-233">Select hello **activity window** toosee details about hello activity run.</span></span> <span data-ttu-id="d98d9-234">Om det finns ett fel, se information om det i hello till höger.</span><span class="sxs-lookup"><span data-stu-id="d98d9-234">If there is an error, you see details about it in hello right pane.</span></span>

### <a name="verify-hello-results"></a><span data-ttu-id="d98d9-235">Verifiera hello resultaten</span><span class="sxs-lookup"><span data-stu-id="d98d9-235">Verify hello results</span></span>

1. <span data-ttu-id="d98d9-236">Starta **Jupyter-anteckningsbok** för HDInsight Spark-kluster genom att gå till: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span><span class="sxs-lookup"><span data-stu-id="d98d9-236">Launch **Jupyter notebook** for your HDInsight Spark cluster by navigating to: https://CLUSTERNAME.azurehdinsight.net/jupyter.</span></span> <span data-ttu-id="d98d9-237">Du kan också starta instrumentpanelen klustret för HDInsight Spark-klustret och sedan starta **Jupyter-anteckningsbok**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-237">You can also launch cluster dashboard for your HDInsight Spark cluster, and then launch **Jupyter Notebook**.</span></span>
2. <span data-ttu-id="d98d9-238">Klicka på **ny** -> **PySpark** toostart en ny anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="d98d9-238">Click **New** -> **PySpark** toostart a new notebook.</span></span>

    ![Ny Jupyter-anteckningsbok](media/data-factory-spark/jupyter-new-book.png)
3. <span data-ttu-id="d98d9-240">Kör hello följande kommando genom att kopiera/klistra in hello text och trycka på **SKIFT + RETUR** hello slutet av andra hello-satsen.</span><span class="sxs-lookup"><span data-stu-id="d98d9-240">Run hello following command by copy/pasting hello text and pressing **SHIFT + ENTER** at hello end of hello second statement.</span></span>  

    ```sql
    %%sql

    SELECT buildingID, (targettemp - actualtemp) AS temp_diff, date FROM hvac WHERE date = \"6/1/13\"
    ```
4. <span data-ttu-id="d98d9-241">Bekräfta att du ser hello data från hello hvac tabell:</span><span class="sxs-lookup"><span data-stu-id="d98d9-241">Confirm that you see hello data from hello hvac table:</span></span>  

    ![Jupyter frågeresultat](media/data-factory-spark/jupyter-notebook-results.png)

<span data-ttu-id="d98d9-243">Se [köra Spark SQL-fråga](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) avsnittet detaljerade anvisningar.</span><span class="sxs-lookup"><span data-stu-id="d98d9-243">See [Run a Spark SQL query](../hdinsight/hdinsight-apache-spark-jupyter-spark-sql.md#run-a-hive-query-using-spark-sql) section for detailed instructions.</span></span> 

### <a name="troubleshooting"></a><span data-ttu-id="d98d9-244">Felsökning</span><span class="sxs-lookup"><span data-stu-id="d98d9-244">Troubleshooting</span></span>
<span data-ttu-id="d98d9-245">Eftersom du ställer in **getDebugInfo** för**alltid**, visas en **loggen** undermapp i hello **pyFiles** mapp i Azure Blob-behållare.</span><span class="sxs-lookup"><span data-stu-id="d98d9-245">Since you set **getDebugInfo** too**Always**, you see a **log** subfolder in hello **pyFiles** folder in your Azure Blob container.</span></span> <span data-ttu-id="d98d9-246">hello-loggfilen i hello loggmappen innehåller ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="d98d9-246">hello log file in hello log folder provides additional details.</span></span> <span data-ttu-id="d98d9-247">Den här loggfilen är användbart när det uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="d98d9-247">This log file is especially useful when there is an error.</span></span> <span data-ttu-id="d98d9-248">I en produktionsmiljö kan du tooset den för**fel**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-248">In a production environment, you may want tooset it too**Failure**.</span></span>

<span data-ttu-id="d98d9-249">Ytterligare felsökningsinformation, hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="d98d9-249">For further troubleshooting, do hello following steps:</span></span>


1. <span data-ttu-id="d98d9-250">Navigera för`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span><span class="sxs-lookup"><span data-stu-id="d98d9-250">Navigate too`https://<CLUSTERNAME>.azurehdinsight.net/yarnui/hn/cluster`.</span></span>

    ![YARN-Användargränssnittet program](media/data-factory-spark/yarnui-application.png)  
2. <span data-ttu-id="d98d9-252">Klicka på **loggar** för en hello kör försök.</span><span class="sxs-lookup"><span data-stu-id="d98d9-252">Click **Logs** for one of hello run attempts.</span></span>

    ![Sidan program](media/data-factory-spark/yarn-applications.png)
3. <span data-ttu-id="d98d9-254">Du bör se ytterligare information om fel hello loggen på sidan.</span><span class="sxs-lookup"><span data-stu-id="d98d9-254">You should see additional error information in hello log page.</span></span>

    ![Logga fel](media/data-factory-spark/yarnui-application-error.png)

<span data-ttu-id="d98d9-256">hello följande avsnitt innehåller information om Data Factory entiteter toouse Apache Spark-kluster och Spark aktivitet i din data factory.</span><span class="sxs-lookup"><span data-stu-id="d98d9-256">hello following sections provide information about Data Factory entities toouse Apache Spark cluster and Spark Activity in your data factory.</span></span>

## <a name="spark-activity-properties"></a><span data-ttu-id="d98d9-257">Spark Aktivitetsegenskaper</span><span class="sxs-lookup"><span data-stu-id="d98d9-257">Spark activity properties</span></span>
<span data-ttu-id="d98d9-258">Här är hello exempel JSON-definitionen för en pipeline med Spark aktivitet:</span><span class="sxs-lookup"><span data-stu-id="d98d9-258">Here is hello sample JSON definition of a pipeline with Spark Activity:</span></span>    

```json
{
    "name": "SparkPipeline",
    "properties": {
        "activities": [
            {
                "type": "HDInsightSpark",
                "typeProperties": {
                    "rootPath": "adfspark\\pyFiles",
                    "entryFilePath": "test.py",
                    "arguments": [ "arg1", "arg2" ],
                    "sparkConfig": {
                        "spark.python.worker.memory": "512m"
                    }
                    "getDebugInfo": "Always"
                },
                "outputs": [
                    {
                        "name": "OutputDataset"
                    }
                ],
                "name": "MySparkActivity",
                "description": "This activity invokes hello Spark program",
                "linkedServiceName": "HDInsightLinkedService"
            }
        ],
        "start": "2017-02-01T00:00:00Z",
        "end": "2017-02-02T00:00:00Z"
    }
}
```

<span data-ttu-id="d98d9-259">hello beskrivs följande tabell hello JSON egenskaper som används i hello JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="d98d9-259">hello following table describes hello JSON properties used in hello JSON definition:</span></span>

| <span data-ttu-id="d98d9-260">Egenskap</span><span class="sxs-lookup"><span data-stu-id="d98d9-260">Property</span></span> | <span data-ttu-id="d98d9-261">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d98d9-261">Description</span></span> | <span data-ttu-id="d98d9-262">Krävs</span><span class="sxs-lookup"><span data-stu-id="d98d9-262">Required</span></span> |
| -------- | ----------- | -------- |
| <span data-ttu-id="d98d9-263">namn</span><span class="sxs-lookup"><span data-stu-id="d98d9-263">name</span></span> | <span data-ttu-id="d98d9-264">Namnet på hello aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="d98d9-264">Name of hello activity in hello pipeline.</span></span> | <span data-ttu-id="d98d9-265">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-265">Yes</span></span> |
| <span data-ttu-id="d98d9-266">description</span><span class="sxs-lookup"><span data-stu-id="d98d9-266">description</span></span> | <span data-ttu-id="d98d9-267">Text som beskriver vilka hello-aktiviteten har.</span><span class="sxs-lookup"><span data-stu-id="d98d9-267">Text describing what hello activity does.</span></span> | <span data-ttu-id="d98d9-268">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-268">No</span></span> |
| <span data-ttu-id="d98d9-269">typ</span><span class="sxs-lookup"><span data-stu-id="d98d9-269">type</span></span> | <span data-ttu-id="d98d9-270">Den här egenskapen måste anges tooHDInsightSpark.</span><span class="sxs-lookup"><span data-stu-id="d98d9-270">This property must be set tooHDInsightSpark.</span></span> | <span data-ttu-id="d98d9-271">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-271">Yes</span></span> |
| <span data-ttu-id="d98d9-272">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="d98d9-272">linkedServiceName</span></span> | <span data-ttu-id="d98d9-273">Namnet på hello HDInsight länkade tjänsten på vilka hello Spark programmet körs.</span><span class="sxs-lookup"><span data-stu-id="d98d9-273">Name of hello HDInsight linked service on which hello Spark program runs.</span></span> | <span data-ttu-id="d98d9-274">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-274">Yes</span></span> |
| <span data-ttu-id="d98d9-275">rootPath</span><span class="sxs-lookup"><span data-stu-id="d98d9-275">rootPath</span></span> | <span data-ttu-id="d98d9-276">hello Azure Blob-behållaren och mappen som innehåller hello Spark-fil.</span><span class="sxs-lookup"><span data-stu-id="d98d9-276">hello Azure Blob container and folder that contains hello Spark file.</span></span> <span data-ttu-id="d98d9-277">hello-filnamnet är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="d98d9-277">hello file name is case-sensitive.</span></span> | <span data-ttu-id="d98d9-278">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-278">Yes</span></span> |
| <span data-ttu-id="d98d9-279">entryFilePath</span><span class="sxs-lookup"><span data-stu-id="d98d9-279">entryFilePath</span></span> | <span data-ttu-id="d98d9-280">Relativ sökväg toohello rotmapp hello Spark kodpaketet.</span><span class="sxs-lookup"><span data-stu-id="d98d9-280">Relative path toohello root folder of hello Spark code/package.</span></span> | <span data-ttu-id="d98d9-281">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-281">Yes</span></span> |
| <span data-ttu-id="d98d9-282">Klassnamn</span><span class="sxs-lookup"><span data-stu-id="d98d9-282">className</span></span> | <span data-ttu-id="d98d9-283">Programmets Java/Spark huvudsakliga klass</span><span class="sxs-lookup"><span data-stu-id="d98d9-283">Application's Java/Spark main class</span></span> | <span data-ttu-id="d98d9-284">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-284">No</span></span> |
| <span data-ttu-id="d98d9-285">Argument</span><span class="sxs-lookup"><span data-stu-id="d98d9-285">arguments</span></span> | <span data-ttu-id="d98d9-286">En lista med kommandoradsargument toohello Spark-program.</span><span class="sxs-lookup"><span data-stu-id="d98d9-286">A list of command-line arguments toohello Spark program.</span></span> | <span data-ttu-id="d98d9-287">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-287">No</span></span> |
| <span data-ttu-id="d98d9-288">proxyUser</span><span class="sxs-lookup"><span data-stu-id="d98d9-288">proxyUser</span></span> | <span data-ttu-id="d98d9-289">hello konto tooimpersonate tooexecute hello Spark användarprogram</span><span class="sxs-lookup"><span data-stu-id="d98d9-289">hello user account tooimpersonate tooexecute hello Spark program</span></span> | <span data-ttu-id="d98d9-290">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-290">No</span></span> |
| <span data-ttu-id="d98d9-291">sparkConfig</span><span class="sxs-lookup"><span data-stu-id="d98d9-291">sparkConfig</span></span> | <span data-ttu-id="d98d9-292">Ange värden för egenskaper för Spark-konfiguration som beskrivs i avsnittet hello: [Spark-konfiguration – programegenskaper](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span><span class="sxs-lookup"><span data-stu-id="d98d9-292">Specify values for Spark configuration properties listed in hello topic: [Spark Configuration - Application properties](https://spark.apache.org/docs/latest/configuration.html#available-properties).</span></span> | <span data-ttu-id="d98d9-293">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-293">No</span></span> |
| <span data-ttu-id="d98d9-294">getDebugInfo</span><span class="sxs-lookup"><span data-stu-id="d98d9-294">getDebugInfo</span></span> | <span data-ttu-id="d98d9-295">Anger när hello Spark loggfilerna kopierade toohello Azure storage som används av HDInsight-kluster (eller) anges av sparkJobLinkedService.</span><span class="sxs-lookup"><span data-stu-id="d98d9-295">Specifies when hello Spark log files are copied toohello Azure storage used by HDInsight cluster (or) specified by sparkJobLinkedService.</span></span> <span data-ttu-id="d98d9-296">Tillåtna värden: None, alltid eller fel.</span><span class="sxs-lookup"><span data-stu-id="d98d9-296">Allowed values: None, Always, or Failure.</span></span> <span data-ttu-id="d98d9-297">Standardvärde: Ingen.</span><span class="sxs-lookup"><span data-stu-id="d98d9-297">Default value: None.</span></span> | <span data-ttu-id="d98d9-298">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-298">No</span></span> |
| <span data-ttu-id="d98d9-299">sparkJobLinkedService</span><span class="sxs-lookup"><span data-stu-id="d98d9-299">sparkJobLinkedService</span></span> | <span data-ttu-id="d98d9-300">hello länkad Azure Storage-tjänst som äger hello Spark fil, beroenden och loggar.</span><span class="sxs-lookup"><span data-stu-id="d98d9-300">hello Azure Storage linked service that holds hello Spark job file, dependencies, and logs.</span></span>  <span data-ttu-id="d98d9-301">Om du inte anger ett värde för den här egenskapen används hello lagring som är associerade med HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="d98d9-301">If you do not specify a value for this property, hello storage associated with HDInsight cluster is used.</span></span> | <span data-ttu-id="d98d9-302">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-302">No</span></span> |

## <a name="folder-structure"></a><span data-ttu-id="d98d9-303">Mappstruktur</span><span class="sxs-lookup"><span data-stu-id="d98d9-303">Folder structure</span></span>
<span data-ttu-id="d98d9-304">hello Spark aktiviteten stöder inte en infogad skriptet som Pig och gör Hive-aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="d98d9-304">hello Spark activity does not support an in-line script as Pig and Hive activities do.</span></span> <span data-ttu-id="d98d9-305">Spark jobb är också mer utökningsbar än Pig/Hive-jobb.</span><span class="sxs-lookup"><span data-stu-id="d98d9-305">Spark jobs are also more extensible than Pig/Hive jobs.</span></span> <span data-ttu-id="d98d9-306">För Spark jobb, du kan ange flera beroenden som jar-paket (placeras i hello java KLASSÖKVÄGEN), python-filer (placerad hello PYTHONPATH) och andra filer.</span><span class="sxs-lookup"><span data-stu-id="d98d9-306">For Spark jobs, you can provide multiple dependencies such as jar packages (placed in hello java CLASSPATH), python files (placed on hello PYTHONPATH), and any other files.</span></span>

<span data-ttu-id="d98d9-307">Skapa hello följande mappstrukturen i hello Azure Blob storage som refereras av hello länkad HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d98d9-307">Create hello following folder structure in hello Azure Blob storage referenced by hello HDInsight linked service.</span></span> <span data-ttu-id="d98d9-308">Därefter kan du överföra beroende filer toohello lämpliga undermappar i hello rotmapp som representeras av **entryFilePath**.</span><span class="sxs-lookup"><span data-stu-id="d98d9-308">Then, upload dependent files toohello appropriate sub folders in hello root folder represented by **entryFilePath**.</span></span> <span data-ttu-id="d98d9-309">Till exempel överföra python filer toohello pyFiles undermapp och jar-filer toohello burkar undermapp hello rotmappen.</span><span class="sxs-lookup"><span data-stu-id="d98d9-309">For example, upload python files toohello pyFiles subfolder and jar files toohello jars subfolder of hello root folder.</span></span> <span data-ttu-id="d98d9-310">Vid körning förväntar Data Factory-tjänsten hello följande mappstrukturen i hello Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="d98d9-310">At runtime, Data Factory service expects hello following folder structure in hello Azure Blob storage:</span></span>     

| <span data-ttu-id="d98d9-311">Sökväg</span><span class="sxs-lookup"><span data-stu-id="d98d9-311">Path</span></span> | <span data-ttu-id="d98d9-312">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d98d9-312">Description</span></span> | <span data-ttu-id="d98d9-313">Krävs</span><span class="sxs-lookup"><span data-stu-id="d98d9-313">Required</span></span> | <span data-ttu-id="d98d9-314">Typ</span><span class="sxs-lookup"><span data-stu-id="d98d9-314">Type</span></span> |
| ---- | ----------- | -------- | ---- |
| <span data-ttu-id="d98d9-315">.</span><span class="sxs-lookup"><span data-stu-id="d98d9-315">.</span></span> | <span data-ttu-id="d98d9-316">hello rotsökvägen för hello Spark jobb i hello länkade lagringstjänsten</span><span class="sxs-lookup"><span data-stu-id="d98d9-316">hello root path of hello Spark job in hello storage linked service</span></span>    | <span data-ttu-id="d98d9-317">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-317">Yes</span></span> | <span data-ttu-id="d98d9-318">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-318">Folder</span></span> |
| <span data-ttu-id="d98d9-319">&lt;användardefinierade&gt;</span><span class="sxs-lookup"><span data-stu-id="d98d9-319">&lt;user defined &gt;</span></span> | <span data-ttu-id="d98d9-320">Hej sökväg som pekar toohello post-filen för hello Spark jobb</span><span class="sxs-lookup"><span data-stu-id="d98d9-320">hello path pointing toohello entry file of hello Spark job</span></span> | <span data-ttu-id="d98d9-321">Ja</span><span class="sxs-lookup"><span data-stu-id="d98d9-321">Yes</span></span> | <span data-ttu-id="d98d9-322">Fil</span><span class="sxs-lookup"><span data-stu-id="d98d9-322">File</span></span> |
| <span data-ttu-id="d98d9-323">. / JAR: er</span><span class="sxs-lookup"><span data-stu-id="d98d9-323">./jars</span></span> | <span data-ttu-id="d98d9-324">Alla filer under den här mappen överförs och placeras på hello java klassökväg hello kluster</span><span class="sxs-lookup"><span data-stu-id="d98d9-324">All files under this folder are uploaded and placed on hello java classpath of hello cluster</span></span> | <span data-ttu-id="d98d9-325">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-325">No</span></span> | <span data-ttu-id="d98d9-326">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-326">Folder</span></span> |
| <span data-ttu-id="d98d9-327">. / pyFiles</span><span class="sxs-lookup"><span data-stu-id="d98d9-327">./pyFiles</span></span> | <span data-ttu-id="d98d9-328">Alla filer under den här mappen överförs och placeras på hello PYTHONPATH hello-klustret</span><span class="sxs-lookup"><span data-stu-id="d98d9-328">All files under this folder are uploaded and placed on hello PYTHONPATH of hello cluster</span></span> | <span data-ttu-id="d98d9-329">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-329">No</span></span> | <span data-ttu-id="d98d9-330">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-330">Folder</span></span> |
| <span data-ttu-id="d98d9-331">. / filer</span><span class="sxs-lookup"><span data-stu-id="d98d9-331">./files</span></span> | <span data-ttu-id="d98d9-332">Alla filer under den här mappen överförs och placeras på utföraren arbetskatalogen</span><span class="sxs-lookup"><span data-stu-id="d98d9-332">All files under this folder are uploaded and placed on executor working directory</span></span> | <span data-ttu-id="d98d9-333">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-333">No</span></span> | <span data-ttu-id="d98d9-334">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-334">Folder</span></span> |
| <span data-ttu-id="d98d9-335">. / arkiveras</span><span class="sxs-lookup"><span data-stu-id="d98d9-335">./archives</span></span> | <span data-ttu-id="d98d9-336">Alla filer under den här mappen dekomprimeras</span><span class="sxs-lookup"><span data-stu-id="d98d9-336">All files under this folder are uncompressed</span></span> | <span data-ttu-id="d98d9-337">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-337">No</span></span> | <span data-ttu-id="d98d9-338">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-338">Folder</span></span> |
| <span data-ttu-id="d98d9-339">. / loggar</span><span class="sxs-lookup"><span data-stu-id="d98d9-339">./logs</span></span> | <span data-ttu-id="d98d9-340">hello mappen där loggar från hello Spark-kluster lagras.</span><span class="sxs-lookup"><span data-stu-id="d98d9-340">hello folder where logs from hello Spark cluster are stored.</span></span>| <span data-ttu-id="d98d9-341">Nej</span><span class="sxs-lookup"><span data-stu-id="d98d9-341">No</span></span> | <span data-ttu-id="d98d9-342">Mapp</span><span class="sxs-lookup"><span data-stu-id="d98d9-342">Folder</span></span> |

<span data-ttu-id="d98d9-343">Här är ett exempel på en lagringsplats som innehåller två Spark jobbfiler i hello Azure Blob Storage som refereras av hello länkad HDInsight-tjänst.</span><span class="sxs-lookup"><span data-stu-id="d98d9-343">Here is an example for a storage containing two Spark job files in hello Azure Blob Storage referenced by hello HDInsight linked service.</span></span>

```
SparkJob1
    main.jar
    files
        input1.txt
        input2.txt
    jars
        package1.jar
        package2.jar
    logs

SparkJob2
    main.py
    pyFiles
        scrip1.py
        script2.py
    logs
```
