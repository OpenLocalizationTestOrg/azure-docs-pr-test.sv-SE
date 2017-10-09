---
title: "aaaUpdate Machine Learning-modeller med hjälp av Azure Data Factory | Microsoft Docs"
description: "Beskriver hur toocreate skapar förutsägbara pipelines med hjälp av Azure Data Factory och Azure Machine Learning"
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
ms.openlocfilehash: 6e5e4d2cfd245c7a9ed3bb9cdacca1f7f82b9620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a><span data-ttu-id="a7892-103">Uppdatera Azure Machine Learning-modeller med Uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-103">Updating Azure Machine Learning models using Update Resource Activity</span></span>

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [<span data-ttu-id="a7892-104">Hive-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-104">Hive Activity</span></span>](data-factory-hive-activity.md) 
> * [<span data-ttu-id="a7892-105">Pig-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-105">Pig Activity</span></span>](data-factory-pig-activity.md)
> * [<span data-ttu-id="a7892-106">MapReduce Activity</span><span class="sxs-lookup"><span data-stu-id="a7892-106">MapReduce Activity</span></span>](data-factory-map-reduce.md)
> * [<span data-ttu-id="a7892-107">Hadoop Streaming Activity</span><span class="sxs-lookup"><span data-stu-id="a7892-107">Hadoop Streaming Activity</span></span>](data-factory-hadoop-streaming-activity.md)
> * [<span data-ttu-id="a7892-108">Spark-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-108">Spark Activity</span></span>](data-factory-spark.md)
> * [<span data-ttu-id="a7892-109">Machine Learning Batch-körningsaktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-109">Machine Learning Batch Execution Activity</span></span>](data-factory-azure-ml-batch-execution-activity.md)
> * [<span data-ttu-id="a7892-110">Machine Learning-uppdateringsresursaktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-110">Machine Learning Update Resource Activity</span></span>](data-factory-azure-ml-update-resource-activity.md)
> * [<span data-ttu-id="a7892-111">Lagrad proceduraktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-111">Stored Procedure Activity</span></span>](data-factory-stored-proc-activity.md)
> * [<span data-ttu-id="a7892-112">Data Lake Analytics U-SQL-aktivitet</span><span class="sxs-lookup"><span data-stu-id="a7892-112">Data Lake Analytics U-SQL Activity</span></span>](data-factory-usql-activity.md)
> * [<span data-ttu-id="a7892-113">Anpassad aktivitet för .NET</span><span class="sxs-lookup"><span data-stu-id="a7892-113">.NET Custom Activity</span></span>](data-factory-use-custom-activities.md)

<span data-ttu-id="a7892-114">Den här artikeln kompletterar hello huvudsakliga Azure Data Factory - Azure Machine Learning integration artikel: [skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span><span class="sxs-lookup"><span data-stu-id="a7892-114">This article complements hello main Azure Data Factory - Azure Machine Learning integration article: [Create predictive pipelines using Azure Machine Learning and Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md).</span></span> <span data-ttu-id="a7892-115">Om du inte redan har gjort granska hello Huvudartikel innan du läser igenom den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a7892-115">If you haven't already done so, review hello main article before reading through this article.</span></span> 

## <a name="overview"></a><span data-ttu-id="a7892-116">Översikt</span><span class="sxs-lookup"><span data-stu-id="a7892-116">Overview</span></span>
<span data-ttu-id="a7892-117">Över tiden måste hello förutsägelsemodeller i hello Azure ML bedömningsprofil experiment toobe retrained med nya indatauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="a7892-117">Over time, hello predictive models in hello Azure ML scoring experiments need toobe retrained using new input datasets.</span></span> <span data-ttu-id="a7892-118">När du är klar med omtränings vill du tooupdate hello bedömningen webbtjänst med hello retrained ML-modell.</span><span class="sxs-lookup"><span data-stu-id="a7892-118">After you are done with retraining, you want tooupdate hello scoring web service with hello retrained ML model.</span></span> <span data-ttu-id="a7892-119">hello typiska steg tooenable omtränings och uppdaterar Azure ML-modeller via webbtjänster är:</span><span class="sxs-lookup"><span data-stu-id="a7892-119">hello typical steps tooenable retraining and updating Azure ML models via web services are:</span></span>

1. <span data-ttu-id="a7892-120">Skapa ett experiment i [Azure ML Studio](https://studio.azureml.net).</span><span class="sxs-lookup"><span data-stu-id="a7892-120">Create an experiment in [Azure ML Studio](https://studio.azureml.net).</span></span>
2. <span data-ttu-id="a7892-121">När du är nöjd med hello modellen, använder du Azure ML Studio toopublish webbtjänster för båda hello **träningsexperiment** och poängsättning /**prediktivt experiment**.</span><span class="sxs-lookup"><span data-stu-id="a7892-121">When you are satisfied with hello model, use Azure ML Studio toopublish web services for both hello **training experiment** and scoring/**predictive experiment**.</span></span>

<span data-ttu-id="a7892-122">hello beskrivs följande tabell hello webbtjänster som används i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a7892-122">hello following table describes hello web services used in this example.</span></span>  <span data-ttu-id="a7892-123">Se [träna om Machine Learning-modeller via programmering](../machine-learning/machine-learning-retrain-models-programmatically.md) mer information.</span><span class="sxs-lookup"><span data-stu-id="a7892-123">See [Retrain Machine Learning models programmatically](../machine-learning/machine-learning-retrain-models-programmatically.md) for details.</span></span>

- <span data-ttu-id="a7892-124">**Utbildning webbtjänsten** – tar emot träningsdata och producerar tränade modeller.</span><span class="sxs-lookup"><span data-stu-id="a7892-124">**Training web service** - Receives training data and produces trained models.</span></span> <span data-ttu-id="a7892-125">hello utdata från hello omtränings är en .ilearner-fil i ett Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="a7892-125">hello output of hello retraining is an .ilearner file in an Azure Blob storage.</span></span> <span data-ttu-id="a7892-126">Hej **standard endpoint** skapas automatiskt för dig när du publicerar hello utbildning experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a7892-126">hello **default endpoint** is automatically created for you when you publish hello training experiment as a web service.</span></span> <span data-ttu-id="a7892-127">Du kan skapa flera slutpunkter men hello exemplet används endast hello standardslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="a7892-127">You can create more endpoints but hello example uses only hello default endpoint.</span></span>
- <span data-ttu-id="a7892-128">**Bedömningen webbtjänsten** – tar emot exempel omärkta data och gör förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="a7892-128">**Scoring web service** - Receives unlabeled data examples and makes predictions.</span></span> <span data-ttu-id="a7892-129">hello utdata för förutsägelse kan ha olika former, till exempel en CSV-fil eller rader i en Azure SQL database, beroende på hello konfiguration av hello experiment.</span><span class="sxs-lookup"><span data-stu-id="a7892-129">hello output of prediction could have various forms, such as a .csv file or rows in an Azure SQL database, depending on hello configuration of hello experiment.</span></span> <span data-ttu-id="a7892-130">hello standardslutpunkten skapas automatiskt för dig när du publicerar hello prediktivt experiment som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="a7892-130">hello default endpoint is automatically created for you when you publish hello predictive experiment as a web service.</span></span> 

<span data-ttu-id="a7892-131">hello visar följande bild hello förhållandet mellan utbildning och poängberäkningen slutpunkter i Azure ML.</span><span class="sxs-lookup"><span data-stu-id="a7892-131">hello following picture depicts hello relationship between training and scoring endpoints in Azure ML.</span></span>

![Webbtjänster](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

<span data-ttu-id="a7892-133">Du kan anropa hello **utbildning webbtjänsten** med hjälp av hello **Azure ML-Batchkörningsaktivitet**.</span><span class="sxs-lookup"><span data-stu-id="a7892-133">You can invoke hello **training web service** by using hello **Azure ML Batch Execution Activity**.</span></span> <span data-ttu-id="a7892-134">Anropa en webbtjänst för utbildning är samma som anropar en Azure ML-webbtjänst (bedömningen webbtjänsten) för bedömningsprofil data.</span><span class="sxs-lookup"><span data-stu-id="a7892-134">Invoking a training web service is same as invoking an Azure ML web service (scoring web service) for scoring data.</span></span> <span data-ttu-id="a7892-135">Hej föregående avsnitt omfattar hur tooinvoke en Azure ML-webbtjänst från ett Azure Data Factory pipeline i detalj.</span><span class="sxs-lookup"><span data-stu-id="a7892-135">hello preceding sections cover how tooinvoke an Azure ML web service from an Azure Data Factory pipeline in detail.</span></span> 

<span data-ttu-id="a7892-136">Du kan anropa hello **bedömningen webbtjänsten** med hjälp av hello **Azure ML-Uppdateringsresursaktivitet** tooupdate hello-webbtjänst med hello nyligen tränade modellen.</span><span class="sxs-lookup"><span data-stu-id="a7892-136">You can invoke hello **scoring web service** by using hello **Azure ML Update Resource Activity** tooupdate hello web service with hello newly trained model.</span></span> <span data-ttu-id="a7892-137">hello följande exempel innehåller definitioner av länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a7892-137">hello following examples provide linked service definitions:</span></span> 

## <a name="scoring-web-service-is-a-classic-web-service"></a><span data-ttu-id="a7892-138">Bedömningen webbtjänst är en klassiska webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a7892-138">Scoring web service is a classic web service</span></span>
<span data-ttu-id="a7892-139">Om hello bedömningen webbtjänst är en **klassiska webbtjänsten**, skapa hello andra **inte är standard och uppdateras endpoint** med hjälp av hello [Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="a7892-139">If hello scoring web service is a **classic web service**, create hello second **non-default and updatable endpoint** by using hello [Azure portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="a7892-140">Se [skapa slutpunkter](../machine-learning/machine-learning-create-endpoint.md) artikel anvisningar.</span><span class="sxs-lookup"><span data-stu-id="a7892-140">See [Create Endpoints](../machine-learning/machine-learning-create-endpoint.md) article for steps.</span></span> <span data-ttu-id="a7892-141">När du har skapat hello icke-uppdateringsbar standardslutpunkten hello följande:</span><span class="sxs-lookup"><span data-stu-id="a7892-141">After you create hello non-default updatable endpoint, do hello following steps:</span></span>

* <span data-ttu-id="a7892-142">Klicka på **BATCH EXECUTION** tooget hello URI värde för hello **mlEndpoint** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a7892-142">Click **BATCH EXECUTION** tooget hello URI value for hello **mlEndpoint** JSON property.</span></span>
* <span data-ttu-id="a7892-143">Klicka på **uppdatering resurs** länka tooget hello URI-värde för hello **updateResourceEndpoint** JSON-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="a7892-143">Click **UPDATE RESOURCE** link tooget hello URI value for hello **updateResourceEndpoint** JSON property.</span></span> <span data-ttu-id="a7892-144">hello API-nyckeln finns på hello endpoint själva sidan (i hello längst ned till höger).</span><span class="sxs-lookup"><span data-stu-id="a7892-144">hello API key is on hello endpoint page itself (in hello bottom-right corner).</span></span>

![Uppdatera slutpunkten](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

<span data-ttu-id="a7892-146">hello följande exempel innehåller ett exempel JSON-definitionen för hello länkad AzureML-tjänst.</span><span class="sxs-lookup"><span data-stu-id="a7892-146">hello following example provides a sample JSON definition for hello AzureML linked service.</span></span> <span data-ttu-id="a7892-147">hello länkade tjänsten använder hello apiKey för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a7892-147">hello linked service uses hello apiKey for authentication.</span></span>  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a><span data-ttu-id="a7892-148">Bedömningen webbtjänsten är Azure Resource Manager-webbtjänst</span><span class="sxs-lookup"><span data-stu-id="a7892-148">Scoring web service is Azure Resource Manager web service</span></span> 
<span data-ttu-id="a7892-149">Om hello-webbtjänsten är hello ny typ av webbtjänsten som Exponerar en Azure Resource Manager-slutpunkt, du behöver inte tooadd hello andra **icke-förvalt** slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a7892-149">If hello web service is hello new type of web service that exposes an Azure Resource Manager endpoint, you do not need tooadd hello second **non-default** endpoint.</span></span> <span data-ttu-id="a7892-150">Hej **updateResourceEndpoint** i hello länkad tjänst har hello format:</span><span class="sxs-lookup"><span data-stu-id="a7892-150">hello **updateResourceEndpoint** in hello linked service is of hello format:</span></span> 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

<span data-ttu-id="a7892-151">Du kan hämta värden för platshållare i hello URL när du frågar hello webbtjänst på hello [Azure Machine Learning Web Services-portalen](https://services.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="a7892-151">You can get values for place holders in hello URL when querying hello web service on hello [Azure Machine Learning Web Services Portal](https://services.azureml.net/).</span></span> <span data-ttu-id="a7892-152">hello ny typ av uppdatering resurs slutpunkten kräver ett token för AAD (Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="a7892-152">hello new type of update resource endpoint requires an AAD (Azure Active Directory) token.</span></span> <span data-ttu-id="a7892-153">Ange **servicePrincipalId** och **servicePrincipalKey**i AzureML länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-153">Specify **servicePrincipalId** and **servicePrincipalKey**in AzureML linked service.</span></span> <span data-ttu-id="a7892-154">Se [hur toocreate huvudnamn och tilldela behörigheter toomanage Azure-resurs](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a7892-154">See [how toocreate service principal and assign permissions toomanage Azure resource](../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="a7892-155">Här är ett exempel på en länkad AzureML-tjänstedefinition:</span><span class="sxs-lookup"><span data-stu-id="a7892-155">Here is a sample AzureML linked service definition:</span></span> 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "hello linked service for AML web service.",
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

<span data-ttu-id="a7892-156">hello visar följande scenario mer information.</span><span class="sxs-lookup"><span data-stu-id="a7892-156">hello following scenario provides more details.</span></span> <span data-ttu-id="a7892-157">Det finns ett exempel för omtränings och uppdatera Azure ML modeller från ett Azure Data Factory-pipelinen.</span><span class="sxs-lookup"><span data-stu-id="a7892-157">It has an example for retraining and updating Azure ML models from an Azure Data Factory pipeline.</span></span>

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a><span data-ttu-id="a7892-158">Scenario: omtränings och uppdaterar en Azure ML-modell</span><span class="sxs-lookup"><span data-stu-id="a7892-158">Scenario: retraining and updating an Azure ML model</span></span>
<span data-ttu-id="a7892-159">Det här avsnittet innehåller en exempel-pipeline som använder hello **Azure ML-batchkörning aktiviteten** tooretrain en modell.</span><span class="sxs-lookup"><span data-stu-id="a7892-159">This section provides a sample pipeline that uses hello **Azure ML Batch Execution activity** tooretrain a model.</span></span> <span data-ttu-id="a7892-160">hello pipeline använder också hello **Azure ML-uppdatering resurshanteraren aktiviteten** tooupdate hello modell i hello poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-160">hello pipeline also uses hello **Azure ML Update Resource activity** tooupdate hello model in hello scoring web service.</span></span> <span data-ttu-id="a7892-161">hello avsnittet ger också JSON kodavsnitt för alla hello länkade tjänster, datauppsättningar och pipeline i hello exempel.</span><span class="sxs-lookup"><span data-stu-id="a7892-161">hello section also provides JSON snippets for all hello linked services, datasets, and pipeline in hello example.</span></span>

<span data-ttu-id="a7892-162">Här är hello diagramvyn för hello exempel pipeline.</span><span class="sxs-lookup"><span data-stu-id="a7892-162">Here is hello diagram view of hello sample pipeline.</span></span> <span data-ttu-id="a7892-163">Som du ser hello Azure ML-Batchkörningsaktivitet tar hello utbildning indata och skapar en utbildning utdata (iLearner-fil).</span><span class="sxs-lookup"><span data-stu-id="a7892-163">As you can see, hello Azure ML Batch Execution Activity takes hello training input and produces a training output (iLearner file).</span></span> <span data-ttu-id="a7892-164">hello Azure ML-Uppdateringsresursaktivitet tar den här utbildning utdata och uppdateringar hello modell i hello bedömningen webbtjänstslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a7892-164">hello Azure ML Update Resource Activity takes this training output and updates hello model in hello scoring web service endpoint.</span></span> <span data-ttu-id="a7892-165">hello-Uppdateringsresursaktivitet inte producerar några utdata.</span><span class="sxs-lookup"><span data-stu-id="a7892-165">hello Update Resource Activity does not produce any output.</span></span> <span data-ttu-id="a7892-166">Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a7892-166">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a><span data-ttu-id="a7892-168">Azure Blob storage länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a7892-168">Azure Blob storage linked service:</span></span>
<span data-ttu-id="a7892-169">hello Azure Storage innehåller hello följande data:</span><span class="sxs-lookup"><span data-stu-id="a7892-169">hello Azure Storage holds hello following data:</span></span>

* <span data-ttu-id="a7892-170">utbildningsdata.</span><span class="sxs-lookup"><span data-stu-id="a7892-170">training data.</span></span> <span data-ttu-id="a7892-171">hello indata för webbtjänsten för hello Azure ML-utbildning.</span><span class="sxs-lookup"><span data-stu-id="a7892-171">hello input data for hello Azure ML training web service.</span></span>  
* <span data-ttu-id="a7892-172">iLearner-fil.</span><span class="sxs-lookup"><span data-stu-id="a7892-172">iLearner file.</span></span> <span data-ttu-id="a7892-173">hello utdata från hello Azure ML utbildning-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-173">hello output from hello Azure ML training web service.</span></span> <span data-ttu-id="a7892-174">Den här filen är också hello inkommande toohello uppdatering resurs aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="a7892-174">This file is also hello input toohello Update Resource activity.</span></span>  

<span data-ttu-id="a7892-175">Här är hello exempel JSON-definitionen av hello länkade tjänsten:</span><span class="sxs-lookup"><span data-stu-id="a7892-175">Here is hello sample JSON definition of hello linked service:</span></span>

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

### <a name="training-input-dataset"></a><span data-ttu-id="a7892-176">Utbildning inkommande datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="a7892-176">Training input dataset:</span></span>
<span data-ttu-id="a7892-177">hello representerar följande datauppsättning hello inkommande träningsdata för webbtjänsten för hello Azure ML-utbildning.</span><span class="sxs-lookup"><span data-stu-id="a7892-177">hello following dataset represents hello input training data for hello Azure ML training web service.</span></span> <span data-ttu-id="a7892-178">hello Azure ML-batchkörning aktiviteten tar den här datauppsättningen som indata.</span><span class="sxs-lookup"><span data-stu-id="a7892-178">hello Azure ML Batch Execution activity takes this dataset as an input.</span></span>

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

### <a name="training-output-dataset"></a><span data-ttu-id="a7892-179">Utbildning datamängd för utdata:</span><span class="sxs-lookup"><span data-stu-id="a7892-179">Training output dataset:</span></span>
<span data-ttu-id="a7892-180">hello representerar följande datauppsättning hello iLearner utdatafilen från hello Azure ML utbildning webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-180">hello following dataset represents hello output iLearner file from hello Azure ML training web service.</span></span> <span data-ttu-id="a7892-181">hello Azure ML-Batchkörningsaktivitet ger denna dataset.</span><span class="sxs-lookup"><span data-stu-id="a7892-181">hello Azure ML Batch Execution Activity produces this dataset.</span></span> <span data-ttu-id="a7892-182">Den här datauppsättningen är också hello inkommande toohello Azure ML-uppdatering resurshanteraren aktivitet.</span><span class="sxs-lookup"><span data-stu-id="a7892-182">This dataset is also hello input toohello Azure ML Update Resource activity.</span></span>

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

### <a name="linked-service-for-azure-ml-training-endpoint"></a><span data-ttu-id="a7892-183">Länkad tjänst för Azure ML utbildning slutpunkt</span><span class="sxs-lookup"><span data-stu-id="a7892-183">Linked service for Azure ML training endpoint</span></span>
<span data-ttu-id="a7892-184">hello följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar toohello standardslutpunkten av hello utbildning web service.</span><span class="sxs-lookup"><span data-stu-id="a7892-184">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello default endpoint of hello training web service.</span></span>

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

<span data-ttu-id="a7892-185">I **Azure ML Studio**, hello följande tooget värden för **mlEndpoint** och **apiKey**:</span><span class="sxs-lookup"><span data-stu-id="a7892-185">In **Azure ML Studio**, do hello following tooget values for **mlEndpoint** and **apiKey**:</span></span>

1. <span data-ttu-id="a7892-186">Klicka på **WEB SERVICES** på hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="a7892-186">Click **WEB SERVICES** on hello left menu.</span></span>
2. <span data-ttu-id="a7892-187">Klicka på hello **utbildning webbtjänsten** i hello lista över webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="a7892-187">Click hello **training web service** in hello list of web services.</span></span>
3. <span data-ttu-id="a7892-188">Klicka på Kopiera bredvid för**API-nyckel** textruta.</span><span class="sxs-lookup"><span data-stu-id="a7892-188">Click copy next too**API key** text box.</span></span> <span data-ttu-id="a7892-189">Klistra in hello nyckel i hello Urklipp i hello Data Factory JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="a7892-189">Paste hello key in hello clipboard into hello Data Factory JSON editor.</span></span>
4. <span data-ttu-id="a7892-190">I hello **Azure ML studio**, klickar du på **BATCH EXECUTION** länk.</span><span class="sxs-lookup"><span data-stu-id="a7892-190">In hello **Azure ML studio**, click **BATCH EXECUTION** link.</span></span>
5. <span data-ttu-id="a7892-191">Kopiera hello **Begärd URI** från hello **begära** avsnittet och klistra in den i hello Data Factory JSON-redigerare.</span><span class="sxs-lookup"><span data-stu-id="a7892-191">Copy hello **Request URI** from hello **Request** section and paste it into hello Data Factory JSON editor.</span></span>   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a><span data-ttu-id="a7892-192">Länkad tjänst för Azure ML uppdateras bedömningsprofil slutpunkt:</span><span class="sxs-lookup"><span data-stu-id="a7892-192">Linked Service for Azure ML updatable scoring endpoint:</span></span>
<span data-ttu-id="a7892-193">hello följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar toohello icke-standard uppdateras slutpunkten för hello poängsättning av webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-193">hello following JSON snippet defines an Azure Machine Learning linked service that points toohello non-default updatable endpoint of hello scoring web service.</span></span>  

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

### <a name="placeholder-output-dataset"></a><span data-ttu-id="a7892-194">Platshållare för utdatauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="a7892-194">Placeholder output dataset:</span></span>
<span data-ttu-id="a7892-195">hello Azure ML-uppdatering resurshanteraren aktivitet genererar inga utdata.</span><span class="sxs-lookup"><span data-stu-id="a7892-195">hello Azure ML Update Resource activity does not generate any output.</span></span> <span data-ttu-id="a7892-196">Azure Data Factory kräver dock en utdata dataset toodrive hello schemat för en pipeline.</span><span class="sxs-lookup"><span data-stu-id="a7892-196">However, Azure Data Factory requires an output dataset toodrive hello schedule of a pipeline.</span></span> <span data-ttu-id="a7892-197">Därför kan använder vi en datamängd dummy/platshållare i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="a7892-197">Therefore, we use a dummy/placeholder dataset in this example.</span></span>  

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

### <a name="pipeline"></a><span data-ttu-id="a7892-198">Pipeline</span><span class="sxs-lookup"><span data-stu-id="a7892-198">Pipeline</span></span>
<span data-ttu-id="a7892-199">hello pipeline har två aktiviteter: **AzureMLBatchExecution** och **AzureMLUpdateResource**.</span><span class="sxs-lookup"><span data-stu-id="a7892-199">hello pipeline has two activities: **AzureMLBatchExecution** and **AzureMLUpdateResource**.</span></span> <span data-ttu-id="a7892-200">hello Azure ML-batchkörning aktiviteten tar hello utbildningsdata som indata och genererar en iLearner-fil som utdata.</span><span class="sxs-lookup"><span data-stu-id="a7892-200">hello Azure ML Batch Execution activity takes hello training data as input and produces an iLearner file as an output.</span></span> <span data-ttu-id="a7892-201">hello-aktivitet anropar hello utbildning webbtjänst (träningsexperiment visas som en webbtjänst) med hello indata utbildning data och tar emot hello ilearner-fil från hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="a7892-201">hello activity invokes hello training web service (training experiment exposed as a web service) with hello input training data and receives hello ilearner file from hello webservice.</span></span> <span data-ttu-id="a7892-202">Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="a7892-202">hello placeholderBlob is just a dummy output dataset that is required by hello Azure Data Factory service toorun hello pipeline.</span></span>

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
