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
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Uppdatera Azure Machine Learning-modeller med Uppdateringsresursaktivitet

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-aktivitet](data-factory-hive-activity.md) 
> * [Pig-aktivitet](data-factory-pig-activity.md)
> * [MapReduce Activity](data-factory-map-reduce.md)
> * [Hadoop Streaming Activity](data-factory-hadoop-streaming-activity.md)
> * [Spark-aktivitet](data-factory-spark.md)
> * [Machine Learning Batch-körningsaktivitet](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-uppdateringsresursaktivitet](data-factory-azure-ml-update-resource-activity.md)
> * [Lagrad proceduraktivitet](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-aktivitet](data-factory-usql-activity.md)
> * [Anpassad aktivitet för .NET](data-factory-use-custom-activities.md)

Den här artikeln kompletterar hello huvudsakliga Azure Data Factory - Azure Machine Learning integration artikel: [skapa förutsägande pipelines med hjälp av Azure Machine Learning och Azure Data Factory](data-factory-azure-ml-batch-execution-activity.md). Om du inte redan har gjort granska hello Huvudartikel innan du läser igenom den här artikeln. 

## <a name="overview"></a>Översikt
Över tiden måste hello förutsägelsemodeller i hello Azure ML bedömningsprofil experiment toobe retrained med nya indatauppsättningar. När du är klar med omtränings vill du tooupdate hello bedömningen webbtjänst med hello retrained ML-modell. hello typiska steg tooenable omtränings och uppdaterar Azure ML-modeller via webbtjänster är:

