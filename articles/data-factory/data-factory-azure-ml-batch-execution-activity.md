---
title: "Skapa förutsägande data pipelines med Azure Data Factory | Microsoft Docs"
description: "Beskriver hur du skapar skapa förutsägande pipelines med hjälp av Azure Data Factory och Azure Machine Learning"
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
ms.openlocfilehash: d8e2c9583fc909e4e015e2d40473d2754529d8ac
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a><span data-ttu-id="b184d-103">Skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b184d-103">Create predictive pipelines using Azure Machine Learning and Azure Data Factory</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="b184d-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="b184d-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="b184d-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="b184d-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="b184d-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="b184d-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="b184d-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="b184d-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="b184d-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="b184d-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="b184d-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="b184d-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="b184d-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

## <a name="introduction"></a><span data-ttu-id="b184d-114">Introduktion</span><span class="sxs-lookup"><span data-stu-id="b184d-114">Introduction</span></span>

### <a name="azure-machine-learning"></a><span data-ttu-id="b184d-115">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b184d-115">Azure Machine Learning</span></span>
<span data-ttu-id="b184d-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) gör det möjligt att bygga, testa och distribuera prediktiva Analyslösningar.</span><span class="sxs-lookup"><span data-stu-id="b184d-116">[Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) enables you to build, test, and deploy predictive analytics solutions.</span></span> <span data-ttu-id="b184d-117">Från en övergripande synsätt görs i tre steg:</span><span class="sxs-lookup"><span data-stu-id="b184d-117">From a high-level point of view, it is done in three steps:</span></span>

1. <span data-ttu-id="b184d-118">**Skapa en träningsexperiment**.</span><span class="sxs-lookup"><span data-stu-id="b184d-118">**Create a training experiment**.</span></span> <span data-ttu-id="b184d-119">Du kan göra det här steget med hjälp av Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="b184d-119">You do this step by using the Azure ML Studio.</span></span> <span data-ttu-id="b184d-120">ML studio är en gemensam visual utvecklingsmiljö som används för att träna och testa en förutsägelseanalysmodell med hjälp av utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="b184d-120">The ML studio is a collaborative visual development environment that you use to train and test a predictive analytics model using training data.</span></span>
2. <span data-ttu-id="b184d-121">**Konvertera den till en prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="b184d-121">**Convert it to a predictive experiment**.</span></span> <span data-ttu-id="b184d-122">När din modell har tränats med befintliga data och du är redo att använda den för att samla in nya data, förbereda och förenkla experimentet för resultatfunktioner.</span><span class="sxs-lookup"><span data-stu-id="b184d-122">Once your model has been trained with existing data and you are ready to use it to score new data, you prepare and streamline your experiment for scoring.</span></span>
3. <span data-ttu-id="b184d-123">**Distribuera den som en webbtjänst**.</span><span class="sxs-lookup"><span data-stu-id="b184d-123">**Deploy it as a web service**.</span></span> <span data-ttu-id="b184d-124">Du kan publicera experimentet bedömningsprofil som Azure-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b184d-124">You can publish your scoring experiment as an Azure web service.</span></span> <span data-ttu-id="b184d-125">Du kan skicka data till din modell via den här slutet för webbtjänst och ta emot resultatet förutsägelser från modellen.</span><span class="sxs-lookup"><span data-stu-id="b184d-125">You can send data to your model via this web service end point and receive result predictions fro the model.</span></span>  

### <a name="azure-data-factory"></a><span data-ttu-id="b184d-126">Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="b184d-126">Azure Data Factory</span></span>
<span data-ttu-id="b184d-127">Data Factory är en molnbaserad dataintegreringstjänst som samordnar och automatiserar **förflyttning** och **transformering** av data.</span><span class="sxs-lookup"><span data-stu-id="b184d-127">Data Factory is a cloud-based data integration service that orchestrates and automates the **movement** and **transformation** of data.</span></span> <span data-ttu-id="b184d-128">Du kan skapa data integration lösningar med hjälp av Azure Data Factory som kan mata in data från olika datakällor, transformera/bearbeta data och publicera Resultatdata till datalager.</span><span class="sxs-lookup"><span data-stu-id="b184d-128">You can create data integration solutions using Azure Data Factory that can ingest data from various data stores, transform/process the data, and publish the result data to the data stores.</span></span>

<span data-ttu-id="b184d-129">Med Data Factory-tjänsten kan du skapa datapipelines som flyttar och transformerar data och kör pipelines enligt ett angivet schema (varje timme, varje dag, varje vecka osv.).</span><span class="sxs-lookup"><span data-stu-id="b184d-129">Data Factory service allows you to create data pipelines that move and transform data, and then run the pipelines on a specified schedule (hourly, daily, weekly, etc.).</span></span> <span data-ttu-id="b184d-130">Den innehåller också omfattande visualiseringar för att visa härkomst och beroenden mellan dina datapipelines och övervaka alla datapipelines från en enda enhetlig vy för att enkelt hitta problem och konfigurera övervakningsaviseringar.</span><span class="sxs-lookup"><span data-stu-id="b184d-130">It also provides rich visualizations to display the lineage and dependencies between your data pipelines, and monitor all your data pipelines from a single unified view to easily pinpoint issues and setup monitoring alerts.</span></span>

<span data-ttu-id="b184d-131">Se [introduktion till Azure Data Factory](data-factory-introduction.md) och [skapa din första pipeline](data-factory-build-your-first-pipeline.md) artiklar för att snabbt komma igång med Azure Data Factory-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b184d-131">See [Introduction to Azure Data Factory](data-factory-introduction.md) and [Build your first pipeline](data-factory-build-your-first-pipeline.md) articles to quickly get started with the Azure Data Factory service.</span></span>

### <a name="data-factory-and-machine-learning-together"></a><span data-ttu-id="b184d-132">Data Factory och Machine Learning tillsammans</span><span class="sxs-lookup"><span data-stu-id="b184d-132">Data Factory and Machine Learning together</span></span>
<span data-ttu-id="b184d-133">Azure Data Factory kan du enkelt skapa pipelines som använder en publicerade [Azure Machine Learning] [ azure-machine-learning] webbtjänsten för förutsägelseanalys.</span><span class="sxs-lookup"><span data-stu-id="b184d-133">Azure Data Factory enables you to easily create pipelines that use a published [Azure Machine Learning][azure-machine-learning] web service for predictive analytics.</span></span> <span data-ttu-id="b184d-134">Med hjälp av den **Batchkörningsaktivitet** i Azure Data Factory-pipelinen, du kan anropa en Azure ML-webbtjänst för att göra förutsägelser på data i batch.</span><span class="sxs-lookup"><span data-stu-id="b184d-134">Using the **Batch Execution Activity** in an Azure Data Factory pipeline, you can invoke an Azure ML web service to make predictions on the data in batch.</span></span> <span data-ttu-id="b184d-135">Se [anropa en Azure ML-webbtjänst med hjälp av den Batchkörningsaktivitet](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) information.</span><span class="sxs-lookup"><span data-stu-id="b184d-135">See [Invoking an Azure ML web service using the Batch Execution Activity](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) section for details.</span></span>

