---
title: "aaaCreate förutsägande data pipelines med Azure Data Factory | Microsoft Docs"
description: "Beskriver hur toocreate skapar förutsägbara pipelines med hjälp av Azure Data Factory och Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a><span data-ttu-id="48655-103">Skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="48655-103">Create predictive pipelines using Azure Machine Learning and Azure Data Factory</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="48655-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="48655-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="48655-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="48655-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="48655-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="48655-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="48655-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="48655-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="48655-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="48655-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="48655-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="48655-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="48655-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="48655-114">Introduktion</span><span class="sxs-lookup"><span data-stu-id="48655-114">Introduction</span></span>

### <a name="azure-machine-learning"></a><span data-ttu-id="48655-115">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="48655-115">Azure Machine Learning</span></span>
<span data-ttu-id="48655-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) aktiverar toobuild, testa och distribuera prediktiva Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="48655-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) enables you toobuild, test, and deploy predictive analytics solutions.</span></span> <span data-ttu-id="48655-117">Från en övergripande synsätt görs i tre steg:</span><span class="sxs-lookup"><span data-stu-id="48655-117">From a high-level point of view, it is done in three steps:</span></span>

1. <span data-ttu-id="48655-118">**Skapa en träningsexperiment**.</span><span class="sxs-lookup"><span data-stu-id="48655-118">**Create a training experiment**.</span></span> <span data-ttu-id="48655-119">Du kan göra det här steget med hello Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="48655-119">You do this step by using hello Azure ML Studio.</span></span> <span data-ttu-id="48655-120">hello ML studio är en gemensam visual utvecklingsmiljö om du använder tootrain och testa en förutsägelseanalysmodell med hjälp av utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="48655-120">hello ML studio is a collaborative visual development environment that you use tootrain and test a predictive analytics model using training data.</span></span>
2. <span data-ttu-id="48655-121">**Konvertera den tooa prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="48655-121">**Convert it tooa predictive experiment**.</span></span> <span data-ttu-id="48655-122">När din modell har tränats med befintliga data och du är redo toouse den tooscore nya data, du förbereda och förenkla experimentet för resultatfunktioner.</span><span class="sxs-lookup"><span data-stu-id="48655-122">Once your model has been trained with existing data and you are ready toouse it tooscore new data, you prepare and streamline your experiment for scoring.</span></span>
3. <span data-ttu-id="48655-123">**Distribuera den som en webbtjänst**.</span><span class="sxs-lookup"><span data-stu-id="48655-123">**Deploy it as a web service**.</span></span> <span data-ttu-id="48655-124">Du kan publicera experimentet bedömningsprofil som Azure-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48655-124">You can publish your scoring experiment as an Azure web service.</span></span> <span data-ttu-id="48655-125">Du kan skicka tooyour datamodellen via den här slutet för webbtjänst och ta emot resultatet förutsägelser från hello modellen.</span><span class="sxs-lookup"><span data-stu-id="48655-125">You can send data tooyour model via this web service end point and receive result predictions fro hello model.</span></span>  

### <a name="azure-data-factory"></a><span data-ttu-id="48655-126">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="48655-126">Azure Data Factory</span></span>
<span data-ttu-id="48655-127">Data Factory är en molnbaserad integration datatjänst som samordnar och automatiserar hello **flytt** och **omvandling** av data.</span><span class="sxs-lookup"><span data-stu-id="48655-127">Data Factory is a cloud-based data integration service that orchestrates and automates hello **movement** and **transformation** of data.</span></span> <span data-ttu-id="48655-128">Du kan skapa data integration lösningar med hjälp av Azure Data Factory som kan mata in data från olika datakällor, transformera/bearbeta hello data och publicera hello resultatet data toohello datalager.</span><span class="sxs-lookup"><span data-stu-id="48655-128">You can create data integration solutions using Azure Data Factory that can ingest data from various data stores, transform/process hello data, and publish hello result data toohello data stores.</span></span>

<span data-ttu-id="48655-129">Data Factory-tjänsten kan du toocreate data pipelines som flyttas och transformera data och kör sedan hello pipelines enligt ett angivet schema (varje timme, varje dag, vecka, etc.).</span><span class="sxs-lookup"><span data-stu-id="48655-129">Data Factory service allows you toocreate data pipelines that move and transform data, and then run hello pipelines on a specified schedule (hourly, daily, weekly, etc.).</span></span> <span data-ttu-id="48655-130">Dessutom ger omfattande visualiseringar toodisplay hello härkomst och beroenden mellan dina data pipelines och övervaka alla data pipelines från en enda enhetlig vy tooeasily hitta problem och konfigurera övervakningsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="48655-130">It also provides rich visualizations toodisplay hello lineage and dependencies between your data pipelines, and monitor all your data pipelines from a single unified view tooeasily pinpoint issues and setup monitoring alerts.</span></span>

<span data-ttu-id="48655-131">Se [introduktion tooAzure Data Factory](data-factory-introduction.md) och [skapa din första pipeline](data-factory-build-your-first-pipeline.md) artiklar tooquickly Kom igång med hello Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="48655-131">See [Introduction tooAzure Data Factory](data-factory-introduction.md) and [Build your first pipeline](data-factory-build-your-first-pipeline.md) articles tooquickly get started with hello Azure Data Factory service.</span></span>

