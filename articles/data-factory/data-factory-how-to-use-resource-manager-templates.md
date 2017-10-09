---
title: aaaUse Resource Manager-mallar i Data Factory | Microsoft Docs
description: "Lär dig hur toocreate och använda Azure Resource Manager-mallar toocreate Data Factory entiteter."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 60d5dbd29494420006aed6d5bd9a10a63c36bec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-templates-toocreate-azure-data-factory-entities"></a>Använda mallar toocreate Azure Data Factory entiteter
## <a name="overview"></a>Översikt
När du använder Azure Data Factory för dina behov för integrering av data, kan du själv återanvända hello samma mönster mellan olika miljöer eller implementera hello samma uppgift upprepade gånger inom hello samma lösning. Mallar kan du implementera och hantera dessa scenarier på ett enkelt sätt. Mallar i Azure Data Factory är idealisk för scenarier som omfattar återanvändning och upprepas.

Överväg att hello situation där en organisation har 10 produktionsanläggningarna över hälsningsmeddelande. hello loggar från varje lagras i en separat lokal SQL Server-databas. hello företaget vill ha toobuild ett enda datalager i hello molnet för ad hoc-analys. Vill också toohave hello samma logik men olika konfigurationer för utveckling, test- och miljöer.

I det här fallet en uppgift måste toobe upprepas i hello samma miljö, men med olika värden över hello 10 datafabriker för varje anläggning. I praktiken **upprepning** finns. Templating tillåter hello abstraktion av den här allmänna flödet (det vill säga rörledningar med hello samma aktiviteter i varje data factory), men använder en separat parameter-fil för varje anläggning.

Eftersom hello organisationen vill toodeploy dessa 10 datafabriker flera gånger mellan olika miljöer, mallar kan dessutom använda detta **återanvändning** genom att använda en separat parameter filer för utveckling, testa, och produktionsmiljöer.

## <a name="templating-with-azure-resource-manager"></a>Templating med Azure Resource Manager
[Azure Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md#template-deployment) är ett bra sätt tooachieve templating i Azure Data Factory. Resource Manager-mallar kan du definiera hello-infrastrukturen och konfigurationen av din Azure-lösning via en JSON-fil. Eftersom Azure Resource Manager-Mallar fungerar med alla/de flesta Azure-tjänster, kan ofta använda tooeasily hantera alla resurser för dina Azure tillgångar. Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn mer om hello Resource Manager-mallar i allmänhet.

## <a name="tutorials"></a>Självstudier
Se hello följande självstudiekurser för stegvisa instruktioner toocreate Data Factory entiteter med hjälp av Resource Manager-mallar:

* [Självstudier: Skapa en pipeline toocopy data med hjälp av Azure Resource Manager-mall](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Självstudier: Skapa en pipeline tooprocess data med hjälp av Azure Resource Manager-mall](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory-mallar på GitHub
Kolla hello följande Azure Snabbstart mallar på GitHub:

* [Skapa ett Data factory toocopy data från Azure Blob Storage tooAzure SQL-databas](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Skapa en datafabrik med Hive aktivitet på Azure HDInsight-kluster](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [Skapa ett Data factory toocopy data från Salesforce tooAzure Blobbar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Skapa en datafabrik som länkar aktiviteter: kopierar data från en FTP-server tooAzure Blobbar anropar en hive-skript på en på-begäran HDInsight-kluster tootransform hello data och kopierar resultatet till Azure SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Känna sig fria tooshare dina Azure Data Factory-mallar på [Azure Snabbstart](https://azure.microsoft.com/documentation/templates/). Se toohello [bidrag guiden](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) när du arbetar med mallar som kan delas via den här databasen.

hello följande avsnitt innehåller information om hur du definierar Data Factory resurser i en Resource Manager-mall.

## <a name="defining-data-factory-resources-in-templates"></a>Definiera Data Factory resurser i mallar
hello översta mallen för att definiera en datafabrik är:

```JSON
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
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a>Definiera en datafabrik
Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
Hej dataFactoryName har definierats i ”variabler” som:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Definiera länkade tjänster

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Se [länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) eller [Compute länkade tjänster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) mer information om hello JSON-egenskaperna för hello specifik länkade tjänst som du vill toodeploy. hello ”dependsOn”-parametern anger namnet på hello motsvarande data factory. Ett exempel för att definiera en länkad tjänst för Azure Storage framgår hello JSON-definitionen:

### <a name="define-datasets"></a>Definiera datauppsättningar

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
Se för[stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) mer information om hello JSON-egenskaper för hello specifika dataset-typen gärna toodeploy. Obs hello ”dependsOn”-parametern anger namnet på hello motsvarande data factory och lagring länkade tjänsten. Ett exempel på definierar typen av datauppsättning för Azure-blobblagring visas i hello JSON-definitionen:

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a>Definiera pipelines

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

Se för[definierar pipelines](data-factory-create-pipelines.md#pipeline-json) för information om hello JSON-egenskaper för att definiera hello specifika pipeline och aktiviteter som du vill toodeploy. Obs hello ”dependsOn”-parametern anger namnet på hello data factory och motsvarande länkade tjänster eller datauppsättningar. Ett exempel på en pipeline som kopierar data från Azure Blob Storage tooAzure SQL-databasen visas i följande JSON-fragment hello:

```JSON
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
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a>Parameterisera Data Factory-mall
Metodtips för Parameterisera finns [bästa praxis för att skapa mallar för Azure Resource Manager](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) artikel. I allmänhet ska parametrar minimeras, särskilt om variabler kan användas i stället. Ange bara parametrar i hello följande scenarier:

* Inställningarna varierar beroende på miljö (exempel: utveckling, test och produktion)
* Hemligheter (till exempel lösenord)

Om du behöver toopull hemligheter från [Azure Key Vault](../key-vault/key-vault-get-started.md) när du distribuerar Azure Data Factory-enheter med hjälp av mallar kan du ange hello **nyckelvalv** och **hemliga namnet** enligt Hej följande exempel:

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> Exportera mallar för befintliga datafabriker inte stöds för närvarande ännu, är det i hello fungerar.
>
>