1. Skapa ett experiment i [Azure ML Studio](https://studio.azureml.net).
2. När du är nöjd med hello modellen, använder du Azure ML Studio toopublish webbtjänster för båda hello **träningsexperiment** och poängsättning /**prediktivt experiment**.

hello beskrivs följande tabell hello webbtjänster som används i det här exemplet.  Se [träna om Machine Learning-modeller via programmering](../machine-learning/machine-learning-retrain-models-programmatically.md) mer information.

- **Utbildning webbtjänsten** – tar emot träningsdata och producerar tränade modeller. hello utdata från hello omtränings är en .ilearner-fil i ett Azure Blob storage. Hej **standard endpoint** skapas automatiskt för dig när du publicerar hello utbildning experiment som en webbtjänst. Du kan skapa flera slutpunkter men hello exemplet används endast hello standardslutpunkten.
- **Bedömningen webbtjänsten** – tar emot exempel omärkta data och gör förutsägelser. hello utdata för förutsägelse kan ha olika former, till exempel en CSV-fil eller rader i en Azure SQL database, beroende på hello konfiguration av hello experiment. hello standardslutpunkten skapas automatiskt för dig när du publicerar hello prediktivt experiment som en webbtjänst. 

hello visar följande bild hello förhållandet mellan utbildning och poängberäkningen slutpunkter i Azure ML.

![Webbtjänster](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Du kan anropa hello **utbildning webbtjänsten** med hjälp av hello **Azure ML-Batchkörningsaktivitet**. Anropa en webbtjänst för utbildning är samma som anropar en Azure ML-webbtjänst (bedömningen webbtjänsten) för bedömningsprofil data. Hej föregående avsnitt omfattar hur tooinvoke en Azure ML-webbtjänst från ett Azure Data Factory pipeline i detalj. 

Du kan anropa hello **bedömningen webbtjänsten** med hjälp av hello **Azure ML-Uppdateringsresursaktivitet** tooupdate hello-webbtjänst med hello nyligen tränade modellen. hello följande exempel innehåller definitioner av länkade tjänsten: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Bedömningen webbtjänst är en klassiska webbtjänst
Om hello bedömningen webbtjänst är en **klassiska webbtjänsten**, skapa hello andra **inte är standard och uppdateras endpoint** med hjälp av hello [Azure-portalen](https://manage.windowsazure.com). Se [skapa slutpunkter](../machine-learning/machine-learning-create-endpoint.md) artikel anvisningar. När du har skapat hello icke-uppdateringsbar standardslutpunkten hello följande:

* Klicka på **BATCH EXECUTION** tooget hello URI värde för hello **mlEndpoint** JSON-egenskapen.
* Klicka på **uppdatering resurs** länka tooget hello URI-värde för hello **updateResourceEndpoint** JSON-egenskapen. hello API-nyckeln finns på hello endpoint själva sidan (i hello längst ned till höger).

![Uppdatera slutpunkten](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

hello följande exempel innehåller ett exempel JSON-definitionen för hello länkad AzureML-tjänst. hello länkade tjänsten använder hello apiKey för autentisering.  

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

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Bedömningen webbtjänsten är Azure Resource Manager-webbtjänst 
Om hello-webbtjänsten är hello ny typ av webbtjänsten som Exponerar en Azure Resource Manager-slutpunkt, du behöver inte tooadd hello andra **icke-förvalt** slutpunkt. Hej **updateResourceEndpoint** i hello länkad tjänst har hello format: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Du kan hämta värden för platshållare i hello URL när du frågar hello webbtjänst på hello [Azure Machine Learning Web Services-portalen](https://services.azureml.net/). hello ny typ av uppdatering resurs slutpunkten kräver ett token för AAD (Azure Active Directory). Ange **servicePrincipalId** och **servicePrincipalKey**i AzureML länkade tjänsten. Se [hur toocreate huvudnamn och tilldela behörigheter toomanage Azure-resurs](../azure-resource-manager/resource-group-create-service-principal-portal.md). Här är ett exempel på en länkad AzureML-tjänstedefinition: 

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

hello visar följande scenario mer information. Det finns ett exempel för omtränings och uppdatera Azure ML modeller från ett Azure Data Factory-pipelinen.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Scenario: omtränings och uppdaterar en Azure ML-modell
Det här avsnittet innehåller en exempel-pipeline som använder hello **Azure ML-batchkörning aktiviteten** tooretrain en modell. hello pipeline använder också hello **Azure ML-uppdatering resurshanteraren aktiviteten** tooupdate hello modell i hello poängsättning av webbtjänsten. hello avsnittet ger också JSON kodavsnitt för alla hello länkade tjänster, datauppsättningar och pipeline i hello exempel.

Här är hello diagramvyn för hello exempel pipeline. Som du ser hello Azure ML-Batchkörningsaktivitet tar hello utbildning indata och skapar en utbildning utdata (iLearner-fil). hello Azure ML-Uppdateringsresursaktivitet tar den här utbildning utdata och uppdateringar hello modell i hello bedömningen webbtjänstslutpunkt. hello-Uppdateringsresursaktivitet inte producerar några utdata. Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.

![Pipeline-diagram](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Azure Blob storage länkade tjänsten:
hello Azure Storage innehåller hello följande data:

* utbildningsdata. hello indata för webbtjänsten för hello Azure ML-utbildning.  
* iLearner-fil. hello utdata från hello Azure ML utbildning-webbtjänsten. Den här filen är också hello inkommande toohello uppdatering resurs aktiviteten.  

Här är hello exempel JSON-definitionen av hello länkade tjänsten:

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

### <a name="training-input-dataset"></a>Utbildning inkommande datauppsättningen:
hello representerar följande datauppsättning hello inkommande träningsdata för webbtjänsten för hello Azure ML-utbildning. hello Azure ML-batchkörning aktiviteten tar den här datauppsättningen som indata.

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

### <a name="training-output-dataset"></a>Utbildning datamängd för utdata:
hello representerar följande datauppsättning hello iLearner utdatafilen från hello Azure ML utbildning webbtjänsten. hello Azure ML-Batchkörningsaktivitet ger denna dataset. Den här datauppsättningen är också hello inkommande toohello Azure ML-uppdatering resurshanteraren aktivitet.

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

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Länkad tjänst för Azure ML utbildning slutpunkt
hello följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar toohello standardslutpunkten av hello utbildning web service.

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

I **Azure ML Studio**, hello följande tooget värden för **mlEndpoint** och **apiKey**:

1. Klicka på **WEB SERVICES** på hello vänstra menyn.
2. Klicka på hello **utbildning webbtjänsten** i hello lista över webbtjänster.
3. Klicka på Kopiera bredvid för**API-nyckel** textruta. Klistra in hello nyckel i hello Urklipp i hello Data Factory JSON-redigerare.
4. I hello **Azure ML studio**, klickar du på **BATCH EXECUTION** länk.
5. Kopiera hello **Begärd URI** från hello **begära** avsnittet och klistra in den i hello Data Factory JSON-redigerare.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Länkad tjänst för Azure ML uppdateras bedömningsprofil slutpunkt:
hello följande JSON-fragment definierar en Azure Machine Learning länkade tjänst som pekar toohello icke-standard uppdateras slutpunkten för hello poängsättning av webbtjänsten.  

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

### <a name="placeholder-output-dataset"></a>Platshållare för utdatauppsättningen:
hello Azure ML-uppdatering resurshanteraren aktivitet genererar inga utdata. Azure Data Factory kräver dock en utdata dataset toodrive hello schemat för en pipeline. Därför kan använder vi en datamängd dummy/platshållare i det här exemplet.  

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

### <a name="pipeline"></a>Pipeline
hello pipeline har två aktiviteter: **AzureMLBatchExecution** och **AzureMLUpdateResource**. hello Azure ML-batchkörning aktiviteten tar hello utbildningsdata som indata och genererar en iLearner-fil som utdata. hello-aktivitet anropar hello utbildning webbtjänst (träningsexperiment visas som en webbtjänst) med hello indata utbildning data och tar emot hello ilearner-fil från hello-webbtjänsten. Hej placeholderBlob är bara en dummy datamängd för utdata som krävs av hello Azure Data Factory-tjänsten toorun hello pipeline.

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