### <a name="data-factory-and-machine-learning-together"></a><span data-ttu-id="48655-132">Data Factory och Machine Learning tillsammans</span><span class="sxs-lookup"><span data-stu-id="48655-132">Data Factory and Machine Learning together</span></span>
<span data-ttu-id="48655-133">Azure Data Factory aktiverar du tooeasily skapa pipelines som använder en publicerade [Azure Machine Learning] [ azure-machine-learning] webbtjänsten för förutsägelseanalys.</span><span class="sxs-lookup"><span data-stu-id="48655-133">Azure Data Factory enables you tooeasily create pipelines that use a published [Azure Machine Learning][azure-machine-learning] web service for predictive analytics.</span></span> <span data-ttu-id="48655-134">Med hjälp av hello **Batchkörningsaktivitet** i Azure Data Factory-pipelinen, du kan anropa en Azure ML web service toomake förutsägelser på hello data i en batch.</span><span class="sxs-lookup"><span data-stu-id="48655-134">Using hello **Batch Execution Activity** in an Azure Data Factory pipeline, you can invoke an Azure ML web service toomake predictions on hello data in batch.</span></span> <span data-ttu-id="48655-135">Se [anropar en Azure ML web service använder hello Batchkörningsaktivitet](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) information.</span><span class="sxs-lookup"><span data-stu-id="48655-135">See [Invoking an Azure ML web service using hello Batch Execution Activity](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) section for details.</span></span>

<span data-ttu-id="48655-136">Över tiden måste hello förutsägelsemodeller i hello Azure ML bedömningsprofil experiment toobe retrained med nya indatauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="48655-136">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="48655-137">Du kan träna om Azure ML-modell från Data Factory-pipelinen genom att göra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="48655-137">You can retrain an Azure ML model from a Data Factory pipeline by doing hello following steps:</span></span>

1. <span data-ttu-id="48655-138">Publicera hello träningsexperiment (inte prediktivt experiment) som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48655-138">Publish hello training experiment (not predictive experiment) as a web service.</span></span> <span data-ttu-id="48655-139">Du kan göra det här steget i hello Azure ML Studio som du gjorde tooexpose prediktivt experiment som en webbtjänst i hello föregående scenario.</span><span class="sxs-lookup"><span data-stu-id="48655-139">You do this step in hello Azure ML Studio as you did tooexpose predictive experiment as a web service in hello previous scenario.</span></span>
2. <span data-ttu-id="48655-140">Använd hello Azure ML-Batchkörningsaktivitet tooinvoke hello webbtjänsten hello träningsexperiment.</span><span class="sxs-lookup"><span data-stu-id="48655-140">Use hello Azure ML Batch Execution Activity tooinvoke hello web service for hello training experiment.</span></span> <span data-ttu-id="48655-141">I princip kan du använda hello Azure ML-batchkörning aktiviteten tooinvoke både utbildning webbtjänsten och poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="48655-141">Basically, you can use hello Azure ML Batch Execution activity tooinvoke both training web service and scoring web service.</span></span>

<span data-ttu-id="48655-142">När du är klar med omtränings uppdatera hello bedömningen webbtjänst (prediktivt experiment visas som en webbtjänst) med hello nyligen tränade modellen med hjälp av hello **Azure ML uppdatera resurs aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="48655-142">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="48655-143">Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="48655-143">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

## <a name="invoking-a-web-service-using-batch-execution-activity"></a><span data-ttu-id="48655-144">Anropa en webbtjänst med hjälp av Batchkörningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-144">Invoking a web service using Batch Execution Activity</span></span>
<span data-ttu-id="48655-145">Du använder Azure Data Factory tooorchestrate dataförflyttning och bearbetning och sedan utföra batchkörning med hjälp av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="48655-145">You use Azure Data Factory tooorchestrate data movement and processing, and then perform batch execution using Azure Machine Learning.</span></span> <span data-ttu-id="48655-146">Här är hello översta steg:</span><span class="sxs-lookup"><span data-stu-id="48655-146">Here are hello top-level steps:</span></span>

