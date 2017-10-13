---
title: "Självstudie: Skapa en pipeline med en Resource Manager-mall | Microsoft Docs"
description: "I de här självstudierna skapar du ett exempel på en Azure Data Factory-pipeline med hjälp av en Azure Resource Manager-mall. Pipelinen kopierar data från Azure Blob Storage till Azure SQL Database."
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
robots: noindex
ms.openlocfilehash: 85855a3696529eae4f977e9e75800d6fa32b7cc0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="tutorial-use-azure-resource-manager-template-to-create-a-data-factory-pipeline-to-copy-data"></a>Självstudier: Använd Azure Resource Manager-mall för att skapa Data Factory-pipeline för att kopiera data 
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

I den här självstudien får du se hur du skapar en Azure Data Factory med en Azure Resource Manager-mall. Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager. Det transformerar inte indata för att generera utdata. Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster).

I den här självstudien får du skapa en pipeline i en aktivitet: kopieringsaktivitet. Kopieringsaktiviteten kopierar data från källans datalager till mottagarens datalager. En lista över datakällor som stöds som källor och mottagare finns i [datalager som stöds](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Aktiviteten drivs av en globalt tillgänglig tjänst som kan kopiera data mellan olika datalager på ett säkert, tillförlitligt och skalbart sätt. Se artikeln [Dataförflyttningsaktiviteter](data-factory-data-movement-activities.md) för information om kopieringsaktiviteten.

En pipeline kan ha fler än en aktivitet. Du kan länka två aktiviteter (köra en aktivitet efter en annan) genom att ställa in datauppsättningen för utdata för en aktivitet som den inkommande datauppsättningen för den andra aktiviteten. Mer information finns i [flera aktiviteter i en pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> Datapipelinen i den här självstudien kopierar data från ett källdatalager till ett måldatalager. Om du vill se en självstudie som visar hur du omvandlar data med Azure Data Factory går du till [Tutorial: Build a pipeline to transform data using Hadoop cluster](data-factory-build-your-first-pipeline.md) (Självstudie: Bygg en pipeline för att omvandla data med Hadoop-kluster). 

## <a name="prerequisites"></a>Krav
* Gå igenom [Självstudier – Översikt och förutsättningar](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) och slutför **förutsättningsstegen**.
* Följ instruktionerna i artikeln [Så här installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) för att installera den senaste versionen av Azure PowerShell på datorn. I den här självstudien använder du PowerShell för att distribuera Data Factory-enheter. 
* (valfritt) Se [Redigera Azure Resource Manager-mallar](../../azure-resource-manager/resource-group-authoring-templates.md) om du vill lära dig mer om Azure Resource Manager-mallar.

## <a name="in-this-tutorial"></a>I den här självstudien
I den här självstudien får skapa du en datafabrik med följande Data Factory-enheter:

| Entitet | Beskrivning |
| --- | --- |
| Länkad Azure-lagringstjänst |Länkar ditt Azure Storage-konto till datafabriken. Azure Storage är källdatalagret och Azure SQL-databasen är det mottagande datalagret för kopieringsaktiviteten i självstudien. Det anger lagringskontot som innehåller indatan för kopieringsaktiviteten. |
| Länkad Azure SQL Database-tjänst |Länkar din Azure SQL-databas till datafabriken. Det anger Azure SQL-databasen som innehåller utdatan för kopieringsaktiviteten. |
| Indatauppsättning för Azure-blob |Hänvisar till den länkade Azure Storage-tjänsten. Den länkade tjänsten hänvisar till ett Azure Storage-konto och datauppsättningen för Azure-bloben anger behållaren, mappen och filnamnet i lagringsutrymmet som innehåller indatan. |
| Utdatauppsättning för Azure SQL |Hänvisar till den länkade Azure SQL-tjänsten. Den länkade Azure SQL-tjänsten refererar till en Azure SQL-server och SQL Azure-datauppsättningen anger namnet på den tabell som innehåller utdatan. |
| Datapipeline |Pipelinen har en aktivitet av typen Kopiera som använder Azure-blobdatauppsättningen som indata och Azure SQL-datauppsättningen som utdata. Kopieringsaktiviteten kopierar data från en Azure-blob till en tabell i Azure SQL-databasen. |

En datafabrik kan ha en eller flera pipelines. En pipeline kan innehålla en eller flera aktiviteter. Det finns två typer av aktiviteter: [dataflyttningsaktiviteter](data-factory-data-movement-activities.md) och [datatransformeringsaktiviteter](data-factory-data-transformation-activities.md). I den här självstudien får du skapa en pipeline i en aktivitet (kopieringsaktivitet).

![Kopiera Azure-blob till Azure SQL Database](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

Följande avsnitt innehåller den fullständiga Resource Manager-mallen för att definiera Data Factory-entiteter så att du snabbt kan gå igenom självstudien och testa mallen. Om du vill förstå hur varje Data Factory-entitet definieras kan du läsa avsnittet [Data Factory-entiteter i mallen](#data-factory-entities-in-the-template).

## <a name="data-factory-json-template"></a>Data Factory JSON-mall
Resource Manager-mallen på den högsta nivån för att definiera en datafabrik är: 

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
Skapa en JSON-fil med namnet **ADFTutorialARM.json** i mappen **C:\ADFGetStarted** med följande innehåll:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
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
                  "description": "Copy data frm Azure blob to Azure SQL",
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
Skapa en JSON-fil med namnet **ADFCopyTutorialARM-Parameters.json** som innehåller parametrar för Azure Resource Manager-mallen. 

> [!IMPORTANT]
> Ange namn och nyckel för Azure Storage-kontot och parametrarna storageAccountName och storageAccountKey.  
> 
> Ange Azure SQL-server, databas, användare och lösenord för parametrarna sqlServerName, databaseName, sqlServerUserName och sqlServerPassword.  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of the Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for the Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of the Azure SQL server>" },
        "databaseName": { "value": "<Name of the Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for the user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> Du kan ha separata JSON-filer med parametrar för utveckling, testning och produktionsmiljöer som du kan använda med samma Data Factory JSON-mall. Genom att använda ett Power Shell-skript kan du automatisera distributionen av Data Factory-entiteter i dessa miljöer.  
> 
> 

## <a name="create-data-factory"></a>Skapa en datafabrik
1. Starta **Azure PowerShell** och kör följande kommando:
   * Kör följande kommando och ange användarnamnet och lösenordet som du använder för att logga in på Azure-portalen.
   
    ```PowerShell
    Login-AzureRmAccount    
    ```  
   * Kör följande kommando för att visa alla prenumerationer för det här kontot.
   
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * Kör följande kommando för att välja den prenumeration som du vill arbeta med.
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. Kör följande kommando för att distribuera Data Factory-entiteter med hjälp av Resource Manager-mallen som du skapade i steg 1.

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Övervaka pipeline

1. Logga in på [Azure Portal](https://portal.azure.com) med ditt Azure-konto.
2. Klicka på **Datafabriker** på den vänstra menyn (eller) klicka på **Fler tjänster** och klicka på **Datafabriker** under kategorin **INFORMATION + ANALYS**.
   
    ![Menyn Datafabriker](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. På sidan **Datafabriker** kan du söka efter och hitta din datafabrik (AzureBlobToAzureSQLDatabaseDF). 
   
    ![Sök efter datafabrik](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Klicka på din Azure-datafabrik. Du kan se startsidan för datafabriken.
   
    ![Datafabrikens startsida](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. Se [Övervaka datauppsättningar och pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) för instruktioner om hur övervakar en pipeline och datauppsättningar som du har skapat i den här självstudien. Visual Studio stöder för närvarande inte övervakning av Data Factory-pipelines.
7. När sektorn har statusen **Klar** verifierar du att data har kopierats till tabellen **tom** i Azure SQL-databasen.


Se [Övervaka datauppsättningar och pipeline](data-factory-monitor-manage-pipelines.md) för instruktioner om hur du använder Azures portalblad till att övervaka pipelinen och datauppsättningar som du har skapat i den här självstudien.

Mer information om hur du använder övervaknings- och hanteringsappen för att övervaka dina data-pipelines finns i [Övervaka och hantera Azure Data Factory-pipelines med övervakningsappen](data-factory-monitor-manage-app.md).

## <a name="data-factory-entities-in-the-template"></a>Data Factory-entiteter i mallen
### <a name="define-data-factory"></a>Definiera en datafabrik
Du definierar en datafabrik i Resource Manager-mallen enligt följande exempel:  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

DataFactoryName definieras som: 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

Det är en unik sträng som baseras på resursgruppens ID.  

### <a name="defining-data-factory-entities"></a>Definiera Data Factory-entiteter
Följande Data Factory-entiteter har definierats i JSON-mallen: 

1. [Länkad Azure Storage-tjänst](#azure-storage-linked-service)
2. [Länkad Azure SQL-tjänst](#azure-sql-database-linked-service)
3. [Azure-blobdatauppsättning](#azure-blob-dataset)
4. [Azure SQL-datauppsättning](#azure-sql-dataset)
5. [Datapipeline med en kopieringsaktivitet](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Länkad Azure-lagringstjänst
AzureStorageLinkedService länkar ditt Azure Storage-konto till datafabriken. Du har skapat en behållare och överfört data till det här lagringskontot som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Du anger namnet och nyckeln för Azure Storage-kontot i det här avsnittet. Se [Länkad Azure Storage-tjänst](data-factory-azure-blob-connector.md#azure-storage-linked-service) om du vill ha information om JSON-egenskaper som används för att definiera en länkad Azure Storage-tjänst. 

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

connectionString använder parametrarna storageAccountName och storageAccountKey. Värdena för dessa parametrar skickades med hjälp av en konfigurationsfil. Definitionen använder också variabler: azureStorageLinkedService och dataFactoryName definieras i mallen. 

#### <a name="azure-sql-database-linked-service"></a>Länkad Azure SQL Database-tjänst
AzureSqlLinkedService länkar din Azure SQL-databas till datafabriken. Data som kopieras från blob-lagringen sparas i den här databasen. Du har skapat den tomma tabellen i den här databasen som en del av [förhandskraven](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Du anger Azure SQL-servernamnet, databasnamnet, användarnamnet och lösenordet i det här avsnittet. Se [Länkad Azure SQL-tjänst](data-factory-azure-sql-connector.md#linked-service-properties) om du vill ha information om JSON-egenskaper som används för att definiera en länkad Azure SQL-tjänst.  

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

connectionString använder parametrarna sqlservernamn, databaseName, sqlServerUserName och sqlServerPassword som skickas med hjälp av en konfigurationsfil. Definitionen använder också följande variabler från mallen: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure-blobdatauppsättning
Den länkade Azure storage-tjänsten anger anslutningssträngen som Data Factory-tjänsten använder vid körning för att ansluta till ditt Azure storage-konto. Du anger namnen på blob-behållare, mappar och filer som innehåller indatan i Azure-blobuppsättningsdefinitionen. Se [Egenskaper för Azure-blobdatauppsättning](data-factory-azure-blob-connector.md#dataset-properties) om du vill ha information om JSON-egenskaper som används för att definiera en Azure-blobdatauppsättning. 

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
Du anger namnet på tabellen i Azure SQL-databasen som innehåller kopierade data från Azure Blob Storage. Se [Egenskaper för Azure SQL-datauppsättning](data-factory-azure-sql-connector.md#dataset-properties) om du vill ha information om JSON-egenskaper som används för att definiera en Azure SQL-datauppsättning. 

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
Du definierar en pipeline som kopierar data från Azure-blobdatauppsättningen till Azure SQL-datauppsättningen. Se [Pipeline-JSON](data-factory-create-pipelines.md#pipeline-json) för beskrivningar av JSON-element som används för att definiera en pipeline i det här exemplet. 

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
              "description": "Copy data frm Azure blob to Azure SQL",
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

## <a name="reuse-the-template"></a>Återanvända mallen
I självstudien skapade du en mall för att definiera Data Factory-entiteter och en mall för att skicka värden för parametrar. Pipeline kopierar data från ett Azure Storage-konto till en Azure SQL-databas som har angetts via parametrar. Om du vill använda samma mall för att distribuera Data Factory-entiteter till olika miljöer skapar du en parameterfil för varje miljö och använder den när du distribuerar till den miljön.     

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

Observera att det första kommandot använder parameterfilen för utvecklingsmiljön, det andra för testmiljön och det tredje för produktionsmiljön.  

Du kan även återanvända mallen för att utföra upprepade uppgifter. Du behöver till exempel skapa många datafabriker med en eller flera pipelines som implementerar samma logik, men alla datafabriker använder olika konton för Storage och SQL Database. I det här scenariot använder du samma mall i samma miljö (utvecklings-, test- eller produktionsmiljö) med olika parameterfiler för att skapa datafabriker.   

## <a name="next-steps"></a>Nästa steg
I den här kursen används Azure blob storage som ett datalager för källa och en Azure SQL-databas som ett dataarkiv som mål i en kopieringsåtgärd. Följande tabell innehåller en lista över datalager som stöds som källor och mål av kopieringsaktiviteten: 

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

För mer information om hur du kopierar data till/från ett datalager klickar du på länken för datalagret i tabellen.
