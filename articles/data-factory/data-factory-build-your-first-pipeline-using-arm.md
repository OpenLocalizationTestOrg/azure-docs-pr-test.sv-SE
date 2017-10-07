---
title: "aaaBuild din första data factory (Resource Manager-mall) | Microsoft Docs"
description: "I de här självstudierna skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av en Azure Resource Manager-mall."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: eb9e70b9-a13a-4a27-8256-2759496be470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: fa08cd1ac3a0e5c5bf4bd4c6bd9dfa6dba9f4319
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Självstudie: Skapa din första Azure-datafabrik med hjälp av en Azure Resource Manager-mall
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-mall](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST-API](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

I den här artikeln använder du en Azure Resource Manager-mall toocreate din första Azure data factory. toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.

hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**. Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata. hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider. 

> [!NOTE]
> hello data pipeline i den här handledningen omvandlar indata tooproduce utdata. En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> hello pipeline i den här självstudiekursen har endast en aktivitet av typen: HDInsightHive. En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory). 

## <a name="prerequisites"></a>Krav
* Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.
* Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn.
* Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn om Azure Resource Manager-mallar. 

## <a name="in-this-tutorial"></a>I den här självstudien
| Entitet | Beskrivning |
| --- | --- |
| Länkad Azure-lagringstjänst |Länkar din Azure Storage-konto toohello data factory. hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet. |
| HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran) |Länkar en på begäran HDInsight-kluster toohello data factory. hello-kluster skapas automatiskt åt dig tooprocess data och tas bort när hello bearbetningen är klar. |
| Indatauppsättning för Azure-blobb |Refererar toohello länkad Azure Storage-tjänst. hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello indata. |
| Utdatauppsättning för Azure-blobb |Refererar toohello länkad Azure Storage-tjänst. hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello utdata. |
| Datapipeline |hello pipeline har en aktivitet av typen HDInsightHive som konsumerar hello inkommande dataset och producerar hello utdata datauppsättningen. |

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Det finns två typer av aktiviteter: [dataflyttningsaktiviteter](data-factory-data-movement-activities.md) och [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md). I den här självstudien får du skapa en pipeline i en aktivitet (Hive-aktivitet).