<span data-ttu-id="b184d-136">Förutsägelsemodeller i Azure ML bedömningen experiment måste vara retrained med nya indatauppsättningar med tiden.</span><span class="sxs-lookup"><span data-stu-id="b184d-136">Over time, the predictive models in the Azure ML scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="b184d-137">Du kan träna om Azure ML-modell från Data Factory-pipelinen genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b184d-137">You can retrain an Azure ML model from a Data Factory pipeline by doing the following steps:</span></span>

1. <span data-ttu-id="b184d-138">Publicera träningsexperiment (inte prediktivt experiment) som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b184d-138">Publish the training experiment (not predictive experiment) as a web service.</span></span> <span data-ttu-id="b184d-139">Du kan göra det här steget i Azure ML Studio som du gjorde för att exponera prediktivt experiment som en webbtjänst i scenariot ovan.</span><span class="sxs-lookup"><span data-stu-id="b184d-139">You do this step in the Azure ML Studio as you did to expose predictive experiment as a web service in the previous scenario.</span></span>
2. <span data-ttu-id="b184d-140">Använda Azure ML-Batchkörningsaktivitet för att anropa webbtjänsten för utbildning experimentet.</span><span class="sxs-lookup"><span data-stu-id="b184d-140">Use the Azure ML Batch Execution Activity to invoke the web service for the training experiment.</span></span> <span data-ttu-id="b184d-141">I princip kan du använda Azure ML-batchkörning aktiviteten anropa både utbildning webbtjänsten och webbtjänsten för bedömningsprofil.</span><span class="sxs-lookup"><span data-stu-id="b184d-141">Basically, you can use the Azure ML Batch Execution activity to invoke both training web service and scoring web service.</span></span>

<span data-ttu-id="b184d-142">När du är klar med omtränings uppdatera bedömningsprofil webbtjänsten (prediktivt experiment visas som en webbtjänst) med den nyligen tränade modellen med hjälp av den **Azure ML uppdatera resurs aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="b184d-142">After you are done with retraining, update the scoring web service (predictive experiment exposed as a web service) with the newly trained model by using the **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="b184d-143">Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="b184d-143">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

## <a name="invoking-a-web-service-using-batch-execution-activity"></a><span data-ttu-id="b184d-144">Anropa en webbtjänst med hjälp av Batchkörningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-144">Invoking a web service using Batch Execution Activity</span></span>
<span data-ttu-id="b184d-145">Du använder Azure Data Factory för att samordna dataförflyttning och bearbetning och sedan utför med hjälp av Azure Machine Learning-batchkörning.</span><span class="sxs-lookup"><span data-stu-id="b184d-145">You use Azure Data Factory to orchestrate data movement and processing, and then perform batch execution using Azure Machine Learning.</span></span> <span data-ttu-id="b184d-146">Här är de högsta nivån steg:</span><span class="sxs-lookup"><span data-stu-id="b184d-146">Here are the top-level steps:</span></span>

