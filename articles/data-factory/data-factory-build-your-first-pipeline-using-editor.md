---
title: "Skapa din första datafabrik (Azure Portal) | Microsoft Docs"
description: "I den här självstudien skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av Data Factory-redigeraren i Azure Portal."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: d5b14e9e-e358-45be-943c-5297435d402d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 9c958aecb841fa02349c6b9e5e1984f6ba4fb611
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-portal"></a><span data-ttu-id="6efa4-103">Självstudier: Skapa din första Azure-datafabrik med Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6efa4-103">Tutorial: Build your first Azure data factory using Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6efa4-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="6efa4-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="6efa4-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6efa4-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="6efa4-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6efa4-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="6efa4-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6efa4-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="6efa4-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6efa4-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="6efa4-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="6efa4-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)


<span data-ttu-id="6efa4-110">I den här artikeln får du lära dig hur du använder [Azure Portal](https://portal.azure.com/) till att skapa din första Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6efa4-110">In this article, you learn how to use [Azure portal](https://portal.azure.com/) to create your first Azure data factory.</span></span> <span data-ttu-id="6efa4-111">Om du vill gå igenom självstudien med andra verktyg/SDK:er kan du välja något av alternativen i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6efa4-111">To do the tutorial using other tools/SDKs, select one of the options from the drop-down list.</span></span> 

<span data-ttu-id="6efa4-112">Pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive-aktivitet**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="6efa4-113">Aktiviteten kör ett Hive-skript i ett Azure HDInsight-kluster som omvandlar indata för till utdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="6efa4-114">Denna pipeline är schemalagd att köras en gång i månaden mellan angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="6efa4-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="6efa4-115">Datapipelinen i den här självstudien transformerar indata för att generera utdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-115">The data pipeline in this tutorial transforms input data to produce output data.</span></span> <span data-ttu-id="6efa4-116">En självstudiekurs om hur du kopierar data med Azure Data Factory finns i [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) (Självstudie: Kopiera data från Blob Storage till SQL Database).</span><span class="sxs-lookup"><span data-stu-id="6efa4-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="6efa4-117">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-117">A pipeline can have more than one activity.</span></span> <span data-ttu-id="6efa4-118">Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="6efa4-118">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="6efa4-119">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="6efa4-119">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6efa4-120">Krav</span><span class="sxs-lookup"><span data-stu-id="6efa4-120">Prerequisites</span></span>
1. <span data-ttu-id="6efa4-121">Läs igenom artikeln [Självstudier – översikt](data-factory-build-your-first-pipeline.md) och slutför de **nödvändiga** stegen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-121">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span>
2. <span data-ttu-id="6efa4-122">Den här artikeln ger inte någon konceptuell översikt över Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6efa4-122">This article does not provide a conceptual overview of the Azure Data Factory service.</span></span> <span data-ttu-id="6efa4-123">Vi rekommenderar att du läser igenom den detaljerade översikt över tjänsten som finns i [Introduktion till Azure Data Factory](data-factory-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6efa4-123">We recommend that you go through [Introduction to Azure Data Factory](data-factory-introduction.md) article for a detailed overview of the service.</span></span>  

## <a name="create-data-factory"></a><span data-ttu-id="6efa4-124">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="6efa4-124">Create data factory</span></span>
<span data-ttu-id="6efa4-125">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="6efa4-125">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="6efa4-126">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6efa4-126">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="6efa4-127">Det kan exempelvis vara en kopieringsaktivitet som kopierar data från en källa till ett måldataarkiv och en HDInsight Hive-aktivitet som kör Hive-skript för att transformera indata till produktutdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-127">For example, a Copy Activity to copy data from a source to a destination data store and a HDInsight Hive activity to run a Hive script to transform input data to product output data.</span></span> <span data-ttu-id="6efa4-128">Låt oss börja med att skapa datafabriken i det här steget.</span><span class="sxs-lookup"><span data-stu-id="6efa4-128">Let's start with creating the data factory in this step.</span></span>

1. <span data-ttu-id="6efa4-129">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="6efa4-129">Log in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="6efa4-130">Klicka på **NY** på den vänstra menyn, klicka på **Data + Analys**, och klicka på **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-130">Click **NEW** on the left menu, click **Data + Analytics**, and click **Data Factory**.</span></span>

   ![Bladet Skapa](./media/data-factory-build-your-first-pipeline-using-editor/create-blade.png)
3. <span data-ttu-id="6efa4-132">På bladet **Ny datafabrik** anger du **GetStartedDF** som namn.</span><span class="sxs-lookup"><span data-stu-id="6efa4-132">In the **New data factory** blade, enter **GetStartedDF** for the Name.</span></span>

   ![Bladet Ny datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/new-data-factory-blade.png)

   > [!IMPORTANT]
   > <span data-ttu-id="6efa4-134">Namnet på Azure Data Factory måste vara **globalt unikt**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-134">The name of the Azure data factory must be **globally unique**.</span></span> <span data-ttu-id="6efa4-135">Om du tar emot felet **Datafabriksnamnet "GetStartedDF" är inte tillgängligt**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-135">If you receive the error: **Data factory name “GetStartedDF” is not available**.</span></span> <span data-ttu-id="6efa4-136">Ändra datafabrikens namn (till exempelvs dittnamnGetStartedDF) och försök skapa igen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-136">Change the name of the data factory (for example, yournameGetStartedDF) and try creating again.</span></span> <span data-ttu-id="6efa4-137">Se artikeln [Data Factory – namnregler](data-factory-naming-rules.md) för namnregler för Data Factory-artefakter.</span><span class="sxs-lookup"><span data-stu-id="6efa4-137">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
   >
   > <span data-ttu-id="6efa4-138">Namnet på datafabriken kan komma att registreras som ett **DNS**-namn i framtiden och blir då synligt offentligt.</span><span class="sxs-lookup"><span data-stu-id="6efa4-138">The name of the data factory may be registered as a **DNS** name in the future and hence become publically visible.</span></span>
   >
   >
4. <span data-ttu-id="6efa4-139">Välj den **Azure-prenumeration** där du vill att datafabriken ska skapas.</span><span class="sxs-lookup"><span data-stu-id="6efa4-139">Select the **Azure subscription** where you want the data factory to be created.</span></span>
5. <span data-ttu-id="6efa4-140">Välj befintlig **resursgrupp** eller skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6efa4-140">Select existing **resource group** or create a resource group.</span></span> <span data-ttu-id="6efa4-141">Skapa en resursgrupp för självstudien med namnet: **ADFGetStartedRG**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-141">For the tutorial, create a resource group named: **ADFGetStartedRG**.</span></span>
6. <span data-ttu-id="6efa4-142">Välj **plats** för datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-142">Select the **location** for the data factory.</span></span> <span data-ttu-id="6efa4-143">Endast regioner som stöds av tjänsten Data Factory visas i listrutan.</span><span class="sxs-lookup"><span data-stu-id="6efa4-143">Only regions supported by the Data Factory service are shown in the drop-down list.</span></span>
7. <span data-ttu-id="6efa4-144">Välj **fäst till instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-144">Select **Pin to dashboard**.</span></span> 
8. <span data-ttu-id="6efa4-145">Klicka på **Skapa** på bladet **Ny datafabrik**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-145">Click **Create** on the **New data factory** blade.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="6efa4-146">Om du vill skapa Data Factory-instanser måste du vara medlem i [Data Factory-deltagarrollen](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) på gruppnivå/resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="6efa4-146">To create Data Factory instances, you must be a member of the [Data Factory Contributor](../active-directory/role-based-access-built-in-roles.md#data-factory-contributor) role at the subscription/resource group level.</span></span>
   >
   >
7. <span data-ttu-id="6efa4-147">På instrumentpanelen visas följande panel med statusen: Distribuerar datafabrik.</span><span class="sxs-lookup"><span data-stu-id="6efa4-147">On the dashboard, you see the following tile with status: Deploying data factory.</span></span>    

   ![Skapar datafabrikstatus](./media/data-factory-build-your-first-pipeline-using-editor/creating-data-factory-image.png)
8. <span data-ttu-id="6efa4-149">Grattis!</span><span class="sxs-lookup"><span data-stu-id="6efa4-149">Congratulations!</span></span> <span data-ttu-id="6efa4-150">Du har skapat din första datafabrik.</span><span class="sxs-lookup"><span data-stu-id="6efa4-150">You have successfully created your first data factory.</span></span> <span data-ttu-id="6efa4-151">När datafabriken har skapats visas datafabrikssidan med innehållet i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-151">After the data factory has been created successfully, you see the data factory page, which shows you the contents of the data factory.</span></span>     

    ![Bladet Datafabrik](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-blade.png)

<span data-ttu-id="6efa4-153">Du måste först skapa några Data Factory-entiteter innan du skapar en pipeline i datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-153">Before creating a pipeline in the data factory, you need to create a few Data Factory entities first.</span></span> <span data-ttu-id="6efa4-154">Du skapar först länkade tjänster för att länka datalager/beräkningar till ditt datalager, sedan definierar du de indata- och utdatauppsättningar som ska representera in- och utdata i länkade datalager och därefter skapar du pipelinen med en aktivitet som använder dessa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="6efa4-154">You first create linked services to link data stores/computes to your data store, define input and output datasets to represent input/output data in linked data stores, and then create the pipeline with an activity that uses these datasets.</span></span>

## <a name="create-linked-services"></a><span data-ttu-id="6efa4-155">Skapa länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="6efa4-155">Create linked services</span></span>
<span data-ttu-id="6efa4-156">I det här steget länkar du ditt Azure Storage-konto och ett Azure HDInsight-kluster på begäran till din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="6efa4-156">In this step, you link your Azure Storage account and an on-demand Azure HDInsight cluster to your data factory.</span></span> <span data-ttu-id="6efa4-157">In- och utdata för pipelinen i det här exemplet lagras i Azure Storage-kontot.</span><span class="sxs-lookup"><span data-stu-id="6efa4-157">The Azure Storage account holds the input and output data for the pipeline in this sample.</span></span> <span data-ttu-id="6efa4-158">En länkad HDInsight-tjänst används för att köra Hive-skriptet som anges i pipeline-aktiviteten i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-158">The HDInsight linked service is used to run a Hive script specified in the activity of the pipeline in this sample.</span></span> <span data-ttu-id="6efa4-159">Identifiera vilka [beräkningstjänster](data-factory-data-movement-activities.md)/[för datalager](data-factory-compute-linked-services.md) som används i scenariot och länka dessa tjänster till datafabriken genom att skapa länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="6efa4-159">Identify what [data store](data-factory-data-movement-activities.md)/[compute services](data-factory-compute-linked-services.md) are used in your scenario and link those services to the data factory by creating linked services.</span></span>  

### <a name="create-azure-storage-linked-service"></a><span data-ttu-id="6efa4-160">Skapa en länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="6efa4-160">Create Azure Storage linked service</span></span>
<span data-ttu-id="6efa4-161">I det här steget länkar du ditt Azure Storage-konto till din datafabrik.</span><span class="sxs-lookup"><span data-stu-id="6efa4-161">In this step, you link your Azure Storage account to your data factory.</span></span> <span data-ttu-id="6efa4-162">I de här självstudierna använder du samma Azure Storage-konto för att lagra indata/utdata och HQL-skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-162">In this tutorial, you use the same Azure Storage account to store input/output data and the HQL script file.</span></span>

1. <span data-ttu-id="6efa4-163">Klicka på **Författare och distribution** på bladet **DATA FACTORY** för **GetStartedDF**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-163">Click **Author and deploy** on the **DATA FACTORY** blade for **GetStartedDF**.</span></span> <span data-ttu-id="6efa4-164">Du bör se Data Factory-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="6efa4-164">You should see the Data Factory Editor.</span></span>

   ![Ikonen Författare och distribution](./media/data-factory-build-your-first-pipeline-using-editor/data-factory-author-deploy.png)
2. <span data-ttu-id="6efa4-166">Klicka på **Nytt datalager** och välj **Azure-lagring**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-166">Click **New data store** and choose **Azure storage**.</span></span>

   ![Ny datalagring - Azure Storage - meny](./media/data-factory-build-your-first-pipeline-using-editor/new-data-store-azure-storage-menu.png)
3. <span data-ttu-id="6efa4-168">Du bör se JSON-skriptet för att skapa en länkad Azure-lagringstjänst i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="6efa4-168">You should see the JSON script for creating an Azure Storage linked service in the editor.</span></span>

   ![Länkad Azure-lagringstjänst](./media/data-factory-build-your-first-pipeline-using-editor/azure-storage-linked-service.png)
4. <span data-ttu-id="6efa4-170">Ersätt **kontonamn** med namnet på ditt Azure-lagringskonto och **kontonyckel** med åtkomstnyckeln för Azure-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="6efa4-170">Replace **account name** with the name of your Azure storage account and **account key** with the access key of the Azure storage account.</span></span> <span data-ttu-id="6efa4-171">Information om hur du hämtar lagringsåtkomstnyckeln finns i avsnitten om hur du visar, kopierar och återskapar åtkomstnycklar i [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account) (Hantera ditt lagringskonto).</span><span class="sxs-lookup"><span data-stu-id="6efa4-171">To learn how to get your storage access key, see the information about how to view, copy, and regenerate storage access keys in [Manage your storage account](../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span>
5. <span data-ttu-id="6efa4-172">Klicka på **Distribuera** i kommandofältet för att distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6efa4-172">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![Knappen Distribuera](./media/data-factory-build-your-first-pipeline-using-editor/deploy-button.png)

   <span data-ttu-id="6efa4-174">När den länkade tjänsten har distribuerats, bör fönstret **Draft-1** försvinna och du ser **AzureStorageLinkedService** i trädvyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="6efa4-174">After the linked service is deployed successfully, the **Draft-1** window should disappear and you see **AzureStorageLinkedService** in the tree view on the left.</span></span>

    ![Länkad lagringstjänst i menyn](./media/data-factory-build-your-first-pipeline-using-editor/StorageLinkedServiceInTree.png)    

### <a name="create-azure-hdinsight-linked-service"></a><span data-ttu-id="6efa4-176">Skapa en Azure HDInsight-länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="6efa4-176">Create Azure HDInsight linked service</span></span>
<span data-ttu-id="6efa4-177">I det här steget ska du länka ett HDInsight-kluster på begäran till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-177">In this step, you link an on-demand HDInsight cluster to your data factory.</span></span> <span data-ttu-id="6efa4-178">HDInsight-klustret skapas automatiskt vid körning och tas bort när bearbetningen är klar. Det är inaktivt under en angiven tidsrymd.</span><span class="sxs-lookup"><span data-stu-id="6efa4-178">The HDInsight cluster is automatically created at runtime and deleted after it is done processing and idle for the specified amount of time.</span></span>

1. <span data-ttu-id="6efa4-179">I **Data Factory-redigeraren**, klicka på **... Fler**, klicka på **Ny beräkning**, och välj **På begäran HDInsight-kluster**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-179">In the **Data Factory Editor**, click **... More**, click **New compute**, and select **On-demand HDInsight cluster**.</span></span>

    ![Ny beräkning](./media/data-factory-build-your-first-pipeline-using-editor/new-compute-menu.png)
2. <span data-ttu-id="6efa4-181">Kopiera och klistra in följande kodfragment till fönstret **Draft-1**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-181">Copy and paste the following snippet to the **Draft-1** window.</span></span> <span data-ttu-id="6efa4-182">JSON-kodfragmentet beskriver de egenskaper som används för att skapa HDInsight-klustret på begäran.</span><span class="sxs-lookup"><span data-stu-id="6efa4-182">The JSON snippet describes the properties that are used to create the HDInsight cluster on-demand.</span></span>

    ```JSON
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "AzureStorageLinkedService"
            }
        }
    }
    ```

    <span data-ttu-id="6efa4-183">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="6efa4-183">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="6efa4-184">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6efa4-184">Property</span></span> | <span data-ttu-id="6efa4-185">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6efa4-185">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="6efa4-186">ClusterSize</span><span class="sxs-lookup"><span data-stu-id="6efa4-186">ClusterSize</span></span> |<span data-ttu-id="6efa4-187">Anger HDInsight-klustrets storlek.</span><span class="sxs-lookup"><span data-stu-id="6efa4-187">Specifies the size of the HDInsight cluster.</span></span> |
   | <span data-ttu-id="6efa4-188">TimeToLive</span><span class="sxs-lookup"><span data-stu-id="6efa4-188">TimeToLive</span></span> | <span data-ttu-id="6efa4-189">Anger inaktivitetstiden för HDInsight-klustret innan det tas bort.</span><span class="sxs-lookup"><span data-stu-id="6efa4-189">Specifies that the idle time for the HDInsight cluster, before it is deleted.</span></span> |
   | <span data-ttu-id="6efa4-190">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="6efa4-190">linkedServiceName</span></span> | <span data-ttu-id="6efa4-191">Anger lagringskontot som används för att spara loggarna som genereras av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6efa4-191">Specifies the storage account that is used to store the logs that are generated by HDInsight.</span></span> |

    <span data-ttu-id="6efa4-192">Observera följande punkter:</span><span class="sxs-lookup"><span data-stu-id="6efa4-192">Note the following points:</span></span>

   * <span data-ttu-id="6efa4-193">Data Factory skapar ett **Linux-baserat** HDInsight-kluster åt dig med JSON.</span><span class="sxs-lookup"><span data-stu-id="6efa4-193">The Data Factory creates a **Linux-based** HDInsight cluster for you with the JSON.</span></span> <span data-ttu-id="6efa4-194">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6efa4-194">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
   * <span data-ttu-id="6efa4-195">Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="6efa4-195">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="6efa4-196">Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6efa4-196">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
   * <span data-ttu-id="6efa4-197">HDInsight-klustret skapar en **standardbehållare** i den blobblagring som du angav i JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="6efa4-197">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="6efa4-198">HDInsight tar inte bort den här behållaren när klustret tas bort.</span><span class="sxs-lookup"><span data-stu-id="6efa4-198">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="6efa4-199">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="6efa4-199">This behavior is by design.</span></span> <span data-ttu-id="6efa4-200">Med en HDInsight-länkad tjänst på begäran skapas ett HDInsight-kluster varje gång en sektor bearbetas, såvida det inte finns ett befintligt live-kluster (**timeToLive**).</span><span class="sxs-lookup"><span data-stu-id="6efa4-200">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice is processed unless there is an existing live cluster (**timeToLive**).</span></span> <span data-ttu-id="6efa4-201">Klustret tas bort automatiskt när bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="6efa4-201">The cluster is automatically deleted when the processing is done.</span></span>

       <span data-ttu-id="6efa4-202">Allteftersom fler sektorer bearbetas kan du se mång behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6efa4-202">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="6efa4-203">Om du inte behöver dem för att felsöka jobb, kan du ta bort dem för att minska lagringskostnaderna.</span><span class="sxs-lookup"><span data-stu-id="6efa4-203">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="6efa4-204">Namnen på de här behållarna följer ett mönster: ”adf**datafabrikensnamn**-**denlänkadetjänstensnamn**-datumtidsstämpel”.</span><span class="sxs-lookup"><span data-stu-id="6efa4-204">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="6efa4-205">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) till att ta bort behållare i din Azure Blob-lagring.</span><span class="sxs-lookup"><span data-stu-id="6efa4-205">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

     <span data-ttu-id="6efa4-206">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6efa4-206">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>
3. <span data-ttu-id="6efa4-207">Klicka på **Distribuera** i kommandofältet för att distribuera den länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6efa4-207">Click **Deploy** on the command bar to deploy the linked service.</span></span>

    ![Distribuera på begäran länkad HDInsight-tjänst](./media/data-factory-build-your-first-pipeline-using-editor/ondemand-hdinsight-deploy.png)
4. <span data-ttu-id="6efa4-209">Kontrollera att du ser både **AzureStorageLinkedService** och **HDInsightOnDemandLinkedService** i trädvyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="6efa4-209">Confirm that you see both **AzureStorageLinkedService** and **HDInsightOnDemandLinkedService** in the tree view on the left.</span></span>

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-linked-services.png)

## <a name="create-datasets"></a><span data-ttu-id="6efa4-211">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="6efa4-211">Create datasets</span></span>
<span data-ttu-id="6efa4-212">I det här steget skapar du datauppsättningar som ska representera in- och utdata för Hive-bearbetning.</span><span class="sxs-lookup"><span data-stu-id="6efa4-212">In this step, you create datasets to represent the input and output data for Hive processing.</span></span> <span data-ttu-id="6efa4-213">Dessa datauppsättningar avser den **AzureStorageLinkedService** som du skapade tidigare i självstudien.</span><span class="sxs-lookup"><span data-stu-id="6efa4-213">These datasets refer to the **AzureStorageLinkedService** you have created earlier in this tutorial.</span></span> <span data-ttu-id="6efa4-214">Den länkade tjänsten pekar på ett Azure-lagringskonto och datauppsättningarna anger behållare, mapp och filnamn i det lagringsutrymme som innehåller indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-214">The linked service points to an Azure Storage account and datasets specify container, folder, file name in the storage that holds input and output data.</span></span>   

### <a name="create-input-dataset"></a><span data-ttu-id="6efa4-215">Skapa indatauppsättning</span><span class="sxs-lookup"><span data-stu-id="6efa4-215">Create input dataset</span></span>
1. <span data-ttu-id="6efa4-216">I **Data Factory-redigeraren**, klicka på **... Fler** på kommandofältet klickar du på **Ny datamängd** och välj **Azure Blob-lagring**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-216">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>

    ![Ny datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/new-data-set.png)
2. <span data-ttu-id="6efa4-218">Kopiera och klistra in följande kodfragment till fönstret Draft-1.</span><span class="sxs-lookup"><span data-stu-id="6efa4-218">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="6efa4-219">I JSON-kodfragmentet skapar du en datauppsättning med namnet **AzureBlobInput** som representerar indata för en aktivitet i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-219">In the JSON snippet, you are creating a dataset called **AzureBlobInput** that represents input data for an activity in the pipeline.</span></span> <span data-ttu-id="6efa4-220">Dessutom kan du ange att indata finns i blobbehållaren **adfgetstarted** och i mappen **inputdata**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-220">In addition, you specify that the input data is located in the blob container called **adfgetstarted** and the folder called **inputdata**.</span></span>

    ```JSON
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    <span data-ttu-id="6efa4-221">Följande tabell innehåller beskrivningar av de JSON-egenskaper som användes i kodfragmentet:</span><span class="sxs-lookup"><span data-stu-id="6efa4-221">The following table provides descriptions for the JSON properties used in the snippet:</span></span>

   | <span data-ttu-id="6efa4-222">Egenskap</span><span class="sxs-lookup"><span data-stu-id="6efa4-222">Property</span></span> | <span data-ttu-id="6efa4-223">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6efa4-223">Description</span></span> |
   |:--- |:--- |
   | <span data-ttu-id="6efa4-224">typ</span><span class="sxs-lookup"><span data-stu-id="6efa4-224">type</span></span> |<span data-ttu-id="6efa4-225">Typegenskapen har angetts till **AzureBlob** eftersom det finns data i Azure Blob-lagringen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-225">The type property is set to **AzureBlob** because data resides in an Azure blob storage.</span></span> |
   | <span data-ttu-id="6efa4-226">linkedServiceName</span><span class="sxs-lookup"><span data-stu-id="6efa4-226">linkedServiceName</span></span> |<span data-ttu-id="6efa4-227">Refererar till **AzureStorageLinkedService** som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6efa4-227">Refers to the **AzureStorageLinkedService** you created earlier.</span></span> |
   | <span data-ttu-id="6efa4-228">folderPath</span><span class="sxs-lookup"><span data-stu-id="6efa4-228">folderPath</span></span> | <span data-ttu-id="6efa4-229">Anger vilken **blobbehållare** och **mapp** som innehåller indatablobbar.</span><span class="sxs-lookup"><span data-stu-id="6efa4-229">Specifies the blob **container** and the **folder** that contains input blobs.</span></span> | 
   | <span data-ttu-id="6efa4-230">fileName</span><span class="sxs-lookup"><span data-stu-id="6efa4-230">fileName</span></span> |<span data-ttu-id="6efa4-231">Den här egenskapen är valfri.</span><span class="sxs-lookup"><span data-stu-id="6efa4-231">This property is optional.</span></span> <span data-ttu-id="6efa4-232">Om du tar bort egenskapen kommer alla filer från folderPath hämtas.</span><span class="sxs-lookup"><span data-stu-id="6efa4-232">If you omit this property, all the files from the folderPath are picked.</span></span> <span data-ttu-id="6efa4-233">I det här fallet bearbetas bara **input.log**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-233">In this tutorial, only the **input.log** is processed.</span></span> |
   | <span data-ttu-id="6efa4-234">typ</span><span class="sxs-lookup"><span data-stu-id="6efa4-234">type</span></span> |<span data-ttu-id="6efa4-235">Loggfilerna är i textformat, så vi använder **TextFormat**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-235">The log files are in text format, so we use **TextFormat**.</span></span> |
   | <span data-ttu-id="6efa4-236">columnDelimiter</span><span class="sxs-lookup"><span data-stu-id="6efa4-236">columnDelimiter</span></span> |<span data-ttu-id="6efa4-237">kolumner i loggfilerna avgränsas med **kommatecken (`,`)**</span><span class="sxs-lookup"><span data-stu-id="6efa4-237">columns in the log files are delimited by **comma character (`,`)**</span></span> |
   | <span data-ttu-id="6efa4-238">frekvens/intervall</span><span class="sxs-lookup"><span data-stu-id="6efa4-238">frequency/interval</span></span> |<span data-ttu-id="6efa4-239">frekvensen är **månad** och intervallet är **1**, vilket innebär att indatasektorerna är tillgängliga en gång i månaden.</span><span class="sxs-lookup"><span data-stu-id="6efa4-239">frequency set to **Month** and interval is **1**, which means that the input slices are available monthly.</span></span> |
   | <span data-ttu-id="6efa4-240">extern</span><span class="sxs-lookup"><span data-stu-id="6efa4-240">external</span></span> | <span data-ttu-id="6efa4-241">Den här egenskapen anges som **true** om indatan inte skapades av denna pipeline.</span><span class="sxs-lookup"><span data-stu-id="6efa4-241">This property is set to **true** if the input data is not generated by this pipeline.</span></span> <span data-ttu-id="6efa4-242">I den här självstudiekursen genereras inte input.log-filen av denna pipeline, så vi ställa in egenskapen på true.</span><span class="sxs-lookup"><span data-stu-id="6efa4-242">In this tutorial, the input.log file is not generated by this pipeline, so we set the property to true.</span></span> |

    <span data-ttu-id="6efa4-243">Mer information om de här JSON-egenskaperna finns i artikeln [Azure Blob-anslutningsapp](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="6efa4-243">For more information about these JSON properties, see [Azure Blob connector article](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
3. <span data-ttu-id="6efa4-244">Klicka på **Distribuera** i kommandofältet för att distribuera den nyskapade datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-244">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span> <span data-ttu-id="6efa4-245">Du bör se datauppsättningen i trädvyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="6efa4-245">You should see the dataset in the tree view on the left.</span></span>

### <a name="create-output-dataset"></a><span data-ttu-id="6efa4-246">Skapa datauppsättning för utdata</span><span class="sxs-lookup"><span data-stu-id="6efa4-246">Create output dataset</span></span>
<span data-ttu-id="6efa4-247">Nu skapar du den utdatauppsättning som representerar de utdata som lagras i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6efa4-247">Now, you create the output dataset to represent the output data stored in the Azure Blob storage.</span></span>

1. <span data-ttu-id="6efa4-248">I **Data Factory-redigeraren**, klicka på **... Fler** på kommandofältet klickar du på **Ny datamängd** och välj **Azure Blob-lagring**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-248">In the **Data Factory Editor**, click **... More** on the command bar, click **New dataset**, and select **Azure Blob storage**.</span></span>  
2. <span data-ttu-id="6efa4-249">Kopiera och klistra in följande kodfragment till fönstret Draft-1.</span><span class="sxs-lookup"><span data-stu-id="6efa4-249">Copy and paste the following snippet to the Draft-1 window.</span></span> <span data-ttu-id="6efa4-250">I JSON-kodfragmentet skapar du en datauppsättning som kallas **AzureBlobOutput** och anger strukturen för de data som produceras av Hive-skriptet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-250">In the JSON snippet, you are creating a dataset called **AzureBlobOutput**, and specifying the structure of the data that is produced by the Hive script.</span></span> <span data-ttu-id="6efa4-251">Dessutom kan du ange att resultaten lagras i blobbehållaren **adfgetstarted** och i mappen **partitioneddata**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-251">In addition, you specify that the results are stored in the blob container called **adfgetstarted** and the folder called **partitioneddata**.</span></span> <span data-ttu-id="6efa4-252">I avsnittet **tillgänglighet** anges att utdatauppsättningen skapas månadsvis.</span><span class="sxs-lookup"><span data-stu-id="6efa4-252">The **availability** section specifies that the output dataset is produced on a monthly basis.</span></span>

    ```JSON
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Month",
          "interval": 1
        }
      }
    }
    ```
    <span data-ttu-id="6efa4-253">Se **Skapa datauppsättning för indata** för beskrivningar av dessa egenskaper.</span><span class="sxs-lookup"><span data-stu-id="6efa4-253">See **Create the input dataset** section for descriptions of these properties.</span></span> <span data-ttu-id="6efa4-254">Du anger inte den externa egenskapen för en utdatauppsättning, eftersom datauppsättningen produceras av Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6efa4-254">You do not set the external property on an output dataset as the dataset is produced by the Data Factory service.</span></span>
3. <span data-ttu-id="6efa4-255">Klicka på **Distribuera** i kommandofältet för att distribuera den nyskapade datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-255">Click **Deploy** on the command bar to deploy the newly created dataset.</span></span>
4. <span data-ttu-id="6efa4-256">Kontrollera att datauppsättningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="6efa4-256">Verify that the dataset is created successfully.</span></span>

    ![Trädvy med länkade tjänster](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-data-set.png)

## <a name="create-pipeline"></a><span data-ttu-id="6efa4-258">Skapa pipeline</span><span class="sxs-lookup"><span data-stu-id="6efa4-258">Create pipeline</span></span>
<span data-ttu-id="6efa4-259">I det här steget ska du skapa din första pipeline med en **HDInsightHive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-259">In this step, you create your first pipeline with a **HDInsightHive** activity.</span></span> <span data-ttu-id="6efa4-260">Indatasektorn är tillgänglig månadsvis (frequency: Month, Interval: 1), utdatasektorn skapas varje månad och schemaegenskapen för aktiviteten har också inställningen Month.</span><span class="sxs-lookup"><span data-stu-id="6efa4-260">Input slice is available monthly (frequency: Month, interval: 1), output slice is produced monthly, and the scheduler property for the activity is also set to monthly.</span></span> <span data-ttu-id="6efa4-261">Inställningarna för utdatauppsättningen och aktivitetsschemaläggaren måste matcha.</span><span class="sxs-lookup"><span data-stu-id="6efa4-261">The settings for the output dataset and the activity scheduler must match.</span></span> <span data-ttu-id="6efa4-262">För närvarande är det utdatauppsättningen som skapar schemat. Därför måste du skapa en utdatauppsättning även om aktiviteten inte genererar några utdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-262">Currently, output dataset is what drives the schedule, so you must create an output dataset even if the activity does not produce any output.</span></span> <span data-ttu-id="6efa4-263">Om aktiviteten inte får några indata, kan du hoppa över att skapa indatauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-263">If the activity doesn't take any input, you can skip creating the input dataset.</span></span> <span data-ttu-id="6efa4-264">I slutet av det här avsnittet beskrivs de egenskaper som användes i följande JSON.</span><span class="sxs-lookup"><span data-stu-id="6efa4-264">The properties used in the following JSON are explained at the end of this section.</span></span>

1. <span data-ttu-id="6efa4-265">I **Data Factory-redigeraren** klickar du på **Tre punkter (...) Fler kommandon** och sedan på **Ny pipeline**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-265">In the **Data Factory Editor**, click **Ellipsis (…) More commands** and then click **New pipeline**.</span></span>

    ![knappen ny pipeline](./media/data-factory-build-your-first-pipeline-using-editor/new-pipeline-button.png)
2. <span data-ttu-id="6efa4-267">Kopiera och klistra in följande kodfragment till fönstret Draft-1.</span><span class="sxs-lookup"><span data-stu-id="6efa4-267">Copy and paste the following snippet to the Draft-1 window.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="6efa4-268">Ersätt **storageaccountname** med namnet på ditt lagringskonto i JSON.</span><span class="sxs-lookup"><span data-stu-id="6efa4-268">Replace **storageaccountname** with the name of your storage account in the JSON.</span></span>
   >
   >

    ```JSON
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "AzureStorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
                        }
                    ],
                    "policy": {
                        "concurrency": 1,
                        "retry": 3
                    },
                    "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                    },
                    "name": "RunSampleHiveActivity",
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```

    <span data-ttu-id="6efa4-269">I JSON-kodfragmentet skapar du en pipeline med en enda aktivitet, som använder Hive till att bearbeta data i ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="6efa4-269">In the JSON snippet, you are creating a pipeline that consists of a single activity that uses Hive to process Data on an HDInsight cluster.</span></span>

    <span data-ttu-id="6efa4-270">Hive-skriptfilen **partitionweblogs.hql** lagras i Azure-lagringskontot (anges med scriptLinkedService, kallas **AzureStorageLinkedService**), och i mappen **skript** i behållaren **adfgetstarted**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-270">The Hive script file, **partitionweblogs.hql**, is stored in the Azure storage account (specified by the scriptLinkedService, called **AzureStorageLinkedService**), and in **script** folder in the container **adfgetstarted**.</span></span>

    <span data-ttu-id="6efa4-271">Avsnittet **Definierar** används för att ange körningsinställningar som skickas till Hive-skriptet som Hive-konfigurationsvärden (t.ex. ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span><span class="sxs-lookup"><span data-stu-id="6efa4-271">The **defines** section is used to specify the runtime settings that are passed to the hive script as Hive configuration values (e.g ${hiveconf:inputtable}, ${hiveconf:partitionedtable}).</span></span>

    <span data-ttu-id="6efa4-272">Egenskaperna **start** och **slut** för pipelinen anger den aktiva perioden för pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-272">The **start** and **end** properties of the pipeline specifies the active period of the pipeline.</span></span>

    <span data-ttu-id="6efa4-273">I JSON-aktiviteten anger du att Hive-skriptet körs i den beräkning som anges av **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-273">In the activity JSON, you specify that the Hive script runs on the compute specified by the **linkedServiceName** – **HDInsightOnDemandLinkedService**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6efa4-274">Information om JSON-egenskaper som används i exemplet finns i avsnittet om Pipeline-JSON i [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) (Pipelines och aktiviteter i Azure Data Factory).</span><span class="sxs-lookup"><span data-stu-id="6efa4-274">See "Pipeline JSON" in [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md) for details about JSON properties used in the example.</span></span>
   >
   >
3. <span data-ttu-id="6efa4-275">Kontrollera följande:</span><span class="sxs-lookup"><span data-stu-id="6efa4-275">Confirm the following:</span></span>

   1. <span data-ttu-id="6efa4-276">**Input.log**-filen finns i mappen **inputdata** i behållaren **adfgetstarted** i Azure-blobblagringen</span><span class="sxs-lookup"><span data-stu-id="6efa4-276">**input.log** file exists in the **inputdata** folder of the **adfgetstarted** container in the Azure blob storage</span></span>
   2. <span data-ttu-id="6efa4-277">**partitionweblogs.hql**-filen finns i mappen **script** i behållaren **adfgetstarted** i Azure-blobblagringen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-277">**partitionweblogs.hql** file exists in the **script** folder of the **adfgetstarted** container in the Azure blob storage.</span></span> <span data-ttu-id="6efa4-278">Slutför de nödvändiga stegen i [Självstudier - översikt](data-factory-build-your-first-pipeline.md) om du inte ser de här filerna.</span><span class="sxs-lookup"><span data-stu-id="6efa4-278">Complete the prerequisite steps in the [Tutorial Overview](data-factory-build-your-first-pipeline.md) if you don't see these files.</span></span>
   3. <span data-ttu-id="6efa4-279">Kontrollera att du ersatte **storageaccountname** med namnet på ditt lagringskonto i JSON-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-279">Confirm that you replaced **storageaccountname** with the name of your storage account in the pipeline JSON.</span></span>
4. <span data-ttu-id="6efa4-280">Klicka på **Distribuera** i kommandofältet för att distribuera pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-280">Click **Deploy** on the command bar to deploy the pipeline.</span></span> <span data-ttu-id="6efa4-281">Eftersom tiderna för **start** och **slut** har angetts tidigare och **isPaused** har angetts till false, kommer pipelinen (aktiviteten i pipelinen) köras omedelbart efter att du har distribuerat.</span><span class="sxs-lookup"><span data-stu-id="6efa4-281">Since the **start** and **end** times are set in the past and **isPaused** is set to false, the pipeline (activity in the pipeline) runs immediately after you deploy.</span></span>
5. <span data-ttu-id="6efa4-282">Kontrollera att du ser pipelinen i trädvyn.</span><span class="sxs-lookup"><span data-stu-id="6efa4-282">Confirm that you see the pipeline in the tree view.</span></span>

    ![Trädvy med pipeline](./media/data-factory-build-your-first-pipeline-using-editor/tree-view-pipeline.png)
6. <span data-ttu-id="6efa4-284">Grattis, du har skapat din första pipeline!</span><span class="sxs-lookup"><span data-stu-id="6efa4-284">Congratulations, you have successfully created your first pipeline!</span></span>

## <a name="monitor-pipeline"></a><span data-ttu-id="6efa4-285">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="6efa4-285">Monitor pipeline</span></span>
### <a name="monitor-pipeline-using-diagram-view"></a><span data-ttu-id="6efa4-286">Övervaka pipeline med diagramvyn</span><span class="sxs-lookup"><span data-stu-id="6efa4-286">Monitor pipeline using Diagram View</span></span>
1. <span data-ttu-id="6efa4-287">Klicka på **X** för att stänga bladen i Data Factory-redigeraren och för att gå tillbaka till Data Factory-bladet och klicka på **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-287">Click **X** to close Data Factory Editor blades and to navigate back to the Data Factory blade, and click **Diagram**.</span></span>

    ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-editor/diagram-tile.png)
2. <span data-ttu-id="6efa4-289">I diagramvyn visas en översikt över pipelines och datauppsättningar som används i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="6efa4-289">In the Diagram View, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>

    ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-editor/diagram-view-2.png)
3. <span data-ttu-id="6efa4-291">Högerklicka på pipelinen i diagrammet om du vill visa alla aktiviteter i pipelinen. Klicka sedan på Öppna pipeline.</span><span class="sxs-lookup"><span data-stu-id="6efa4-291">To view all activities in the pipeline, right-click pipeline in the diagram and click Open Pipeline.</span></span>

    ![Menyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-menu.png)
4. <span data-ttu-id="6efa4-293">Kontrollera att du ser HDInsightHive-aktiviteten i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-293">Confirm that you see the HDInsightHive activity in the pipeline.</span></span>

    ![Vyn Öppna pipeline](./media/data-factory-build-your-first-pipeline-using-editor/open-pipeline-view.png)

    <span data-ttu-id="6efa4-295">Om du vill gå tillbaka till den föregående vyn klickar du på **Datafabrik** i adressfältmenyn längst upp.</span><span class="sxs-lookup"><span data-stu-id="6efa4-295">To navigate back to the previous view, click **Data factory** in the breadcrumb menu at the top.</span></span>
5. <span data-ttu-id="6efa4-296">Dubbelklicka på datauppsättningen **AzureBlobInput** i **diagramvyn**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-296">In the **Diagram View**, double-click the dataset **AzureBlobInput**.</span></span> <span data-ttu-id="6efa4-297">Kontrollera att sektorn har statusen **Klar**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-297">Confirm that the slice is in **Ready** state.</span></span> <span data-ttu-id="6efa4-298">Det kan ta några minuter innan sektorn visas med statusen Klar.</span><span class="sxs-lookup"><span data-stu-id="6efa4-298">It may take a couple of minutes for the slice to show up in Ready state.</span></span> <span data-ttu-id="6efa4-299">Om det inte händer trots att du har väntat ett tag, kontrollerar du om du har indatafilen (input.log) placerad i rätt behållare (adfgetstarted) och mapp (inputdata).</span><span class="sxs-lookup"><span data-stu-id="6efa4-299">If it does not happen after you wait for sometime, see if you have the input file (input.log) placed in the right container (adfgetstarted) and folder (inputdata).</span></span>

   ![Indatasektor med statusen Klar](./media/data-factory-build-your-first-pipeline-using-editor/input-slice-ready.png)
6. <span data-ttu-id="6efa4-301">Klicka på **X** för att stänga bladet **AzureBlobInput**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-301">Click **X** to close **AzureBlobInput** blade.</span></span>
7. <span data-ttu-id="6efa4-302">Dubbelklicka på datauppsättningen **AzureBlobOutput** i **diagramvyn**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-302">In the **Diagram View**, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="6efa4-303">Den sektor som för närvarande bearbetas visas.</span><span class="sxs-lookup"><span data-stu-id="6efa4-303">You see that the slice that is currently being processed.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-blade.png)
8. <span data-ttu-id="6efa4-305">När bearbetningen är klar visas sektorn med statusen **Klar**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-305">When processing is done, you see the slice in **Ready** state.</span></span>

   ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-editor/dataset-slice-ready.png)  

   > [!IMPORTANT]
   > <span data-ttu-id="6efa4-307">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="6efa4-307">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="6efa4-308">Förvänta dig därför att det       tar **cirka 30 minuter** för pipelinen att bearbeta sektorn.</span><span class="sxs-lookup"><span data-stu-id="6efa4-308">Therefore, expect the pipeline to       take **approximately 30 minutes** to process the slice.</span></span>
   >
   >

9. <span data-ttu-id="6efa4-309">När sektorn har statusen **Klar**, kontrollerar du mappen **partitioneddata** i behållaren **adfgetstarted** i blobblagringen för utdatan.</span><span class="sxs-lookup"><span data-stu-id="6efa4-309">When the slice is in **Ready** state, check the **partitioneddata** folder in the **adfgetstarted** container in your blob storage for the output data.</span></span>  

   ![utdata](./media/data-factory-build-your-first-pipeline-using-editor/three-ouptut-files.png)
10. <span data-ttu-id="6efa4-311">Klicka på sektorn om du vill se information om den i ett **Datasektor**-blad.</span><span class="sxs-lookup"><span data-stu-id="6efa4-311">Click the slice to see details about it in a **Data slice** blade.</span></span>

   ![Information om datasektorn](./media/data-factory-build-your-first-pipeline-using-editor/data-slice-details.png)  
11. <span data-ttu-id="6efa4-313">Klicka på en aktivitet som körs i **Lista med aktivitetskörningar** för att se information om en aktivitetskörning (Hive-aktivitet i vårt exempel) i ett fönster med **Aktivitetskörningsinformation**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-313">Click an activity run in the **Activity runs list** to see details about an activity run (Hive activity in our scenario) in an **Activity run details** window.</span></span>   

   ![Aktivitetskörningsinformation](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-blade.png)    

   <span data-ttu-id="6efa4-315">Du kan se Hive-frågan som kördes och statusinformation i loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="6efa4-315">From the log files, you can see the Hive query that was executed and status information.</span></span> <span data-ttu-id="6efa4-316">Dessa loggar är användbara vid felsökning av eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="6efa4-316">These logs are useful for troubleshooting any issues.</span></span>
   <span data-ttu-id="6efa4-317">Se artikeln [Övervaka och hantera pipelines med Azure-portalblad](data-factory-monitor-manage-pipelines.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6efa4-317">See [Monitor and manage pipelines using Azure portal blades](data-factory-monitor-manage-pipelines.md) article for more details.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6efa4-318">Indatafilen tas bort när sektorn har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="6efa4-318">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="6efa4-319">Om du vill köra sektorn eller gå igenom självstudien igen överför du därför indatafilen (input.log) till indatamappen i behållaren adfgetstarted.</span><span class="sxs-lookup"><span data-stu-id="6efa4-319">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the inputdata folder of the adfgetstarted container.</span></span>
>
>

### <a name="monitor-pipeline-using-monitor--manage-app"></a><span data-ttu-id="6efa4-320">Övervaka pipeline med övervaknings- och hanteringsappen</span><span class="sxs-lookup"><span data-stu-id="6efa4-320">Monitor pipeline using Monitor & Manage App</span></span>
<span data-ttu-id="6efa4-321">Du kan också använda övervaknings- och hanteringsprogrammet till att övervaka dina pipelines.</span><span class="sxs-lookup"><span data-stu-id="6efa4-321">You can also use Monitor & Manage application to monitor your pipelines.</span></span> <span data-ttu-id="6efa4-322">Se [Övervaka och hantera Azure Data Factory-pipelines med övervaknings- och hanteringsappen](data-factory-monitor-manage-app.md) för mer information om att använda programmet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-322">For detailed information about using this application, see [Monitor and manage Azure Data Factory pipelines using Monitoring and Management App](data-factory-monitor-manage-app.md).</span></span>

1. <span data-ttu-id="6efa4-323">Klicka på ikonen **Övervaka och hantera** på datafabrikens startsida.</span><span class="sxs-lookup"><span data-stu-id="6efa4-323">Click **Monitor & Manage** tile on the home page for your data factory.</span></span>

    ![Ikonen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-tile.png)
2. <span data-ttu-id="6efa4-325">Du bör se **Övervakaren och hantera program**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-325">You should see **Monitor & Manage application**.</span></span> <span data-ttu-id="6efa4-326">Ändra **Starttid** och **Sluttid** så att de matchar starttid och sluttid i pipelinen. Klicka sedan på **Tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-326">Change the **Start time** and **End time** to match start and end times of your pipeline, and click **Apply**.</span></span>

    ![Appen Övervaka och hantera](./media/data-factory-build-your-first-pipeline-using-editor/monitor-and-manage-app.png)
3. <span data-ttu-id="6efa4-328">Välj ett aktivitetsfönster i listan **Aktivitetsfönster** om du vill se information om det.</span><span class="sxs-lookup"><span data-stu-id="6efa4-328">Select an activity window in the **Activity Windows** list to see details about it.</span></span>

    ![Information om aktivitetsfönstret](./media/data-factory-build-your-first-pipeline-using-editor/activity-window-details.png)

## <a name="summary"></a><span data-ttu-id="6efa4-330">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="6efa4-330">Summary</span></span>
<span data-ttu-id="6efa4-331">I den här självstudien skapade du en Azure-datafabrik som bearbetar data genom att köra ett Hive-skript i ett Hadoop-kluster i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6efa4-331">In this tutorial, you created an Azure data factory to process data by running Hive script on a HDInsight hadoop cluster.</span></span> <span data-ttu-id="6efa4-332">Du utförde följande steg med hjälp av Data Factory-redigeraren i Azure Portal:</span><span class="sxs-lookup"><span data-stu-id="6efa4-332">You used the Data Factory Editor in the Azure portal to do the following steps:</span></span>  

1. <span data-ttu-id="6efa4-333">Du skapade en Azure **Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="6efa4-333">Created an Azure **data factory**.</span></span>
2. <span data-ttu-id="6efa4-334">Du skapade två **länkade tjänster**:</span><span class="sxs-lookup"><span data-stu-id="6efa4-334">Created two **linked services**:</span></span>
   1. <span data-ttu-id="6efa4-335">En länkad **Azure Storage-**tjänst som länkar din Azure Blob-lagring med in-/utdatafiler till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-335">**Azure Storage** linked service to link your Azure blob storage that holds input/output files to the data factory.</span></span>
   2. <span data-ttu-id="6efa4-336">En länkad **Azure HDInsight**-tjänst på begäran som länkar ett Hadoop-kluster i HDInsight på begäran till datafabriken.</span><span class="sxs-lookup"><span data-stu-id="6efa4-336">**Azure HDInsight** on-demand linked service to link an on-demand HDInsight Hadoop cluster to the data factory.</span></span> <span data-ttu-id="6efa4-337">Azure Data Factory skapar ett Hadoop-kluster i HDInsight i rätt tid för att bearbeta indata och skapa utdata.</span><span class="sxs-lookup"><span data-stu-id="6efa4-337">Azure Data Factory creates a HDInsight Hadoop cluster just-in-time to process input data and produce output data.</span></span>
3. <span data-ttu-id="6efa4-338">Du skapade två **datauppsättningar** som beskriver in- och utdata för Hive-aktiviteten för HDInsight i pipelinen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-338">Created two **datasets**, which describe input and output data for HDInsight Hive activity in the pipeline.</span></span>
4. <span data-ttu-id="6efa4-339">Du skapade en **pipeline** med en **HDInsight Hive**-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6efa4-339">Created a **pipeline** with a **HDInsight Hive** activity.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6efa4-340">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6efa4-340">Next Steps</span></span>
<span data-ttu-id="6efa4-341">I den här artikeln har du skapat en pipeline med en transformeringsaktivitet (HDInsight-aktivitet) som kör ett Hive-skript på ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="6efa4-341">In this article, you have created a pipeline with a transformation activity (HDInsight Activity) that runs a Hive script on an on-demand HDInsight cluster.</span></span> <span data-ttu-id="6efa4-342">Om du vill se hur du använder en kopieringsaktivitet till att kopiera data från en Azure-blobb till Azure SQL kan du läsa mer i [Självstudie: Kopiera data från en Azure-blobb till Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6efa4-342">To see how to use a Copy Activity to copy data from an Azure Blob to Azure SQL, see [Tutorial: Copy data from an Azure blob to Azure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="6efa4-343">Se även</span><span class="sxs-lookup"><span data-stu-id="6efa4-343">See Also</span></span>
| <span data-ttu-id="6efa4-344">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="6efa4-344">Topic</span></span> | <span data-ttu-id="6efa4-345">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6efa4-345">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="6efa4-346">Pipelines</span><span class="sxs-lookup"><span data-stu-id="6efa4-346">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="6efa4-347">I den här artikeln beskriver vi pipelines och aktiviteter i Azure Data Factory och hur du kan använda dem för att konstruera datadrivna arbetsflöden från slutpunkt till slutpunkt för ditt scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="6efa4-347">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="6efa4-348">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="6efa4-348">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="6efa4-349">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6efa4-349">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="6efa4-350">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="6efa4-350">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="6efa4-351">I den här artikeln beskrivs aspekter för schemaläggning och körning av Azure Data Factory-programmodellen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-351">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="6efa4-352">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="6efa4-352">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="6efa4-353">Den här artikeln beskriver hur du övervakar, hanterar och felsöker pipelines med övervaknings- och hanteringsappen.</span><span class="sxs-lookup"><span data-stu-id="6efa4-353">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |
