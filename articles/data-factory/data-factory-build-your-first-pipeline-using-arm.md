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
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a><span data-ttu-id="6c06b-103">Självstudie: Skapa din första Azure-datafabrik med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6c06b-103">Tutorial: Build your first Azure data factory using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c06b-104">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="6c06b-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="6c06b-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6c06b-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="6c06b-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6c06b-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="6c06b-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c06b-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="6c06b-108">Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="6c06b-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="6c06b-109">REST-API</span><span class="sxs-lookup"><span data-stu-id="6c06b-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

<span data-ttu-id="6c06b-110">I den här artikeln använder du en Azure Resource Manager-mall toocreate din första Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="6c06b-110">In this article, you use an Azure Resource Manager template toocreate your first Azure data factory.</span></span> <span data-ttu-id="6c06b-111">toodo hello självstudier med andra verktyg/SDK: er, Välj ett alternativ för hello hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="6c06b-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="6c06b-112">hello pipeline i den här självstudiekursen har en aktivitet: **HDInsight Hive aktiviteten**.</span><span class="sxs-lookup"><span data-stu-id="6c06b-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="6c06b-113">Den här aktiviteten körs en hive-skript på ett Azure HDInsight-kluster att transformeringar inkommande data tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="6c06b-114">hello pipeline är schemalagda toorun när en månad mellan hello angivna start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="6c06b-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="6c06b-115">hello data pipeline i den här handledningen omvandlar indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="6c06b-116">En självstudiekurs om hur toocopy data med hjälp av Azure Data Factory finns [Självstudier: kopiera data från Blob Storage tooSQL databasen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="6c06b-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="6c06b-117">hello pipeline i den här självstudiekursen har endast en aktivitet av typen: HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="6c06b-117">hello pipeline in this tutorial has only one activity of type: HDInsightHive.</span></span> <span data-ttu-id="6c06b-118">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="6c06b-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="6c06b-119">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6c06b-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="6c06b-120">Mer detaljerad information finns i [Scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) (Schemaläggning och utförande i Data Factory).</span><span class="sxs-lookup"><span data-stu-id="6c06b-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="6c06b-121">Krav</span><span class="sxs-lookup"><span data-stu-id="6c06b-121">Prerequisites</span></span>
* <span data-ttu-id="6c06b-122">Läs igenom [kursen översikt](data-factory-build-your-first-pipeline.md) artikeln och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="6c06b-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="6c06b-123">Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="6c06b-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="6c06b-124">Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn om Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="6c06b-124">See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span> 

## <a name="in-this-tutorial"></a><span data-ttu-id="6c06b-125">I den här självstudien</span><span class="sxs-lookup"><span data-stu-id="6c06b-125">In this tutorial</span></span>
| <span data-ttu-id="6c06b-126">Entitet</span><span class="sxs-lookup"><span data-stu-id="6c06b-126">Entity</span></span> | <span data-ttu-id="6c06b-127">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c06b-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6c06b-128">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="6c06b-128">Azure Storage linked service</span></span> |<span data-ttu-id="6c06b-129">Länkar din Azure Storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="6c06b-129">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="6c06b-130">hello Azure Storage-konto innehåller hello inkommande och utgående data för hello pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6c06b-130">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> |
| <span data-ttu-id="6c06b-131">HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)</span><span class="sxs-lookup"><span data-stu-id="6c06b-131">HDInsight on-demand linked service</span></span> |<span data-ttu-id="6c06b-132">Länkar en på begäran HDInsight-kluster toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="6c06b-132">Links an on-demand HDInsight cluster toohello data factory.</span></span> <span data-ttu-id="6c06b-133">hello-kluster skapas automatiskt åt dig tooprocess data och tas bort när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="6c06b-133">hello cluster is automatically created for you tooprocess data and is deleted after hello processing is done.</span></span> |
| <span data-ttu-id="6c06b-134">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-134">Azure Blob input dataset</span></span> |<span data-ttu-id="6c06b-135">Refererar toohello länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c06b-135">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="6c06b-136">hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-136">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="6c06b-137">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-137">Azure Blob output dataset</span></span> |<span data-ttu-id="6c06b-138">Refererar toohello länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="6c06b-138">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="6c06b-139">hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-139">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello output data.</span></span> |
| <span data-ttu-id="6c06b-140">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="6c06b-140">Data pipeline</span></span> |<span data-ttu-id="6c06b-141">hello pipeline har en aktivitet av typen HDInsightHive som konsumerar hello inkommande dataset och producerar hello utdata datauppsättningen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-141">hello pipeline has one activity of type HDInsightHive, which consumes hello input dataset and produces hello output dataset.</span></span> |

