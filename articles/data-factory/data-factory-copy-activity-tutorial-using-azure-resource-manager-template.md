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
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a>Självstudier: Använd Azure Resource Manager mallen toocreate Data Factory pipeline toocopy data 
> [!div class="op_single_selector"]
> * [Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Guiden Kopiera](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET-API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

De här självstudierna visar hur toouse toocreate en Azure Resource Manager-mall för ett Azure data factory. hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. Det inte transformera indata tooproduce utdata. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md).

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Hej kopieringsaktiviteten kopierar data från ett datalager för data som stöds store tooa stöds mottagare. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbar sätt. Läs mer om hello Kopieringsaktiviteten [Data Movement aktiviteter](data-factory-data-movement-activities.md).

En pipeline kan ha fler än en aktivitet. Och du kan länka två aktiviteter (köra en aktivitet efter ett annat) genom att ange hello datamängd för utdata för en aktivitet som hello indatauppsättning av hello andra aktiviteter. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> hello data pipeline i den här kursen kopierar data från en källa data store tooa målarkiv data. En självstudiekurs om hur tootransform data med hjälp av Azure Data Factory finns [Självstudier: skapa en pipeline tootransform data med Hadoop-kluster](data-factory-build-your-first-pipeline.md). 

## <a name="prerequisites"></a>Krav
* Gå igenom [kursen översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och fullständig hello **nödvändiga** steg.
* Följ instruktionerna i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) artikel tooinstall senaste versionen av Azure PowerShell på datorn. I den här kursen använder du PowerShell toodeploy Data Factory entiteter. 
* (valfritt) Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn om Azure Resource Manager-mallar.

## <a name="in-this-tutorial"></a>I den här självstudien
I den här självstudiekursen skapar du en datafabrik med hello följande Data Factory-enheter:

| Entitet | Beskrivning |
| --- | --- |
| Länkad Azure-lagringstjänst |Länkar din Azure Storage-konto toohello data factory. Azure Storage är hello källa datalager och Azure SQL-databas är hello sink-datalagret för hello kopieringsaktiviteten i hello självstudiekursen. Det anger hello storage-konto som innehåller hello indata för hello kopieringsaktiviteten. |
| Länkad Azure SQL Database-tjänst |Länkar din Azure SQL database toohello data factory. Det anger hello Azure SQL-databas som innehåller hello utdata för hello kopieringsaktiviteten. |
| Indatauppsättning för Azure-blobb |Refererar toohello länkad Azure Storage-tjänst. hello länkade tjänsten refererar tooan Azure Storage-konto och hello Azure-blobbdatauppsättning anger hello behållare, mappen och filnamnet i hello lagring som innehåller hello indata. |
| Utdatauppsättning för Azure SQL |Refererar toohello Azure SQL-länkade tjänsten. hello Azure SQL-länkade tjänsten refererar tooan Azure SQL-server och hello Azure SQL dataset anger hello namnet på hello-tabell som innehåller hello utdata. |
| Datapipeline |hello pipeline har en aktivitet av Skriv kopia som tar hello Azure blobbdatauppsättning som indata och hello Azure SQL-dataset som utdata. Hej kopieringsaktiviteten kopierar data från en tabell med Azure blob tooa i hello Azure SQL-databas. |

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Det finns två typer av aktiviteter: [dataflyttningsaktiviteter](data-factory-data-movement-activities.md) och [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md). I den här självstudien får du skapa en pipeline i en aktivitet (kopieringsaktivitet).

