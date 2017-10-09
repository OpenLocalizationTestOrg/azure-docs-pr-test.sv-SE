---
title: "Självstudie: Skapa en pipeline med en Resource Manager-mall | Microsoft Docs"
description: "I de här självstudierna skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av en Azure Resource Manager-mall. Den här pipelinen kopierar data från Azure blob storage tooan Azure SQL-databas."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1274e11a-e004-4df5-af07-850b2de7c15e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 1c7567cb0423f7ce3e0cab2d77a4d861b70eb56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="95ab0-104">Självstudier: Använd Azure Resource Manager mallen toocreate Data Factory pipeline toocopy data</span><span class="sxs-lookup"><span data-stu-id="95ab0-104">Tutorial: Use Azure Resource Manager template toocreate a Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95ab0-105">Översikt och förutsättningar</span><span class="sxs-lookup"><span data-stu-id="95ab0-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="95ab0-106">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="95ab0-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="95ab0-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="95ab0-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="95ab0-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95ab0-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="95ab0-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="95ab0-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="95ab0-110">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="95ab0-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="95ab0-111">REST API</span><span class="sxs-lookup"><span data-stu-id="95ab0-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="95ab0-112">.NET-API</span><span class="sxs-lookup"><span data-stu-id="95ab0-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="95ab0-113">De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för ett Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-113">This tutorial shows you how toouse an Azure Resource Manager template toocreate an Azure data factory.</span></span> <span data-ttu-id="95ab0-114">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="95ab0-114">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="95ab0-115">Det inte transformera indata tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="95ab0-115">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="95ab0-116">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-116">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="95ab0-117">I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="95ab0-117">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="95ab0-118">Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare.</span><span class="sxs-lookup"><span data-stu-id="95ab0-118">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="95ab0-119">En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="95ab0-119">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="95ab0-120">hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt.</span><span class="sxs-lookup"><span data-stu-id="95ab0-120">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="95ab0-121">Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-121">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="95ab0-122">En pipeline kan ha fler än en aktivitet.</span><span class="sxs-lookup"><span data-stu-id="95ab0-122">A pipeline can have more than one activity.</span></span> <span data-ttu-id="95ab0-123">Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="95ab0-123">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="95ab0-124">Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="95ab0-124">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="95ab0-125">hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data.</span><span class="sxs-lookup"><span data-stu-id="95ab0-125">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="95ab0-126">En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-126">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="95ab0-127">Krav</span><span class="sxs-lookup"><span data-stu-id="95ab0-127">Prerequisites</span></span>
* <span data-ttu-id="95ab0-128">Gå igenom [kursen översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och fullständig hello **nödvändiga** steg.</span><span class="sxs-lookup"><span data-stu-id="95ab0-128">Go through [Tutorial Overview and Prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="95ab0-129">Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn.</span><span class="sxs-lookup"><span data-stu-id="95ab0-129">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span> <span data-ttu-id="95ab0-130">I den här kursen använder du PowerShell toodeploy Data Factory entiteter.</span><span class="sxs-lookup"><span data-stu-id="95ab0-130">In this tutorial, you use PowerShell toodeploy Data Factory entities.</span></span> 
* <span data-ttu-id="95ab0-131">(valfritt) Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn om Azure Resource Manager-mallar.</span><span class="sxs-lookup"><span data-stu-id="95ab0-131">(optional) See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span>

## <a name="in-this-tutorial"></a><span data-ttu-id="95ab0-132">I den här självstudien</span><span class="sxs-lookup"><span data-stu-id="95ab0-132">In this tutorial</span></span>
<span data-ttu-id="95ab0-133">I den här självstudiekursen skapar du en datafabrik med hello följande Data Factory-enheter:</span><span class="sxs-lookup"><span data-stu-id="95ab0-133">In this tutorial, you create a data factory with hello following Data Factory entities:</span></span>

| <span data-ttu-id="95ab0-134">Entitet</span><span class="sxs-lookup"><span data-stu-id="95ab0-134">Entity</span></span> | <span data-ttu-id="95ab0-135">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="95ab0-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="95ab0-136">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-136">Azure Storage linked service</span></span> |<span data-ttu-id="95ab0-137">Länkar din Azure Storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-137">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="95ab0-138">Azure Storage är hello källa datalager och Azure SQL-databas är hello sink-datalagret för hello kopieringsaktiviteten i hello självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-138">Azure Storage is hello source data store and Azure SQL database is hello sink data store for hello copy activity in hello tutorial.</span></span> <span data-ttu-id="95ab0-139">Det anger hello storage-konto som innehåller hello indata för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="95ab0-139">It specifies hello storage account that contains hello input data for hello copy activity.</span></span> |
| <span data-ttu-id="95ab0-140">Länkad Azure SQL Database-tjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-140">Azure SQL Database linked service</span></span> |<span data-ttu-id="95ab0-141">Länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-141">Links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="95ab0-142">Det anger hello Azure SQL-databas som innehåller hello utdata för hello kopieringsaktiviteten.</span><span class="sxs-lookup"><span data-stu-id="95ab0-142">It specifies hello Azure SQL database that holds hello output data for hello copy activity.</span></span> |
| <span data-ttu-id="95ab0-143">Indatauppsättning för Azure-blobb</span><span class="sxs-lookup"><span data-stu-id="95ab0-143">Azure Blob input dataset</span></span> |<span data-ttu-id="95ab0-144">Refererar toohello länkad Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="95ab0-144">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="95ab0-145">hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="95ab0-145">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="95ab0-146">Utdatauppsättning för Azure SQL</span><span class="sxs-lookup"><span data-stu-id="95ab0-146">Azure SQL output dataset</span></span> |<span data-ttu-id="95ab0-147">Refererar toohello Azure SQL-länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="95ab0-147">Refers toohello Azure SQL linked service.</span></span> <span data-ttu-id="95ab0-148">hello Azure SQL-länkade tjänsten refererar tooan Azure SQL-server och hello Azure SQL dataset anger hello namnet på hello-tabell som innehåller hello utdata.</span><span class="sxs-lookup"><span data-stu-id="95ab0-148">hello Azure SQL linked service refers tooan Azure SQL server and hello Azure SQL dataset specifies hello name of hello table that holds hello output data.</span></span> |
| <span data-ttu-id="95ab0-149">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="95ab0-149">Data pipeline</span></span> |<span data-ttu-id="95ab0-150">hello pipeline har en aktivitet av Skriv kopia som tar hello Azure blobbdatauppsättning som indata och hello Azure SQL-dataset som utdata.</span><span class="sxs-lookup"><span data-stu-id="95ab0-150">hello pipeline has one activity of type Copy that takes hello Azure blob dataset as an input and hello Azure SQL dataset as an output.</span></span> <span data-ttu-id="95ab0-151">Hej kopieringsaktiviteten kopierar data från en tabell med Azure blob tooa i hello Azure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="95ab0-151">hello copy activity copies data from an Azure blob tooa table in hello Azure SQL database.</span></span> |

<span data-ttu-id="95ab0-152">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="95ab0-152">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="95ab0-153">En pipeline kan innehålla en eller flera aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="95ab0-153">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="95ab0-154">Det finns två typer av aktiviteter: [dataflyttningsaktiviteter](data-factory-data-movement-activities.md) och [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-154">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="95ab0-155">I den här självstudien får du skapa en pipeline i en aktivitet (kopieringsaktivitet).</span><span class="sxs-lookup"><span data-stu-id="95ab0-155">In this tutorial, you create a pipeline with one activity (copy activity).</span></span>

![Kopiera Azure Blob tooAzure SQL-databas](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

<span data-ttu-id="95ab0-157">hello följande avsnitt innehåller hello fullständig Resource Manager-mall för att definiera Data Factory-enheter så att du snabbt kan köras via hello självstudier och testa hello-mallen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-157">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="95ab0-158">toounderstand hur varje Data Factory-enhet definieras finns [Data Factory-entiteter i hello mallen](#data-factory-entities-in-the-template) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="95ab0-158">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="95ab0-159">Data Factory JSON-mall</span><span class="sxs-lookup"><span data-stu-id="95ab0-159">Data Factory JSON template</span></span>
<span data-ttu-id="95ab0-160">hello översta Resource Manager-mall för att definiera en datafabrik är:</span><span class="sxs-lookup"><span data-stu-id="95ab0-160">hello top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="95ab0-161">Skapa en JSON-fil med namnet **ADFCopyTutorialARM.json** i **C:\ADFGetStarted** mapp med hello följande innehåll:</span><span class="sxs-lookup"><span data-stu-id="95ab0-161">Create a JSON file named **ADFCopyTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello data toobe copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of hello blob in hello container that has hello data toobe copied tooAzure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Server that will hold hello output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Database in hello Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of hello user that has access toohello Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for hello user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in hello Azure SQL Database that will hold hello copied data." } 
      } 
    },
    "variables": {
      "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
      "azureSqlLinkedServiceName": "AzureSqlLinkedService",
      "azureStorageLinkedServiceName": "AzureStorageLinkedService",
      "blobInputDatasetName": "BlobInputDataset",
      "sqlOutputDatasetName": "SQLOutputDataset",
      "pipelineName": "Blob2SQLPipeline"
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
            "name": "[variables('azureSqlLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlDatabase",
              "description": "Azure SQL linked service",
              "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
              "structure": [
                {
                  "name": "Column0",
                  "type": "String"
                },
                {
                  "name": "Column1",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
                }
              },
              "availability": {
                "frequency": "Hour",
                "interval": 1
              },
              "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('sqlOutputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureSqlLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlTable",
              "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
              "structure": [
                {
                  "name": "FirstName",
                  "type": "String"
                },
                {
                  "name": "LastName",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
              },
              "availability": {
                "frequency": "Hour",
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
              "[variables('azureSqlLinkedServiceName')]",
              "[variables('blobInputDatasetName')]",
              "[variables('sqlOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "activities": [
                {
                  "name": "CopyFromAzureBlobToAzureSQL",
                  "description": "Copy data frm Azure blob tooAzure SQL",
                  "type": "Copy",
                  "inputs": [
                    {
                      "name": "[variables('blobInputDatasetName')]"
                    }
                  ],
                  "outputs": [
                    {
                      "name": "[variables('sqlOutputDatasetName')]"
                    }
                  ],
                  "typeProperties": {
                    "source": {
                      "type": "BlobSource"
                    },
                    "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                  },
                  "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                  }
                }
              ],
              "start": "2017-05-11T00:00:00Z",
              "end": "2017-05-12T00:00:00Z"
            }
          }
        ]
      }
    ]
  }
