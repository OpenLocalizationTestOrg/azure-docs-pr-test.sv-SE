---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "aaaLoad data från Azure blob-lagring till Azure SQL Data Warehouse (Azure Data Factory) | Microsoft Docs"
description: "Läs tooload data med Azure Data Factory"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 689fb02e-eb98-4f7c-81e6-6c1d22d53901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: barbkess
ms.custom: loading
ms.openlocfilehash: 29a220679a11cedefb0dfd06c0a6838f81a90447
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a><span data-ttu-id="656e1-103">Läs in data från Azure blobblagring till Azure SQL Data Warehouse (Azure Data Factory)</span><span class="sxs-lookup"><span data-stu-id="656e1-103">Load data from Azure blob storage into Azure SQL Data Warehouse (Azure Data Factory)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="656e1-104">Data Factory</span><span class="sxs-lookup"><span data-stu-id="656e1-104">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="656e1-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="656e1-105">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 <span data-ttu-id="656e1-106">De här självstudierna visar hur toocreate en pipeline i Azure Data Factory toomove data från Azure Storage Blob tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-106">This tutorial shows you how toocreate a pipeline in Azure Data Factory toomove data from Azure Storage Blob tooSQL Data Warehouse.</span></span> <span data-ttu-id="656e1-107">Med följande steg hello kommer du att:</span><span class="sxs-lookup"><span data-stu-id="656e1-107">With hello following steps you will:</span></span>