![Kopiera Azure Blob tooAzure SQL-databas](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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
Skapa en JSON-fil med namnet **ADFCopyTutorialARM.json** i **C:\ADFGetStarted** mapp med hello följande innehåll:

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

## <a name="parameters-json"></a>JSON-parametrar
Skapa en JSON-fil med namnet **ADFCopyTutorialARM Parameters.json** som innehåller parametrar för hello Azure Resource Manager-mall. 

> [!IMPORTANT]
> Ange namn och nyckel för Azure Storage-kontot och parametrarna storageAccountName och storageAccountKey.  
> 
> Ange Azure SQL-server, databas, användare och lösenord för parametrarna sqlServerName, databaseName, sqlServerUserName och sqlServerPassword.  

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
   * Hello kör följande kommando tooselect hello prenumeration som du vill toowork med.
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. Kör följande kommando toodeploy Data Factory enheter med hjälp av hello Resource Manager-mall som du skapade i steg 1 hello.

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Övervaka pipeline

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ditt Azure-konto.
2. Klicka på **datafabriker** på hello vänstra menyn (eller) klickar du på **fler tjänster** och på **datafabriker** under **INTELLIGENCE + analys** kategori.
   
    ![Menyn Datafabriker](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. I hello **datafabriker** sidan, söka efter och hitta din data factory (AzureBlobToAzureSQLDatabaseDF). 
   
    ![Sök efter datafabrik](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klicka på din Azure-datafabrik. Du ser hello startsidan för hello data factory.
   
    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. Följ anvisningarna från [övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline och datauppsättningar som du har skapat i den här kursen. Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines.
7. När ett segment är i hello **klar** tillstånd och kontrollera att hello data är kopierade toohello **tomma** tabellen i hello Azure SQL-databasen.


Mer information om hur toouse Azure portal blad toomonitor pipeline och datauppsättningar som du har skapat i den här kursen finns [övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) .

Mer information om hur toouse hello övervaka och hantera program toomonitor data rörledningar finns [övervaka och hantera Azure Data Factory pipelines med övervakning App](data-factory-monitor-manage-app.md).

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
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

Det är en unik sträng baserat på hello resurs grupp-ID.  

### <a name="defining-data-factory-entities"></a>Definiera Data Factory-entiteter
hello har följande Data Factory-enheter definierats i hello JSON-mall: 

1. [Länkad Azure Storage-tjänst](#azure-storage-linked-service)
2. [Länkad Azure SQL-tjänst](#azure-sql-database-linked-service)
3. [Azure-blobdatauppsättning](#azure-blob-dataset)
4. [Azure SQL-datauppsättning](#azure-sql-dataset)
5. [Datapipeline med en kopieringsaktivitet](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
Hej AzureStorageLinkedService länkar din Azure storage-konto toohello data factory. Du har skapat en behållare och överföra data toothis storage-konto som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Du kan ange hello namn och nyckel för Azure storage-konto i det här avsnittet. Se [Azure länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) mer information om toodefine för JSON-egenskaper som används för ett Azure Storage länkade tjänsten. 

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

hello connectionString använder hello storageAccountName och storageAccountKey parametrar. hello värden för dessa parametrar som skickas med hjälp av en konfigurationsfil. hello definition använder också variabler: azureStroageLinkedService och dataFactoryName som definierats i mallen för hello. 

#### <a name="azure-sql-database-linked-service"></a>Länkad Azure SQL Database-tjänst
AzureSqlLinkedService länkar din Azure SQL database toohello data factory. hello-data som kopieras från hello blob storage lagras i den här databasen. Du har skapat hello tomma tabellen i den här databasen som en del av [krav](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Du kan ange hello Azure SQL-servernamnet, databasnamnet, användarnamn och lösenord i det här avsnittet. Se [Azure SQL länkade tjänsten](data-factory-azure-sql-connector.md#linked-service-properties) mer information om toodefine för JSON-egenskaper som används för en Azure SQL länkade tjänsten.  

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

hello connectionString använder sqlServerName, databaseName, sqlServerUserName och sqlServerPassword parametrar som skickas med hjälp av en konfigurationsfil. hello definition använder också hello följande variabler från hello mall: azureSqlLinkedServiceName dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure-blobdatauppsättning
hello länkad Azure storage-tjänst anger hello anslutningssträngen som Data Factory-tjänsten använder vid körning tooconnect tooyour Azure storage-konto. I Azure blob-datauppsättningsdefinitionen anger du namn på blob-behållare, mapp och fil som innehåller hello indata. Se [Azure Blob-egenskaper för datamängd](data-factory-azure-blob-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure-blobbdatauppsättning. 

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

#### <a name="azure-sql-dataset"></a>Azure SQL-datauppsättning
Du kan ange hello namnet på hello tabell i hello Azure SQL-databas som innehåller hello kopierade data från hello Azure Blob storage. Se [Azure SQL-egenskaper för datamängd](data-factory-azure-sql-connector.md#dataset-properties) mer information om toodefine JSON-egenskaper som används för en Azure SQL-datamängd. 

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

#### <a name="data-pipeline"></a>Datapipeline
Du kan definiera en pipeline som kopierar data från hello Azure dataset toohello SQL Azure-blobbdatauppsättning. Se [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json) beskrivningar av JSON-element som används toodefine en pipeline i det här exemplet. 

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

## <a name="reuse-hello-template"></a>Återanvända hello mall
I hello självstudierna kommer skapat du en mall för att definiera Data Factory entiteter och en mall för att skicka värden för parametrar. hello pipeline kopierar data från en Azure Storage-konto tooan Azure SQL database angetts via parametrar. toouse hello samma mall toodeploy Data Factory entiteter toodifferent miljöer kan du skapa en parameterfil för varje miljö och använda den när du distribuerar toothat miljö.     

Exempel:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

Observera att hello första kommandot använder parameterfil för hello utvecklingsmiljö, andra en för hello testmiljö och hello tredje en för hello-produktionsmiljö.  

Du kan även återanvända hello mallen tooperform återkommande uppgifter. Till exempel behöver du toocreate många datafabriker med en eller flera pipelines som implementerar hello samma logik men varje datafabriken använder olika konton för lagring och SQL-databas. I det här scenariot kan du använda hello samma mall i hello samma miljö (dev-, test- eller produktionsmiljö) med olika parametern filer toocreate datafabriker.   

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. hello innehåller följande tabell en lista över datakällor som stöds som källor och mål av hello kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

toolearn om hur toocopy till eller från en databas, klicka länken hello för hello datalager i hello tabell.
