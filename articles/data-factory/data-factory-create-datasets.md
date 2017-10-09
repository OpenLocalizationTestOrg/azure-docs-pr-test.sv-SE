---
title: "aaaCreate datauppsättningar i Azure Data Factory | Microsoft Docs"
description: "Lär dig hur toocreate datauppsättningar i Azure Data Factory med exempel på egenskaper som förskjutning och anchorDateTime."
keywords: Skapa dataset, dataset exempel, offset exempel
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 0614cd24-2ff0-49d3-9301-06052fd4f92a
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: shlo
ms.openlocfilehash: 181859ed250595d756df73e9ebcac08d9e7184c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="datasets-in-azure-data-factory"></a><span data-ttu-id="0576e-104">Datauppsättningar i Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="0576e-104">Datasets in Azure Data Factory</span></span>
<span data-ttu-id="0576e-105">Den här artikeln beskriver vilka datauppsättningar är hur de har definierats i JSON-format och hur de används i Azure Data Factory rörledningar.</span><span class="sxs-lookup"><span data-stu-id="0576e-105">This article describes what datasets are, how they are defined in JSON format, and how they are used in Azure Data Factory pipelines.</span></span> <span data-ttu-id="0576e-106">Den innehåller information om varje avsnitt (till exempel struktur, tillgänglighet och princip) i hello dataset JSON-definitionen.</span><span class="sxs-lookup"><span data-stu-id="0576e-106">It provides details about each section (for example, structure, availability, and policy) in hello dataset JSON definition.</span></span> <span data-ttu-id="0576e-107">hello artikeln innehåller även exempel för att använda hello **offset**, **anchorDateTime**, och **style** egenskaper i en dataset JSON-definition.</span><span class="sxs-lookup"><span data-stu-id="0576e-107">hello article also provides examples for using hello **offset**, **anchorDateTime**, and **style** properties in a dataset JSON definition.</span></span>