hello följande avsnitt innehåller hello fullständig Resource Manager-mall för att definiera Data Factory-enheter så att du snabbt kan köras via hello självstudier och testa hello-mallen. toounderstand hur varje Data Factory-enhet definieras finns [Data Factory-entiteter i hello mallen](#data-factory-entities-in-the-template) avsnitt.

## <a name="data-factory-json-template"></a>Data Factory JSON-mall
hello översta Resource Manager-mall för att definiera en datafabrik är: 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```
Skapa en JSON-fil med namnet **ADFTutorialARM.json** i **C:\ADFGetStarted** mapp med hello följande innehåll:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello input/output data." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
          "blobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
          "inputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that has hello input file." } },
          "inputBlobName": { "type": "string", "metadata": { "description": "Name of hello input file/blob." } },
          "outputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that will hold hello transformed data." } },
          "hiveScriptFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that contains hello Hive query file." } },
          "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of hello hive query (HQL) file." } }
    },
    "variables": {
          "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
          "blobInputDatasetName": "AzureBlobInput",
          "blobOutputDatasetName": "AzureBlobOutput",
          "pipelineName": "HiveTransformPipeline"
    },
    "resources": [
      {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US",
        "resources": [
          {
            "type": "linkedservices",
            "name": "[variables('azureStorageLinkedServiceName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
            }
          },
          {
            "type": "linkedservices",
            "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "HDInsightOnDemand",
                  "typeProperties": {
                    "version": "3.5",
                    "clusterSize": 1,
                    "timeToLive": "00:05:00",
                    "osType": "Linux",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                  }
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobInputDatasetName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "typeProperties": {
                    "fileName": "[parameters('inputBlobName')]",
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                    "format": {
                          "type": "TextFormat",
                          "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Month",
                    "interval": 1
                  },
                  "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobOutputDatasetName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "typeProperties": {
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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
          },
          {
            "type": "datapipelines",
            "name": "[variables('pipelineName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('hdInsightOnDemandLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('blobOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "description": "Pipeline that transforms data using Hive script.",
                  "activities": [
                {
                      "type": "HDInsightHive",
                      "typeProperties": {
                        "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                        "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                        "defines": {
                              "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                              "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                        }
                      },
                      "inputs": [
                        {
                              "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                              "name": "[variables('blobOutputDatasetName')]"
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
                      "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                }
                  ],
                  "start": "2017-07-01T00:00:00Z",
                  "end": "2017-07-02T00:00:00Z",
                  "isPaused": false
              }
          }
        ]
      }
    ]
}
```

> [!NOTE]
> Du hittar ytterligare ett exempel på en Resource Manager-mall för att skapa en Azure-datafabrik i [Självstudier: skapa en pipeline med kopieringsaktivitet via en Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  
> 
> 

## <a name="parameters-json"></a>JSON-parametrar
Skapa en JSON-fil med namnet **ADFTutorialARM Parameters.json** som innehåller parametrar för hello Azure Resource Manager-mall.  

> [!IMPORTANT]
> Ange hello namn och nyckel för Azure Storage-konto för hello **storageAccountName** och **storageAccountKey** parametrar i den här parameterfilen. 
> 
> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "value": "<Name of your Azure Storage account>"
        },
        "storageAccountKey": {
            "value": "<Key of your Azure Storage account>"
        },
        "blobContainer": {
            "value": "adfgetstarted"
        },
        "inputBlobFolder": {
            "value": "inputdata"
        },
        "inputBlobName": {
            "value": "input.log"
        },
        "outputBlobFolder": {
            "value": "partitioneddata"
        },
        "hiveScriptFolder": {
              "value": "script"
        },
        "hiveScriptFile": {
              "value": "partitionweblogs.hql"
        }
    }
}
```

> [!IMPORTANT]
> Du kan ha separat parameter JSON-filer för utveckling, testning och produktionsmiljöer som du kan använda med hello samma Data Factory JSON-mall. Genom att använda ett Power Shell-skript kan du automatisera distributionen av Data Factory-entiteter i dessa miljöer. 
> 
> 

## <a name="create-data-factory"></a>Skapa en datafabrik
1. Starta **Azure PowerShell** och hello kör följande kommando: 
   * Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * Hello kör följande kommando tooselect hello prenumeration som du vill toowork med. Den här prenumerationen bör hello samtidigt som hello något du använde i hello Azure-portalen.
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. Kör följande kommando toodeploy Data Factory enheter med hjälp av hello Resource Manager-mall som du skapade i steg 1 hello. 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Övervaka pipeline
1. När du loggar in toohello [Azure-portalen](https://portal.azure.com/), klickar du på **Bläddra** och välj **datafabriker**.
     ![Bläddra igenom datafabrikerna](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2. I hello **Datafabriker** bladet, klickar du på hello data factory (**TutorialFactoryARM**) du skapade.    
3. I hello **Datafabriken** klickar du på bladet för din data factory **Diagram**.

     ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. I hello **diagramvyn**du se en översikt över hello pipelines och datauppsättningar som används i den här kursen.
   
   ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. Dubbelklicka på hello dataset i hello diagramvyn, **AzureBlobOutput**. Du ser att hello-segment som håller på att behandlas.
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. När bearbetningen är klar visas hello sektor i **klar** tillstånd. Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter). Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. När hello sektorn är i **klar** tillstånd, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.  

Se [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) anvisningar för hur toouse hello Azure portal blad toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.

Du kan också använda övervaka och hantera appen toomonitor pipelines dina data. Finns [övervaka och hantera Azure Data Factory pipelines med övervakning App](data-factory-monitor-manage-app.md) mer information om hur du använder hello program. 

> [!IMPORTANT]
> hello indatafilen hämtar bort när hello segment har bearbetats. Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.
> 
> 

## <a name="data-factory-entities-in-hello-template"></a>Data Factory-entiteter i hello mall
### <a name="define-data-factory"></a>Definiera en datafabrik
Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
Hej dataFactoryName definieras som: 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
Det är en unik sträng baserat på hello resurs grupp-ID.  

### <a name="defining-data-factory-entities"></a>Definiera Data Factory-entiteter
hello har följande Data Factory-enheter definierats i hello JSON-mall: 

* [Länkad Azure Storage-tjänst](#azure-storage-linked-service)
* [Länkad tjänst för HDInsight på begäran](#hdinsight-on-demand-linked-service)
* [Indatauppsättning för Azure-blobb](#azure-blob-input-dataset)
* [Utdatauppsättning för Azure-blobb](#azure-blob-output-dataset)
* [Datapipeline med en kopieringsaktivitet](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet. Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten. 

```json
{
    "type": "linkedservices",
    "name": "[variables('azureStorageLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureStorage",
        "description": "Azure Storage linked service",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
Hej **connectionString** använder hello storageAccountName och storageAccountKey parametrar. hello värden för dessa parametrar som skickas med hjälp av en konfigurationsfil. hello definition använder också variabler: azureStroageLinkedService och dataFactoryName som definierats i mallen för hello. 

#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)
Se [Compute länkade tjänster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikeln för information om toodefine för JSON-egenskaper som används för en länkad tjänst för HDInsight-på begäran.  

```json
{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
        }
    }
}
```
Observera följande punkter hello: 

* hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello ovan JSON. Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information. 
* Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran. Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.
* Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**). HDInsight tar inte bort den här behållaren när hello kluster har tagits bort. Det här beteendet är avsiktligt. Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.
  
    Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage. Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad. hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”. Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.

Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.

#### <a name="azure-blob-input-dataset"></a>Indatauppsättning för Azure-blobb
Du kan ange hello namnen på blob-behållare, mapp och fil som innehåller hello indata. Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning. 

```json
{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true
    }
}
```
Den här definitionen använder följande parametrar som definierats i mallen för parametern hello: blobContainer inputBlobFolder, och inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Utdatauppsättning för Azure-blobb
Du kan ange hello namnen på blob-behållaren och mappen som innehåller hello utdata. Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.  

```json
{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
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

Den här definitionen använder följande parametrar som definierats i mallen för hello parametern hello: blobContainer och outputBlobFolder. 

#### <a name="data-pipeline"></a>Datapipeline
Du definierar en pipeline som transformerar data genom att köra ett registreringsdatafilskript på ett Azure HD Insight-kluster på begäran. Se [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet. 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('hdInsightOnDemandLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('blobOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "description": "Pipeline that transforms data using Hive script.",
        "activities": [
        {
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                    "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                    "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
            },
            "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
            ],
            "outputs": [
            {
                "name": "[variables('blobOutputDatasetName')]"
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
            "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
        }
        ],
        "start": "2017-07-01T00:00:00Z",
        "end": "2017-07-02T00:00:00Z",
        "isPaused": false
    }
}
```

## <a name="reuse-hello-template"></a>Återanvända hello mall
I hello självstudierna kommer skapat du en mall för att definiera Data Factory entiteter och en mall för att skicka värden för parametrar. toouse hello samma mall toodeploy Data Factory entiteter toodifferent miljöer kan du skapa en parameterfil för varje miljö och använda den när du distribuerar toothat miljö.     

Exempel:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
Observera att hello första kommandot använder parameterfil för hello utvecklingsmiljö, andra en för hello testmiljö och hello tredje en för hello-produktionsmiljö.  

Du kan även återanvända hello mallen tooperform återkommande uppgifter. Till exempel behöver du toocreate många datafabriker med en eller flera pipelines som implementerar hello samma logik men varje data factory använder olika Azure storage och Azure SQL Database-konton. I det här scenariot kan du använda hello samma mall i hello samma miljö (dev-, test- eller produktionsmiljö) med olika parametern filer toocreate datafabriker. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Resource Manager-mall för att skapa en gateway
Här är ett exempel Resource Manager-mall för att skapa en logisk gateway i hello tillbaka. Installera en gateway på din lokala dator eller virtuell Azure IaaS-dator och registrera hello gateway med Data Factory-tjänsten med hjälp av en nyckel. Se [Flytta data mellan lokalt system och moln](data-factory-move-data-between-onprem-and-cloud.md) för mer information.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
    },
    "variables": {
        "dataFactoryName":  "GatewayUsingArmDF",
        "apiVersion": "2015-10-01",
        "singleQuote": "'"
    },
    "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "eastus",
            "resources": [
                {
                    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                    "type": "gateways",
                    "apiVersion": "[variables('apiVersion')]",
                    "name": "GatewayUsingARM",
                    "properties": {
                        "description": "my gateway"
                    }
                }            
            ]
        }
    ]
}
```
Den här mallen skapar en datafabrik som heter GatewayUsingArmDF med en gateway med namnet GatewayUsingARM. 

## <a name="see-also"></a>Se även
| Avsnitt | Beskrivning |
|:--- |:--- |
| [Pipelines](data-factory-create-pipelines.md) |Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag. |
| [Datauppsättningar](data-factory-create-datasets.md) |I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory. |
| [Schemaläggning och körning](data-factory-scheduling-and-execution.md) |Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell. |
| [Övervaka och hantera pipelines med övervakningsappen](data-factory-monitor-manage-app.md) |Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen. |