<span data-ttu-id="6c06b-142">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="6c06b-142">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="6c06b-143">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="6c06b-143">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="6c06b-144">Det finns två typer av aktiviteter: [dataflyttningsaktiviteter](data-factory-data-movement-activities.md) och [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="6c06b-144">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="6c06b-145">I den här självstudien får du skapa en pipeline i en aktivitet (Hive-aktivitet).</span><span class="sxs-lookup"><span data-stu-id="6c06b-145">In this tutorial, you create a pipeline with one activity (Hive activity).</span></span>

<span data-ttu-id="6c06b-146">hello följande avsnitt innehåller hello fullständig Resource Manager-mall för att definiera Data Factory-enheter så att du snabbt kan köras via hello självstudier och testa hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-146">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="6c06b-147">toounderstand hur varje Data Factory-enhet definieras finns [Data Factory-entiteter i hello mallen](#data-factory-entities-in-the-template) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6c06b-147">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="6c06b-148">Data Factory JSON-mall</span><span class="sxs-lookup"><span data-stu-id="6c06b-148">Data Factory JSON template</span></span>
<span data-ttu-id="6c06b-149">hello översta Resource Manager-mall för att definiera en datafabrik är:</span><span class="sxs-lookup"><span data-stu-id="6c06b-149">hello top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="6c06b-150">Skapa en JSON-fil med namnet **ADFTutorialARM.json** i **C:\ADFGetStarted** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="6c06b-150">Create a JSON file named **ADFTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

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
> <span data-ttu-id="6c06b-151">Du hittar ytterligare ett exempel på en Resource Manager-mall för att skapa en Azure-datafabrik i [Självstudier: skapa en pipeline med kopieringsaktivitet via en Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="6c06b-151">You can find another example of Resource Manager template for creating an Azure data factory on [Tutorial: Create a pipeline with Copy Activity using an Azure Resource Manager template](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span></span>  
> 
> 

## <a name="parameters-json"></a><span data-ttu-id="6c06b-152">JSON-parametrar</span><span class="sxs-lookup"><span data-stu-id="6c06b-152">Parameters JSON</span></span>
<span data-ttu-id="6c06b-153">Skapa en JSON-fil med namnet **ADFTutorialARM Parameters.json** som innehåller parametrar för hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="6c06b-153">Create a JSON file named **ADFTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="6c06b-154">Ange hello namn och nyckel för Azure Storage-konto för hello **storageAccountName** och **storageAccountKey** parametrar i den här parameterfilen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-154">Specify hello name and key of your Azure Storage account for hello **storageAccountName** and **storageAccountKey** parameters in this parameter file.</span></span> 
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
> <span data-ttu-id="6c06b-155">Du kan ha separat parameter JSON-filer för utveckling, testning och produktionsmiljöer som du kan använda med hello samma Data Factory JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="6c06b-155">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="6c06b-156">Genom att använda ett Power Shell-skript kan du automatisera distributionen av Data Factory-entiteter i dessa miljöer.</span><span class="sxs-lookup"><span data-stu-id="6c06b-156">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span> 
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="6c06b-157">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="6c06b-157">Create data factory</span></span>
1. <span data-ttu-id="6c06b-158">Starta **Azure PowerShell** och hello kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="6c06b-158">Start **Azure PowerShell** and run hello following command:</span></span> 
   * <span data-ttu-id="6c06b-159">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-159">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * <span data-ttu-id="6c06b-160">Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="6c06b-160">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * <span data-ttu-id="6c06b-161">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="6c06b-161">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="6c06b-162">Den här prenumerationen bör hello samtidigt som hello något du använde i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-162">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. <span data-ttu-id="6c06b-163">Kör följande kommando toodeploy Data Factory enheter med hjälp av hello Resource Manager-mall som du skapade i steg 1 hello.</span><span class="sxs-lookup"><span data-stu-id="6c06b-163">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span> 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="6c06b-164">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="6c06b-164">Monitor pipeline</span></span>
1. <span data-ttu-id="6c06b-165">När du loggar in toohello [Azure-portalen](https://portal.azure.com/), klickar du på **Bläddra** och välj **datafabriker**.</span><span class="sxs-lookup"><span data-stu-id="6c06b-165">After logging in toohello [Azure portal](https://portal.azure.com/), Click **Browse** and select **Data factories**.</span></span>
     <span data-ttu-id="6c06b-166">![Bläddra igenom datafabrikerna](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span><span class="sxs-lookup"><span data-stu-id="6c06b-166">![Browse->Data factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span></span>
2. <span data-ttu-id="6c06b-167">I hello **Datafabriker** bladet, klickar du på hello data factory (**TutorialFactoryARM**) du skapade.</span><span class="sxs-lookup"><span data-stu-id="6c06b-167">In hello **Data Factories** blade, click hello data factory (**TutorialFactoryARM**) you created.</span></span>    
3. <span data-ttu-id="6c06b-168">I hello **Datafabriken** klickar du på bladet för din data factory **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="6c06b-168">In hello **Data Factory** blade for your data factory, click **Diagram**.</span></span>

     ![Ikonen Diagram](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. <span data-ttu-id="6c06b-170">I hello **diagramvyn**du se en översikt över hello pipelines och datauppsättningar som används i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-170">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>
   
   ![Diagramvy](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. <span data-ttu-id="6c06b-172">Dubbelklicka på hello dataset i hello diagramvyn, **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="6c06b-172">In hello Diagram View, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="6c06b-173">Du ser att hello-segment som håller på att behandlas.</span><span class="sxs-lookup"><span data-stu-id="6c06b-173">You see that hello slice that is currently being processed.</span></span>
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. <span data-ttu-id="6c06b-175">När bearbetningen är klar visas hello sektor i **klar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="6c06b-175">When processing is done, you see hello slice in **Ready** state.</span></span> <span data-ttu-id="6c06b-176">Att skapa ett HDInsight-kluster på begäran kan ta lite längre tid (cirka 20 minuter).</span><span class="sxs-lookup"><span data-stu-id="6c06b-176">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="6c06b-177">Därför förvänta sig hello pipeline tootake **cirka 30 minuter** tooprocess hello sektorn.</span><span class="sxs-lookup"><span data-stu-id="6c06b-177">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
   
    ![Datauppsättning](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. <span data-ttu-id="6c06b-179">När hello sektorn är i **klar** tillstånd, kontrollera hello **partitioneddata** mapp i hello **adfgetstarted** behållare i blobblagring för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-179">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

<span data-ttu-id="6c06b-180">Se [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) anvisningar för hur toouse hello Azure portal blad toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-180">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal blades toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

<span data-ttu-id="6c06b-181">Du kan också använda övervaka och hantera appen toomonitor pipelines dina data.</span><span class="sxs-lookup"><span data-stu-id="6c06b-181">You can also use Monitor and Manage App toomonitor your data pipelines.</span></span> <span data-ttu-id="6c06b-182">Finns [övervaka och hantera Azure Data Factory pipelines med övervakning App](data-factory-monitor-manage-app.md) mer information om hur du använder hello program.</span><span class="sxs-lookup"><span data-stu-id="6c06b-182">See [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) for details about using hello application.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6c06b-183">hello indatafilen hämtar bort när hello segment har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="6c06b-183">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="6c06b-184">Om du vill toorerun hello segment eller hello kursen igen överför därför hello indatafilen (input.log) toohello inputdata mapp för hello adfgetstarted behållare.</span><span class="sxs-lookup"><span data-stu-id="6c06b-184">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
> 
> 

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="6c06b-185">Data Factory-entiteter i hello mall</span><span class="sxs-lookup"><span data-stu-id="6c06b-185">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="6c06b-186">Definiera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="6c06b-186">Define data factory</span></span>
<span data-ttu-id="6c06b-187">Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="6c06b-187">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
<span data-ttu-id="6c06b-188">Hej dataFactoryName definieras som:</span><span class="sxs-lookup"><span data-stu-id="6c06b-188">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
<span data-ttu-id="6c06b-189">Det är en unik sträng baserat på hello resurs grupp-ID.</span><span class="sxs-lookup"><span data-stu-id="6c06b-189">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="6c06b-190">Definiera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="6c06b-190">Defining Data Factory entities</span></span>
<span data-ttu-id="6c06b-191">hello har följande Data Factory-enheter definierats i hello JSON-mall:</span><span class="sxs-lookup"><span data-stu-id="6c06b-191">hello following Data Factory entities are defined in hello JSON template:</span></span> 

* [<span data-ttu-id="6c06b-192">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="6c06b-192">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="6c06b-193">Länkad tjänst för HDInsight på begäran</span><span class="sxs-lookup"><span data-stu-id="6c06b-193">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="6c06b-194">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-194">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="6c06b-195">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-195">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="6c06b-196">Datapipeline med en kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="6c06b-196">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="6c06b-197">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="6c06b-197">Azure Storage linked service</span></span>
<span data-ttu-id="6c06b-198">Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6c06b-198">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="6c06b-199">Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="6c06b-199">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

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
<span data-ttu-id="6c06b-200">Hej **connectionString** använder hello storageAccountName och storageAccountKey parametrar.</span><span class="sxs-lookup"><span data-stu-id="6c06b-200">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="6c06b-201">hello värden för dessa parametrar som skickas med hjälp av en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="6c06b-201">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="6c06b-202">hello definition använder också variabler: azureStroageLinkedService och dataFactoryName som definierats i mallen för hello.</span><span class="sxs-lookup"><span data-stu-id="6c06b-202">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="6c06b-203">HDInsight on-demand linked service (Länkad tjänst för HDInsight på begäran)</span><span class="sxs-lookup"><span data-stu-id="6c06b-203">HDInsight on-demand linked service</span></span>
<span data-ttu-id="6c06b-204">Se [Compute länkade tjänster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) artikeln för information om toodefine för JSON-egenskaper som används för en länkad tjänst för HDInsight-på begäran.</span><span class="sxs-lookup"><span data-stu-id="6c06b-204">See [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

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
<span data-ttu-id="6c06b-205">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="6c06b-205">Note hello following points:</span></span> 

* <span data-ttu-id="6c06b-206">hello Data Factory skapar en **Linux-baserade** HDInsight-kluster du hello ovan JSON.</span><span class="sxs-lookup"><span data-stu-id="6c06b-206">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="6c06b-207">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6c06b-207">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span> 
* <span data-ttu-id="6c06b-208">Du kan använda **ditt eget HDInsight-kluster** i stället för att använda ett HDInsight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="6c06b-208">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="6c06b-209">Se [HDInsight-länkad tjänst](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6c06b-209">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="6c06b-210">Hej HDInsight-kluster skapas en **standardbehållaren** i hello blob storage som du angav i hello JSON (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="6c06b-210">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="6c06b-211">HDInsight tar inte bort den här behållaren när hello kluster har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="6c06b-211">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="6c06b-212">Det här beteendet är avsiktligt.</span><span class="sxs-lookup"><span data-stu-id="6c06b-212">This behavior is by design.</span></span> <span data-ttu-id="6c06b-213">Med länkad HDInsight-tjänsten på begäran, ett HDInsight-kluster skapas varje gång ett segment måste toobe bearbetas såvida det inte finns ett befintligt live kluster (**timeToLive**) och tas bort när hello bearbetningen är klar.</span><span class="sxs-lookup"><span data-stu-id="6c06b-213">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>
  
    <span data-ttu-id="6c06b-214">Allteftersom fler sektorer bearbetas kan du se många behållare i ditt Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="6c06b-214">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="6c06b-215">Om du inte behöver dem för felsökning av hello jobb, kanske du vill toodelete dem tooreduce hello lagring kostnad.</span><span class="sxs-lookup"><span data-stu-id="6c06b-215">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="6c06b-216">hello namnen på de här behållarna följer ett mönster ”: adf**yourdatafactoryname**-**linkedservicename**- datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="6c06b-216">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="6c06b-217">Använd verktyg som [Microsoft Lagringsutforskaren](http://storageexplorer.com/) toodelete behållare i din Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="6c06b-217">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="6c06b-218">Se [HDInsight-länkad tjänst på begäran](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6c06b-218">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="6c06b-219">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-219">Azure blob input dataset</span></span>
<span data-ttu-id="6c06b-220">Du kan ange hello namnen på blob-behållare, mapp och fil som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-220">You specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="6c06b-221">Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="6c06b-221">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

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
<span data-ttu-id="6c06b-222">Den här definitionen använder följande parametrar som definierats i mallen för parametern hello: blobContainer inputBlobFolder, och inputBlobName.</span><span class="sxs-lookup"><span data-stu-id="6c06b-222">This definition uses hello following parameters defined in parameter template: blobContainer, inputBlobFolder, and inputBlobName.</span></span> 

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="6c06b-223">Utdatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="6c06b-223">Azure Blob output dataset</span></span>
<span data-ttu-id="6c06b-224">Du kan ange hello namnen på blob-behållaren och mappen som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="6c06b-224">You specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="6c06b-225">Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="6c06b-225">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

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

<span data-ttu-id="6c06b-226">Den här definitionen använder följande parametrar som definierats i mallen för hello parametern hello: blobContainer och outputBlobFolder.</span><span class="sxs-lookup"><span data-stu-id="6c06b-226">This definition uses hello following parameters defined in hello parameter template: blobContainer and outputBlobFolder.</span></span> 

#### <a name="data-pipeline"></a><span data-ttu-id="6c06b-227">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="6c06b-227">Data pipeline</span></span>
<span data-ttu-id="6c06b-228">Du definierar en pipeline som transformerar data genom att köra ett registreringsdatafilskript på ett Azure HD Insight-kluster på begäran.</span><span class="sxs-lookup"><span data-stu-id="6c06b-228">You define a pipeline that transform data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="6c06b-229">Se [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="6c06b-229">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

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

## <a name="reuse-hello-template"></a><span data-ttu-id="6c06b-230">Återanvända hello mall</span><span class="sxs-lookup"><span data-stu-id="6c06b-230">Reuse hello template</span></span>
<span data-ttu-id="6c06b-231">I hello självstudierna kommer skapat du en mall för att definiera Data Factory entiteter och en mall för att skicka värden för parametrar.</span><span class="sxs-lookup"><span data-stu-id="6c06b-231">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="6c06b-232">toouse hello samma mall toodeploy Data Factory entiteter toodifferent miljöer kan du skapa en parameterfil för varje miljö och använda den när du distribuerar toothat miljö.</span><span class="sxs-lookup"><span data-stu-id="6c06b-232">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="6c06b-233">Exempel:</span><span class="sxs-lookup"><span data-stu-id="6c06b-233">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
<span data-ttu-id="6c06b-234">Observera att hello första kommandot använder parameterfil för hello utvecklingsmiljö, andra en för hello testmiljö och hello tredje en för hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="6c06b-234">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="6c06b-235">Du kan även återanvända hello mallen tooperform återkommande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="6c06b-235">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="6c06b-236">Till exempel behöver du toocreate många datafabriker med en eller flera pipelines som implementerar hello samma logik men varje data factory använder olika Azure storage och Azure SQL Database-konton.</span><span class="sxs-lookup"><span data-stu-id="6c06b-236">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Azure storage and Azure SQL Database accounts.</span></span> <span data-ttu-id="6c06b-237">I det här scenariot kan du använda hello samma mall i hello samma miljö (dev-, test- eller produktionsmiljö) med olika parametern filer toocreate datafabriker.</span><span class="sxs-lookup"><span data-stu-id="6c06b-237">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span> 

## <a name="resource-manager-template-for-creating-a-gateway"></a><span data-ttu-id="6c06b-238">Resource Manager-mall för att skapa en gateway</span><span class="sxs-lookup"><span data-stu-id="6c06b-238">Resource Manager template for creating a gateway</span></span>
<span data-ttu-id="6c06b-239">Här är ett exempel Resource Manager-mall för att skapa en logisk gateway i hello tillbaka.</span><span class="sxs-lookup"><span data-stu-id="6c06b-239">Here is a sample Resource Manager template for creating a logical gateway in hello back.</span></span> <span data-ttu-id="6c06b-240">Installera en gateway på din lokala dator eller virtuell Azure IaaS-dator och registrera hello gateway med Data Factory-tjänsten med hjälp av en nyckel.</span><span class="sxs-lookup"><span data-stu-id="6c06b-240">Install a gateway on your on-premises computer or Azure IaaS VM and register hello gateway with Data Factory service using a key.</span></span> <span data-ttu-id="6c06b-241">Se [Flytta data mellan lokalt system och moln](data-factory-move-data-between-onprem-and-cloud.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="6c06b-241">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for details.</span></span>

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
<span data-ttu-id="6c06b-242">Den här mallen skapar en datafabrik som heter GatewayUsingArmDF med en gateway med namnet GatewayUsingARM.</span><span class="sxs-lookup"><span data-stu-id="6c06b-242">This template creates a data factory named GatewayUsingArmDF with a gateway named: GatewayUsingARM.</span></span> 

## <a name="see-also"></a><span data-ttu-id="6c06b-243">Se även</span><span class="sxs-lookup"><span data-stu-id="6c06b-243">See Also</span></span>
| <span data-ttu-id="6c06b-244">Avsnitt</span><span class="sxs-lookup"><span data-stu-id="6c06b-244">Topic</span></span> | <span data-ttu-id="6c06b-245">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="6c06b-245">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="6c06b-246">Pipelines</span><span class="sxs-lookup"><span data-stu-id="6c06b-246">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="6c06b-247">Den här artikeln hjälper dig att förstå pipelines och aktiviteter i Azure Data Factory och hur toouse dem tooconstruct slutpunkt till slutpunkt datadrivna arbetsflöden för din scenario eller ditt företag.</span><span class="sxs-lookup"><span data-stu-id="6c06b-247">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="6c06b-248">Datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="6c06b-248">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="6c06b-249">I den här artikeln förklaras hur datauppsättningar fungerar i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="6c06b-249">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="6c06b-250">Schemaläggning och körning</span><span class="sxs-lookup"><span data-stu-id="6c06b-250">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="6c06b-251">Den här artikeln förklarar hello schemaläggning och körning av aspekter av Azure Data Factory programmodell.</span><span class="sxs-lookup"><span data-stu-id="6c06b-251">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="6c06b-252">Övervaka och hantera pipelines med övervakningsappen</span><span class="sxs-lookup"><span data-stu-id="6c06b-252">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="6c06b-253">Den här artikeln beskriver hur toomonitor, hantera och felsöka pipelines med hello övervakning & Management-appen.</span><span class="sxs-lookup"><span data-stu-id="6c06b-253">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |

