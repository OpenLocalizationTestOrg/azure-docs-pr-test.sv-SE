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
# <a name="use-templates-toocreate-azure-data-factory-entities"></a><span data-ttu-id="2c8bd-103">Använda mallar toocreate Azure Data Factory entiteter</span><span class="sxs-lookup"><span data-stu-id="2c8bd-103">Use templates toocreate Azure Data Factory entities</span></span>
## <a name="overview"></a><span data-ttu-id="2c8bd-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2c8bd-104">Overview</span></span>
<span data-ttu-id="2c8bd-105">När du använder Azure Data Factory för dina behov för integrering av data, kan du själv återanvända hello samma mönster mellan olika miljöer eller implementera hello samma uppgift upprepade gånger inom hello samma lösning.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-105">While using Azure Data Factory for your data integration needs, you may find yourself reusing hello same pattern across different environments or implementing hello same task repetitively within hello same solution.</span></span> <span data-ttu-id="2c8bd-106">Mallar kan du implementera och hantera dessa scenarier på ett enkelt sätt.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-106">Templates help you implement and manage these scenarios in an easy manner.</span></span> <span data-ttu-id="2c8bd-107">Mallar i Azure Data Factory är idealisk för scenarier som omfattar återanvändning och upprepas.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-107">Templates in Azure Data Factory are ideal for scenarios that involve reusability and repetition.</span></span>

<span data-ttu-id="2c8bd-108">Överväg att hello situation där en organisation har 10 produktionsanläggningarna över hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-108">Consider hello situation where an organization has 10 manufacturing plants across hello world.</span></span> <span data-ttu-id="2c8bd-109">hello loggar från varje lagras i en separat lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-109">hello logs from each plant are stored in a separate on-premises SQL Server database.</span></span> <span data-ttu-id="2c8bd-110">hello företaget vill ha toobuild ett enda datalager i hello molnet för ad hoc-analys.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-110">hello company wants toobuild a single data warehouse in hello cloud for ad-hoc analytics.</span></span> <span data-ttu-id="2c8bd-111">Vill också toohave hello samma logik men olika konfigurationer för utveckling, test- och miljöer.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-111">It also wants toohave hello same logic but different configurations for development, test, and production environments.</span></span>

<span data-ttu-id="2c8bd-112">I det här fallet en uppgift måste toobe upprepas i hello samma miljö, men med olika värden över hello 10 datafabriker för varje anläggning.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-112">In this case, a task needs toobe repeated within hello same environment, but with different values across hello 10 data factories for each manufacturing plant.</span></span> <span data-ttu-id="2c8bd-113">I praktiken **upprepning** finns.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-113">In effect, **repetition** is present.</span></span> <span data-ttu-id="2c8bd-114">Templating tillåter hello abstraktion av den här allmänna flödet (det vill säga rörledningar med hello samma aktiviteter i varje data factory), men använder en separat parameter-fil för varje anläggning.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-114">Templating allows hello abstraction of this generic flow (that is, pipelines having hello same activities in each data factory), but uses a separate parameter file for each manufacturing plant.</span></span>

<span data-ttu-id="2c8bd-115">Eftersom hello organisationen vill toodeploy dessa 10 datafabriker flera gånger mellan olika miljöer, mallar kan dessutom använda detta **återanvändning** genom att använda en separat parameter filer för utveckling, testa, och produktionsmiljöer.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-115">Furthermore, as hello organization wants toodeploy these 10 data factories multiple times across different environments, templates can use this **reusability** by utilizing separate parameter files for development, test, and production environments.</span></span>

## <a name="templating-with-azure-resource-manager"></a><span data-ttu-id="2c8bd-116">Templating med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="2c8bd-116">Templating with Azure Resource Manager</span></span>
<span data-ttu-id="2c8bd-117">[Azure Resource Manager-mallar](../azure-resource-manager/resource-group-overview.md#template-deployment) är ett bra sätt tooachieve templating i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-117">[Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md#template-deployment) are a great way tooachieve templating in Azure Data Factory.</span></span> <span data-ttu-id="2c8bd-118">Resource Manager-mallar kan du definiera hello-infrastrukturen och konfigurationen av din Azure-lösning via en JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-118">Resource Manager templates define hello infrastructure and configuration of your Azure solution through a JSON file.</span></span> <span data-ttu-id="2c8bd-119">Eftersom Azure Resource Manager-Mallar fungerar med alla/de flesta Azure-tjänster, kan ofta använda tooeasily hantera alla resurser för dina Azure tillgångar.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-119">Because Azure Resource Manager templates work with all/most Azure services, it can be widely used tooeasily manage all resources of your Azure assets.</span></span> <span data-ttu-id="2c8bd-120">Se [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) toolearn mer om hello Resource Manager-mallar i allmänhet.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-120">See [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn more about hello Resource Manager Templates in general.</span></span>

## <a name="tutorials"></a><span data-ttu-id="2c8bd-121">Självstudier</span><span class="sxs-lookup"><span data-stu-id="2c8bd-121">Tutorials</span></span>
<span data-ttu-id="2c8bd-122">Se hello följande självstudiekurser för stegvisa instruktioner toocreate Data Factory entiteter med hjälp av Resource Manager-mallar:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-122">See hello following tutorials for step-by-step instructions toocreate Data Factory entities by using Resource Manager templates:</span></span>

* [<span data-ttu-id="2c8bd-123">Självstudier: Skapa en pipeline toocopy data med hjälp av Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="2c8bd-123">Tutorial: Create a pipeline toocopy data by using Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="2c8bd-124">Självstudier: Skapa en pipeline tooprocess data med hjälp av Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="2c8bd-124">Tutorial: Create a pipeline tooprocess data by using Azure Resource Manager template</span></span>](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a><span data-ttu-id="2c8bd-125">Data Factory-mallar på GitHub</span><span class="sxs-lookup"><span data-stu-id="2c8bd-125">Data Factory templates on GitHub</span></span>
<span data-ttu-id="2c8bd-126">Kolla hello följande Azure Snabbstart mallar på GitHub:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-126">Check out hello following Azure quick start templates on GitHub:</span></span>

* [<span data-ttu-id="2c8bd-127">Skapa ett Data factory toocopy data från Azure Blob Storage tooAzure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="2c8bd-127">Create a Data factory toocopy data from Azure Blob Storage tooAzure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [<span data-ttu-id="2c8bd-128">Skapa en datafabrik med Hive aktivitet på Azure HDInsight-kluster</span><span class="sxs-lookup"><span data-stu-id="2c8bd-128">Create a Data factory with Hive activity on Azure HDInsight cluster</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [<span data-ttu-id="2c8bd-129">Skapa ett Data factory toocopy data från Salesforce tooAzure Blobbar</span><span class="sxs-lookup"><span data-stu-id="2c8bd-129">Create a Data factory toocopy data from Salesforce tooAzure Blobs</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [<span data-ttu-id="2c8bd-130">Skapa en datafabrik som länkar aktiviteter: kopierar data från en FTP-server tooAzure Blobbar anropar en hive-skript på en på-begäran HDInsight-kluster tootransform hello data och kopierar resultatet till Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="2c8bd-130">Create a Data factory that chains activities: copies data from an FTP server tooAzure Blobs, invokes a hive script on an on-demand HDInsight cluster tootransform hello data, and copies result into Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

<span data-ttu-id="2c8bd-131">Känna sig fria tooshare dina Azure Data Factory-mallar på [Azure Snabbstart](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="2c8bd-131">Feel free tooshare your Azure Data Factory templates at [Azure Quick start](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="2c8bd-132">Se toohello [bidrag guiden](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) när du arbetar med mallar som kan delas via den här databasen.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-132">Refer toohello [contribution guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) while developing templates that can be shared via this repository.</span></span>

<span data-ttu-id="2c8bd-133">hello följande avsnitt innehåller information om hur du definierar Data Factory resurser i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-133">hello following sections provide details about defining Data Factory resources in a Resource Manager template.</span></span>

## <a name="defining-data-factory-resources-in-templates"></a><span data-ttu-id="2c8bd-134">Definiera Data Factory resurser i mallar</span><span class="sxs-lookup"><span data-stu-id="2c8bd-134">Defining Data Factory resources in templates</span></span>
<span data-ttu-id="2c8bd-135">hello översta mallen för att definiera en datafabrik är:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-135">hello top-level template for defining a data factory is:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="2c8bd-136">Definiera en datafabrik</span><span class="sxs-lookup"><span data-stu-id="2c8bd-136">Define data factory</span></span>
<span data-ttu-id="2c8bd-137">Du kan definiera en datafabrik i hello Resource Manager-mall som visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-137">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
<span data-ttu-id="2c8bd-138">Hej dataFactoryName har definierats i ”variabler” som:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-138">hello dataFactoryName is defined in “variables” as:</span></span>

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a><span data-ttu-id="2c8bd-139">Definiera länkade tjänster</span><span class="sxs-lookup"><span data-stu-id="2c8bd-139">Define linked services</span></span>

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

<span data-ttu-id="2c8bd-140">Se [länkade lagringstjänsten](data-factory-azure-blob-connector.md#azure-storage-linked-service) eller [Compute länkade tjänster](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) mer information om hello JSON-egenskaperna för hello specifik länkade tjänst som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-140">See [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [Compute Linked Services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details about hello JSON properties for hello specific linked service you wish toodeploy.</span></span> <span data-ttu-id="2c8bd-141">hello ”dependsOn”-parametern anger namnet på hello motsvarande data factory.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-141">hello “dependsOn” parameter specifies name of hello corresponding data factory.</span></span> <span data-ttu-id="2c8bd-142">Ett exempel för att definiera en länkad tjänst för Azure Storage framgår hello JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-142">An example of defining a linked service for Azure Storage is shown in hello following JSON definition:</span></span>

### <a name="define-datasets"></a><span data-ttu-id="2c8bd-143">Definiera datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="2c8bd-143">Define datasets</span></span>

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
<span data-ttu-id="2c8bd-144">Se för[stöds datalager](data-factory-data-movement-activities.md#supported-data-stores-and-formats) mer information om hello JSON-egenskaper för hello specifika dataset-typen gärna toodeploy.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-144">Refer too[Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for details about hello JSON properties for hello specific dataset type you wish toodeploy.</span></span> <span data-ttu-id="2c8bd-145">Obs hello ”dependsOn”-parametern anger namnet på hello motsvarande data factory och lagring länkade tjänsten.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-145">Note hello “dependsOn” parameter specifies name of hello corresponding data factory and storage linked service.</span></span> <span data-ttu-id="2c8bd-146">Ett exempel på definierar typen av datauppsättning för Azure-blobblagring visas i hello JSON-definitionen:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-146">An example of defining dataset type of Azure blob storage is shown in hello following JSON definition:</span></span>

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

### <a name="define-pipelines"></a><span data-ttu-id="2c8bd-147">Definiera pipelines</span><span class="sxs-lookup"><span data-stu-id="2c8bd-147">Define pipelines</span></span>

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

<span data-ttu-id="2c8bd-148">Se för[definierar pipelines](data-factory-create-pipelines.md#pipeline-json) för information om hello JSON-egenskaper för att definiera hello specifika pipeline och aktiviteter som du vill toodeploy.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-148">Refer too[defining pipelines](data-factory-create-pipelines.md#pipeline-json) for details about hello JSON properties for defining hello specific pipeline and activities you wish toodeploy.</span></span> <span data-ttu-id="2c8bd-149">Obs hello ”dependsOn”-parametern anger namnet på hello data factory och motsvarande länkade tjänster eller datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-149">Note hello “dependsOn” parameter specifies name of hello data factory, and any corresponding linked services or datasets.</span></span> <span data-ttu-id="2c8bd-150">Ett exempel på en pipeline som kopierar data från Azure Blob Storage tooAzure SQL-databasen visas i följande JSON-fragment hello:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-150">An example of a pipeline that copies data from Azure Blob Storage tooAzure SQL Database is shown in hello following JSON snippet:</span></span>

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
## <a name="parameterizing-data-factory-template"></a><span data-ttu-id="2c8bd-151">Parameterisera Data Factory-mall</span><span class="sxs-lookup"><span data-stu-id="2c8bd-151">Parameterizing Data Factory template</span></span>
<span data-ttu-id="2c8bd-152">Metodtips för Parameterisera finns [bästa praxis för att skapa mallar för Azure Resource Manager](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) artikel.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-152">For best practices on parameterizing, see [Best practices for creating Azure Resource Manager templates](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) article.</span></span> <span data-ttu-id="2c8bd-153">I allmänhet ska parametrar minimeras, särskilt om variabler kan användas i stället.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-153">In general, parameter usage should be minimized, especially if variables can be used instead.</span></span> <span data-ttu-id="2c8bd-154">Ange bara parametrar i hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-154">Only provide parameters in hello following scenarios:</span></span>

* <span data-ttu-id="2c8bd-155">Inställningarna varierar beroende på miljö (exempel: utveckling, test och produktion)</span><span class="sxs-lookup"><span data-stu-id="2c8bd-155">Settings vary by environment (example: development, test, and production)</span></span>
* <span data-ttu-id="2c8bd-156">Hemligheter (till exempel lösenord)</span><span class="sxs-lookup"><span data-stu-id="2c8bd-156">Secrets (such as passwords)</span></span>

<span data-ttu-id="2c8bd-157">Om du behöver toopull hemligheter från [Azure Key Vault](../key-vault/key-vault-get-started.md) när du distribuerar Azure Data Factory-enheter med hjälp av mallar kan du ange hello **nyckelvalv** och **hemliga namnet** enligt Hej följande exempel:</span><span class="sxs-lookup"><span data-stu-id="2c8bd-157">If you need toopull secrets from [Azure Key Vault](../key-vault/key-vault-get-started.md) when deploying Azure Data Factory entities using templates, specify hello **key vault** and **secret name** as shown in hello following example:</span></span>

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
> <span data-ttu-id="2c8bd-158">Exportera mallar för befintliga datafabriker inte stöds för närvarande ännu, är det i hello fungerar.</span><span class="sxs-lookup"><span data-stu-id="2c8bd-158">While exporting templates for existing data factories is currently not yet supported, it is in hello works.</span></span>
>
>