1. <span data-ttu-id="b184d-147">Skapa en Azure Machine Learning länkad tjänst.</span><span class="sxs-lookup"><span data-stu-id="b184d-147">Create an Azure Machine Learning linked service.</span></span> <span data-ttu-id="b184d-148">Behöver du följande värden:</span><span class="sxs-lookup"><span data-stu-id="b184d-148">You need the following values:</span></span>

   1. <span data-ttu-id="b184d-149">**Begärande-URI** för batchkörning API.</span><span class="sxs-lookup"><span data-stu-id="b184d-149">**Request URI** for the Batch Execution API.</span></span> <span data-ttu-id="b184d-150">Du kan hitta begärd URI genom att klicka på den **BATCH EXECUTION** länken i sidan tjänster.</span><span class="sxs-lookup"><span data-stu-id="b184d-150">You can find the Request URI by clicking the **BATCH EXECUTION** link in the web services page.</span></span>
   2. <span data-ttu-id="b184d-151">**API-nyckel** publicerade Azure Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b184d-151">**API key** for the published Azure Machine Learning web service.</span></span> <span data-ttu-id="b184d-152">API-nyckeln hittar du genom att klicka på den webbtjänst som du har publicerat.</span><span class="sxs-lookup"><span data-stu-id="b184d-152">You can find the API key by clicking the web service that you have published.</span></span>
   3. <span data-ttu-id="b184d-153">Använd den **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b184d-153">Use the **AzureMLBatchExecution** activity.</span></span>

      ![Machine Learning-instrumentpanelen](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Batch URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-to-data-in-azure-blob-storage"></a><span data-ttu-id="b184d-156">Scenario: Försök med hjälp av Web service indata/utdata som refererar till data i Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="b184d-156">Scenario: Experiments using Web service inputs/outputs that refer to data in Azure Blob Storage</span></span>
<span data-ttu-id="b184d-157">I det här scenariot Azure Machine Learning-webbtjänsten gör förutsägelser med hjälp av data från en fil i ett Azure blob storage och lagrar förutsägelse resultaten i blob storage.</span><span class="sxs-lookup"><span data-stu-id="b184d-157">In this scenario, the Azure Machine Learning Web service makes predictions using data from a file in an Azure blob storage and stores the prediction results in the blob storage.</span></span> <span data-ttu-id="b184d-158">Följande JSON definierar Data Factory-pipelinen med en AzureMLBatchExecution aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b184d-158">The following JSON defines a Data Factory pipeline with an AzureMLBatchExecution activity.</span></span> <span data-ttu-id="b184d-159">Aktiviteten har datauppsättningen **DecisionTreeInputBlob** som indata och **DecisionTreeResultBlob** som utdata.</span><span class="sxs-lookup"><span data-stu-id="b184d-159">The activity has the dataset **DecisionTreeInputBlob** as input and **DecisionTreeResultBlob** as the output.</span></span> <span data-ttu-id="b184d-160">Den **DecisionTreeInputBlob** skickas som indata till webbtjänsten genom att använda den **webServiceInput** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b184d-160">The **DecisionTreeInputBlob** is passed as an input to the web service by using the **webServiceInput** JSON property.</span></span> <span data-ttu-id="b184d-161">Den **DecisionTreeResultBlob** skickas som utdata till webbtjänsten genom att använda den **webServiceOutputs** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b184d-161">The **DecisionTreeResultBlob** is passed as an output to the Web service by using the **webServiceOutputs** JSON property.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="b184d-162">Om webbtjänsten tar flera inmatningar kan använda den **webServiceInputs** egenskapen istället för att använda **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="b184d-162">If the web service takes multiple inputs, use the **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="b184d-163">Finns det [webbtjänst kräver flera indata](#web-service-requires-multiple-inputs) avsnittet ett exempel på hur du använder egenskapen webServiceInputs.</span><span class="sxs-lookup"><span data-stu-id="b184d-163">See the [Web service requires multiple inputs](#web-service-requires-multiple-inputs) section for an example of using the webServiceInputs property.</span></span>
>
> <span data-ttu-id="b184d-164">Datauppsättningar som refererar till den **webServiceInput**/**webServiceInputs** och **webServiceOutputs** egenskaper (i **typeProperties**) måste också tas med i aktiviteten **indata** och **matar ut**.</span><span class="sxs-lookup"><span data-stu-id="b184d-164">Datasets that are referenced by the **webServiceInput**/**webServiceInputs** and **webServiceOutputs** properties (in **typeProperties**) must also be included in the Activity **inputs** and **outputs**.</span></span>
>
> <span data-ttu-id="b184d-165">Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar.</span><span class="sxs-lookup"><span data-stu-id="b184d-165">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="b184d-166">De namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha namnen i experiment.</span><span class="sxs-lookup"><span data-stu-id="b184d-166">The names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match the names in the experiments.</span></span> <span data-ttu-id="b184d-167">Du kan visa nyttolasten i begäran av exemplet på Batch Execution sidan för din Azure ML-slutpunkt att verifiera förväntade mappningen.</span><span class="sxs-lookup"><span data-stu-id="b184d-167">You can view the sample request payload on the Batch Execution Help page for your Azure ML endpoint to verify the expected mapping.</span></span>
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
> <span data-ttu-id="b184d-168">Endast indata och utdata för aktiviteten AzureMLBatchExecution kan skickas som parametrar till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b184d-168">Only inputs and outputs of the AzureMLBatchExecution activity can be passed as parameters to the Web service.</span></span> <span data-ttu-id="b184d-169">Ovanstående JSON-kodutdrag är till exempel DecisionTreeInputBlob indata till aktiviteten AzureMLBatchExecution som skickas som indata till webbtjänsten via webServiceInput-parametern.</span><span class="sxs-lookup"><span data-stu-id="b184d-169">For example, in the above JSON snippet, DecisionTreeInputBlob is an input to the AzureMLBatchExecution activity, which is passed as an input to the Web service via webServiceInput parameter.</span></span>   
>
>

### <a name="example"></a><span data-ttu-id="b184d-170">Exempel</span><span class="sxs-lookup"><span data-stu-id="b184d-170">Example</span></span>
<span data-ttu-id="b184d-171">Det här exemplet använder Azure Storage för att lagra både inkommande och utgående data.</span><span class="sxs-lookup"><span data-stu-id="b184d-171">This example uses Azure Storage to hold both the input and output data.</span></span>

<span data-ttu-id="b184d-172">Vi rekommenderar att du går igenom den [skapa din första pipeline med Data Factory] [ adf-build-1st-pipeline] självstudiekursen innan du fortsätter med det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b184d-172">We recommend that you go through the [Build your first pipeline with Data Factory][adf-build-1st-pipeline] tutorial before going through this example.</span></span> <span data-ttu-id="b184d-173">Använd Data Factory-Redigeraren för att skapa de artefakter som Data Factory (länkade tjänster, datauppsättningar, rörledningar) i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b184d-173">Use the Data Factory Editor to create Data Factory artifacts (linked services, datasets, pipeline) in this example.</span></span>   

1. <span data-ttu-id="b184d-174">Skapa en **länkade tjänsten** för din **Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="b184d-174">Create a **linked service** for your **Azure Storage**.</span></span> <span data-ttu-id="b184d-175">Om inkommande och utgående filer finns i olika storage-konton, måste två länkade tjänster.</span><span class="sxs-lookup"><span data-stu-id="b184d-175">If the input and output files are in different storage accounts, you need two linked services.</span></span> <span data-ttu-id="b184d-176">Här är en JSON-exempel:</span><span class="sxs-lookup"><span data-stu-id="b184d-176">Here is a JSON example:</span></span>

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
2. <span data-ttu-id="b184d-177">Skapa den **inkommande** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="b184d-177">Create the **input** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="b184d-178">Till skillnad från vissa andra Data Factory datauppsättningar, måste dessa data inte innehålla både **folderPath** och **fileName** värden.</span><span class="sxs-lookup"><span data-stu-id="b184d-178">Unlike some other Data Factory datasets, these datasets must contain both **folderPath** and **fileName** values.</span></span> <span data-ttu-id="b184d-179">Du kan använda partitionering för att varje batchkörning (varje datasektorn) att bearbeta eller skapa unika inkommande och utgående filer.</span><span class="sxs-lookup"><span data-stu-id="b184d-179">You can use partitioning to cause each batch execution (each data slice) to process or produce unique input and output files.</span></span> <span data-ttu-id="b184d-180">Du kan behöva ta någon överordnad aktivitet om du vill omvandla indata till CSV-filformatet och placera det i lagringskontot för varje segment.</span><span class="sxs-lookup"><span data-stu-id="b184d-180">You may need to include some upstream activity to transform the input into the CSV file format and place it in the storage account for each slice.</span></span> <span data-ttu-id="b184d-181">I så fall kan du inte innehåller den **externa** och **externalData** inställningar som visas i följande exempel och din DecisionTreeInputBlob skulle vara datamängd för utdata för en annan aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b184d-181">In that case, you would not include the **external** and **externalData** settings shown in the following example, and your DecisionTreeInputBlob would be the output dataset of a different Activity.</span></span>

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

    <span data-ttu-id="b184d-182">Inkommande csv-filen måste ha rubrikraden kolumn.</span><span class="sxs-lookup"><span data-stu-id="b184d-182">Your input csv file must have the column header row.</span></span> <span data-ttu-id="b184d-183">Om du använder den **Kopieringsaktiviteten** för att skapa/flytta CSV-filen till blob-lagring, bör du ställa in egenskapen sink **blobWriterAddHeader** till **SANT**.</span><span class="sxs-lookup"><span data-stu-id="b184d-183">If you are using the **Copy Activity** to create/move the csv into the blob storage, you should set the sink property **blobWriterAddHeader** to **true**.</span></span> <span data-ttu-id="b184d-184">Exempel:</span><span class="sxs-lookup"><span data-stu-id="b184d-184">For example:</span></span>

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    <span data-ttu-id="b184d-185">Om csv-filen saknar rubrikraden, kan du se följande fel: **fel i aktiviteten: fel vid läsning av strängen. Oväntad token: StartObject. Sökvägen '', rad 1, position 1**.</span><span class="sxs-lookup"><span data-stu-id="b184d-185">If the csv file does not have the header row, you may see the following error: **Error in Activity: Error reading string. Unexpected token: StartObject. Path '', line 1, position 1**.</span></span>
3. <span data-ttu-id="b184d-186">Skapa den **utdata** Azure Data Factory **dataset**.</span><span class="sxs-lookup"><span data-stu-id="b184d-186">Create the **output** Azure Data Factory **dataset**.</span></span> <span data-ttu-id="b184d-187">Det här exemplet använder partitionering för att skapa en unik Utdatasökväg för varje segment-körning.</span><span class="sxs-lookup"><span data-stu-id="b184d-187">This example uses partitioning to create a unique output path for each slice execution.</span></span> <span data-ttu-id="b184d-188">Aktiviteten skulle utan partitionering, skriva över filen.</span><span class="sxs-lookup"><span data-stu-id="b184d-188">Without the partitioning, the activity would overwrite the file.</span></span>

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
4. <span data-ttu-id="b184d-189">Skapa en **länkade tjänsten** av typen: **AzureMLLinkedService**, tillhandahålla API-nyckeln och modellen batch execution URL.</span><span class="sxs-lookup"><span data-stu-id="b184d-189">Create a **linked service** of type: **AzureMLLinkedService**, providing the API key and model batch execution URL.</span></span>

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
5. <span data-ttu-id="b184d-190">Slutligen skapar en pipeline som innehåller en **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b184d-190">Finally, author a pipeline containing an **AzureMLBatchExecution** Activity.</span></span> <span data-ttu-id="b184d-191">Vid körning utför pipeline följande steg:</span><span class="sxs-lookup"><span data-stu-id="b184d-191">At runtime, pipeline performs the following steps:</span></span>

   1. <span data-ttu-id="b184d-192">Hämtar platsen för indatafilen från dina indata datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="b184d-192">Gets the location of the input file from your input datasets.</span></span>
   2. <span data-ttu-id="b184d-193">Anropar batchkörning Azure Machine Learning API</span><span class="sxs-lookup"><span data-stu-id="b184d-193">Invokes the Azure Machine Learning batch execution API</span></span>
   3. <span data-ttu-id="b184d-194">Kopierar batch utförande-utdatan till blob som anges i datamängd för utdata.</span><span class="sxs-lookup"><span data-stu-id="b184d-194">Copies the batch execution output to the blob given in your output dataset.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b184d-195">AzureMLBatchExecution aktivitet kan ha noll eller flera in- och utdata för en eller flera.</span><span class="sxs-lookup"><span data-stu-id="b184d-195">AzureMLBatchExecution activity can have zero or more inputs and one or more outputs.</span></span>
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

      <span data-ttu-id="b184d-196">Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="b184d-196">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="b184d-197">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="b184d-197">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="b184d-198">Den **end** tid är valfritt.</span><span class="sxs-lookup"><span data-stu-id="b184d-198">The **end** time is optional.</span></span> <span data-ttu-id="b184d-199">Om du inte anger värdet för den **end** egenskapen, det beräknas som ”**start + 48 timmar.**”</span><span class="sxs-lookup"><span data-stu-id="b184d-199">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="b184d-200">Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b184d-200">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span> <span data-ttu-id="b184d-201">Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b184d-201">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

      > [!NOTE]
      > <span data-ttu-id="b184d-202">Ange indata för AzureMLBatchExecution är aktiviteten valfria.</span><span class="sxs-lookup"><span data-stu-id="b184d-202">Specifying input for the AzureMLBatchExecution activity is optional.</span></span>
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-to-refer-to-data-in-various-storages"></a><span data-ttu-id="b184d-203">Scenario: Försök med Reader/Writer-moduler för att referera till data i olika lagringsobjekt</span><span class="sxs-lookup"><span data-stu-id="b184d-203">Scenario: Experiments using Reader/Writer Modules to refer to data in various storages</span></span>
<span data-ttu-id="b184d-204">Ett annat vanligt scenario när du skapar Azure ML experiment är att använda läsare och skrivare moduler.</span><span class="sxs-lookup"><span data-stu-id="b184d-204">Another common scenario when creating Azure ML experiments is to use Reader and Writer modules.</span></span> <span data-ttu-id="b184d-205">Modul för dataläsare används för att läsa in data i ett experiment och modulen skrivaren är att spara data från dina experiment.</span><span class="sxs-lookup"><span data-stu-id="b184d-205">The reader module is used to load data into an experiment and the writer module is to save data from your experiments.</span></span> <span data-ttu-id="b184d-206">Mer information om läsare och skrivare finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library.</span><span class="sxs-lookup"><span data-stu-id="b184d-206">For details about reader and writer modules, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span>     

<span data-ttu-id="b184d-207">När du använder modulerna som läsare och skrivare, är bra idé att använda en Web service-parameter för varje egenskap för dessa moduler reader/writer.</span><span class="sxs-lookup"><span data-stu-id="b184d-207">When using the reader and writer modules, it is good practice to use a Web service parameter for each property of these reader/writer modules.</span></span> <span data-ttu-id="b184d-208">Dessa webb-parametrar kan du konfigurera värden under körningen.</span><span class="sxs-lookup"><span data-stu-id="b184d-208">These web parameters enable you to configure the values during runtime.</span></span> <span data-ttu-id="b184d-209">Du kan till exempel skapa ett experiment med en modul för läsare som använder en Azure SQL Database: XXX.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="b184d-209">For example, you could create an experiment with a reader module that uses an Azure SQL Database: XXX.database.windows.net.</span></span> <span data-ttu-id="b184d-210">När webbtjänsten har distribuerats, som du vill aktivera användare för webbtjänsten att ange en annan Azure SQL-Server som kallas YYY.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="b184d-210">After the web service has been deployed, you want to enable the consumers of the web service to specify another Azure SQL Server called YYY.database.windows.net.</span></span> <span data-ttu-id="b184d-211">Du kan använda en Web service-parametern så att det här värdet som ska konfigureras.</span><span class="sxs-lookup"><span data-stu-id="b184d-211">You can use a Web service parameter to allow this value to be configured.</span></span>

> [!NOTE]
> <span data-ttu-id="b184d-212">Webbtjänst och utdata skiljer sig från webbtjänstparametrar.</span><span class="sxs-lookup"><span data-stu-id="b184d-212">Web service input and output are different from Web service parameters.</span></span> <span data-ttu-id="b184d-213">I det första scenariot har du sett hur indata och utdata kan anges för en Azure ML-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="b184d-213">In the first scenario, you have seen how an input and output can be specified for an Azure ML Web service.</span></span> <span data-ttu-id="b184d-214">I det här scenariot skicka parametrar för en webbtjänst som är kopplade till egenskaperna för läsare/skrivare moduler.</span><span class="sxs-lookup"><span data-stu-id="b184d-214">In this scenario, you pass parameters for a Web service that correspond to properties of reader/writer modules.</span></span>
>
>

<span data-ttu-id="b184d-215">Nu ska vi titta på ett scenario för att använda webbtjänstparametrar.</span><span class="sxs-lookup"><span data-stu-id="b184d-215">Let's look at a scenario for using Web service parameters.</span></span> <span data-ttu-id="b184d-216">Du har en distribuerad Azure Machine Learning-webbtjänst som använder en modul för dataläsare för att läsa data från en av de datakällor som stöds av Azure Machine Learning (till exempel: Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="b184d-216">You have a deployed Azure Machine Learning web service that uses a reader module to read data from one of the data sources supported by Azure Machine Learning (for example: Azure SQL Database).</span></span> <span data-ttu-id="b184d-217">När batch-körningen har utförts skrivs resultaten med skrivarmodul (Azure SQL Database).</span><span class="sxs-lookup"><span data-stu-id="b184d-217">After the batch execution is performed, the results are written using a Writer module (Azure SQL Database).</span></span>  <span data-ttu-id="b184d-218">Ingen service indata och utdata har definierats i experiment.</span><span class="sxs-lookup"><span data-stu-id="b184d-218">No web service inputs and outputs are defined in the experiments.</span></span> <span data-ttu-id="b184d-219">I detta fall rekommenderar vi att du konfigurerar relevanta webbtjänstparametrar för läsare och skrivare moduler.</span><span class="sxs-lookup"><span data-stu-id="b184d-219">In this case, we recommend that you configure relevant web service parameters for the reader and writer modules.</span></span> <span data-ttu-id="b184d-220">Den här konfigurationen kan du reader/writer konfigureras när du använder aktiviteten AzureMLBatchExecution-moduler.</span><span class="sxs-lookup"><span data-stu-id="b184d-220">This configuration allows the reader/writer modules to be configured when using the AzureMLBatchExecution activity.</span></span> <span data-ttu-id="b184d-221">Du anger webbtjänstparametrar i den **globalParameters** avsnittet i JSON-aktivitet på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="b184d-221">You specify Web service parameters in the **globalParameters** section in the activity JSON as follows.</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

<span data-ttu-id="b184d-222">Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i skicka värden för Web service parametrar som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b184d-222">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for the Web service parameters as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="b184d-223">Web service-parametrar är skiftlägeskänsliga, så se till att de namn som du anger i aktiviteten JSON matchar de som visas av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b184d-223">The Web service parameters are case-sensitive, so ensure that the names you specify in the activity JSON match the ones exposed by the Web service.</span></span>
>
>

### <a name="using-a-reader-module-to-read-data-from-multiple-files-in-azure-blob"></a><span data-ttu-id="b184d-224">Med hjälp av en modul för dataläsare för att läsa data från flera filer i Azure Blob</span><span class="sxs-lookup"><span data-stu-id="b184d-224">Using a Reader module to read data from multiple files in Azure Blob</span></span>
<span data-ttu-id="b184d-225">Stordata rörledningar med aktiviteter, till exempel Pig och Hive kan ge en eller flera utgående filer utan filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="b184d-225">Big data pipelines with activities such as Pig and Hive can produce one or more output files with no extensions.</span></span> <span data-ttu-id="b184d-226">När du anger en extern Hive-tabell, kan data för den externa Hive-tabellen lagras i Azure blob storage med följande namn 000000_0.</span><span class="sxs-lookup"><span data-stu-id="b184d-226">For example, when you specify an external Hive table, the data for the external Hive table can be stored in Azure blob storage with the following name 000000_0.</span></span> <span data-ttu-id="b184d-227">Du kan använda modulen läsare i ett experiment för att läsa flera filer och använda dem för förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="b184d-227">You can use the reader module in an experiment to read multiple files, and use them for predictions.</span></span>

<span data-ttu-id="b184d-228">När du använder modulen läsare i en Azure Machine Learning-experiment kan ange du Azure Blob som indata.</span><span class="sxs-lookup"><span data-stu-id="b184d-228">When using the reader module in an Azure Machine Learning experiment, you can specify Azure Blob as an input.</span></span> <span data-ttu-id="b184d-229">Filerna i Azure blob storage kan vara utdatafilerna (exempel: 000000_0) som produceras av ett Pig och Hive-skript som körs på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="b184d-229">The files in the Azure blob storage can be the output files (Example: 000000_0) that are produced by a Pig and Hive script running on HDInsight.</span></span> <span data-ttu-id="b184d-230">Modul för dataläsare kan du läsa filer (med inga) genom att konfigurera den **sökvägen till behållaren directory/blob**.</span><span class="sxs-lookup"><span data-stu-id="b184d-230">The reader module allows you to read files (with no extensions) by configuring the **Path to container, directory/blob**.</span></span> <span data-ttu-id="b184d-231">Den **sökvägen till behållaren** pekar på behållaren och **directory/blob** pekar på mappen som innehåller filerna som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="b184d-231">The **Path to container** points to the container and **directory/blob** points to folder that contains the files as shown in the following image.</span></span> <span data-ttu-id="b184d-232">Asterisken som är \*) **anger att alla filer i mappen/behållare (det vill säga år-aggregateddata-data = månad/2014-6 /\*)** läses som en del av experimentet.</span><span class="sxs-lookup"><span data-stu-id="b184d-232">The asterisk that is, \*) **specifies that all the files in the container/folder (that is, data/aggregateddata/year=2014/month-6/\*)** are read as part of the experiment.</span></span>