> [!NOTE]
> <span data-ttu-id="0576e-108">Om du är ny tooData Factory finns [introduktion tooAzure Data Factory](data-factory-introduction.md) en översikt.</span><span class="sxs-lookup"><span data-stu-id="0576e-108">If you are new tooData Factory, see [Introduction tooAzure Data Factory](data-factory-introduction.md) for an overview.</span></span> <span data-ttu-id="0576e-109">Om du inte har praktisk erfarenhet av att skapa datafabriker kan du få en bättre förståelse av läsning hello [data transformation kursen](data-factory-build-your-first-pipeline.md) och hello [data movement kursen](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-109">If you do not have hands-on experience with creating data factories, you can gain a better understanding by reading hello [data transformation tutorial](data-factory-build-your-first-pipeline.md) and hello [data movement tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> 

## <a name="overview"></a><span data-ttu-id="0576e-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="0576e-110">Overview</span></span>
<span data-ttu-id="0576e-111">En datafabrik kan ha en eller flera pipelines.</span><span class="sxs-lookup"><span data-stu-id="0576e-111">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="0576e-112">En **pipeline** är en logisk gruppering av **aktiviteter** som tillsammans utföra en uppgift.</span><span class="sxs-lookup"><span data-stu-id="0576e-112">A **pipeline** is a logical grouping of **activities** that together perform a task.</span></span> <span data-ttu-id="0576e-113">hello aktiviteter i en pipeline definiera åtgärder tooperform på dina data.</span><span class="sxs-lookup"><span data-stu-id="0576e-113">hello activities in a pipeline define actions tooperform on your data.</span></span> <span data-ttu-id="0576e-114">Du kan till exempel använda en aktivitet toocopy data från en lokal SQL Server-tooAzure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0576e-114">For example, you might use a copy activity toocopy data from an on-premises SQL Server tooAzure Blob storage.</span></span> <span data-ttu-id="0576e-115">Du kan sedan använda en Hive-aktivitet som kör en Hive-skript på en Azure HDInsight-kluster tooprocess data från Blob storage tooproduce utdata.</span><span class="sxs-lookup"><span data-stu-id="0576e-115">Then, you might use a Hive activity that runs a Hive script on an Azure HDInsight cluster tooprocess data from Blob storage tooproduce output data.</span></span> <span data-ttu-id="0576e-116">Slutligen kan du använda en andra kopia aktiviteten toocopy hello utgående data tooAzure SQL Data Warehouse ovanpå vilka business intelligence (BI) reporting lösningar har skapats.</span><span class="sxs-lookup"><span data-stu-id="0576e-116">Finally, you might use a second copy activity toocopy hello output data tooAzure SQL Data Warehouse, on top of which business intelligence (BI) reporting solutions are built.</span></span> <span data-ttu-id="0576e-117">Mer information om pipelines och aktiviteter finns [Pipelines och aktiviteter i Azure Data Factory](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-117">For more information about pipelines and activities, see [Pipelines and activities in Azure Data Factory](data-factory-create-pipelines.md).</span></span>

<span data-ttu-id="0576e-118">En aktivitet kan ta noll eller fler indata **datauppsättningar**, och skapa en eller flera utdata-datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="0576e-118">An activity can take zero or more input **datasets**, and produce one or more output datasets.</span></span> <span data-ttu-id="0576e-119">En inkommande datauppsättning representerar hello indata för en aktivitet i pipelinen hello och en datamängd för utdata representerar hello utdata för hello-aktivitet.</span><span class="sxs-lookup"><span data-stu-id="0576e-119">An input dataset represents hello input for an activity in hello pipeline, and an output dataset represents hello output for hello activity.</span></span> <span data-ttu-id="0576e-120">Datauppsättningar identifierar data inom olika datalager, till exempel tabeller, filer, mappar och dokument.</span><span class="sxs-lookup"><span data-stu-id="0576e-120">Datasets identify data within different data stores, such as tables, files, folders, and documents.</span></span> <span data-ttu-id="0576e-121">Azure-blobbdatauppsättning anger hello blob-behållaren och mappen i Blob storage från vilka hello pipeline bör du läsa hello data.</span><span class="sxs-lookup"><span data-stu-id="0576e-121">For example, an Azure Blob dataset specifies hello blob container and folder in Blob storage from which hello pipeline should read hello data.</span></span> 

<span data-ttu-id="0576e-122">Innan du skapar en datamängd, skapa en **länkade tjänsten** toolink dina data lagras toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="0576e-122">Before you create a dataset, create a **linked service** toolink your data store toohello data factory.</span></span> <span data-ttu-id="0576e-123">Länkade tjänster liknar anslutningssträngar som definierar hello anslutningsinformation som behövs för Data Factory tooconnect tooexternal resurser.</span><span class="sxs-lookup"><span data-stu-id="0576e-123">Linked services are much like connection strings, which define hello connection information needed for Data Factory tooconnect tooexternal resources.</span></span> <span data-ttu-id="0576e-124">Datauppsättningar identifiera data inom hello länkade datalager, till exempel SQL-tabeller, filer, mappar och dokument.</span><span class="sxs-lookup"><span data-stu-id="0576e-124">Datasets identify data within hello linked data stores, such as SQL tables, files, folders, and documents.</span></span> <span data-ttu-id="0576e-125">Till exempel länkade ett Azure Storage tjänsten länkar en storage-konto toohello data factory.</span><span class="sxs-lookup"><span data-stu-id="0576e-125">For example, an Azure Storage linked service links a storage account toohello data factory.</span></span> <span data-ttu-id="0576e-126">Azure-blobbdatauppsättning representerar hello blob-behållaren och hello-mapp som innehåller hello inkommande blobbar toobe bearbetas.</span><span class="sxs-lookup"><span data-stu-id="0576e-126">An Azure Blob dataset represents hello blob container and hello folder that contains hello input blobs toobe processed.</span></span> 

<span data-ttu-id="0576e-127">Här är ett exempelscenario.</span><span class="sxs-lookup"><span data-stu-id="0576e-127">Here is a sample scenario.</span></span> <span data-ttu-id="0576e-128">toocopy data från Blob storage tooa SQL-databas, som du skapar två länkade tjänster: Azure Storage- och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0576e-128">toocopy data from Blob storage tooa SQL database, you create two linked services: Azure Storage and Azure SQL Database.</span></span> <span data-ttu-id="0576e-129">Skapa sedan två datamängder: Azure Blob-datauppsättningen (varvid refererar toohello länkad Azure Storage service) och Azure SQL-tabell datauppsättningen (varvid refererar toohello Azure SQL Database länkad tjänst).</span><span class="sxs-lookup"><span data-stu-id="0576e-129">Then, create two datasets: Azure Blob dataset (which refers toohello Azure Storage linked service) and Azure SQL Table dataset (which refers toohello Azure SQL Database linked service).</span></span> <span data-ttu-id="0576e-130">hello Azure Storage- och Azure SQL Database länkade tjänster innehålla anslutningssträngar som Data Factory använder respektive vid körning tooconnect tooyour Azure Storage och Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="0576e-130">hello Azure Storage and Azure SQL Database linked services contain connection strings that Data Factory uses at runtime tooconnect tooyour Azure Storage and Azure SQL Database, respectively.</span></span> <span data-ttu-id="0576e-131">hello Azure-blobbdatauppsättning anger hello blob-behållaren och blob-mapp som innehåller hello inkommande blobbar i Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0576e-131">hello Azure Blob dataset specifies hello blob container and blob folder that contains hello input blobs in your Blob storage.</span></span> <span data-ttu-id="0576e-132">hello Azure SQL-tabell dataset anger hello SQL-tabell i SQL-databasen toowhich hello data toobe kopieras.</span><span class="sxs-lookup"><span data-stu-id="0576e-132">hello Azure SQL Table dataset specifies hello SQL table in your SQL database toowhich hello data is toobe copied.</span></span>

<span data-ttu-id="0576e-133">hello följande diagram visar hello relationerna mellan pipeline, aktivitet, datamängd och länkade tjänsten i Data Factory:</span><span class="sxs-lookup"><span data-stu-id="0576e-133">hello following diagram shows hello relationships among pipeline, activity, dataset, and linked service in Data Factory:</span></span> 

![Förhållandet mellan pipeline, aktivitet, dataset, länkade tjänster](media/data-factory-create-datasets/relationship-between-data-factory-entities.png)

## <a name="dataset-json"></a><span data-ttu-id="0576e-135">Datauppsättnings-JSON</span><span class="sxs-lookup"><span data-stu-id="0576e-135">Dataset JSON</span></span>
<span data-ttu-id="0576e-136">En datamängd i Data Factory har definierats i JSON-format på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0576e-136">A dataset in Data Factory is defined in JSON format as follows:</span></span>

```json
{
    "name": "<name of dataset>",
    "properties": {
        "type": "<type of dataset: AzureBlob, AzureSql etc...>",
        "external": <boolean flag tooindicate external data. only for input datasets>,
        "linkedServiceName": "<Name of hello linked service that refers tooa data store.>",
        "structure": [
            {
                "name": "<Name of hello column>",
                "type": "<Name of hello type>"
            }
        ],
        "typeProperties": {
            "<type specific property>": "<value>",
            "<type specific property 2>": "<value 2>",
        },
        "availability": {
            "frequency": "<Specifies hello time unit for data slice production. Supported frequency: Minute, Hour, Day, Week, Month>",
            "interval": "<Specifies hello interval within hello defined frequency. For example, frequency set too'Hour' and interval set too1 indicates that new data slices should be produced hourly>"
        },
       "policy":
        {      
        }
    }
}
```

<span data-ttu-id="0576e-137">hello följande tabell beskriver egenskaper i hello ovan JSON:</span><span class="sxs-lookup"><span data-stu-id="0576e-137">hello following table describes properties in hello above JSON:</span></span>   

| <span data-ttu-id="0576e-138">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0576e-138">Property</span></span> | <span data-ttu-id="0576e-139">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0576e-139">Description</span></span> | <span data-ttu-id="0576e-140">Krävs</span><span class="sxs-lookup"><span data-stu-id="0576e-140">Required</span></span> | <span data-ttu-id="0576e-141">Standard</span><span class="sxs-lookup"><span data-stu-id="0576e-141">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0576e-142">namn</span><span class="sxs-lookup"><span data-stu-id="0576e-142">name</span></span> |<span data-ttu-id="0576e-143">Namnet på hello dataset.</span><span class="sxs-lookup"><span data-stu-id="0576e-143">Name of hello dataset.</span></span> <span data-ttu-id="0576e-144">Se [Azure Data Factory - namngivningsregler](data-factory-naming-rules.md) för namngivningsregler.</span><span class="sxs-lookup"><span data-stu-id="0576e-144">See [Azure Data Factory - Naming rules](data-factory-naming-rules.md) for naming rules.</span></span> |<span data-ttu-id="0576e-145">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-145">Yes</span></span> |<span data-ttu-id="0576e-146">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-146">NA</span></span> |
| <span data-ttu-id="0576e-147">typ</span><span class="sxs-lookup"><span data-stu-id="0576e-147">type</span></span> |<span data-ttu-id="0576e-148">typ av hello dataset.</span><span class="sxs-lookup"><span data-stu-id="0576e-148">Type of hello dataset.</span></span> <span data-ttu-id="0576e-149">Ange en hello-typer som stöds av Data Factory (till exempel: AzureBlob, AzureSqlTable).</span><span class="sxs-lookup"><span data-stu-id="0576e-149">Specify one of hello types supported by Data Factory (for example: AzureBlob, AzureSqlTable).</span></span> <br/><br/><span data-ttu-id="0576e-150">Mer information finns i [datauppsättningstypen](#Type).</span><span class="sxs-lookup"><span data-stu-id="0576e-150">For details, see [Dataset type](#Type).</span></span> |<span data-ttu-id="0576e-151">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-151">Yes</span></span> |<span data-ttu-id="0576e-152">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-152">NA</span></span> |
| <span data-ttu-id="0576e-153">struktur</span><span class="sxs-lookup"><span data-stu-id="0576e-153">structure</span></span> |<span data-ttu-id="0576e-154">Schemat för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="0576e-154">Schema of hello dataset.</span></span><br/><br/><span data-ttu-id="0576e-155">Mer information finns i [datauppsättningsstrukturen](#Structure).</span><span class="sxs-lookup"><span data-stu-id="0576e-155">For details, see [Dataset structure](#Structure).</span></span> |<span data-ttu-id="0576e-156">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-156">No</span></span> |<span data-ttu-id="0576e-157">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-157">NA</span></span> |
| <span data-ttu-id="0576e-158">typeProperties</span><span class="sxs-lookup"><span data-stu-id="0576e-158">typeProperties</span></span> | <span data-ttu-id="0576e-159">hello Typegenskaper är olika för varje typ (till exempel: Azure Blob, Azure SQL-tabell).</span><span class="sxs-lookup"><span data-stu-id="0576e-159">hello type properties are different for each type (for example: Azure Blob, Azure SQL table).</span></span> <span data-ttu-id="0576e-160">Mer information om hello stöds typer och egenskaper finns [datauppsättningstypen](#Type).</span><span class="sxs-lookup"><span data-stu-id="0576e-160">For details on hello supported types and their properties, see [Dataset type](#Type).</span></span> |<span data-ttu-id="0576e-161">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-161">Yes</span></span> |<span data-ttu-id="0576e-162">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-162">NA</span></span> |
| <span data-ttu-id="0576e-163">extern</span><span class="sxs-lookup"><span data-stu-id="0576e-163">external</span></span> | <span data-ttu-id="0576e-164">Boolesk flagga toospecify om en datamängd uttryckligen produceras av en data factory-pipelinen eller inte.</span><span class="sxs-lookup"><span data-stu-id="0576e-164">Boolean flag toospecify whether a dataset is explicitly produced by a data factory pipeline or not.</span></span> <span data-ttu-id="0576e-165">Ange den här flaggan tootrue om hello inkommande datamängden för en aktivitet inte tillverkas av hello aktuella pipeline.</span><span class="sxs-lookup"><span data-stu-id="0576e-165">If hello input dataset for an activity is not produced by hello current pipeline, set this flag tootrue.</span></span> <span data-ttu-id="0576e-166">Ange den här flaggan tootrue för hello inkommande dataset hello första aktivitet i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="0576e-166">Set this flag tootrue for hello input dataset of hello first activity in hello pipeline.</span></span>  |<span data-ttu-id="0576e-167">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-167">No</span></span> |<span data-ttu-id="0576e-168">FALSKT</span><span class="sxs-lookup"><span data-stu-id="0576e-168">false</span></span> |
| <span data-ttu-id="0576e-169">availability</span><span class="sxs-lookup"><span data-stu-id="0576e-169">availability</span></span> | <span data-ttu-id="0576e-170">Definierar hello bearbetning fönstret (till exempel varje timme eller varje dag) eller hello segmentering modellen för hello dataset produktion.</span><span class="sxs-lookup"><span data-stu-id="0576e-170">Defines hello processing window (for example, hourly or daily) or hello slicing model for hello dataset production.</span></span> <span data-ttu-id="0576e-171">Varje dataenhet förbrukats och produceras av en aktivitet körs kallas en datasektorn.</span><span class="sxs-lookup"><span data-stu-id="0576e-171">Each unit of data consumed and produced by an activity run is called a data slice.</span></span> <span data-ttu-id="0576e-172">Om hello tillgängligheten för en datamängd för utdata är uppsättningen toodaily (frekvens - dag, intervallet - 1), skapas en sektor dagligen.</span><span class="sxs-lookup"><span data-stu-id="0576e-172">If hello availability of an output dataset is set toodaily (frequency - Day, interval - 1), a slice is produced daily.</span></span> <br/><br/><span data-ttu-id="0576e-173">Mer information finns i [Dataset tillgänglighet](#Availability).</span><span class="sxs-lookup"><span data-stu-id="0576e-173">For details, see [Dataset availability](#Availability).</span></span> <br/><br/><span data-ttu-id="0576e-174">Mer information om hello dataset segmentering modellen finns hello [schemaläggning och körning](data-factory-scheduling-and-execution.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="0576e-174">For details on hello dataset slicing model, see hello [Scheduling and execution](data-factory-scheduling-and-execution.md) article.</span></span> |<span data-ttu-id="0576e-175">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-175">Yes</span></span> |<span data-ttu-id="0576e-176">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-176">NA</span></span> |
| <span data-ttu-id="0576e-177">policy</span><span class="sxs-lookup"><span data-stu-id="0576e-177">policy</span></span> |<span data-ttu-id="0576e-178">Definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="0576e-178">Defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span> <br/><br/><span data-ttu-id="0576e-179">Mer information finns i hello [Dataset princip](#Policy) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0576e-179">For details, see hello [Dataset policy](#Policy) section.</span></span> |<span data-ttu-id="0576e-180">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-180">No</span></span> |<span data-ttu-id="0576e-181">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-181">NA</span></span> |

## <a name="dataset-example"></a><span data-ttu-id="0576e-182">DataSet-exempel</span><span class="sxs-lookup"><span data-stu-id="0576e-182">Dataset example</span></span>
<span data-ttu-id="0576e-183">I följande exempel hello, hello datauppsättning representerar en tabell med namnet **mytable prefix** i en SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0576e-183">In hello following example, hello dataset represents a table named **MyTable** in a SQL database.</span></span>

```json
{
    "name": "DatasetSample",
    "properties": {
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties":
        {
            "tableName": "MyTable"
        },
        "availability":
        {
            "frequency": "Day",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="0576e-184">Observera följande punkter hello:</span><span class="sxs-lookup"><span data-stu-id="0576e-184">Note hello following points:</span></span>

* <span data-ttu-id="0576e-185">**typen** tooAzureSqlTable anges.</span><span class="sxs-lookup"><span data-stu-id="0576e-185">**type** is set tooAzureSqlTable.</span></span>
* <span data-ttu-id="0576e-186">**tableName** typegenskapen (specifika tooAzureSqlTable typ) anges tooMyTable.</span><span class="sxs-lookup"><span data-stu-id="0576e-186">**tableName** type property (specific tooAzureSqlTable type) is set tooMyTable.</span></span>
* <span data-ttu-id="0576e-187">**linkedServiceName** refererar tooa länkad tjänst av typen AzureSqlDatabase som definieras i hello nästa JSON-fragment.</span><span class="sxs-lookup"><span data-stu-id="0576e-187">**linkedServiceName** refers tooa linked service of type AzureSqlDatabase, which is defined in hello next JSON snippet.</span></span> 
* <span data-ttu-id="0576e-188">**tillgänglighet frekvens** anges tooDay, och **intervall** too1 anges.</span><span class="sxs-lookup"><span data-stu-id="0576e-188">**availability frequency** is set tooDay, and **interval** is set too1.</span></span> <span data-ttu-id="0576e-189">Det innebär att hello datamängdssektor skapas varje dag.</span><span class="sxs-lookup"><span data-stu-id="0576e-189">This means that hello dataset slice is produced daily.</span></span>  

<span data-ttu-id="0576e-190">**AzureSqlLinkedService** definieras enligt följande:</span><span class="sxs-lookup"><span data-stu-id="0576e-190">**AzureSqlLinkedService** is defined as follows:</span></span>

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30"
        }
    }
}
```

<span data-ttu-id="0576e-191">I föregående JSON fragment hello:</span><span class="sxs-lookup"><span data-stu-id="0576e-191">In hello preceding JSON snippet:</span></span>

* <span data-ttu-id="0576e-192">**typen** tooAzureSqlDatabase anges.</span><span class="sxs-lookup"><span data-stu-id="0576e-192">**type** is set tooAzureSqlDatabase.</span></span>
* <span data-ttu-id="0576e-193">**connectionString** typegenskapen anger information tooconnect tooa SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0576e-193">**connectionString** type property specifies information tooconnect tooa SQL database.</span></span>  

<span data-ttu-id="0576e-194">Som du ser hello länkad tjänst definierar hur tooconnect tooa SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="0576e-194">As you can see, hello linked service defines how tooconnect tooa SQL database.</span></span> <span data-ttu-id="0576e-195">hello dataset definierar vilka tabellen används som indata och utdata för hello aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="0576e-195">hello dataset defines what table is used as an input and output for hello activity in a pipeline.</span></span>   

> [!IMPORTANT]
> <span data-ttu-id="0576e-196">Om ett DataSet-objekt produceras av hello pipeline, bör det markeras som **externa**.</span><span class="sxs-lookup"><span data-stu-id="0576e-196">Unless a dataset is being produced by hello pipeline, it should be marked as **external**.</span></span> <span data-ttu-id="0576e-197">Den här inställningen gäller vanligtvis tooinputs första aktivitet i en pipeline.</span><span class="sxs-lookup"><span data-stu-id="0576e-197">This setting generally applies tooinputs of first activity in a pipeline.</span></span>   


## <span data-ttu-id="0576e-198"><a name="Type"></a>Typen av datauppsättning</span><span class="sxs-lookup"><span data-stu-id="0576e-198"><a name="Type"></a> Dataset type</span></span>
<span data-ttu-id="0576e-199">hello beror hello dataset på hello dataarkiv som du använder.</span><span class="sxs-lookup"><span data-stu-id="0576e-199">hello type of hello dataset depends on hello data store you use.</span></span> <span data-ttu-id="0576e-200">Se följande tabell för en lista över datakällor som stöds av Data Factory hello.</span><span class="sxs-lookup"><span data-stu-id="0576e-200">See hello following table for a list of data stores supported by Data Factory.</span></span> <span data-ttu-id="0576e-201">Klicka på en data store toolearn hur toocreate en länkad tjänst och en datauppsättning för dessa data lagras.</span><span class="sxs-lookup"><span data-stu-id="0576e-201">Click a data store toolearn how toocreate a linked service and a dataset for that data store.</span></span>

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

> [!NOTE]
> <span data-ttu-id="0576e-202">Data som lagras med * kan vara lokalt eller på Azure-infrastrukturen som en tjänst (IaaS).</span><span class="sxs-lookup"><span data-stu-id="0576e-202">Data stores with * can be on-premises or on Azure infrastructure as a service (IaaS).</span></span> <span data-ttu-id="0576e-203">Dessa datalager kräver tooinstall [Data Management Gateway](data-factory-data-management-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-203">These data stores require you tooinstall [Data Management Gateway](data-factory-data-management-gateway.md).</span></span>

<span data-ttu-id="0576e-204">I exemplet hello hello föregående avsnitt hello typ hello dataset är inställd för**AzureSqlTable**.</span><span class="sxs-lookup"><span data-stu-id="0576e-204">In hello example in hello previous section, hello type of hello dataset is set too**AzureSqlTable**.</span></span> <span data-ttu-id="0576e-205">På samma sätt för ett Azure-blobbdatauppsättning hello typ hello dataset är inställd för**AzureBlob**, enligt följande JSON hello:</span><span class="sxs-lookup"><span data-stu-id="0576e-205">Similarly, for an Azure Blob dataset, hello type of hello dataset is set too**AzureBlob**, as shown in hello following JSON:</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "fileName": "input.log",
            "folderPath": "adfgetstarted/inputdata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

## <span data-ttu-id="0576e-206"><a name="Structure"></a>DataSet-struktur</span><span class="sxs-lookup"><span data-stu-id="0576e-206"><a name="Structure"></a>Dataset structure</span></span>
<span data-ttu-id="0576e-207">Hej **struktur** avsnittet är valfritt.</span><span class="sxs-lookup"><span data-stu-id="0576e-207">hello **structure** section is optional.</span></span> <span data-ttu-id="0576e-208">Den definierar hello schemat för hello dataset av som innehåller en samling med namn och datatyperna för kolumnerna.</span><span class="sxs-lookup"><span data-stu-id="0576e-208">It defines hello schema of hello dataset by containing a collection of names and data types of columns.</span></span> <span data-ttu-id="0576e-209">Du använder hello struktur avsnittet tooprovide typinformation används tooconvert typer och mappa kolumner från hello källa toohello mål.</span><span class="sxs-lookup"><span data-stu-id="0576e-209">You use hello structure section tooprovide type information that is used tooconvert types and map columns from hello source toohello destination.</span></span> <span data-ttu-id="0576e-210">I följande exempel hello, hello datamängden har tre kolumner: `slicetimestamp`, `projectname`, och `pageviews`.</span><span class="sxs-lookup"><span data-stu-id="0576e-210">In hello following example, hello dataset has three columns: `slicetimestamp`, `projectname`, and `pageviews`.</span></span> <span data-ttu-id="0576e-211">De är av typen String, String och Decimal, respektive.</span><span class="sxs-lookup"><span data-stu-id="0576e-211">They are of type String, String, and Decimal, respectively.</span></span>

```json
structure:  
[
    { "name": "slicetimestamp", "type": "String"},
    { "name": "projectname", "type": "String"},
    { "name": "pageviews", "type": "Decimal"}
]
```

<span data-ttu-id="0576e-212">Varje kolumn i hello strukturen innehåller hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="0576e-212">Each column in hello structure contains hello following properties:</span></span>

| <span data-ttu-id="0576e-213">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0576e-213">Property</span></span> | <span data-ttu-id="0576e-214">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0576e-214">Description</span></span> | <span data-ttu-id="0576e-215">Krävs</span><span class="sxs-lookup"><span data-stu-id="0576e-215">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0576e-216">namn</span><span class="sxs-lookup"><span data-stu-id="0576e-216">name</span></span> |<span data-ttu-id="0576e-217">Namnet på hello-kolumn.</span><span class="sxs-lookup"><span data-stu-id="0576e-217">Name of hello column.</span></span> |<span data-ttu-id="0576e-218">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-218">Yes</span></span> |
| <span data-ttu-id="0576e-219">typ</span><span class="sxs-lookup"><span data-stu-id="0576e-219">type</span></span> |<span data-ttu-id="0576e-220">Datatypen för kolumnen hello.</span><span class="sxs-lookup"><span data-stu-id="0576e-220">Data type of hello column.</span></span>  |<span data-ttu-id="0576e-221">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-221">No</span></span> |
| <span data-ttu-id="0576e-222">Kultur</span><span class="sxs-lookup"><span data-stu-id="0576e-222">culture</span></span> |<span data-ttu-id="0576e-223">. NET-baserade kultur toobe används när hello typen är en .NET-typ: `Datetime` eller `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="0576e-223">.NET-based culture toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> <span data-ttu-id="0576e-224">hello standardvärdet är `en-us`.</span><span class="sxs-lookup"><span data-stu-id="0576e-224">hello default is `en-us`.</span></span> |<span data-ttu-id="0576e-225">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-225">No</span></span> |
| <span data-ttu-id="0576e-226">Format</span><span class="sxs-lookup"><span data-stu-id="0576e-226">format</span></span> |<span data-ttu-id="0576e-227">Formatera strängen toobe används när hello typen är en .NET-typ: `Datetime` eller `Datetimeoffset`.</span><span class="sxs-lookup"><span data-stu-id="0576e-227">Format string toobe used when hello type is a .NET type: `Datetime` or `Datetimeoffset`.</span></span> |<span data-ttu-id="0576e-228">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-228">No</span></span> |

<span data-ttu-id="0576e-229">hello följande riktlinjer hjälper dig att avgöra när tooinclude struktur information och vilka tooinclude i hello **struktur** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0576e-229">hello following guidelines help you determine when tooinclude structure information, and what tooinclude in hello **structure** section.</span></span>

* <span data-ttu-id="0576e-230">**För strukturerade datakällor**, ange hello struktur avsnittet om du vill mappa källkolumner kolumner toosink och deras namn är inte hello samma.</span><span class="sxs-lookup"><span data-stu-id="0576e-230">**For structured data sources**, specify hello structure section only if you want map source columns toosink columns, and their names are not hello same.</span></span> <span data-ttu-id="0576e-231">Den här typen av datakälla för strukturerade lagrar data schema och ange information tillsammans med själva hello informationen.</span><span class="sxs-lookup"><span data-stu-id="0576e-231">This kind of structured data source stores data schema and type information along with hello data itself.</span></span> <span data-ttu-id="0576e-232">Exempel på strukturerade datakällor är SQL Server, Oracle och Azure-tabellen.</span><span class="sxs-lookup"><span data-stu-id="0576e-232">Examples of structured data sources include SQL Server, Oracle, and Azure table.</span></span> 
  
    <span data-ttu-id="0576e-233">Som typen finns redan för strukturerade datakällor, kan inkludera du inte typinformation när du inkluderar hello struktur avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0576e-233">As type information is already available for structured data sources, you should not include type information when you do include hello structure section.</span></span>
* <span data-ttu-id="0576e-234">**För schemat för skrivskyddade datakällor (särskilt Blob storage)**, du kan välja toostore data utan att spara schemat eller typ information med hello data.</span><span class="sxs-lookup"><span data-stu-id="0576e-234">**For schema on read data sources (specifically Blob storage)**, you can choose toostore data without storing any schema or type information with hello data.</span></span> <span data-ttu-id="0576e-235">Inkludera struktur för dessa typer av datakällor om du vill toomap källkolumner kolumner toosink.</span><span class="sxs-lookup"><span data-stu-id="0576e-235">For these types of data sources, include structure when you want toomap source columns toosink columns.</span></span> <span data-ttu-id="0576e-236">Inkludera även struktur när hello datauppsättningen utgör indata för en kopieringsaktiviteten och datatyper i källan dataset ska vara konverterade toonative typer för hello sink.</span><span class="sxs-lookup"><span data-stu-id="0576e-236">Also include structure when hello dataset is an input for a copy activity, and data types of source dataset should be converted toonative types for hello sink.</span></span> 
    
    <span data-ttu-id="0576e-237">Data Factory stöder följande värden för att ange information i strukturen hello: **Int16, Int32, Int64, Single, Double, Decimal, Byte [], Boolean, String, Guid, Datetime, Datetimeoffset och Timespan**.</span><span class="sxs-lookup"><span data-stu-id="0576e-237">Data Factory supports hello following values for providing type information in structure: **Int16, Int32, Int64, Single, Double, Decimal, Byte[], Boolean, String, Guid, Datetime, Datetimeoffset, and Timespan**.</span></span> <span data-ttu-id="0576e-238">Dessa värden är Common Language Specification (CLS)-kompatibel,. NET-baserade typen värden.</span><span class="sxs-lookup"><span data-stu-id="0576e-238">These values are Common Language Specification (CLS)-compliant, .NET-based type values.</span></span>

<span data-ttu-id="0576e-239">Data Factory utför konverteringar automatiskt när du flyttar från en källdata datalager tooa sink-datalagret.</span><span class="sxs-lookup"><span data-stu-id="0576e-239">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span> 
  

## <a name="dataset-availability"></a><span data-ttu-id="0576e-240">DataSet-tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="0576e-240">Dataset availability</span></span>
<span data-ttu-id="0576e-241">Hej **tillgänglighet** avsnitt i en dataset definierar hello bearbetning fönstret (till exempel varje timme, varje dag eller varje vecka) för hello dataset.</span><span class="sxs-lookup"><span data-stu-id="0576e-241">hello **availability** section in a dataset defines hello processing window (for example, hourly, daily, or weekly) for hello dataset.</span></span> <span data-ttu-id="0576e-242">Läs mer om aktiviteten windows [schemaläggning och körning](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-242">For more information about activity windows, see [Scheduling and execution](data-factory-scheduling-and-execution.md).</span></span>

<span data-ttu-id="0576e-243">hello efter tillgänglighet avsnittet anger att hello datamängd för utdata är antingen produceras per timma eller hello inkommande dataset finns varje timme:</span><span class="sxs-lookup"><span data-stu-id="0576e-243">hello following availability section specifies that hello output dataset is either produced hourly, or hello input dataset is available hourly:</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 1    
}
```

<span data-ttu-id="0576e-244">Om hello pipeline har följande start- och sluttider hello:</span><span class="sxs-lookup"><span data-stu-id="0576e-244">If hello pipeline has hello following start and end times:</span></span>  

```json
    "start": "2016-08-25T00:00:00Z",
    "end": "2016-08-25T05:00:00Z",
```

<span data-ttu-id="0576e-245">hello datamängd för utdata skapas varje timme inom hello pipeline start- och sluttider.</span><span class="sxs-lookup"><span data-stu-id="0576e-245">hello output dataset is produced hourly within hello pipeline start and end times.</span></span> <span data-ttu-id="0576e-246">Det finns därför fem dataset-segment som producerats av denna pipeline, en för varje aktivitetsfönstret (12: 00 - 1 AM, 1 AM - 2 AM, 02: 00 - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM).</span><span class="sxs-lookup"><span data-stu-id="0576e-246">Therefore, there are five dataset slices produced by this pipeline, one for each activity window (12 AM - 1 AM, 1 AM - 2 AM, 2 AM - 3 AM, 3 AM - 4 AM, 4 AM - 5 AM).</span></span> 

<span data-ttu-id="0576e-247">hello beskriver följande tabell egenskaper som kan användas i hello tillgänglighet avsnittet:</span><span class="sxs-lookup"><span data-stu-id="0576e-247">hello following table describes properties you can use in hello availability section:</span></span>

| <span data-ttu-id="0576e-248">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0576e-248">Property</span></span> | <span data-ttu-id="0576e-249">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0576e-249">Description</span></span> | <span data-ttu-id="0576e-250">Krävs</span><span class="sxs-lookup"><span data-stu-id="0576e-250">Required</span></span> | <span data-ttu-id="0576e-251">Standard</span><span class="sxs-lookup"><span data-stu-id="0576e-251">Default</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0576e-252">frequency</span><span class="sxs-lookup"><span data-stu-id="0576e-252">frequency</span></span> |<span data-ttu-id="0576e-253">Anger hello tidsenhet för dataset sektorn produktion.</span><span class="sxs-lookup"><span data-stu-id="0576e-253">Specifies hello time unit for dataset slice production.</span></span><br/><br/><span data-ttu-id="0576e-254"><b>Stöd för frekvens</b>: minut, timma, dag, vecka, månad</span><span class="sxs-lookup"><span data-stu-id="0576e-254"><b>Supported frequency</b>: Minute, Hour, Day, Week, Month</span></span> |<span data-ttu-id="0576e-255">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-255">Yes</span></span> |<span data-ttu-id="0576e-256">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-256">NA</span></span> |
| <span data-ttu-id="0576e-257">interval</span><span class="sxs-lookup"><span data-stu-id="0576e-257">interval</span></span> |<span data-ttu-id="0576e-258">Anger en multiplikator för frekvens.</span><span class="sxs-lookup"><span data-stu-id="0576e-258">Specifies a multiplier for frequency.</span></span><br/><br/><span data-ttu-id="0576e-259">”X frekvensintervall” avgör hur ofta hello segment skapas.</span><span class="sxs-lookup"><span data-stu-id="0576e-259">"Frequency x interval" determines how often hello slice is produced.</span></span> <span data-ttu-id="0576e-260">Till exempel om du behöver hello dataset toobe segmenterat timme du ange <b>frekvens</b> för<b>timme</b>, och <b>intervall</b> för<b>1</b>.</span><span class="sxs-lookup"><span data-stu-id="0576e-260">For example, if you need hello dataset toobe sliced on an hourly basis, you set <b>frequency</b> too<b>Hour</b>, and <b>interval</b> too<b>1</b>.</span></span><br/><br/><span data-ttu-id="0576e-261">Observera att om du anger **frekvens** som **minut**, bör du ange hello intervall toono som är mindre än 15.</span><span class="sxs-lookup"><span data-stu-id="0576e-261">Note that if you specify **frequency** as **Minute**, you should set hello interval toono less than 15.</span></span> |<span data-ttu-id="0576e-262">Ja</span><span class="sxs-lookup"><span data-stu-id="0576e-262">Yes</span></span> |<span data-ttu-id="0576e-263">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-263">NA</span></span> |
| <span data-ttu-id="0576e-264">format</span><span class="sxs-lookup"><span data-stu-id="0576e-264">style</span></span> |<span data-ttu-id="0576e-265">Anger om hello segment ska produceras i hello början eller slutet av hello intervall.</span><span class="sxs-lookup"><span data-stu-id="0576e-265">Specifies whether hello slice should be produced at hello start or end of hello interval.</span></span><ul><li><span data-ttu-id="0576e-266">StartOfInterval</span><span class="sxs-lookup"><span data-stu-id="0576e-266">StartOfInterval</span></span></li><li><span data-ttu-id="0576e-267">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="0576e-267">EndOfInterval</span></span></li></ul><span data-ttu-id="0576e-268">Om **frekvens** har angetts för**månad**, och **style** har angetts för**EndOfInterval**, hello segment skapas på hello sista dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="0576e-268">If **frequency** is set too**Month**, and **style** is set too**EndOfInterval**, hello slice is produced on hello last day of month.</span></span> <span data-ttu-id="0576e-269">Om **style** har angetts för**StartOfInterval**, hello segment skapas på hello första dagen i månaden.</span><span class="sxs-lookup"><span data-stu-id="0576e-269">If **style** is set too**StartOfInterval**, hello slice is produced on hello first day of month.</span></span><br/><br/><span data-ttu-id="0576e-270">Om **frekvens** har angetts för**dag**, och **style** har angetts för**EndOfInterval**, hello segment skapas i hello senaste timmen på dagen hello.</span><span class="sxs-lookup"><span data-stu-id="0576e-270">If **frequency** is set too**Day**, and **style** is set too**EndOfInterval**, hello slice is produced in hello last hour of hello day.</span></span><br/><br/><span data-ttu-id="0576e-271">Om **frekvens** har angetts för**timme**, och **style** har angetts för**EndOfInterval**, hello sektorn produceras hello slutet av hello timme.</span><span class="sxs-lookup"><span data-stu-id="0576e-271">If **frequency** is set too**Hour**, and **style** is set too**EndOfInterval**, hello slice is produced at hello end of hello hour.</span></span> <span data-ttu-id="0576e-272">För ett segment för hello 1 PM - 14: 00 period, till exempel produceras hello sektorn klockan 2.</span><span class="sxs-lookup"><span data-stu-id="0576e-272">For example, for a slice for hello 1 PM - 2 PM period, hello slice is produced at 2 PM.</span></span> |<span data-ttu-id="0576e-273">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-273">No</span></span> |<span data-ttu-id="0576e-274">EndOfInterval</span><span class="sxs-lookup"><span data-stu-id="0576e-274">EndOfInterval</span></span> |
| <span data-ttu-id="0576e-275">anchorDateTime</span><span class="sxs-lookup"><span data-stu-id="0576e-275">anchorDateTime</span></span> |<span data-ttu-id="0576e-276">Definierar hello absolut placering i tid som används av hello scheduler toocompute dataset sektorn gränser.</span><span class="sxs-lookup"><span data-stu-id="0576e-276">Defines hello absolute position in time used by hello scheduler toocompute dataset slice boundaries.</span></span> <br/><br/><span data-ttu-id="0576e-277">Observera att om den här propoerty har datumdelar som är mer detaljerad än hello angivna frekvensen hello mer detaljerade delar ignoreras.</span><span class="sxs-lookup"><span data-stu-id="0576e-277">Note that if this propoerty has date parts that are more granular than hello specified frequency, hello more granular parts are ignored.</span></span> <span data-ttu-id="0576e-278">Till exempel, om hello **intervall** är **varje timme** (frekvens: timme och intervall: 1), och hello **anchorDateTime** innehåller **minuter och sekunder**, hello minuter och sekunder delar av **anchorDateTime** ignoreras.</span><span class="sxs-lookup"><span data-stu-id="0576e-278">For example, if hello **interval** is **hourly** (frequency: hour and interval: 1), and hello **anchorDateTime** contains **minutes and seconds**, then hello minutes and seconds parts of **anchorDateTime** are ignored.</span></span> |<span data-ttu-id="0576e-279">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-279">No</span></span> |<span data-ttu-id="0576e-280">01/01/0001</span><span class="sxs-lookup"><span data-stu-id="0576e-280">01/01/0001</span></span> |
| <span data-ttu-id="0576e-281">förskjutning</span><span class="sxs-lookup"><span data-stu-id="0576e-281">offset</span></span> |<span data-ttu-id="0576e-282">TimeSpan genom vilka hello början och slutet av alla dataset segment flyttat.</span><span class="sxs-lookup"><span data-stu-id="0576e-282">Timespan by which hello start and end of all dataset slices are shifted.</span></span> <br/><br/><span data-ttu-id="0576e-283">Observera att om båda **anchorDateTime** och **offset** anges, hello resultatet är hello kombineras SKIFT.</span><span class="sxs-lookup"><span data-stu-id="0576e-283">Note that if both **anchorDateTime** and **offset** are specified, hello result is hello combined shift.</span></span> |<span data-ttu-id="0576e-284">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-284">No</span></span> |<span data-ttu-id="0576e-285">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-285">NA</span></span> |

### <a name="offset-example"></a><span data-ttu-id="0576e-286">Offset exempel</span><span class="sxs-lookup"><span data-stu-id="0576e-286">offset example</span></span>
<span data-ttu-id="0576e-287">Som standard varje dag (`"frequency": "Day", "interval": 1`) segment som börjar vid 12: 00 (midnatt) Coordinated Universal Time (UTC).</span><span class="sxs-lookup"><span data-stu-id="0576e-287">By default, daily (`"frequency": "Day", "interval": 1`) slices start at 12 AM (midnight) Coordinated Universal Time (UTC).</span></span> <span data-ttu-id="0576e-288">Om du vill hello start tid toobe 06: 00 UTC-tid i stället anger du hello förskjutning som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="0576e-288">If you want hello start time toobe 6 AM UTC time instead, set hello offset as shown in hello following snippet:</span></span> 

```json
"availability":
{
    "frequency": "Day",
    "interval": 1,
    "offset": "06:00:00"
}
```
### <a name="anchordatetime-example"></a><span data-ttu-id="0576e-289">anchorDateTime-exempel</span><span class="sxs-lookup"><span data-stu-id="0576e-289">anchorDateTime example</span></span>
<span data-ttu-id="0576e-290">I följande exempel hello, produceras hello dataset var 23: e timme.</span><span class="sxs-lookup"><span data-stu-id="0576e-290">In hello following example, hello dataset is produced once every 23 hours.</span></span> <span data-ttu-id="0576e-291">hello första sektorn startar vid hello tid som anges av **anchorDateTime**, som har angetts för`2017-04-19T08:00:00` (UTC).</span><span class="sxs-lookup"><span data-stu-id="0576e-291">hello first slice starts at hello time specified by **anchorDateTime**, which is set too`2017-04-19T08:00:00` (UTC).</span></span>

```json
"availability":    
{    
    "frequency": "Hour",        
    "interval": 23,    
    "anchorDateTime":"2017-04-19T08:00:00"    
}
```

### <a name="offsetstyle-example"></a><span data-ttu-id="0576e-292">förskjutning/style-exempel</span><span class="sxs-lookup"><span data-stu-id="0576e-292">offset/style example</span></span>
<span data-ttu-id="0576e-293">hello följande dataset är månadsvis, och skapas på hello 3: e varje månad kl 8:00 (`3.08:00:00`):</span><span class="sxs-lookup"><span data-stu-id="0576e-293">hello following dataset is monthly, and is produced on hello 3rd of every month at 8:00 AM (`3.08:00:00`):</span></span>

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "offset": "3.08:00:00", 
    "style": "StartOfInterval"
}
```

## <span data-ttu-id="0576e-294"><a name="Policy"></a>DataSet-princip</span><span class="sxs-lookup"><span data-stu-id="0576e-294"><a name="Policy"></a>Dataset policy</span></span>
<span data-ttu-id="0576e-295">Hej **princip** avsnitt i hello datauppsättningsdefinitionen definierar hello kriterier eller hello villkor som hello dataset segment måste vara uppfyllda.</span><span class="sxs-lookup"><span data-stu-id="0576e-295">hello **policy** section in hello dataset definition defines hello criteria or hello condition that hello dataset slices must fulfill.</span></span>

### <a name="validation-policies"></a><span data-ttu-id="0576e-296">Verifieringsprinciper</span><span class="sxs-lookup"><span data-stu-id="0576e-296">Validation policies</span></span>
| <span data-ttu-id="0576e-297">Principnamn</span><span class="sxs-lookup"><span data-stu-id="0576e-297">Policy name</span></span> | <span data-ttu-id="0576e-298">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0576e-298">Description</span></span> | <span data-ttu-id="0576e-299">Används för</span><span class="sxs-lookup"><span data-stu-id="0576e-299">Applied too</span></span>| <span data-ttu-id="0576e-300">Krävs</span><span class="sxs-lookup"><span data-stu-id="0576e-300">Required</span></span> | <span data-ttu-id="0576e-301">Standard</span><span class="sxs-lookup"><span data-stu-id="0576e-301">Default</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="0576e-302">minimumSizeMB</span><span class="sxs-lookup"><span data-stu-id="0576e-302">minimumSizeMB</span></span> |<span data-ttu-id="0576e-303">Verifierar att hello-data i **Azure Blob storage** uppfyller hello krav på minsta storlek (i megabyte).</span><span class="sxs-lookup"><span data-stu-id="0576e-303">Validates that hello data in **Azure Blob storage** meets hello minimum size requirements (in megabytes).</span></span> |<span data-ttu-id="0576e-304">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="0576e-304">Azure Blob storage</span></span> |<span data-ttu-id="0576e-305">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-305">No</span></span> |<span data-ttu-id="0576e-306">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-306">NA</span></span> |
| <span data-ttu-id="0576e-307">minimumRows</span><span class="sxs-lookup"><span data-stu-id="0576e-307">minimumRows</span></span> |<span data-ttu-id="0576e-308">Verifierar att hello-data i en **Azure SQL database** eller en **Azure-tabellen** innehåller hello minsta antal rader.</span><span class="sxs-lookup"><span data-stu-id="0576e-308">Validates that hello data in an **Azure SQL database** or an **Azure table** contains hello minimum number of rows.</span></span> |<ul><li><span data-ttu-id="0576e-309">Azure SQL-databas</span><span class="sxs-lookup"><span data-stu-id="0576e-309">Azure SQL database</span></span></li><li><span data-ttu-id="0576e-310">Azure-tabellen</span><span class="sxs-lookup"><span data-stu-id="0576e-310">Azure table</span></span></li></ul> |<span data-ttu-id="0576e-311">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-311">No</span></span> |<span data-ttu-id="0576e-312">Ej tillämpligt</span><span class="sxs-lookup"><span data-stu-id="0576e-312">NA</span></span> |

#### <a name="examples"></a><span data-ttu-id="0576e-313">Exempel</span><span class="sxs-lookup"><span data-stu-id="0576e-313">Examples</span></span>
<span data-ttu-id="0576e-314">**minimumSizeMB:**</span><span class="sxs-lookup"><span data-stu-id="0576e-314">**minimumSizeMB:**</span></span>

```json
"policy":

{
    "validation":
    {
        "minimumSizeMB": 10.0
    }
}
```

<span data-ttu-id="0576e-315">**minimumRows:**</span><span class="sxs-lookup"><span data-stu-id="0576e-315">**minimumRows:**</span></span>

```json
"policy":
{
    "validation":
    {
        "minimumRows": 100
    }
}
```

### <a name="external-datasets"></a><span data-ttu-id="0576e-316">Externa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="0576e-316">External datasets</span></span>
<span data-ttu-id="0576e-317">Externa datauppsättningar är hello de som inte kommer från en pipeline som körs i hello data factory.</span><span class="sxs-lookup"><span data-stu-id="0576e-317">External datasets are hello ones that are not produced by a running pipeline in hello data factory.</span></span> <span data-ttu-id="0576e-318">Om hello datamängden har markerats som **externa**, hello **ExternalData** princip kan vara definierade tooinfluence hello funktionssätt hello dataset sektorn tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="0576e-318">If hello dataset is marked as **external**, hello **ExternalData** policy may be defined tooinfluence hello behavior of hello dataset slice availability.</span></span>

<span data-ttu-id="0576e-319">Om ett DataSet-objekt produceras av Data Factory, bör det markeras som **externa**.</span><span class="sxs-lookup"><span data-stu-id="0576e-319">Unless a dataset is being produced by Data Factory, it should be marked as **external**.</span></span> <span data-ttu-id="0576e-320">Den här inställningen gäller vanligtvis toohello indata för den första aktiviteten i en pipeline om aktiviteten eller pipeline länkning används.</span><span class="sxs-lookup"><span data-stu-id="0576e-320">This setting generally applies toohello inputs of first activity in a pipeline, unless activity or pipeline chaining is being used.</span></span>

| <span data-ttu-id="0576e-321">Namn</span><span class="sxs-lookup"><span data-stu-id="0576e-321">Name</span></span> | <span data-ttu-id="0576e-322">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0576e-322">Description</span></span> | <span data-ttu-id="0576e-323">Krävs</span><span class="sxs-lookup"><span data-stu-id="0576e-323">Required</span></span> | <span data-ttu-id="0576e-324">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="0576e-324">Default value</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="0576e-325">dataDelay</span><span class="sxs-lookup"><span data-stu-id="0576e-325">dataDelay</span></span> |<span data-ttu-id="0576e-326">hello gång toodelay hello på hello tillgängligheten för hello externa data för hello får sektorn.</span><span class="sxs-lookup"><span data-stu-id="0576e-326">hello time toodelay hello check on hello availability of hello external data for hello given slice.</span></span> <span data-ttu-id="0576e-327">Du kan till exempel fördröja en kontroll av varje timme med den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="0576e-327">For example, you can delay an hourly check by using this setting.</span></span><br/><br/><span data-ttu-id="0576e-328">hello inställningen gäller endast toohello aktuell tid.</span><span class="sxs-lookup"><span data-stu-id="0576e-328">hello setting only applies toohello present time.</span></span>  <span data-ttu-id="0576e-329">Om det är 1:00 PM just nu och det här värdet är 10 minuter, till exempel startas hello validering klockan 13:10.</span><span class="sxs-lookup"><span data-stu-id="0576e-329">For example, if it is 1:00 PM right now and this value is 10 minutes, hello validation starts at 1:10 PM.</span></span><br/><br/><span data-ttu-id="0576e-330">Observera att den här inställningen inte påverkar segment i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="0576e-330">Note that this setting does not affect slices in hello past.</span></span> <span data-ttu-id="0576e-331">Sektorer med **sektorn sluttid** + **dataDelay** < **nu** bearbetas utan fördröjning.</span><span class="sxs-lookup"><span data-stu-id="0576e-331">Slices with **Slice End Time** + **dataDelay** < **Now** are processed without any delay.</span></span><br/><br/><span data-ttu-id="0576e-332">Gånger större än 23:59 timmar måste anges med hjälp av hello `day.hours:minutes:seconds` format.</span><span class="sxs-lookup"><span data-stu-id="0576e-332">Times greater than 23:59 hours should be specified by using hello `day.hours:minutes:seconds` format.</span></span> <span data-ttu-id="0576e-333">Till exempel toospecify 24 timmar, Använd inte 24:00:00.</span><span class="sxs-lookup"><span data-stu-id="0576e-333">For example, toospecify 24 hours, don't use 24:00:00.</span></span> <span data-ttu-id="0576e-334">Använd i stället 1.00:00:00.</span><span class="sxs-lookup"><span data-stu-id="0576e-334">Instead, use 1.00:00:00.</span></span> <span data-ttu-id="0576e-335">Om du använder 24:00:00, behandlas den som 24 dagar (24.00:00:00).</span><span class="sxs-lookup"><span data-stu-id="0576e-335">If you use 24:00:00, it is treated as 24 days (24.00:00:00).</span></span> <span data-ttu-id="0576e-336">Ange 1:04:00:00 för 1 dag och 4 timmar.</span><span class="sxs-lookup"><span data-stu-id="0576e-336">For 1 day and 4 hours, specify 1:04:00:00.</span></span> |<span data-ttu-id="0576e-337">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-337">No</span></span> |<span data-ttu-id="0576e-338">0</span><span class="sxs-lookup"><span data-stu-id="0576e-338">0</span></span> |
| <span data-ttu-id="0576e-339">RetryInterval</span><span class="sxs-lookup"><span data-stu-id="0576e-339">retryInterval</span></span> |<span data-ttu-id="0576e-340">hello väntetiden mellan felet och hello nästa försök.</span><span class="sxs-lookup"><span data-stu-id="0576e-340">hello wait time between a failure and hello next attempt.</span></span> <span data-ttu-id="0576e-341">Den här inställningen gäller toopresent tid.</span><span class="sxs-lookup"><span data-stu-id="0576e-341">This setting applies toopresent time.</span></span> <span data-ttu-id="0576e-342">Om hello föregående försök misslyckades hello nästa försök är efter hello **retryInterval** period.</span><span class="sxs-lookup"><span data-stu-id="0576e-342">If hello previous try failed, hello next try is after hello **retryInterval** period.</span></span> <br/><br/><span data-ttu-id="0576e-343">Om den är 1:00 PM just nu kan börja vi hello första försöket.</span><span class="sxs-lookup"><span data-stu-id="0576e-343">If it is 1:00 PM right now, we begin hello first try.</span></span> <span data-ttu-id="0576e-344">Om hello varaktighet toocomplete hello första verifieringen är 1 minut och hello-åtgärden misslyckades, hello nästa försök görs 1:00 + 1 min. (varaktighet) + 1 min. (Återförsöksintervall) = 1:02 PM.</span><span class="sxs-lookup"><span data-stu-id="0576e-344">If hello duration toocomplete hello first validation check is 1 minute and hello operation failed, hello next retry is at 1:00 + 1min (duration) + 1min (retry interval) = 1:02 PM.</span></span> <br/><br/><span data-ttu-id="0576e-345">För segment i hello senaste finns ingen fördröjning.</span><span class="sxs-lookup"><span data-stu-id="0576e-345">For slices in hello past, there is no delay.</span></span> <span data-ttu-id="0576e-346">hello försök sker omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0576e-346">hello retry happens immediately.</span></span> |<span data-ttu-id="0576e-347">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-347">No</span></span> |<span data-ttu-id="0576e-348">00:01:00 (1 minut)</span><span class="sxs-lookup"><span data-stu-id="0576e-348">00:01:00 (1 minute)</span></span> |
| <span data-ttu-id="0576e-349">retryTimeout</span><span class="sxs-lookup"><span data-stu-id="0576e-349">retryTimeout</span></span> |<span data-ttu-id="0576e-350">hello tidsgräns för varje nytt försök.</span><span class="sxs-lookup"><span data-stu-id="0576e-350">hello timeout for each retry attempt.</span></span><br/><br/><span data-ttu-id="0576e-351">Om den här egenskapen anges too10 minuter hello validering ska slutföras inom 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="0576e-351">If this property is set too10 minutes, hello validation should be completed within 10 minutes.</span></span> <span data-ttu-id="0576e-352">Om det tar längre tid än 10 minuter tooperform hello validering försök hello gånger ut.</span><span class="sxs-lookup"><span data-stu-id="0576e-352">If it takes longer than 10 minutes tooperform hello validation, hello retry times out.</span></span><br/><br/><span data-ttu-id="0576e-353">Om alla försök för timeout för hello validering hello segment har markerats som **orsakade**.</span><span class="sxs-lookup"><span data-stu-id="0576e-353">If all attempts for hello validation time out, hello slice is marked as **TimedOut**.</span></span> |<span data-ttu-id="0576e-354">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-354">No</span></span> |<span data-ttu-id="0576e-355">00:10:00 (10 minuter)</span><span class="sxs-lookup"><span data-stu-id="0576e-355">00:10:00 (10 minutes)</span></span> |
| <span data-ttu-id="0576e-356">maximumRetry</span><span class="sxs-lookup"><span data-stu-id="0576e-356">maximumRetry</span></span> |<span data-ttu-id="0576e-357">hello gånger toocheck hello tillgänglighet för hello externa data.</span><span class="sxs-lookup"><span data-stu-id="0576e-357">hello number of times toocheck for hello availability of hello external data.</span></span> <span data-ttu-id="0576e-358">hello högsta tillåtna värdet är 10.</span><span class="sxs-lookup"><span data-stu-id="0576e-358">hello maximum allowed value is 10.</span></span> |<span data-ttu-id="0576e-359">Nej</span><span class="sxs-lookup"><span data-stu-id="0576e-359">No</span></span> |<span data-ttu-id="0576e-360">3</span><span class="sxs-lookup"><span data-stu-id="0576e-360">3</span></span> |


## <a name="create-datasets"></a><span data-ttu-id="0576e-361">Skapa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="0576e-361">Create datasets</span></span>
<span data-ttu-id="0576e-362">Du kan skapa datauppsättningar på något av dessa verktyg och SDK:</span><span class="sxs-lookup"><span data-stu-id="0576e-362">You can create datasets by using one of these tools or SDKs:</span></span> 

- <span data-ttu-id="0576e-363">Guiden Kopiera</span><span class="sxs-lookup"><span data-stu-id="0576e-363">Copy Wizard</span></span> 
- <span data-ttu-id="0576e-364">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0576e-364">Azure portal</span></span>
- <span data-ttu-id="0576e-365">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0576e-365">Visual Studio</span></span>
- <span data-ttu-id="0576e-366">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0576e-366">PowerShell</span></span>
- <span data-ttu-id="0576e-367">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="0576e-367">Azure Resource Manager template</span></span>
- <span data-ttu-id="0576e-368">REST API</span><span class="sxs-lookup"><span data-stu-id="0576e-368">REST API</span></span>
- <span data-ttu-id="0576e-369">.NET-API</span><span class="sxs-lookup"><span data-stu-id="0576e-369">.NET API</span></span>

<span data-ttu-id="0576e-370">Se hello följande självstudier för stegvisa instruktioner för att skapa pipelines och datauppsättningar på något av dessa verktyg och SDK:</span><span class="sxs-lookup"><span data-stu-id="0576e-370">See hello following tutorials for step-by-step instructions for creating pipelines and datasets by using one of these tools or SDKs:</span></span>
 
- [<span data-ttu-id="0576e-371">Skapa en pipeline med en datatransformeringsaktivitet</span><span class="sxs-lookup"><span data-stu-id="0576e-371">Build a pipeline with a data transformation activity</span></span>](data-factory-build-your-first-pipeline.md)
- [<span data-ttu-id="0576e-372">Skapa en pipeline med en aktivitet för flytt av data</span><span class="sxs-lookup"><span data-stu-id="0576e-372">Build a pipeline with a data movement activity</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)

<span data-ttu-id="0576e-373">När en pipeline skapas och distribueras, kan du hantera och övervaka din pipelines med hjälp av hello Azure portal blad eller hello övervakning och hantering av app.</span><span class="sxs-lookup"><span data-stu-id="0576e-373">After a pipeline is created and deployed, you can manage and monitor your pipelines by using hello Azure portal blades, or hello Monitoring and Management app.</span></span> <span data-ttu-id="0576e-374">Se hello följande avsnitt instruktioner steg för steg:</span><span class="sxs-lookup"><span data-stu-id="0576e-374">See hello following topics for step-by-step instructions:</span></span> 

- [<span data-ttu-id="0576e-375">Övervaka och hantera pipelines med hjälp av Azure portal blad</span><span class="sxs-lookup"><span data-stu-id="0576e-375">Monitor and manage pipelines by using Azure portal blades</span></span>](data-factory-monitor-manage-pipelines.md)
- [<span data-ttu-id="0576e-376">Övervaka och hantera pipelines med hello övervakning och hantering av appen</span><span class="sxs-lookup"><span data-stu-id="0576e-376">Monitor and manage pipelines by using hello Monitoring and Management app</span></span>](data-factory-monitor-manage-app.md)


## <a name="scoped-datasets"></a><span data-ttu-id="0576e-377">Begränsade datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="0576e-377">Scoped datasets</span></span>
<span data-ttu-id="0576e-378">Du kan skapa datauppsättningar som är begränsade tooa pipeline med hjälp av hello **datauppsättningar** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="0576e-378">You can create datasets that are scoped tooa pipeline by using hello **datasets** property.</span></span> <span data-ttu-id="0576e-379">Dessa data kan endast användas av aktiviteter i denna pipeline, inte av aktiviteter i andra pipelines.</span><span class="sxs-lookup"><span data-stu-id="0576e-379">These datasets can only be used by activities within this pipeline, not by activities in other pipelines.</span></span> <span data-ttu-id="0576e-380">hello följande exempel definierar en pipeline med två datamängder (InputDataset rdc och OutputDataset rdc) toobe som används i hello pipeline.</span><span class="sxs-lookup"><span data-stu-id="0576e-380">hello following example defines a pipeline with two datasets (InputDataset-rdc and OutputDataset-rdc) toobe used within hello pipeline.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="0576e-381">Begränsade datauppsättningar kan bara användas med enstaka pipelines (där **pipelineMode** har angetts för**OneTime**).</span><span class="sxs-lookup"><span data-stu-id="0576e-381">Scoped datasets are supported only with one-time pipelines (where **pipelineMode** is set too**OneTime**).</span></span> <span data-ttu-id="0576e-382">Se [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) mer information.</span><span class="sxs-lookup"><span data-stu-id="0576e-382">See [Onetime pipeline](data-factory-create-pipelines.md#onetime-pipeline) for details.</span></span>
>
>

```json
{
    "name": "CopyPipeline-rdc",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-rdc"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-rdc"
                    }
                ],
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1,
                    "style": "StartOfInterval"
                },
                "name": "CopyActivity-0"
            }
        ],
        "start": "2016-02-28T00:00:00Z",
        "end": "2016-02-28T00:00:00Z",
        "isPaused": false,
        "pipelineMode": "OneTime",
        "expirationTime": "15.00:00:00",
        "datasets": [
            {
                "name": "InputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "InputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/input",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": true,
                    "policy": {}
                }
            },
            {
                "name": "OutputDataset-rdc",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "OutputLinkedService-rdc",
                    "typeProperties": {
                        "fileName": "emp.txt",
                        "folderPath": "adftutorial/output",
                        "format": {
                            "type": "TextFormat",
                            "rowDelimiter": "\n",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "external": false,
                    "policy": {}
                }
            }
        ]
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="0576e-383">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0576e-383">Next steps</span></span>
- <span data-ttu-id="0576e-384">Läs mer om pipelines [skapa pipelines](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-384">For more information about pipelines, see [Create pipelines](data-factory-create-pipelines.md).</span></span> 
- <span data-ttu-id="0576e-385">Mer information om hur pipelines schemaläggs och körs finns [schemaläggning och körning i Azure Data Factory](data-factory-scheduling-and-execution.md).</span><span class="sxs-lookup"><span data-stu-id="0576e-385">For more information about how pipelines are scheduled and executed, see [Scheduling and execution in Azure Data Factory](data-factory-scheduling-and-execution.md).</span></span> 