* <span data-ttu-id="656e1-108">Ställa in exempeldata i en Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="656e1-108">Set-up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="656e1-109">Ansluta resurser tooAzure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-109">Connect resources tooAzure Data Factory.</span></span>
* <span data-ttu-id="656e1-110">Skapa en pipeline toomove data från Storage-Blobbar tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-110">Create a pipeline toomove data from Storage Blobs tooSQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="656e1-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="656e1-111">Before you begin</span></span>
<span data-ttu-id="656e1-112">toofamiliarize själv med Azure Data Factory finns [introduktion tooAzure Data Factory][Introduction tooAzure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="656e1-112">toofamiliarize yourself with Azure Data Factory, see [Introduction tooAzure Data Factory][Introduction tooAzure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="656e1-113">Skapa eller identifiera resurser</span><span class="sxs-lookup"><span data-stu-id="656e1-113">Create or identify resources</span></span>
<span data-ttu-id="656e1-114">Innan du påbörjar de här självstudierna måste toohave hello följande resurser.</span><span class="sxs-lookup"><span data-stu-id="656e1-114">Before starting this tutorial, you need toohave hello following resources.</span></span>

* <span data-ttu-id="656e1-115">**Azure Storage Blob**: den här kursen används Azure Storage Blob som datakälla för hello för hello Azure Data Factory-pipelinen och måste därför toohave en tillgänglig toostore hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="656e1-115">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as hello data source for hello Azure Data Factory pipeline, and so you need toohave one available toostore hello sample data.</span></span> <span data-ttu-id="656e1-116">Om du inte har någon redan lär du dig hur för[skapa ett lagringskonto][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="656e1-116">If you don't have one already, learn how too[Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="656e1-117">**SQL Data Warehouse**: den här självstudiekursen flyttar hello data från Azure Storage Blob för SQL Data Warehouse och det måste toohave ett data warehouse online som läses med hello AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="656e1-117">**SQL Data Warehouse**: This tutorial moves hello data from Azure Storage Blob too SQL Data Warehouse and so need toohave a data warehouse online that is loaded with hello AdventureWorksDW sample data.</span></span> <span data-ttu-id="656e1-118">Om du inte redan har ett data warehouse lär du dig hur för[etablera ett][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="656e1-118">If you do not already have a data warehouse, learn how too[provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="656e1-119">Om du har ett data warehouse men inte har etablerat det med hello exempeldata, kan du [läsa in det manuellt][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="656e1-119">If you have a data warehouse but didn't provision it with hello sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="656e1-120">**Azure Data Factory**: Azure Data Factory slutförs hello faktiska belastningen och du behöver toohave något som du kan använda toobuild hello data movement pipeline. Om du inte har någon redan lär du dig hur toocreate en i steg 1 av [Kom igång med Azure Data Factory (Data Factory-redigeraren)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="656e1-120">**Azure Data Factory**: Azure Data Factory will complete hello actual load and so you need toohave one that you can use toobuild hello data movement pipeline.If you don't have one already, learn how toocreate one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="656e1-121">**AZCopy**: du behöver AZCopy toocopy hello exempeldata från din lokala klient tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="656e1-121">**AZCopy**: You need AZCopy toocopy hello sample data from your local client tooyour Azure Storage Blob.</span></span> <span data-ttu-id="656e1-122">Installera instruktioner finns i hello [AZCopy-dokumentationen][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="656e1-122">For install instructions, see hello [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a><span data-ttu-id="656e1-123">Steg 1: Kopiera exempel data tooAzure Lagringsblob</span><span class="sxs-lookup"><span data-stu-id="656e1-123">Step 1: Copy sample data tooAzure Storage Blob</span></span>
<span data-ttu-id="656e1-124">När du har alla hello bitar på plats är klar toocopy exempel data tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="656e1-124">Once you have all of hello pieces ready, you are ready toocopy sample data tooyour Azure Storage Blob.</span></span>

1. <span data-ttu-id="656e1-125">[Hämta exempeldata][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="656e1-125">[Download sample data][Download sample data].</span></span> <span data-ttu-id="656e1-126">Dessa data lägger till ytterligare tre års säljdata tooyour AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="656e1-126">This data will add another three years of sales data tooyour AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="656e1-127">Använd den här AZCopy-kommandot toocopy hello tre års data tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="656e1-127">Use this AZCopy command toocopy hello three years of data tooyour Azure Storage Blob.</span></span>

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a><span data-ttu-id="656e1-128">Steg 2: Anslut resurser tooAzure Data Factory</span><span class="sxs-lookup"><span data-stu-id="656e1-128">Step 2: Connect resources tooAzure Data Factory</span></span>
<span data-ttu-id="656e1-129">Nu när hello data är på plats kan vi skapa hello Azure Data Factory pipeline toomove hello data från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-129">Now that hello data is in place we can create hello Azure Data Factory pipeline toomove hello data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="656e1-130">tooget igång, öppna hello [Azure-portalen] [ Azure portal] och välj din data factory hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="656e1-130">tooget started, open hello [Azure portal][Azure portal] and select your data factory from hello left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="656e1-131">Steg 2.1: Skapa länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="656e1-131">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="656e1-132">Länka ditt Azure storage-konto och SQL Data Warehouse tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-132">Link your Azure storage account and SQL Data Warehouse tooyour data factory.</span></span>  

1. <span data-ttu-id="656e1-133">Först påbörja hello registreringsprocessen genom att klicka på hello ”länkade tjänster” i din data factory och klicka på ”nytt datalager.'</span><span class="sxs-lookup"><span data-stu-id="656e1-133">First, begin hello registration process by clicking hello 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="656e1-134">Välj ett namn tooregister ditt azure storage under, Välj Azure Storage som typ och ange sedan ditt kontonamn och Kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="656e1-134">Choose a name tooregister your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="656e1-135">tooregister SQL Data Warehouse navigera toohello 'författare och distribution-avsnittet, väljer Nytt datalager och 'Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="656e1-135">tooregister SQL Data Warehouse navigate toohello 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="656e1-136">Kopiera och klistra in den här mallen och fyll sedan i din specifika information.</span><span class="sxs-lookup"><span data-stu-id="656e1-136">Copy and paste in this template, and then fill in your specific information.</span></span>

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-hello-dataset"></a><span data-ttu-id="656e1-137">Steg 2.2: Definiera hello dataset</span><span class="sxs-lookup"><span data-stu-id="656e1-137">Step 2.2: Define hello dataset</span></span>
<span data-ttu-id="656e1-138">När du skapar hello länkade tjänster, har vi toodefine hello datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="656e1-138">After creating hello linked services, we will have toodefine hello data sets.</span></span>  <span data-ttu-id="656e1-139">Det här fallet innebär definierar hello strukturen för hello data som flyttas från ditt lagringsutrymme tooyour data warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-139">Here this means defining hello structure of hello data that is being moved from your storage tooyour data warehouse.</span></span>  <span data-ttu-id="656e1-140">Läsa mer om hur du skapar</span><span class="sxs-lookup"><span data-stu-id="656e1-140">You can read more about creating</span></span>

1. <span data-ttu-id="656e1-141">Starta den här processen genom att gå toohello 'författare och distribution-avsnittet i din data factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-141">Start this process by navigating toohello 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="656e1-142">Klicka på ”ny datamängd' Azure Blob storage toolink din lagring tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-142">Click 'New dataset' and then 'Azure Blob storage' toolink your storage tooyour data factory.</span></span>  <span data-ttu-id="656e1-143">Du kan använda hello nedan skriptet toodefine dina data i Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="656e1-143">You can use hello below script toodefine your data in Azure Blob storage:</span></span>

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
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


1. <span data-ttu-id="656e1-144">Nu ska vi också definiera datauppsättningarna för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-144">Now we will also define our dataset for SQL Data Warehouse.</span></span>  <span data-ttu-id="656e1-145">Vi börjar hello samma sätt genom att klicka på ”ny datamängd' och 'Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="656e1-145">We start in hello same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="656e1-146">Steg 3: Skapa och kör din pipeline</span><span class="sxs-lookup"><span data-stu-id="656e1-146">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="656e1-147">Slutligen ska vi ställa in och kör hello pipeline i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-147">Finally, we will set-up and run hello pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="656e1-148">Detta är hello-åtgärd som kommer att slutföra hello faktiska dataflytten.</span><span class="sxs-lookup"><span data-stu-id="656e1-148">This is hello operation that will complete hello actual data movement.</span></span>  <span data-ttu-id="656e1-149">Du hittar en fullständig överblick över hello-åtgärder som du kan utföra med SQL Data Warehouse och Azure Data Factory [här][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="656e1-149">You can find a full view of hello operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="656e1-150">Under hello 'författare och distribution, klicka på 'Fler kommandon' och 'Ny Pipeline'.</span><span class="sxs-lookup"><span data-stu-id="656e1-150">In hello 'Author and Deploy' section now click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="656e1-151">När du har skapat hello pipeline, kan du använda hello nedan kod tootransfer hello tooyour data datalager:</span><span class="sxs-lookup"><span data-stu-id="656e1-151">After you create hello pipeline, you can use hello below code tootransfer hello data tooyour data warehouse:</span></span>

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="656e1-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="656e1-152">Next steps</span></span>
<span data-ttu-id="656e1-153">toolearn starta mer, genom att visa:</span><span class="sxs-lookup"><span data-stu-id="656e1-153">toolearn more, start by viewing:</span></span>

* <span data-ttu-id="656e1-154">[Utbildningsväg för Azure Data Factory][Azure Data Factory learning path].</span><span class="sxs-lookup"><span data-stu-id="656e1-154">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="656e1-155">[Azure SQL Data Warehouse-anslutningsapp][Azure SQL Data Warehouse Connector].</span><span class="sxs-lookup"><span data-stu-id="656e1-155">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="656e1-156">Detta är hello grundläggande referensämnet för att använda Azure Data Factory med Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-156">This is hello core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="656e1-157">De här ämnena ger detaljerad information om Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-157">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="656e1-158">De behandlar Azure SQL Database eller HDinsight, men hello informationen gäller även tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="656e1-158">They discuss Azure SQL Database or HDinsight, but hello information also applies tooAzure SQL Data Warehouse.</span></span>

* <span data-ttu-id="656e1-159">[Självstudier: Kom igång med Azure Data Factory] [ Tutorial: Get started with Azure Data Factory] är hello grundläggande självstudierna för databearbetning med Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="656e1-159">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is hello core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="656e1-160">I den här självstudiekursen kommer du skapa din första pipeline som använder HDInsight tootransform och analysera webbloggar på månatlig basis.</span><span class="sxs-lookup"><span data-stu-id="656e1-160">In this tutorial you will build your first pipeline that uses HDInsight tootransform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="656e1-161">Observera att det inte sker några kopieringsaktiviteter i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="656e1-161">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="656e1-162">[Självstudier: Kopiera data från Azure Storage Blob tooAzure SQL-databas][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="656e1-162">[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span></span> <span data-ttu-id="656e1-163">I den här självstudiekursen skapar du en pipeline i Azure Data Factory toocopy data från Azure Storage Blob tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="656e1-163">In this tutorial, you will create a pipeline in Azure Data Factory toocopy data from Azure Storage Blob tooAzure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction tooAzure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data tooand from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