![Azure Blob-egenskaper](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a><span data-ttu-id="b184d-234">Exempel</span><span class="sxs-lookup"><span data-stu-id="b184d-234">Example</span></span>
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a><span data-ttu-id="b184d-235">Pipeline med AzureMLBatchExecution aktivitet med Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="b184d-235">Pipeline with AzureMLBatchExecution activity with Web Service Parameters</span></span>

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

<span data-ttu-id="b184d-236">I JSON-exemplet ovan:</span><span class="sxs-lookup"><span data-stu-id="b184d-236">In the above JSON example:</span></span>

* <span data-ttu-id="b184d-237">Distribuerade Azure Machine Learning webbtjänsten använder en läsare och skrivare-modulen för att läsa/skriva data från/till en Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b184d-237">The deployed Azure Machine Learning Web service uses a reader and a writer module to read/write data from/to an Azure SQL Database.</span></span> <span data-ttu-id="b184d-238">Den här webbtjänsten visar följande fyra parametrar: databasen servernamnet, databasnamnet, Server-användarkonto och serverlösenord.</span><span class="sxs-lookup"><span data-stu-id="b184d-238">This Web service exposes the following four parameters:  Database server name, Database name, Server user account name, and Server user account password.</span></span>  
* <span data-ttu-id="b184d-239">Båda **starta** och **end** datum och tid måste vara i [ISO-format](http://en.wikipedia.org/wiki/ISO_8601).</span><span class="sxs-lookup"><span data-stu-id="b184d-239">Both **start** and **end** datetimes must be in [ISO format](http://en.wikipedia.org/wiki/ISO_8601).</span></span> <span data-ttu-id="b184d-240">Exempel: 2014-10-14T16:32:41Z.</span><span class="sxs-lookup"><span data-stu-id="b184d-240">For example: 2014-10-14T16:32:41Z.</span></span> <span data-ttu-id="b184d-241">Den **end** tid är valfritt.</span><span class="sxs-lookup"><span data-stu-id="b184d-241">The **end** time is optional.</span></span> <span data-ttu-id="b184d-242">Om du inte anger värdet för den **end** egenskapen, det beräknas som ”**start + 48 timmar.**”</span><span class="sxs-lookup"><span data-stu-id="b184d-242">If you do not specify value for the **end** property, it is calculated as "**start + 48 hours.**"</span></span> <span data-ttu-id="b184d-243">Om du vill köra pipelinen på obestämd tid, anger du **9999-09-09** som värde för **slut**egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b184d-243">To run the pipeline indefinitely, specify **9999-09-09** as the value for the **end** property.</span></span> <span data-ttu-id="b184d-244">Se [Referens för JSON-skript](https://msdn.microsoft.com/library/dn835050.aspx) för information om JSON-egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b184d-244">See [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) for details about JSON properties.</span></span>

### <a name="other-scenarios"></a><span data-ttu-id="b184d-245">Andra scenarier</span><span class="sxs-lookup"><span data-stu-id="b184d-245">Other scenarios</span></span>
#### <a name="web-service-requires-multiple-inputs"></a><span data-ttu-id="b184d-246">Webbtjänsten kräver flera indata</span><span class="sxs-lookup"><span data-stu-id="b184d-246">Web service requires multiple inputs</span></span>
<span data-ttu-id="b184d-247">Om webbtjänsten tar flera inmatningar kan använda den **webServiceInputs** egenskapen istället för att använda **webServiceInput**.</span><span class="sxs-lookup"><span data-stu-id="b184d-247">If the web service takes multiple inputs, use the **webServiceInputs** property instead of using **webServiceInput**.</span></span> <span data-ttu-id="b184d-248">Datauppsättningar som refererar till den **webServiceInputs** måste också tas med i aktiviteten **indata**.</span><span class="sxs-lookup"><span data-stu-id="b184d-248">Datasets that are referenced by the **webServiceInputs** must also be included in the Activity **inputs**.</span></span>

<span data-ttu-id="b184d-249">Ha standardnamnen (”input1”, ”input2”) som du kan anpassa i experimentet Azure ML webbtjänst och portar och globala parametrar.</span><span class="sxs-lookup"><span data-stu-id="b184d-249">In your Azure ML experiment, web service input and output ports and global parameters have default names ("input1", "input2") that you can customize.</span></span> <span data-ttu-id="b184d-250">De namn som du använder för webServiceInputs, webServiceOutputs och globalParameters inställningar måste exakt matcha namnen i experiment.</span><span class="sxs-lookup"><span data-stu-id="b184d-250">The names you use for webServiceInputs, webServiceOutputs, and globalParameters settings must exactly match the names in the experiments.</span></span> <span data-ttu-id="b184d-251">Du kan visa nyttolasten i begäran av exemplet på Batch Execution sidan för din Azure ML-slutpunkt att verifiera förväntade mappningen.</span><span class="sxs-lookup"><span data-stu-id="b184d-251">You can view the sample request payload on the Batch Execution Help page for your Azure ML endpoint to verify the expected mapping.</span></span>

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

#### <a name="web-service-does-not-require-an-input"></a><span data-ttu-id="b184d-252">Webbtjänsten kräver inte indata</span><span class="sxs-lookup"><span data-stu-id="b184d-252">Web Service does not require an input</span></span>
<span data-ttu-id="b184d-253">Azure ML batch execution webbtjänster kan användas för att köra några arbetsflöden för R eller Python exempelskript, som inte kräver några indata.</span><span class="sxs-lookup"><span data-stu-id="b184d-253">Azure ML batch execution web services can be used to run any workflows, for example R or Python scripts, that may not require any inputs.</span></span> <span data-ttu-id="b184d-254">Eller experimentet kan konfigureras med en modul för läsare som inte exponerar någon GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="b184d-254">Or, the experiment might be configured with a Reader module that does not expose any GlobalParameters.</span></span> <span data-ttu-id="b184d-255">I så fall skulle aktiviteten AzureMLBatchExecution konfigureras på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="b184d-255">In that case, the AzureMLBatchExecution Activity would be configured as follows:</span></span>

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

#### <a name="web-service-does-not-require-an-inputoutput"></a><span data-ttu-id="b184d-256">Webbtjänsten kräver inte en in-/ utdata</span><span class="sxs-lookup"><span data-stu-id="b184d-256">Web Service does not require an input/output</span></span>
<span data-ttu-id="b184d-257">Azure ML batch execution webbtjänsten kanske inte har några utdata för webbtjänst konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="b184d-257">The Azure ML batch execution web service might not have any Web Service output configured.</span></span> <span data-ttu-id="b184d-258">Det finns ingen webbtjänsten indata eller utdata eller konfigureras alla GlobalParameters i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="b184d-258">In this example, there is no Web Service input or output, nor are any GlobalParameters configured.</span></span> <span data-ttu-id="b184d-259">Det finns fortfarande utdata konfigurerats på aktiviteten sig själv, men anges inte som en webServiceOutput.</span><span class="sxs-lookup"><span data-stu-id="b184d-259">There is still an output configured on the activity itself, but it is not given as a webServiceOutput.</span></span>

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

#### <a name="web-service-uses-readers-and-writers-and-the-activity-runs-only-when-other-activities-have-succeeded"></a><span data-ttu-id="b184d-260">Web Service använder läsare och skrivare och aktiviteten körs endast när andra aktiviteter har lyckades</span><span class="sxs-lookup"><span data-stu-id="b184d-260">Web Service uses readers and writers, and the activity runs only when other activities have succeeded</span></span>
<span data-ttu-id="b184d-261">Azure ML web service läsare och skrivare moduler kan konfigureras för körning med eller utan någon GlobalParameters.</span><span class="sxs-lookup"><span data-stu-id="b184d-261">The Azure ML web service reader and writer modules might be configured to run with or without any GlobalParameters.</span></span> <span data-ttu-id="b184d-262">Du kanske vill bädda in tjänstanrop i en pipeline som använder dataset beroenden för att anropa tjänsten endast när vissa överordnade bearbetningen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b184d-262">However, you may want to embed service calls in a pipeline that uses dataset dependencies to invoke the service only when some upstream processing has completed.</span></span> <span data-ttu-id="b184d-263">Du kan också utlösa någon annan åtgärd när batch-körningen har slutförts med den här metoden.</span><span class="sxs-lookup"><span data-stu-id="b184d-263">You can also trigger some other action after the batch execution has completed using this approach.</span></span> <span data-ttu-id="b184d-264">I så fall kan du ange de beroenden som använder aktivitetens indata och utdata, utan att namnge någon av dem som webbtjänsten indata eller utdata.</span><span class="sxs-lookup"><span data-stu-id="b184d-264">In that case, you can express the dependencies using activity inputs and outputs, without naming any of them as Web Service inputs or outputs.</span></span>

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

<span data-ttu-id="b184d-265">Den **takeaways** är:</span><span class="sxs-lookup"><span data-stu-id="b184d-265">The **takeaways** are:</span></span>

* <span data-ttu-id="b184d-266">Om slutpunkten experimentet använder en webServiceInput: den representeras av en blobbdatauppsättning och ingår i aktivitetens indata- och egenskapen webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="b184d-266">If your experiment endpoint uses a webServiceInput: it is represented by a blob dataset and is included in the activity inputs and the webServiceInput property.</span></span> <span data-ttu-id="b184d-267">Annars utelämnas egenskapen webServiceInput.</span><span class="sxs-lookup"><span data-stu-id="b184d-267">Otherwise, the webServiceInput property is omitted.</span></span>
* <span data-ttu-id="b184d-268">Om slutpunkten experimentet använder webServiceOutput(s): de representeras av blob-datauppsättningar och ingår i aktivitetsutdata och i egenskapen webServiceOutputs.</span><span class="sxs-lookup"><span data-stu-id="b184d-268">If your experiment endpoint uses webServiceOutput(s): they are represented by blob datasets and are included in the activity outputs and in the webServiceOutputs property.</span></span> <span data-ttu-id="b184d-269">Aktiviteten matar ut och webServiceOutputs mappas av namnet på varje utdata i experimentet.</span><span class="sxs-lookup"><span data-stu-id="b184d-269">The activity outputs and webServiceOutputs are mapped by the name of each output in the experiment.</span></span> <span data-ttu-id="b184d-270">Annars utelämnas egenskapen webServiceOutputs.</span><span class="sxs-lookup"><span data-stu-id="b184d-270">Otherwise, the webServiceOutputs property is omitted.</span></span>
* <span data-ttu-id="b184d-271">Om din experiment slutpunkt visar globalParameter(s), anges de i egenskapen globalParameters aktivitet som nyckel, värde-par.</span><span class="sxs-lookup"><span data-stu-id="b184d-271">If your experiment endpoint exposes globalParameter(s), they are given in the activity globalParameters property as key, value pairs.</span></span> <span data-ttu-id="b184d-272">Annars utelämnas egenskapen globalParameters.</span><span class="sxs-lookup"><span data-stu-id="b184d-272">Otherwise, the globalParameters property is omitted.</span></span> <span data-ttu-id="b184d-273">Nycklarna är skiftlägeskänsliga.</span><span class="sxs-lookup"><span data-stu-id="b184d-273">The keys are case-sensitive.</span></span> <span data-ttu-id="b184d-274">[Azure Data Factory-funktioner](data-factory-functions-variables.md) får användas i värden.</span><span class="sxs-lookup"><span data-stu-id="b184d-274">[Azure Data Factory functions](data-factory-functions-variables.md) may be used in the values.</span></span>
* <span data-ttu-id="b184d-275">Ytterligare datauppsättningar kan ingå i Aktivitetsegenskaper för in- och utdataenheter utan refereras i aktiviteten typeProperties.</span><span class="sxs-lookup"><span data-stu-id="b184d-275">Additional datasets may be included in the Activity inputs and outputs properties, without being referenced in the Activity typeProperties.</span></span> <span data-ttu-id="b184d-276">De här datauppsättningarna styr körning med hjälp av sektorn beroenden men ignoreras annars av aktiviteten AzureMLBatchExecution.</span><span class="sxs-lookup"><span data-stu-id="b184d-276">These datasets govern execution using slice dependencies but are otherwise ignored by the AzureMLBatchExecution Activity.</span></span>


## <a name="updating-models-using-update-resource-activity"></a><span data-ttu-id="b184d-277">Uppdatering av modeller som använder Uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="b184d-277">Updating models using Update Resource Activity</span></span>
<span data-ttu-id="b184d-278">När du är klar med omtränings uppdatera bedömningsprofil webbtjänsten (prediktivt experiment visas som en webbtjänst) med den nyligen tränade modellen med hjälp av den **Azure ML uppdatera resurs aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="b184d-278">After you are done with retraining, update the scoring web service (predictive experiment exposed as a web service) with the newly trained model by using the **Azure ML Update Resource Activity**.</span></span> <span data-ttu-id="b184d-279">Se [uppdatering modeller med Uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md) artikeln för information.</span><span class="sxs-lookup"><span data-stu-id="b184d-279">See [Updating models using Update Resource Activity](data-factory-azure-ml-update-resource-activity.md) article for details.</span></span>

### <a name="reader-and-writer-modules"></a><span data-ttu-id="b184d-280">Läsare och skrivare moduler</span><span class="sxs-lookup"><span data-stu-id="b184d-280">Reader and Writer Modules</span></span>
<span data-ttu-id="b184d-281">Ett vanligt scenario för att använda webbtjänstparametrar som används för Azure SQL-läsare och skrivare.</span><span class="sxs-lookup"><span data-stu-id="b184d-281">A common scenario for using Web service parameters is the use of Azure SQL Readers and Writers.</span></span> <span data-ttu-id="b184d-282">Modul för dataläsare används för att läsa in data i ett experiment från data management-tjänster utanför Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="b184d-282">The reader module is used to load data into an experiment from data management services outside Azure Machine Learning Studio.</span></span> <span data-ttu-id="b184d-283">Modulen skrivaren är att spara data från dina experiment till data management services utanför Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="b184d-283">The writer module is to save data from your experiments into data management services outside Azure Machine Learning Studio.</span></span>  

<span data-ttu-id="b184d-284">Mer information om Azure Blob-/ Azure SQL reader/writer finns [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) och [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) avsnitt i MSDN Library.</span><span class="sxs-lookup"><span data-stu-id="b184d-284">For details about Azure Blob/Azure SQL reader/writer, see [Reader](https://msdn.microsoft.com/library/azure/dn905997.aspx) and [Writer](https://msdn.microsoft.com/library/azure/dn905984.aspx) topics on MSDN Library.</span></span> <span data-ttu-id="b184d-285">Exemplet i det föregående avsnittet använda Azure Blob-läsare och skrivare i Azure Blob.</span><span class="sxs-lookup"><span data-stu-id="b184d-285">The example in the previous section used the Azure Blob reader and Azure Blob writer.</span></span> <span data-ttu-id="b184d-286">Det här avsnittet beskrivs med SQL Azure reader och Azure SQL writer.</span><span class="sxs-lookup"><span data-stu-id="b184d-286">This section discusses using Azure SQL reader and Azure SQL writer.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="b184d-287">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b184d-287">Frequently asked questions</span></span>
<span data-ttu-id="b184d-288">**F:** jag har flera filer som genereras av min stordata pipelines.</span><span class="sxs-lookup"><span data-stu-id="b184d-288">**Q:** I have multiple files that are generated by my big data pipelines.</span></span> <span data-ttu-id="b184d-289">Kan jag använda AzureMLBatchExecution aktiviteten för att fungera med alla filer?</span><span class="sxs-lookup"><span data-stu-id="b184d-289">Can I use the AzureMLBatchExecution Activity to work on all the files?</span></span>

<span data-ttu-id="b184d-290">**S:** Ja.</span><span class="sxs-lookup"><span data-stu-id="b184d-290">**A:** Yes.</span></span> <span data-ttu-id="b184d-291">Finns det **med hjälp av en modul för dataläsare för att läsa data från flera filer i Azure Blob** information.</span><span class="sxs-lookup"><span data-stu-id="b184d-291">See the **Using a Reader module to read data from multiple files in Azure Blob** section for details.</span></span>

## <a name="azure-ml-batch-scoring-activity"></a><span data-ttu-id="b184d-292">Azure ML bedömningen batchaktiviteten</span><span class="sxs-lookup"><span data-stu-id="b184d-292">Azure ML Batch Scoring Activity</span></span>
<span data-ttu-id="b184d-293">Om du använder den **AzureMLBatchScoring** aktivitet för att integrera med Azure Machine Learning, rekommenderar vi att du använder senast **AzureMLBatchExecution** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="b184d-293">If you are using the **AzureMLBatchScoring** activity to integrate with Azure Machine Learning, we recommend that you use the latest **AzureMLBatchExecution** activity.</span></span>

<span data-ttu-id="b184d-294">Aktiviteten AzureMLBatchExecution introduceras i augusti 2015-versionen av Azure SDK och Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="b184d-294">The AzureMLBatchExecution activity is introduced in the August 2015 release of Azure SDK and Azure PowerShell.</span></span>

<span data-ttu-id="b184d-295">Om du vill fortsätta använda aktiviteten AzureMLBatchScoring fortsätta läsa igenom det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b184d-295">If you want to continue using the AzureMLBatchScoring activity, continue reading through this section.</span></span>  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a><span data-ttu-id="b184d-296">Azure ML-Batchbedömningen aktiviteten med Azure Storage för in-/ utdata</span><span class="sxs-lookup"><span data-stu-id="b184d-296">Azure ML Batch Scoring activity using Azure Storage for input/output</span></span>

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

### <a name="web-service-parameters"></a><span data-ttu-id="b184d-297">Webbtjänstparametrar</span><span class="sxs-lookup"><span data-stu-id="b184d-297">Web Service Parameters</span></span>
<span data-ttu-id="b184d-298">Värden för webbtjänstparametrar lägger du till en **typeProperties** avsnittet till den **AzureMLBatchScoringActivty** avsnittet i pipeline-JSON som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b184d-298">To specify values for Web service parameters, add a **typeProperties** section to the **AzureMLBatchScoringActivty** section in the pipeline JSON as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
<span data-ttu-id="b184d-299">Du kan också använda [Data Factory funktioner](data-factory-functions-variables.md) i skicka värden för Web service parametrar som visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="b184d-299">You can also use [Data Factory Functions](data-factory-functions-variables.md) in passing values for the Web service parameters as shown in the following example:</span></span>

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> <span data-ttu-id="b184d-300">Web service-parametrar är skiftlägeskänsliga, så se till att de namn som du anger i aktiviteten JSON matchar de som visas av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="b184d-300">The Web service parameters are case-sensitive, so ensure that the names you specify in the activity JSON match the ones exposed by the Web service.</span></span>
>
>

## <a name="see-also"></a><span data-ttu-id="b184d-301">Se även</span><span class="sxs-lookup"><span data-stu-id="b184d-301">See Also</span></span>
* [<span data-ttu-id="b184d-302">Azure blogginlägg: komma igång med Azure Data Factory och Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b184d-302">Azure blog post: Getting started with Azure Data Factory and Azure Machine Learning</span></span>](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