```

## <a name="parameters-json"></a><span data-ttu-id="95ab0-162">JSON-parametrar</span><span class="sxs-lookup"><span data-stu-id="95ab0-162">Parameters JSON</span></span>
<span data-ttu-id="95ab0-163">Skapa en JSON-fil med namnet **ADFCopyTutorialARM Parameters.json** som innehåller parametrar för hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="95ab0-163">Create a JSON file named **ADFCopyTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="95ab0-164">Ange namn och nyckel för Azure Storage-kontot och parametrarna storageAccountName och storageAccountKey.</span><span class="sxs-lookup"><span data-stu-id="95ab0-164">Specify name and key of your Azure Storage account for storageAccountName and storageAccountKey parameters.</span></span>  
> 
> <span data-ttu-id="95ab0-165">Ange Azure SQL-server, databas, användare och lösenord för parametrarna sqlServerName, databaseName, sqlServerUserName och sqlServerPassword.</span><span class="sxs-lookup"><span data-stu-id="95ab0-165">Specify Azure SQL server, database, user, and password for sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters.</span></span>  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of hello Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for hello Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of hello Azure SQL server>" },
        "databaseName": { "value": "<Name of hello Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of hello user who has access toohello Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for hello user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="95ab0-166">Du kan ha separat parameter JSON-filer för utveckling, testning och produktionsmiljöer som du kan använda med hello samma Data Factory JSON-mall.</span><span class="sxs-lookup"><span data-stu-id="95ab0-166">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="95ab0-167">Genom att använda ett Power Shell-skript kan du automatisera distributionen av Data Factory-entiteter i dessa miljöer.</span><span class="sxs-lookup"><span data-stu-id="95ab0-167">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span>  
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="95ab0-168">Skapa en datafabrik</span><span class="sxs-lookup"><span data-stu-id="95ab0-168">Create data factory</span></span>
1. <span data-ttu-id="95ab0-169">Starta **Azure PowerShell** och hello kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="95ab0-169">Start **Azure PowerShell** and run hello following command:</span></span>
   * <span data-ttu-id="95ab0-170">Kör följande kommando hello och ange hello användarnamn och lösenord som du använder toosign i toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-170">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount    
    ```  
   * <span data-ttu-id="95ab0-171">Kör följande kommando tooview hello alla hello prenumerationer för det här kontot.</span><span class="sxs-lookup"><span data-stu-id="95ab0-171">Run hello following command tooview all hello subscriptions for this account.</span></span>
   
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * <span data-ttu-id="95ab0-172">Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.</span><span class="sxs-lookup"><span data-stu-id="95ab0-172">Run hello following command tooselect hello subscription that you want toowork with.</span></span>
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. <span data-ttu-id="95ab0-173">Kör följande kommando toodeploy Data Factory enheter med hjälp av hello Resource Manager-mall som du skapade i steg 1 hello.</span><span class="sxs-lookup"><span data-stu-id="95ab0-173">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span>

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="95ab0-174">Övervaka pipeline</span><span class="sxs-lookup"><span data-stu-id="95ab0-174">Monitor pipeline</span></span>