1. <span data-ttu-id="48655-147">Skapa en Azure Machine Learning länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="48655-147">Create an Azure Machine Learning linked service.</span></span> <span data-ttu-id="48655-148">Du behöver hello följande värden:</span><span class="sxs-lookup"><span data-stu-id="48655-148">You need hello following values:</span></span>

   1. <span data-ttu-id="48655-149">**Begärande-URI** för hello Batch Execution API.</span><span class="sxs-lookup"><span data-stu-id="48655-149">**Request URI** for hello Batch Execution API.</span></span> <span data-ttu-id="48655-150">Du kan hitta hello Begärd URI genom att klicka på hello **BATCH EXECUTION** länk hello web services-sidan.</span><span class="sxs-lookup"><span data-stu-id="48655-150">You can find hello Request URI by clicking hello **BATCH EXECUTION** link in hello web services page.</span></span>
   2. <span data-ttu-id="48655-151">**API-nyckel** för hello publicerade Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48655-151">**API key** for hello published Azure Machine Learning web service.</span></span> <span data-ttu-id="48655-152">Du hittar hello API-nyckel genom att klicka på hello-webbtjänst som du har publicerat.</span><span class="sxs-lookup"><span data-stu-id="48655-152">You can find hello API key by clicking hello web service that you have published.</span></span>
   3. <span data-ttu-id="48655-153">Använd hello **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-153">Use hello **AzureMLBatchExecution** activity.</span></span>

      ![Machine Learning-instrumentpanelen](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a><span data-ttu-id="48655-156">Scenario: Försök med hjälp av Web service indata/utdata som refererar toodata i Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="48655-156">Scenario: Experiments using Web service inputs/outputs that refer toodata in Azure Blob Storage</span></span>
<span data-ttu-id="48655-157">I det här scenariot hello Azure Machine Learning-webbtjänsten gör förutsägelser med hjälp av data från en fil i ett Azure blob storage och lagrar hello förutsägelse resultat i hello blob storage.</span><span class="sxs-lookup"><span data-stu-id="48655-157">In this scenario, hello Azure Machine Learning Web service makes predictions using data from a file in an Azure blob storage and stores hello prediction results in hello blob storage.</span></span> <span data-ttu-id="48655-158">hello definierar följande JSON Data Factory-pipelinen med en AzureMLBatchExecution aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-158">hello following JSON defines a Data Factory pipeline with an AzureMLBatchExecution activity.</span></span> <span data-ttu-id="48655-159">hello aktiviteten har hello dataset **DecisionTreeInputBlob** som indata och **DecisionTreeResultBlob** som hello utdata.</span><span class="sxs-lookup"><span data-stu-id="48655-159">hello activity has hello dataset **DecisionTreeInputBlob** as input and **DecisionTreeResultBlob** as hello output.</span></span> <span data-ttu-id="48655-160">Hej **DecisionTreeInputBlob** skickas som ett inkommande toohello webbtjänsten genom att använda hello **webServiceInput** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-160">hello **DecisionTreeInputBlob** is passed as an input toohello web service by using hello **webServiceInput** JSON property.</span></span> <span data-ttu-id="48655-161">Hej **DecisionTreeResultBlob** skickas som en webbtjänst för utdata toohello genom att använda hello **webServiceOutputs** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-161">hello **DecisionTreeResultBlob** is passed as an output toohello Web service by using hello **webServiceOutputs** JSON property.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="48655-162">Om hello webbtjänst tar flera inmatningar kan använda hello **webServiceInputs** egenskapen istället för att använda **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="48655-162">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="48655-163">Se hello [webbtjänst kräver flera indata](#web-service-requires-multiple-inputs) för ett exempel på hur hello webServiceInputs egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-163">See hello [Web service requires multiple inputs](#web-service-requires-multiple-inputs) section for an example of using hello webServiceInputs property.</span></span>
>
> <span data-ttu-id="48655-164">Datauppsättningar som refereras av hello **webServiceInput**/**webServiceInputs** och **webServiceOutputs** egenskaper (i  **typeProperties**) måste också tas med i hello aktiviteten **indata** och **matar ut**.</span><span class="sxs-lookup"><span data-stu-id="48655-164">Datasets that are referenced by hello **webServiceInput**/**webServiceInputs** and **webServiceOutputs** properties (in **typeProperties**) must also be included in hello Activity **inputs** and **outputs**.</span></span>
>
> <span data-ttu-id="48655-165">Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar.</span><span class="sxs-lookup"><span data-stu-id="48655-165">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="48655-166">hello-namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha hello namnen i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-166">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="48655-167">Du kan visa hello begärannyttolast för exempel på hello Batch Execution hjälpsidan för din Azure ML endpoint tooverify hello förväntades mappning.</span><span class="sxs-lookup"><span data-stu-id="48655-167">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> <span data-ttu-id="48655-168">Endast indata och utdata för hello AzureMLBatchExecution aktivitet kan skickas som parametrar toohello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="48655-168">Only inputs and outputs of hello AzureMLBatchExecution activity can be passed as parameters toohello Web service.</span></span> <span data-ttu-id="48655-169">Hello ovan JSON fragment är exempelvis DecisionTreeInputBlob en inkommande toohello AzureMLBatchExecution aktivitet som skickas som ett inkommande toohello webbtjänsten via webServiceInput-parametern.</span><span class="sxs-lookup"><span data-stu-id="48655-169">For example, in hello above JSON snippet, DecisionTreeInputBlob is an input toohello AzureMLBatchExecution activity, which is passed as an input toohello Web service via webServiceInput parameter.</span></span>   
>
>

### <a name="example"></a><span data-ttu-id="48655-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="48655-170">Example</span></span>
<span data-ttu-id="48655-171">Det här exemplet använder Azure Storage toohold hello både inkommande och utgående data.</span><span class="sxs-lookup"><span data-stu-id="48655-171">This example uses Azure Storage toohold both hello input and output data.</span></span>

<span data-ttu-id="48655-172">Vi rekommenderar att du går igenom hello [skapa din första pipeline med Data Factory] [ adf-build-1st-pipeline] självstudiekursen innan du fortsätter med det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="48655-172">We recommend that you go through hello [Build your first pipeline with Data Factory][adf-build-1st-pipeline] tutorial before going through this example.</span></span> <span data-ttu-id="48655-173">Använd hello Data Factory-redigeraren toocreate Data Factory artefakter (länkade tjänster, datauppsättningar, rörledningar) i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="48655-173">Use hello Data Factory Editor toocreate Data Factory artifacts (linked services, datasets, pipeline) in this example.</span></span>   

1. <span data-ttu-id="48655-174">Skapa en **länkade tjänsten** för din **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="48655-174">Create a **linked service** for your **Azure Storage**.</span></span> <span data-ttu-id="48655-175">Om hello inkommande och utgående filer finns i olika storage-konton, måste två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="48655-175">If hello input and output files are in different storage accounts, you need two linked services.</span></span> <span data-ttu-id="48655-176">Här är en JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="48655-176">Here is a JSON example:</span></span>

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. <span data-ttu-id="48655-177">Skapa hello **inkommande** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="48655-177">Create hello **input** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="48655-178">Till skillnad från vissa andra Data Factory datauppsättningar, måste dessa data inte innehålla både **folderPath** och **fileName** värden.</span><span class="sxs-lookup"><span data-stu-id="48655-178">Unlike some other Data Factory datasets, these datasets must contain both **folderPath** and **fileName** values.</span></span> <span data-ttu-id="48655-179">Du kan använda partitionering toocause tooprocess varje batch-körningen (varje datasektorn) eller producera unika inkommande och utgående filer.</span><span class="sxs-lookup"><span data-stu-id="48655-179">You can use partitioning toocause each batch execution (each data slice) tooprocess or produce unique input and output files.</span></span> <span data-ttu-id="48655-180">Du kan behöva tooinclude vissa uppströmsaktivitet tootransform hello indata för hello CSV-filformat och placera den i hello storage-konto för varje segment.</span><span class="sxs-lookup"><span data-stu-id="48655-180">You may need tooinclude some upstream activity tootransform hello input into hello CSV file format and place it in hello storage account for each slice.</span></span> <span data-ttu-id="48655-181">I så fall skulle du inte inkludera hello **externa** och **externalData** inställningarna i hello följande exempel och din DecisionTreeInputBlob skulle vara hello datamängd för utdata för en annan aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-181">In that case, you would not include hello **external** and **externalData** settings shown in hello following example, and your DecisionTreeInputBlob would be hello output dataset of a different Activity.</span></span>

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }
    ```

    <span data-ttu-id="48655-182">Inkommande csv-filen måste ha hello Kolumnrubrikrad.</span><span class="sxs-lookup"><span data-stu-id="48655-182">Your input csv file must have hello column header row.</span></span> <span data-ttu-id="48655-183">Om du använder hello **Kopieringsaktiviteten** toocreate/flytta hello csv hello blob-lagring, bör du ange hello sink egenskapen **blobWriterAddHeader** för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="48655-183">If you are using hello **Copy Activity** toocreate/move hello csv into hello blob storage, you should set hello sink property **blobWriterAddHeader** too**true**.</span></span> <span data-ttu-id="48655-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="48655-184">For example:</span></span>

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    <span data-ttu-id="48655-185">Om hello csv-filen saknar hello rubrikraden, du kan se hello följande fel: **fel i aktiviteten: fel vid läsning av strängen. Oväntad token: StartObject. Sökvägen '', rad 1, position 1**.</span><span class="sxs-lookup"><span data-stu-id="48655-185">If hello csv file does not have hello header row, you may see hello following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.</span></span>
3. <span data-ttu-id="48655-186">Skapa hello **utdata** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="48655-186">Create hello **output** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="48655-187">Det här exemplet använder partitionering toocreate en unik Utdatasökväg för varje segment-körning.</span><span class="sxs-lookup"><span data-stu-id="48655-187">This example uses partitioning toocreate a unique output path for each slice execution.</span></span> <span data-ttu-id="48655-188">Utan hello partitionering, skulle hello aktiviteten över hello-fil.</span><span class="sxs-lookup"><span data-stu-id="48655-188">Without hello partitioning, hello activity would overwrite hello file.</span></span>

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. <span data-ttu-id="48655-189">Skapa en **länkade tjänsten** av typen: **AzureMLLinkedService**, tillhandahåller hello API-nyckel och modellerar batch execution URL.</span><span class="sxs-lookup"><span data-stu-id="48655-189">Create a **linked service** of type: **AzureMLLinkedService**, providing hello API key and model batch execution URL.</span></span>

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. <span data-ttu-id="48655-190">Slutligen skapar en pipeline som innehåller en **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-190">Finally, author a pipeline containing an **AzureMLBatchExecution** Activity.</span></span> <span data-ttu-id="48655-191">Vid körning utför pipeline hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="48655-191">At runtime, pipeline performs hello following steps:</span></span>

   1. <span data-ttu-id="48655-192">Hämtar hello platsen för filen med indata-hello från dina indata datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="48655-192">Gets hello location of hello input file from your input datasets.</span></span>
   2. <span data-ttu-id="48655-193">Anropar hello-batchkörning i Azure Machine Learning API</span><span class="sxs-lookup"><span data-stu-id="48655-193">Invokes hello Azure Machine Learning batch execution API</span></span>
   3. <span data-ttu-id="48655-194">Kopior hello batch execution utdata toohello blob i datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="48655-194">Copies hello batch execution output toohello blob given in your output dataset.</span></span>

      > [!NOTE]
      > <span data-ttu-id="48655-195">AzureMLBatchExecution aktivitet kan ha noll eller flera in- och utdata för en eller flera.</span><span class="sxs-lookup"><span data-stu-id="48655-195">AzureMLBatchExecution activity can have zero or more inputs and one or more outputs.</span></span>
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      <span data-ttu-id="48655-196">Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="48655-196">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="48655-197">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="48655-197">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="48655-198">Hej **end** tid är valfritt.</span><span class="sxs-lookup"><span data-stu-id="48655-198">hello **end** time is optional.</span></span> <span data-ttu-id="48655-199">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar.**”</span><span class="sxs-lookup"><span data-stu-id="48655-199">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="48655-200">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-200">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="48655-201">Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="48655-201">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

      > [!NOTE]
      > <span data-ttu-id="48655-202">Ange indata för hello AzureMLBatchExecution aktivitet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="48655-202">Specifying input for hello AzureMLBatchExecution activity is optional.</span></span>
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a><span data-ttu-id="48655-203">Scenario: Försök använda Reader/Writer moduler toorefer toodata i olika lagringsobjekt</span><span class="sxs-lookup"><span data-stu-id="48655-203">Scenario: Experiments using Reader/Writer Modules toorefer toodata in various storages</span></span>
<span data-ttu-id="48655-204">Ett annat vanligt scenario när du skapar Azure ML experiment är toouse läsare och skrivare moduler.</span><span class="sxs-lookup"><span data-stu-id="48655-204">Another common scenario when creating Azure ML experiments is toouse Reader and Writer modules.</span></span> <span data-ttu-id="48655-205">modul för dataläsare för hello är används tooload data till ett experiment hello-skrivarmodul är toosave data från dina experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-205">hello reader module is used tooload data into an experiment and hello writer module is toosave data from your experiments.</span></span> <span data-ttu-id="48655-206">Mer information om läsare och skrivare finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library.</span><span class="sxs-lookup"><span data-stu-id="48655-206">For details about reader and writer modules, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span>     

<span data-ttu-id="48655-207">När du använder hello läsare och skrivare moduler, är det bra toouse en Web service-parameter för varje egenskap för dessa moduler reader/writer.</span><span class="sxs-lookup"><span data-stu-id="48655-207">When using hello reader and writer modules, it is good practice toouse a Web service parameter for each property of these reader/writer modules.</span></span> <span data-ttu-id="48655-208">Dessa webb-parametrar kan du tooconfigure hello värden under körningen.</span><span class="sxs-lookup"><span data-stu-id="48655-208">These web parameters enable you tooconfigure hello values during runtime.</span></span> <span data-ttu-id="48655-209">Du kan till exempel skapa ett experiment med en modul för läsare som använder en Azure SQL Database: XXX.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="48655-209">For example, you could create an experiment with a reader module that uses an Azure SQL Database: XXX.database.windows.net.</span></span> <span data-ttu-id="48655-210">När hello webbtjänst har distribuerats, vill du tooenable hello konsumenter av hello web service toospecify en annan Azure SQL-Server som kallas YYY.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="48655-210">After hello web service has been deployed, you want tooenable hello consumers of hello web service toospecify another Azure SQL Server called YYY.database.windows.net.</span></span> <span data-ttu-id="48655-211">Du kan använda en Web service parametern tooallow det här värdet toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="48655-211">You can use a Web service parameter tooallow this value toobe configured.</span></span>

> [!NOTE]
> <span data-ttu-id="48655-212">Webbtjänst och utdata skiljer sig från webbtjänstparametrar.</span><span class="sxs-lookup"><span data-stu-id="48655-212">Web service input and output are different from Web service parameters.</span></span> <span data-ttu-id="48655-213">I hello första scenariot har du sett hur indata och utdata kan anges för en Azure ML-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="48655-213">In hello first scenario, you have seen how an input and output can be specified for an Azure ML Web service.</span></span> <span data-ttu-id="48655-214">I det här scenariot kan du överföra parametrar för en webbtjänst som motsvarar tooproperties av reader/writer moduler.</span><span class="sxs-lookup"><span data-stu-id="48655-214">In this scenario, you pass parameters for a Web service that correspond tooproperties of reader/writer modules.</span></span>
>
>

<span data-ttu-id="48655-215">Nu ska vi titta på ett scenario för att använda webbtjänstparametrar.</span><span class="sxs-lookup"><span data-stu-id="48655-215">Let's look at a scenario for using Web service parameters.</span></span> <span data-ttu-id="48655-216">Du har en distribuerad Azure Machine Learning-webbtjänst som använder en läsare modulen tooread data från en hello datakällor som stöds av Azure Machine Learning (till exempel: Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="48655-216">You have a deployed Azure Machine Learning web service that uses a reader module tooread data from one of hello data sources supported by Azure Machine Learning (for example: Azure SQL Database).</span></span> <span data-ttu-id="48655-217">När hello batchkörning utförs, skrivs hello resultat med skrivarmodul (Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="48655-217">After hello batch execution is performed, hello results are written using a Writer module (Azure SQL Database).</span></span>  <span data-ttu-id="48655-218">Ingen service indata och utdata har definierats i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-218">No web service inputs and outputs are defined in hello experiments.</span></span> <span data-ttu-id="48655-219">I detta fall rekommenderar vi att du konfigurerar relevanta webbtjänstparametrar för hello läsare och skrivare moduler.</span><span class="sxs-lookup"><span data-stu-id="48655-219">In this case, we recommend that you configure relevant web service parameters for hello reader and writer modules.</span></span> <span data-ttu-id="48655-220">Den här konfigurationen kan hello reader/writer moduler toobe konfigureras när du använder hello AzureMLBatchExecution aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="48655-220">This configuration allows hello reader/writer modules toobe configured when using hello AzureMLBatchExecution activity.</span></span> <span data-ttu-id="48655-221">Du anger webbtjänstparametrar i hello **globalParameters** under hello aktivitets-JSON på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="48655-221">You specify Web service parameters in hello **globalParameters** section in hello activity JSON as follows.</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

<span data-ttu-id="48655-222">Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i och ange värden för hello Web Serviceparametrar som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="48655-222">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="48655-223">hello webbtjänstparametrar är skiftlägeskänsligt, så se till att hello-namn som du anger i hello aktivitet JSON matchar hello som exponeras av hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="48655-223">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a><span data-ttu-id="48655-224">Med hjälp av en läsare modulen tooread data från flera filer i Azure Blob</span><span class="sxs-lookup"><span data-stu-id="48655-224">Using a Reader module tooread data from multiple files in Azure Blob</span></span>
<span data-ttu-id="48655-225">Stordata rörledningar med aktiviteter, till exempel Pig och Hive kan ge en eller flera utgående filer utan filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="48655-225">Big data pipelines with activities such as Pig and Hive can produce one or more output files with no extensions.</span></span> <span data-ttu-id="48655-226">Till exempel när du anger en extern Hive-tabell kan hello data för hello externa Hive-tabell lagras i Azure blob storage med följande namn 000000_0 hello.</span><span class="sxs-lookup"><span data-stu-id="48655-226">For example, when you specify an external Hive table, hello data for hello external Hive table can be stored in Azure blob storage with hello following name 000000_0.</span></span> <span data-ttu-id="48655-227">Du kan använda hello reader modul i ett experiment tooread flera filer och använda dem för förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="48655-227">You can use hello reader module in an experiment tooread multiple files, and use them for predictions.</span></span>

<span data-ttu-id="48655-228">När modulen hello läsare i en Azure Machine Learning-experiment, kan du ange Azure Blob som indata.</span><span class="sxs-lookup"><span data-stu-id="48655-228">When using hello reader module in an Azure Machine Learning experiment, you can specify Azure Blob as an input.</span></span> <span data-ttu-id="48655-229">hello-filer i hello Azure blob-lagring kan vara hello utdatafilerna (exempel: 000000_0) som produceras av ett Pig och Hive-skript som körs på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="48655-229">hello files in hello Azure blob storage can be hello output files (Example: 000000_0) that are produced by a Pig and Hive script running on HDInsight.</span></span> <span data-ttu-id="48655-230">hello modul för dataläsare för kan du tooread filer (med inga) genom att konfigurera hello **sökväg toocontainer, katalog-blob**.</span><span class="sxs-lookup"><span data-stu-id="48655-230">hello reader module allows you tooread files (with no extensions) by configuring hello **Path toocontainer, directory/blob**.</span></span> <span data-ttu-id="48655-231">Hej **sökväg toocontainer** punkter toohello behållare och **directory/blob** pekar toofolder som innehåller hello filer enligt följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="48655-231">hello **Path toocontainer** points toohello container and **directory/blob** points toofolder that contains hello files as shown in hello following image.</span></span> <span data-ttu-id="48655-232">hello asterisk som är \*) **anger att alla hello filer i hello behållare/mappen (det vill säga år-aggregateddata-data = månad/2014-6 /\*)** läses som en del av hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-232">hello asterisk that is, \*) **specifies that all hello files in hello container/folder (that is, data/aggregateddata/year=2014/month-6/\*)** are read as part of hello experiment.</span></span>

![Azure Blob-egenskaper](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a><span data-ttu-id="48655-234">Exempel</span><span class="sxs-lookup"><span data-stu-id="48655-234">Example</span></span>
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a><span data-ttu-id="48655-235">Pipeline med AzureMLBatchExecution aktivitet med Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="48655-235">Pipeline with AzureMLBatchExecution activity with Web Service Parameters</span></span>

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

<span data-ttu-id="48655-236">I hello ovan JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="48655-236">In hello above JSON example:</span></span>

* <span data-ttu-id="48655-237">hello distribuerade tjänsten använder en läsare och skrivare modulen tooread/skriva data från Azure Machine Learning Web / tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="48655-237">hello deployed Azure Machine Learning Web service uses a reader and a writer module tooread/write data from/tooan Azure SQL Database.</span></span> <span data-ttu-id="48655-238">Den här webbtjänsten visar hello följande fyra parametrar: databasen servernamnet, databasnamnet, Server-användarkonto och serverlösenord.</span><span class="sxs-lookup"><span data-stu-id="48655-238">This Web service exposes hello following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>  
* <span data-ttu-id="48655-239">Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="48655-239">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="48655-240">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="48655-240">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="48655-241">Hej **end** tid är valfritt.</span><span class="sxs-lookup"><span data-stu-id="48655-241">hello **end** time is optional.</span></span> <span data-ttu-id="48655-242">Om du inte anger värdet för hello **end** egenskapen, det beräknas som ”**start + 48 timmar.**”</span><span class="sxs-lookup"><span data-stu-id="48655-242">If you do not specify value for hello **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="48655-243">toorun hello pipeline på obestämd tid, ange **9999-09-09** som hello värde hello **end** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-243">toorun hello pipeline indefinitely, specify **9999-09-09** as hello value for hello **end** property.</span></span> <span data-ttu-id="48655-244">Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="48655-244">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

### <a name="other-scenarios"></a><span data-ttu-id="48655-245">Andra scenarier</span><span class="sxs-lookup"><span data-stu-id="48655-245">Other scenarios</span></span>
#### <a name="web-service-requires-multiple-inputs"></a><span data-ttu-id="48655-246">Webbtjänsten kräver flera indata</span><span class="sxs-lookup"><span data-stu-id="48655-246">Web service requires multiple inputs</span></span>
<span data-ttu-id="48655-247">Om hello webbtjänst tar flera inmatningar kan använda hello **webServiceInputs** egenskapen istället för att använda **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="48655-247">If hello web service takes multiple inputs, use hello **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="48655-248">Datauppsättningar som refereras av hello **webServiceInputs** måste också tas med i hello aktiviteten **indata**.</span><span class="sxs-lookup"><span data-stu-id="48655-248">Datasets that are referenced by hello **webServiceInputs** must also be included in hello Activity **inputs**.</span></span>

<span data-ttu-id="48655-249">Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar.</span><span class="sxs-lookup"><span data-stu-id="48655-249">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="48655-250">hello-namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha hello namnen i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-250">hello names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match hello names in hello experiments.</span></span> <span data-ttu-id="48655-251">Du kan visa hello begärannyttolast för exempel på hello Batch Execution hjälpsidan för din Azure ML endpoint tooverify hello förväntades mappning.</span><span class="sxs-lookup"><span data-stu-id="48655-251">You can view hello sample request payload on hello Batch Execution Help page for your Azure ML endpoint tooverify hello expected mapping.</span></span>

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a><span data-ttu-id="48655-252">Webbtjänsten kräver inte indata</span><span class="sxs-lookup"><span data-stu-id="48655-252">Web Service does not require an input</span></span>
<span data-ttu-id="48655-253">Azure ML batch execution webbtjänster kan vara används toorun alla arbetsflöden, till exempel R eller Python-skript som kan inte kräva att alla indata.</span><span class="sxs-lookup"><span data-stu-id="48655-253">Azure ML batch execution web services can be used toorun any workflows, for example R or Python scripts, that may not require any inputs.</span></span> <span data-ttu-id="48655-254">Eller hello experiment kan konfigureras med en modul för läsare som inte exponerar någon GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="48655-254">Or, hello experiment might be configured with a Reader module that does not expose any GlobalParameters.</span></span> <span data-ttu-id="48655-255">I så fall ska hello AzureMLBatchExecution aktiviteten konfigureras på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="48655-255">In that case, hello AzureMLBatchExecution Activity would be configured as follows:</span></span>

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a><span data-ttu-id="48655-256">Webbtjänsten kräver inte en in-/ utdata</span><span class="sxs-lookup"><span data-stu-id="48655-256">Web Service does not require an input/output</span></span>
<span data-ttu-id="48655-257">hello Azure ML batch körningstjänsten kanske inte har några utdata för webbtjänst konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="48655-257">hello Azure ML batch execution web service might not have any Web Service output configured.</span></span> <span data-ttu-id="48655-258">Det finns ingen webbtjänsten indata eller utdata eller konfigureras alla GlobalParameters i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="48655-258">In this example, there is no Web Service input or output, nor are any GlobalParameters configured.</span></span> <span data-ttu-id="48655-259">Det finns fortfarande utdata konfigurerats på hello aktivitet sig själv, men anges inte som en webServiceOutput.</span><span class="sxs-lookup"><span data-stu-id="48655-259">There is still an output configured on hello activity itself, but it is not given as a webServiceOutput.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a><span data-ttu-id="48655-260">Web Service använder läsare och skrivare och hello aktiviteten körs endast när andra aktiviteter har lyckades</span><span class="sxs-lookup"><span data-stu-id="48655-260">Web Service uses readers and writers, and hello activity runs only when other activities have succeeded</span></span>
<span data-ttu-id="48655-261">hello Azure ML web service läsare och skrivare moduler kan vara konfigurerade toorun med eller utan någon GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="48655-261">hello Azure ML web service reader and writer modules might be configured toorun with or without any GlobalParameters.</span></span> <span data-ttu-id="48655-262">Du kanske vill tooembed tjänstanrop i en pipeline som använder dataset beroenden tooinvoke hello tjänst endast när vissa överordnade bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="48655-262">However, you may want tooembed service calls in a pipeline that uses dataset dependencies tooinvoke hello service only when some upstream processing has completed.</span></span> <span data-ttu-id="48655-263">Du kan också utlösa någon annan åtgärd när hello batch-körningen har slutförts med den här metoden.</span><span class="sxs-lookup"><span data-stu-id="48655-263">You can also trigger some other action after hello batch execution has completed using this approach.</span></span> <span data-ttu-id="48655-264">I så fall kan du ange hello beroenden som använder aktivitetens indata och utdata, utan att namnge någon av dem som webbtjänsten indata eller utdata.</span><span class="sxs-lookup"><span data-stu-id="48655-264">In that case, you can express hello dependencies using activity inputs and outputs, without naming any of them as Web Service inputs or outputs.</span></span>

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

<span data-ttu-id="48655-265">Hej **takeaways** är:</span><span class="sxs-lookup"><span data-stu-id="48655-265">hello **takeaways** are:</span></span>

* <span data-ttu-id="48655-266">Om slutpunkten experimentet använder en webServiceInput: den representeras av en blobbdatauppsättning och ingår i hello aktivitetens indata- och hello webServiceInput egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-266">If your experiment endpoint uses a webServiceInput: it is represented by a blob dataset and is included in hello activity inputs and hello webServiceInput property.</span></span> <span data-ttu-id="48655-267">Annars utelämnas hello webServiceInput egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-267">Otherwise, hello webServiceInput property is omitted.</span></span>
* <span data-ttu-id="48655-268">Om slutpunkten experimentet använder webServiceOutput(s): de representeras av blob-datauppsättningar och ingår i hello aktivitetsutdata och hello webServiceOutputs egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-268">If your experiment endpoint uses webServiceOutput(s): they are represented by blob datasets and are included in hello activity outputs and in hello webServiceOutputs property.</span></span> <span data-ttu-id="48655-269">hello aktivitet matar ut och webServiceOutputs mappas av hello namn för varje utdata i hello experiment.</span><span class="sxs-lookup"><span data-stu-id="48655-269">hello activity outputs and webServiceOutputs are mapped by hello name of each output in hello experiment.</span></span> <span data-ttu-id="48655-270">Annars utelämnas hello webServiceOutputs egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-270">Otherwise, hello webServiceOutputs property is omitted.</span></span>
* <span data-ttu-id="48655-271">Om din experiment slutpunkt visar globalParameter(s), anges de i hello aktivitetsegenskap globalParameters som nyckel, värde-par.</span><span class="sxs-lookup"><span data-stu-id="48655-271">If your experiment endpoint exposes globalParameter(s), they are given in hello activity globalParameters property as key, value pairs.</span></span> <span data-ttu-id="48655-272">Annars utelämnas hello globalParameters egenskapen.</span><span class="sxs-lookup"><span data-stu-id="48655-272">Otherwise, hello globalParameters property is omitted.</span></span> <span data-ttu-id="48655-273">hello nycklar är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="48655-273">hello keys are case-sensitive.</span></span> <span data-ttu-id="48655-274">[Azure Data Factory-funktioner](data-factory-functions-variables.md) får användas i hello värden.</span><span class="sxs-lookup"><span data-stu-id="48655-274">[Azure Data Factory functions](data-factory-functions-variables.md) may be used in hello values.</span></span>
* <span data-ttu-id="48655-275">Ytterligare datauppsättningar kan ingå i hello in- och utdataenheter Aktivitetsegenskaper, utan som refereras i hello aktiviteten typeProperties.</span><span class="sxs-lookup"><span data-stu-id="48655-275">Additional datasets may be included in hello Activity inputs and outputs properties, without being referenced in hello Activity typeProperties.</span></span> <span data-ttu-id="48655-276">De här datauppsättningarna styr körning med hjälp av sektorn beroenden men ignoreras annars av hello AzureMLBatchExecution aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-276">These datasets govern execution using slice dependencies but are otherwise ignored by hello AzureMLBatchExecution Activity.</span></span>


## <a name="updating-models-using-update-resource-activity"></a><span data-ttu-id="48655-277">Uppdatering av modeller som använder Uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="48655-277">Updating models using Update Resource Activity</span></span>
<span data-ttu-id="48655-278">När du är klar med omtränings uppdatera hello bedömningen webbtjänst (prediktivt experiment visas som en webbtjänst) med hello nyligen tränade modellen med hjälp av hello **Azure ML uppdatera resurs aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="48655-278">After you are done with retraining, update hello scoring web service (predictive experiment exposed as a web service) with hello newly trained model by using hello **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="48655-279">Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="48655-279">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

### <a name="reader-and-writer-modules"></a><span data-ttu-id="48655-280">Läsare och skrivare moduler</span><span class="sxs-lookup"><span data-stu-id="48655-280">Reader and Writer Modules</span></span>
<span data-ttu-id="48655-281">Ett vanligt scenario för att använda webbtjänstparametrar är hello använda Azure SQL-läsare och skrivare.</span><span class="sxs-lookup"><span data-stu-id="48655-281">A common scenario for using Web service parameters is hello use of Azure SQL Readers and Writers.</span></span> <span data-ttu-id="48655-282">modul för dataläsare för hello är används tooload data till ett experiment från data management-tjänster utanför Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="48655-282">hello reader module is used tooload data into an experiment from data management services outside Azure Machine Learning Studio.</span></span> <span data-ttu-id="48655-283">hello-skrivarmodul är toosave data från dina experiment till data management services utanför Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="48655-283">hello writer module is toosave data from your experiments into data management services outside Azure Machine Learning Studio.</span></span>  

<span data-ttu-id="48655-284">Mer information om Azure Blob-/ Azure SQL reader/writer finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library.</span><span class="sxs-lookup"><span data-stu-id="48655-284">For details about Azure Blob/Azure SQL reader/writer, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span> <span data-ttu-id="48655-285">hello exemplet i föregående avsnitt i hello används hello Azure Blob-läsare och Azure Blob-skrivare.</span><span class="sxs-lookup"><span data-stu-id="48655-285">hello example in hello previous section used hello Azure Blob reader and Azure Blob writer.</span></span> <span data-ttu-id="48655-286">Det här avsnittet beskrivs med SQL Azure reader och Azure SQL writer.</span><span class="sxs-lookup"><span data-stu-id="48655-286">This section discusses using Azure SQL reader and Azure SQL writer.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="48655-287">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="48655-287">Frequently asked questions</span></span>
<span data-ttu-id="48655-288">**F:** jag har flera filer som genereras av min stordata pipelines.</span><span class="sxs-lookup"><span data-stu-id="48655-288">**Q:** I have multiple files that are generated by my big data pipelines.</span></span> <span data-ttu-id="48655-289">Kan jag använda hello AzureMLBatchExecution aktiviteten toowork på alla hello-filer?</span><span class="sxs-lookup"><span data-stu-id="48655-289">Can I use hello AzureMLBatchExecution Activity toowork on all hello files?</span></span>

<span data-ttu-id="48655-290">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="48655-290">**A:** Yes.</span></span> <span data-ttu-id="48655-291">Se hello **med hjälp av en läsare modulen tooread data från flera filer i Azure Blob** information.</span><span class="sxs-lookup"><span data-stu-id="48655-291">See hello **Using a Reader module tooread data from multiple files in Azure Blob** section for details.</span></span>

## <a name="azure-ml-batch-scoring-activity"></a><span data-ttu-id="48655-292">Azure ML bedömningen batchaktiviteten</span><span class="sxs-lookup"><span data-stu-id="48655-292">Azure ML Batch Scoring Activity</span></span>
<span data-ttu-id="48655-293">Om du använder hello **AzureMLBatchScoring** aktiviteten toointegrate med Azure Machine Learning, rekommenderar vi att du använder hello senaste **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="48655-293">If you are using hello **AzureMLBatchScoring** activity toointegrate with Azure Machine Learning, we recommend that you use hello latest **AzureMLBatchExecution** activity.</span></span>

<span data-ttu-id="48655-294">Hej AzureMLBatchExecution aktivitet introducerades i hello augusti 2015-versionen av Azure SDK och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="48655-294">hello AzureMLBatchExecution activity is introduced in hello August 2015 release of Azure SDK and Azure PowerShell.</span></span>

<span data-ttu-id="48655-295">Om du vill toocontinue hello AzureMLBatchScoring aktivitet kan fortsätta läsa igenom det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="48655-295">If you want toocontinue using hello AzureMLBatchScoring activity, continue reading through this section.</span></span>  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a><span data-ttu-id="48655-296">Azure ML-Batchbedömningen aktiviteten med Azure Storage för in-/ utdata</span><span class="sxs-lookup"><span data-stu-id="48655-296">Azure ML Batch Scoring activity using Azure Storage for input/output</span></span>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a><span data-ttu-id="48655-297">Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="48655-297">Web Service Parameters</span></span>
<span data-ttu-id="48655-298">toospecify värden för webbtjänstparametrar, lägga till en **typeProperties** avsnittet toohello **AzureMLBatchScoringActivty** avsnitt i pipeline-hello JSON som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="48655-298">toospecify values for Web service parameters, add a **typeProperties** section toohello **AzureMLBatchScoringActivty** section in hello pipeline JSON as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
<span data-ttu-id="48655-299">Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i och ange värden för hello Web Serviceparametrar som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="48655-299">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for hello Web service parameters as shown in hello following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="48655-300">hello webbtjänstparametrar är skiftlägeskänsligt, så se till att hello-namn som du anger i hello aktivitet JSON matchar hello som exponeras av hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="48655-300">hello Web service parameters are case-sensitive, so ensure that hello names you specify in hello activity JSON match hello ones exposed by hello Web service.</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="48655-301">Se även</span><span class="sxs-lookup"><span data-stu-id="48655-301">See Also</span></span>
* [<span data-ttu-id="48655-302">Azure blogginlägg: komma igång med Azure Data Factory och Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="48655-302">Azure blog post: Getting started with Azure Data Factory and Azure Machine Learning</span></span>](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
