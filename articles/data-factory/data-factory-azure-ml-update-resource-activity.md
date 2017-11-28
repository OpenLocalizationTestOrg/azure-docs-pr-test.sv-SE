---
title: "Uppdatera Machine Learning-modeller med hjälp av Azure Data Factory | Microsoft Docs"
description: "Beskriver hur du skapar skapa förutsägande pipelines med hjälp av Azure Data Factory och Azure Machine Learning"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: e31a7a59d14de4382190b39bd70f3ddf6cf673ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="ba938-103">Uppdatera Azure Machine Learning-modeller med Uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="ba938-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="ba938-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="ba938-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="ba938-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="ba938-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="ba938-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="ba938-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="ba938-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="ba938-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="ba938-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="ba938-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="ba938-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="ba938-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="ba938-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="ba938-114">Den här artikeln kompletterar den huvudsakliga version av Azure Data Factory - Azure Machine Learning integration artikel: [skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="ba938-114">This article complements the main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="ba938-115">Om du inte redan har gjort granska huvudsakliga artikeln innan du läser igenom den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="ba938-115">If you haven't already done so, review the main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="ba938-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="ba938-116">Overview</span></span>
<span data-ttu-id="ba938-117">Förutsägelsemodeller i Azure ML bedömningen experiment måste vara retrained med nya indatauppsättningar med tiden.</span><span class="sxs-lookup"><span data-stu-id="ba938-117">Over time, the predictive models in the Azure ML scoring experiments need to be retrained using new input datasets.</span></span> <span data-ttu-id="ba938-118">När du är klar med omtränings som du vill uppdatera bedömningsprofil webbtjänsten med retrained ML-modell.</span><span class="sxs-lookup"><span data-stu-id="ba938-118">After you are done with retraining, you want to update the scoring web service with the retrained ML model.</span></span> <span data-ttu-id="ba938-119">Vanliga åtgärder för att aktivera omtränings och uppdaterar Azure ML-modeller via webbtjänster är:</span><span class="sxs-lookup"><span data-stu-id="ba938-119">The typical steps to enable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="ba938-120">Skapa ett experiment i [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="ba938-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="ba938-121">När du är nöjd med modellen, använda Azure ML Studio för att publicera webbtjänster för både den **träningsexperiment** och poängsättning /**prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="ba938-121">When you are satisfied with the model, use Azure ML Studio to publish web services for both the **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="ba938-122">I följande tabell beskrivs de webbtjänster som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ba938-122">The following table describes the web services used in this example.</span></span>  <span data-ttu-id="ba938-123">Se [träna om Machine Learning-modeller via programmering](../machine-learning/machine-learning-retrain-models-programmatically.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="ba938-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="ba938-124">**Utbildning webbtjänsten** – tar emot träningsdata och producerar tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="ba938-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="ba938-125">Utdata från den omtränings är en .ilearner-fil i ett Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="ba938-125">The output of the retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="ba938-126">Den **standard endpoint** skapas automatiskt för dig när du publicerar utbildningen experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ba938-126">The **default endpoint** is automatically created for you when you publish the training experiment as a web service.</span></span> <span data-ttu-id="ba938-127">Du kan skapa flera slutpunkter, men i exemplet används endast standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="ba938-127">You can create more endpoints but the example uses only the default endpoint.</span></span>
- <span data-ttu-id="ba938-128">**Bedömningen webbtjänsten** – tar emot exempel omärkta data och gör förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="ba938-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="ba938-129">Utdata för förutsägelse kan ha olika former, till exempel en CSV-fil eller rader i en Azure SQL database, beroende på konfigurationen av experimentet.</span><span class="sxs-lookup"><span data-stu-id="ba938-129">The output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on the configuration of the experiment.</span></span> <span data-ttu-id="ba938-130">Standardslutpunkten skapas automatiskt för dig när du publicerar prediktivt experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="ba938-130">The default endpoint is automatically created for you when you publish the predictive experiment as a web service.</span></span> 

<span data-ttu-id="ba938-131">Följande bild visar relationen mellan utbildning och poängberäkningen slutpunkter i Azure ML.</span><span class="sxs-lookup"><span data-stu-id="ba938-131">The following picture depicts the relationship between training and scoring endpoints in Azure ML.</span></span>

![Webbtjänster](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="ba938-133">Du kan anropa den **utbildning webbtjänsten** med hjälp av den **Azure ML-Batchkörningsaktivitet**.</span><span class="sxs-lookup"><span data-stu-id="ba938-133">You can invoke the **training web service** by using the **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="ba938-134">Anropa en webbtjänst för utbildning är samma som anropar en Azure ML-webbtjänst (bedömningen webbtjänsten) för bedömningsprofil data.</span><span class="sxs-lookup"><span data-stu-id="ba938-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="ba938-135">I föregående avsnitt beskriver hur du anropa en Azure ML-webbtjänst från ett Azure Data Factory-pipelinen i detalj.</span><span class="sxs-lookup"><span data-stu-id="ba938-135">The preceding sections cover how to invoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="ba938-136">Du kan anropa den **bedömningen webbtjänsten** med hjälp av den **Azure ML uppdatera resurs aktiviteten** uppdatera webbtjänsten med den nyligen tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="ba938-136">You can invoke the **scoring web service** by using the **Azure ML Update Resource Activity** to update the web service with the newly trained model.</span></span> <span data-ttu-id="ba938-137">I följande exempel innehåller definitioner av länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="ba938-137">The following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="ba938-138">Bedömningen webbtjänst är en klassiska webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ba938-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="ba938-139">Om bedömningsprofil webbtjänsten är en **klassiska webbtjänsten**, skapa andra **inte är standard och uppdateras endpoint** med hjälp av den [Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ba938-139">If the scoring web service is a **classic web service**, create the second **non-default and updatable endpoint** by using the [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="ba938-140">Se [skapa slutpunkter](../machine-learning/machine-learning-create-endpoint.md) artikel anvisningar.</span><span class="sxs-lookup"><span data-stu-id="ba938-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="ba938-141">När du har skapat icke-uppdateringsbar standardslutpunkten gör du följande steg:</span><span class="sxs-lookup"><span data-stu-id="ba938-141">After you create the non-default updatable endpoint, do the following steps:</span></span>

* <span data-ttu-id="ba938-142">Klicka på **BATCH EXECUTION** att hämta URI-värdet för den **mlEndpoint** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ba938-142">Click **BATCH EXECUTION** to get the URI value for the **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="ba938-143">Klicka på **uppdatering resurs** länken för att visa URI-värdet för den **updateResourceEndpoint** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="ba938-143">Click **UPDATE RESOURCE** link to get the URI value for the **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="ba938-144">API-nyckeln finns på sidan endpoint sig själv (i det nedre högra hörnet).</span><span class="sxs-lookup"><span data-stu-id="ba938-144">The API key is on the endpoint page itself (in the bottom-right corner).</span></span>

![Uppdatera slutpunkten](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="ba938-146">Följande exempel visar ett exempel JSON-definitionen för länkad AzureML-tjänst.</span><span class="sxs-lookup"><span data-stu-id="ba938-146">The following example provides a sample JSON definition for the AzureML linked service.</span></span> <span data-ttu-id="ba938-147">Den länkade tjänsten använder apiKey för autentisering.</span><span class="sxs-lookup"><span data-stu-id="ba938-147">The linked service uses the apiKey for authentication.</span></span>  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="ba938-148">Bedömningen webbtjänsten är Azure Resource Manager-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="ba938-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="ba938-149">Om webbtjänsten är den nya typen av webbtjänsten som Exponerar en Azure Resource Manager-slutpunkt, behöver du inte lägga till andra **icke-förvalt** slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ba938-149">If the web service is the new type of web service that exposes an Azure Resource Manager endpoint, you do not need to add the second **non-default** endpoint.</span></span> <span data-ttu-id="ba938-150">Den **updateResourceEndpoint** i den länkade tjänsten är i formatet:</span><span class="sxs-lookup"><span data-stu-id="ba938-150">The **updateResourceEndpoint** in the linked service is of the format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="ba938-151">Du kan hämta värden för platshållare i Webbadressen när du frågar webbtjänsten på den [Azure Machine Learning Web Services-portalen](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="ba938-151">You can get values for place holders in the URL when querying the web service on the [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="ba938-152">Ny typ av uppdatering resurs slutpunkten kräver ett token för AAD (Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="ba938-152">The new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="ba938-153">Ange **servicePrincipalId** och **servicePrincipalKey**i AzureML länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba938-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="ba938-154">Se [så att skapa tjänstens huvudnamn och tilldela behörigheter för att hantera Azure-resurs](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ba938-154">See [how to create service principal and assign permissions to manage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="ba938-155">Här är ett exempel på en länkad AzureML-tjänstedefinition:</span><span class="sxs-lookup"><span data-stu-id="ba938-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "The linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

<span data-ttu-id="ba938-156">Följande scenario visar mer information.</span><span class="sxs-lookup"><span data-stu-id="ba938-156">The following scenario provides more details.</span></span> <span data-ttu-id="ba938-157">Det finns ett exempel för omtränings och uppdatera Azure ML modeller från ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="ba938-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="ba938-158">Scenario: omtränings och uppdaterar en Azure ML-modell</span><span class="sxs-lookup"><span data-stu-id="ba938-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="ba938-159">Det här avsnittet innehåller en exempel-pipeline som använder den **Azure ML-batchkörning aktiviteten** att träna om en modell.</span><span class="sxs-lookup"><span data-stu-id="ba938-159">This section provides a sample pipeline that uses the **Azure ML Batch Execution activity** to retrain a model.</span></span> <span data-ttu-id="ba938-160">Pipeline använder också den **Azure ML uppdatera resurs aktiviteten** att uppdatera modellen i bedömningsprofil webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba938-160">The pipeline also uses the **Azure ML Update Resource activity** to update the model in the scoring web service.</span></span> <span data-ttu-id="ba938-161">Avsnittet innehåller också kodstycken JSON för alla länkade tjänster, datauppsättningar och pipeline i exemplet.</span><span class="sxs-lookup"><span data-stu-id="ba938-161">The section also provides JSON snippets for all the linked services, datasets, and pipeline in the example.</span></span>

<span data-ttu-id="ba938-162">Här är diagramvyn för exempel pipeline.</span><span class="sxs-lookup"><span data-stu-id="ba938-162">Here is the diagram view of the sample pipeline.</span></span> <span data-ttu-id="ba938-163">Som du kan se Azure ML-Batchkörningsaktivitet tar utbildning indata och skapar en utbildning utdata (iLearner-fil).</span><span class="sxs-lookup"><span data-stu-id="ba938-163">As you can see, the Azure ML Batch Execution Activity takes the training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="ba938-164">Azure ML-Uppdateringsresursaktivitet tar den här utbildning utdata och uppdaterar modellen i bedömningsprofil webbtjänstslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="ba938-164">The Azure ML Update Resource Activity takes this training output and updates the model in the scoring web service endpoint.</span></span> <span data-ttu-id="ba938-165">Den Uppdateringsresursaktivitet genererar inga utdata.</span><span class="sxs-lookup"><span data-stu-id="ba938-165">The Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="ba938-166">PlaceholderBlob är bara en dummy datamängd för utdata som krävs av Azure Data Factory-tjänsten för att köra pipelinen.</span><span class="sxs-lookup"><span data-stu-id="ba938-166">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="ba938-168">Azure Blob storage länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="ba938-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="ba938-169">Azure Storage innehåller följande information:</span><span class="sxs-lookup"><span data-stu-id="ba938-169">The Azure Storage holds the following data:</span></span>

* <span data-ttu-id="ba938-170">utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="ba938-170">training data.</span></span> <span data-ttu-id="ba938-171">Indata för webbtjänsten Azure ML utbildning.</span><span class="sxs-lookup"><span data-stu-id="ba938-171">The input data for the Azure ML training web service.</span></span>  
* <span data-ttu-id="ba938-172">iLearner-fil.</span><span class="sxs-lookup"><span data-stu-id="ba938-172">iLearner file.</span></span> <span data-ttu-id="ba938-173">Utdata från Azure ML utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba938-173">The output from the Azure ML training web service.</span></span> <span data-ttu-id="ba938-174">Den här filen är också indata till aktiviteten uppdatera resurs.</span><span class="sxs-lookup"><span data-stu-id="ba938-174">This file is also the input to the Update Resource activity.</span></span>  

<span data-ttu-id="ba938-175">Här är exempel JSON-definitionen för den länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="ba938-175">Here is the sample JSON definition of the linked service:</span></span>

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a><span data-ttu-id="ba938-176">Utbildning inkommande datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="ba938-176">Training input dataset:</span></span>
<span data-ttu-id="ba938-177">Följande datauppsättningen representerar inkommande utbildning data för webbtjänsten Azure ML utbildning.</span><span class="sxs-lookup"><span data-stu-id="ba938-177">The following dataset represents the input training data for the Azure ML training web service.</span></span> <span data-ttu-id="ba938-178">Azure ML-batchkörning aktiviteten tar den här datauppsättningen som indata.</span><span class="sxs-lookup"><span data-stu-id="ba938-178">The Azure ML Batch Execution activity takes this dataset as an input.</span></span>

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a><span data-ttu-id="ba938-179">Utbildning datamängd för utdata:</span><span class="sxs-lookup"><span data-stu-id="ba938-179">Training output dataset:</span></span>
<span data-ttu-id="ba938-180">Följande datauppsättningen representerar iLearner utdatafilen från Azure ML utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba938-180">The following dataset represents the output iLearner file from the Azure ML training web service.</span></span> <span data-ttu-id="ba938-181">Azure ML-Batchkörningsaktivitet ger denna dataset.</span><span class="sxs-lookup"><span data-stu-id="ba938-181">The Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="ba938-182">Den här datauppsättningen är också indata till aktiviteten Azure ML Update resurs.</span><span class="sxs-lookup"><span data-stu-id="ba938-182">This dataset is also the input to the Azure ML Update Resource activity.</span></span>

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="ba938-183">Länkad tjänst för Azure ML utbildning slutpunkt</span><span class="sxs-lookup"><span data-stu-id="ba938-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="ba938-184">Följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar på standardslutpunkten av utbildning web service.</span><span class="sxs-lookup"><span data-stu-id="ba938-184">The following JSON snippet defines an Azure Machine Learning linked service that points to the default endpoint of the training web service.</span></span>

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

<span data-ttu-id="ba938-185">I **Azure ML Studio**, gör följande för att hämta värden för **mlEndpoint** och **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="ba938-185">In **Azure ML Studio**, do the following to get values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="ba938-186">Klicka på **WEB SERVICES** på den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="ba938-186">Click **WEB SERVICES** on the left menu.</span></span>
2. <span data-ttu-id="ba938-187">Klicka på den **utbildning webbtjänsten** i listan över webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="ba938-187">Click the **training web service** in the list of web services.</span></span>
3. <span data-ttu-id="ba938-188">Klicka på Kopiera bredvid **API-nyckel** textruta.</span><span class="sxs-lookup"><span data-stu-id="ba938-188">Click copy next to **API key** text box.</span></span> <span data-ttu-id="ba938-189">Klistra in nyckeln i Urklipp i Data Factory JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="ba938-189">Paste the key in the clipboard into the Data Factory JSON editor.</span></span>
4. <span data-ttu-id="ba938-190">I den **Azure ML studio**, klickar du på **BATCH EXECUTION** länk.</span><span class="sxs-lookup"><span data-stu-id="ba938-190">In the **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="ba938-191">Kopiera den **Begärd URI** från den **begära** avsnittet och klistra in den i Data Factory JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="ba938-191">Copy the **Request URI** from the **Request** section and paste it into the Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="ba938-192">Länkad tjänst för Azure ML uppdateras bedömningsprofil slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="ba938-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="ba938-193">Följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar på icke-uppdateras standardslutpunkten av webbtjänsten för bedömningsprofil.</span><span class="sxs-lookup"><span data-stu-id="ba938-193">The following JSON snippet defines an Azure Machine Learning linked service that points to the non-default updatable endpoint of the scoring web service.</span></span>  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a><span data-ttu-id="ba938-194">Platshållare för utdatauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="ba938-194">Placeholder output dataset:</span></span>
<span data-ttu-id="ba938-195">Azure ML-uppdatering resurshanteraren aktiviteten genererar inte inga utdata.</span><span class="sxs-lookup"><span data-stu-id="ba938-195">The Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="ba938-196">Azure Data Factory kräver dock en datamängd för utdata för att ge schemat för en pipeline.</span><span class="sxs-lookup"><span data-stu-id="ba938-196">However, Azure Data Factory requires an output dataset to drive the schedule of a pipeline.</span></span> <span data-ttu-id="ba938-197">Därför kan använder vi en datamängd dummy/platshållare i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="ba938-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a><span data-ttu-id="ba938-198">Pipeline</span><span class="sxs-lookup"><span data-stu-id="ba938-198">Pipeline</span></span>
<span data-ttu-id="ba938-199">Pipelinen har två aktiviteter: **AzureMLBatchExecution** och **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="ba938-199">The pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="ba938-200">Azure ML-batchkörning aktiviteten hämtar utbildning data som indata och genererar en iLearner-fil som utdata.</span><span class="sxs-lookup"><span data-stu-id="ba938-200">The Azure ML Batch Execution activity takes the training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="ba938-201">Aktiviteten anropar webbtjänsten utbildning (träningsexperiment visas som en webbtjänst) med data som indata utbildning och tar emot den ilearner-fil från den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="ba938-201">The activity invokes the training web service (training experiment exposed as a web service) with the input training data and receives the ilearner file from the webservice.</span></span> <span data-ttu-id="ba938-202">PlaceholderBlob är bara en dummy datamängd för utdata som krävs av Azure Data Factory-tjänsten för att köra pipelinen.</span><span class="sxs-lookup"><span data-stu-id="ba938-202">The placeholderBlob is just a dummy output dataset that is required by the Azure Data Factory service to run the pipeline.</span></span>

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