1. <span data-ttu-id="95ab0-175">Logga in toohello [Azure-portalen](https://portal.azure.com) med ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="95ab0-175">Log in toohello [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
2. <span data-ttu-id="95ab0-176">Klicka på **datafabriker** på hello vänstra menyn (eller) klickar du på **fler tjänster** och på **datafabriker** under **INTELLIGENCE + analys** kategori.</span><span class="sxs-lookup"><span data-stu-id="95ab0-176">Click **Data factories** on hello left menu (or) click **More services** and click **Data factories** under **INTELLIGENCE + ANALYTICS** category.</span></span>
   
    ![Menyn Datafabriker](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. <span data-ttu-id="95ab0-178">I hello **datafabriker** sidan, söka efter och hitta din data factory (AzureBlobToAzureSQLDatabaseDF).</span><span class="sxs-lookup"><span data-stu-id="95ab0-178">In hello **Data factories** page, search for and find your data factory (AzureBlobToAzureSQLDatabaseDF).</span></span> 
   
    ![Sök efter datafabrik](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. <span data-ttu-id="95ab0-180">Klicka på din Azure-datafabrik.</span><span class="sxs-lookup"><span data-stu-id="95ab0-180">Click your Azure data factory.</span></span> <span data-ttu-id="95ab0-181">Du ser hello startsidan för hello data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-181">You see hello home page for hello data factory.</span></span>
   
    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. <span data-ttu-id="95ab0-183">Följ anvisningarna från [övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-183">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="95ab0-184">Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines.</span><span class="sxs-lookup"><span data-stu-id="95ab0-184">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span>
7. <span data-ttu-id="95ab0-185">När ett segment är i hello **klar** tillstånd och kontrollera att hello data är kopierade toohello **tomma** tabellen i hello Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-185">When a slice is in hello **Ready** state, verify that hello data is copied toohello **emp** table in hello Azure SQL database.</span></span>


<span data-ttu-id="95ab0-186">Mer information om hur toouse Azure portal blad toomonitor pipeline och datauppsättningar som du har skapat i den här kursen finns [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) .</span><span class="sxs-lookup"><span data-stu-id="95ab0-186">For more information on how toouse Azure portal blades toomonitor pipeline and datasets you have created in this tutorial, see [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) .</span></span>

<span data-ttu-id="95ab0-187">Mer information om hur toouse hello övervaka och hantera program toomonitor data rörledningar finns [övervaka och hantera Azure Data Factory pipelines med övervakning App](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-187">For more information on how toouse hello Monitor & Manage application toomonitor your data pipelines, see [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md).</span></span>

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="95ab0-188">Data Factory-entiteter i hello mall</span><span class="sxs-lookup"><span data-stu-id="95ab0-188">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="95ab0-189">Definiera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="95ab0-189">Define data factory</span></span>
<span data-ttu-id="95ab0-190">Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="95ab0-190">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

<span data-ttu-id="95ab0-191">Hej dataFactoryName definieras som:</span><span class="sxs-lookup"><span data-stu-id="95ab0-191">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

<span data-ttu-id="95ab0-192">Det är en unik sträng baserat på hello resurs grupp-ID.</span><span class="sxs-lookup"><span data-stu-id="95ab0-192">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="95ab0-193">Definiera Data Factory-entiteter</span><span class="sxs-lookup"><span data-stu-id="95ab0-193">Defining Data Factory entities</span></span>
<span data-ttu-id="95ab0-194">hello har följande Data Factory-enheter definierats i hello JSON-mall:</span><span class="sxs-lookup"><span data-stu-id="95ab0-194">hello following Data Factory entities are defined in hello JSON template:</span></span> 

1. [<span data-ttu-id="95ab0-195">Länkad Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-195">Azure Storage linked service</span></span>](#azure-storage-linked-service)
2. [<span data-ttu-id="95ab0-196">Länkad Azure SQL-tjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-196">Azure SQL linked service</span></span>](#azure-sql-database-linked-service)
3. [<span data-ttu-id="95ab0-197">Azure-blobdatauppsättning</span><span class="sxs-lookup"><span data-stu-id="95ab0-197">Azure blob dataset</span></span>](#azure-blob-dataset)
4. [<span data-ttu-id="95ab0-198">Azure SQL-datauppsättning</span><span class="sxs-lookup"><span data-stu-id="95ab0-198">Azure SQL dataset</span></span>](#azure-sql-dataset)
5. [<span data-ttu-id="95ab0-199">Datapipeline med en kopieringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="95ab0-199">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="95ab0-200">Länkad Azure-lagringstjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-200">Azure Storage linked service</span></span>
<span data-ttu-id="95ab0-201">Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-201">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="95ab0-202">Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-202">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="95ab0-203">Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="95ab0-203">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="95ab0-204">Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="95ab0-204">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

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

<span data-ttu-id="95ab0-205">hello connectionString använder hello storageAccountName och storageAccountKey parametrar.</span><span class="sxs-lookup"><span data-stu-id="95ab0-205">hello connectionString uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="95ab0-206">hello värden för dessa parametrar som skickas med hjälp av en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="95ab0-206">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="95ab0-207">hello definition använder också variabler: azureStroageLinkedService och dataFactoryName som definierats i mallen för hello.</span><span class="sxs-lookup"><span data-stu-id="95ab0-207">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="azure-sql-database-linked-service"></a><span data-ttu-id="95ab0-208">Länkad Azure SQL Database-tjänst</span><span class="sxs-lookup"><span data-stu-id="95ab0-208">Azure SQL Database linked service</span></span>
<span data-ttu-id="95ab0-209">AzureSqlLinkedService länkar din Azure SQL database toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="95ab0-209">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="95ab0-210">hello-data som kopieras från hello blob storage lagras i den här databasen.</span><span class="sxs-lookup"><span data-stu-id="95ab0-210">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="95ab0-211">Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="95ab0-211">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="95ab0-212">Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="95ab0-212">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="95ab0-213">Se [Azure SQL länkade tjänsten](data-factory-azure-sql-connector.md#linked-service-properties) mer information om toodefine för JSON-egenskaper som används för en Azure SQL länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="95ab0-213">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>  

```json
{
    "type": "linkedservices",
    "name": "[variables('azureSqlLinkedServiceName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlDatabase",
          "description": "Azure SQL linked service",
          "typeProperties": {
            "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
          }
    }
}
```

<span data-ttu-id="95ab0-214">hello connectionString använder sqlServerName, databaseName, sqlServerUserName och sqlServerPassword parametrar som skickas med hjälp av en konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="95ab0-214">hello connectionString uses sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters whose values are passed by using a configuration file.</span></span> <span data-ttu-id="95ab0-215">hello definition använder också hello följande variabler från hello mall: azureSqlLinkedServiceName dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="95ab0-215">hello definition also uses hello following variables from hello template: azureSqlLinkedServiceName, dataFactoryName.</span></span>

#### <a name="azure-blob-dataset"></a><span data-ttu-id="95ab0-216">Azure-blobdatauppsättning</span><span class="sxs-lookup"><span data-stu-id="95ab0-216">Azure blob dataset</span></span>
<span data-ttu-id="95ab0-217">hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="95ab0-217">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="95ab0-218">I Azure blob-datauppsättningsdefinitionen anger du namn på blob-behållare, mapp och fil som innehåller hello indata.</span><span class="sxs-lookup"><span data-stu-id="95ab0-218">In Azure blob dataset definition, you specify names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="95ab0-219">Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="95ab0-219">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

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
        "structure": [
        {
              "name": "Column0",
              "type": "String"
        },
        {
              "name": "Column1",
              "type": "String"
        }
          ],
          "typeProperties": {
            "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
            "fileName": "[parameters('sourceBlobName')]",
            "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Hour",
            "interval": 1
          },
          "external": true
    }
}
```

#### <a name="azure-sql-dataset"></a><span data-ttu-id="95ab0-220">Azure SQL-datauppsättning</span><span class="sxs-lookup"><span data-stu-id="95ab0-220">Azure SQL dataset</span></span>
<span data-ttu-id="95ab0-221">Du kan ange hello namnet på hello tabell i hello Azure SQL-databas som innehåller hello kopierade data från hello Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="95ab0-221">You specify hello name of hello table in hello Azure SQL database that holds hello copied data from hello Azure Blob storage.</span></span> <span data-ttu-id="95ab0-222">Se [Azure SQL-egenskaper för datamängd](data-factory-azure-sql-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure SQL-datamängd.</span><span class="sxs-lookup"><span data-stu-id="95ab0-222">See [Azure SQL dataset properties](data-factory-azure-sql-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure SQL dataset.</span></span> 

```json
{
    "type": "datasets",
    "name": "[variables('sqlOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureSqlLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlTable",
          "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
          "structure": [
        {
              "name": "FirstName",
              "type": "String"
        },
        {
              "name": "LastName",
              "type": "String"
        }
          ],
          "typeProperties": {
            "tableName": "[parameters('targetSQLTable')]"
          },
          "availability": {
            "frequency": "Hour",
            "interval": 1
          }
    }
}
```

#### <a name="data-pipeline"></a><span data-ttu-id="95ab0-223">Datapipeline</span><span class="sxs-lookup"><span data-stu-id="95ab0-223">Data pipeline</span></span>
<span data-ttu-id="95ab0-224">Du kan definiera en pipeline som kopierar data från hello Azure dataset toohello SQL Azure-blobbdatauppsättning.</span><span class="sxs-lookup"><span data-stu-id="95ab0-224">You define a pipeline that copies data from hello Azure blob dataset toohello Azure SQL dataset.</span></span> <span data-ttu-id="95ab0-225">Se [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="95ab0-225">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('azureSqlLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "activities": [
        {
              "name": "CopyFromAzureBlobToAzureSQL",
              "description": "Copy data frm Azure blob tooAzure SQL",
              "type": "Copy",
              "inputs": [
            {
                  "name": "[variables('blobInputDatasetName')]"
            }
              ],
              "outputs": [
            {
                  "name": "[variables('sqlOutputDatasetName')]"
            }
              ],
              "typeProperties": {
                "source": {
                      "type": "BlobSource"
                },
                "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                }
              },
              "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
              }
        }
          ],
          "start": "2017-05-11T00:00:00Z",
          "end": "2017-05-12T00:00:00Z"
    }
}
```

## <a name="reuse-hello-template"></a><span data-ttu-id="95ab0-226">Återanvända hello mall</span><span class="sxs-lookup"><span data-stu-id="95ab0-226">Reuse hello template</span></span>
<span data-ttu-id="95ab0-227">I hello självstudierna kommer skapat du en mall för att definiera Data Factory entiteter och en mall för att skicka värden för parametrar.</span><span class="sxs-lookup"><span data-stu-id="95ab0-227">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="95ab0-228">hello pipeline kopierar data från en Azure Storage-konto tooan Azure SQL database angetts via parametrar.</span><span class="sxs-lookup"><span data-stu-id="95ab0-228">hello pipeline copies data from an Azure Storage account tooan Azure SQL database specified via parameters.</span></span> <span data-ttu-id="95ab0-229">toouse hello samma mall toodeploy Data Factory entiteter toodifferent miljöer kan du skapa en parameterfil för varje miljö och använda den när du distribuerar toothat miljö.</span><span class="sxs-lookup"><span data-stu-id="95ab0-229">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="95ab0-230">Exempel:</span><span class="sxs-lookup"><span data-stu-id="95ab0-230">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

<span data-ttu-id="95ab0-231">Observera att hello första kommandot använder parameterfil för hello utvecklingsmiljö, andra en för hello testmiljö och hello tredje en för hello-produktionsmiljö.</span><span class="sxs-lookup"><span data-stu-id="95ab0-231">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="95ab0-232">Du kan även återanvända hello mallen tooperform återkommande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="95ab0-232">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="95ab0-233">Till exempel behöver du toocreate många datafabriker med en eller flera pipelines som implementerar hello samma logik men varje datafabriken använder olika konton för lagring och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="95ab0-233">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Storage and SQL Database accounts.</span></span> <span data-ttu-id="95ab0-234">I det här scenariot kan du använda hello samma mall i hello samma miljö (dev-, test- eller produktionsmiljö) med olika parametern filer toocreate datafabriker.</span><span class="sxs-lookup"><span data-stu-id="95ab0-234">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="95ab0-235">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="95ab0-235">Next steps</span></span>
<span data-ttu-id="95ab0-236">I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="95ab0-236">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="95ab0-237">hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten:</span><span class="sxs-lookup"><span data-stu-id="95ab0-237">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="95ab0-238">toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="95ab0-238">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
