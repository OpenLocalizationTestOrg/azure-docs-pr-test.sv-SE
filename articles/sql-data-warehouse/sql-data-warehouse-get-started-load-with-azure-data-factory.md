---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: aaaLoad data med Azure Data Factory | Microsoft Docs
description: "Läs tooload data med Azure Data Factory"
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: ac7ddaa7-a3a5-4e15-b3cf-c696d2d105df
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
ms.custom: loading
ms.openlocfilehash: 4186bd88d14be33f90130a41e8df06ce1d7cbab2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-with-azure-data-factory"></a><span data-ttu-id="c9fe9-103">Läs in data med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c9fe9-103">Load Data with Azure Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9fe9-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="c9fe9-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="c9fe9-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="c9fe9-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="c9fe9-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="c9fe9-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="c9fe9-107">BCP</span><span class="sxs-lookup"><span data-stu-id="c9fe9-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)  
> 
> 

<span data-ttu-id="c9fe9-108">De här självstudierna visar hur toocreate en pipeline i Azure Data Factory toomove data från Azure Storage Blob tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-108">This tutorial shows you how toocreate a pipeline in Azure Data Factory toomove data from Azure Storage Blob tooAzure SQL Data Warehouse.</span></span> <span data-ttu-id="c9fe9-109">Med följande steg hello kommer du att:</span><span class="sxs-lookup"><span data-stu-id="c9fe9-109">With hello following steps you will:</span></span>

* <span data-ttu-id="c9fe9-110">Ställa in exempeldata i en Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-110">Set up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="c9fe9-111">Ansluta resurser tooAzure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-111">Connect resources tooAzure Data Factory.</span></span>
* <span data-ttu-id="c9fe9-112">Skapa en pipeline toomove data från Storage-Blobbar tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-112">Create a pipeline toomove data from Storage Blobs tooSQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="c9fe9-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c9fe9-113">Before you begin</span></span>
<span data-ttu-id="c9fe9-114">toofamiliarize själv med Azure Data Factory finns [introduktion tooAzure Data Factory][Introduction tooAzure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-114">toofamiliarize yourself with Azure Data Factory, see [Introduction tooAzure Data Factory][Introduction tooAzure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="c9fe9-115">Skapa eller identifiera resurser</span><span class="sxs-lookup"><span data-stu-id="c9fe9-115">Create or identify resources</span></span>
<span data-ttu-id="c9fe9-116">Innan du påbörjar de här självstudierna måste toohave hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="c9fe9-116">Before starting this tutorial, you need toohave hello following resources:</span></span>

* <span data-ttu-id="c9fe9-117">**Azure Storage Blob**: den här kursen används Azure Storage Blob som datakälla för hello för hello Azure Data Factory-pipelinen och måste därför toohave en tillgänglig toostore hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-117">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as hello data source for hello Azure Data Factory pipeline, and so you need toohave one available toostore hello sample data.</span></span> <span data-ttu-id="c9fe9-118">Om du inte har någon redan lär du dig hur för[skapa ett lagringskonto][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-118">If you don't have one already, learn how too[Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="c9fe9-119">**SQL Data Warehouse**: den här självstudiekursen flyttar hello data från Azure Storage Blob för SQL Data Warehouse och det måste toohave ett data warehouse online som läses med hello AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-119">**SQL Data Warehouse**: This tutorial moves hello data from Azure Storage Blob too SQL Data Warehouse and so need toohave a data warehouse online that is loaded with hello AdventureWorksDW sample data.</span></span> <span data-ttu-id="c9fe9-120">Om du inte redan har ett data warehouse lär du dig hur för[etablera ett][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-120">If you do not already have a data warehouse, learn how too[provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="c9fe9-121">Om du har ett data warehouse men inte har etablerat det med hello exempeldata, kan du [läsa in det manuellt][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-121">If you have a data warehouse but didn't provision it with hello sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="c9fe9-122">**Azure Data Factory**: Azure Data Factory har slutförts hello faktiska belastningen och du behöver toohave något som du kan använda toobuild hello data movement pipeline.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-122">**Azure Data Factory**: Azure Data Factory completes hello actual load and so you need toohave one that you can use toobuild hello data movement pipeline.</span></span> <span data-ttu-id="c9fe9-123">Om du inte har någon redan lär du dig hur toocreate en i steg 1 av [Kom igång med Azure Data Factory (Data Factory-redigeraren)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-123">If you don't have one already, learn how toocreate one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="c9fe9-124">**AZCopy**: du behöver AZCopy toocopy hello exempeldata från din lokala klient tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-124">**AZCopy**: You need AZCopy toocopy hello sample data from your local client tooyour Azure Storage Blob.</span></span> <span data-ttu-id="c9fe9-125">Installera instruktioner finns i hello [AZCopy-dokumentationen][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-125">For install instructions, see hello [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a><span data-ttu-id="c9fe9-126">Steg 1: Kopiera exempel data tooAzure Lagringsblob</span><span class="sxs-lookup"><span data-stu-id="c9fe9-126">Step 1: Copy sample data tooAzure Storage Blob</span></span>
<span data-ttu-id="c9fe9-127">När du har alla hello bitar på plats är klar toocopy exempel data tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-127">Once you have all hello pieces ready, you are ready toocopy sample data tooyour Azure Storage Blob.</span></span>

1. <span data-ttu-id="c9fe9-128">[Hämta exempeldata][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-128">[Download sample data][Download sample data].</span></span> <span data-ttu-id="c9fe9-129">Dessa data lägger till ytterligare tre års säljdata tooyour AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-129">This data adds another three years of sales data tooyour AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="c9fe9-130">Använd den här AZCopy-kommandot toocopy hello tre års data tooyour Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-130">Use this AZCopy command toocopy hello three years of data tooyour Azure Storage Blob.</span></span>
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-tooazure-data-factory"></a><span data-ttu-id="c9fe9-131">Steg 2: Anslut resurser tooAzure Data Factory</span><span class="sxs-lookup"><span data-stu-id="c9fe9-131">Step 2: Connect resources tooAzure Data Factory</span></span>
<span data-ttu-id="c9fe9-132">Nu när hello data är på plats kan vi skapa hello Azure Data Factory pipeline toomove hello data från Azure blob storage till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-132">Now that hello data is in place we can create hello Azure Data Factory pipeline toomove hello data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="c9fe9-133">tooget igång, öppna hello [Azure-portalen] [ Azure portal] och välj din data factory hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-133">tooget started, open hello [Azure portal][Azure portal] and select your data factory from hello left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="c9fe9-134">Steg 2.1: Skapa länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="c9fe9-134">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="c9fe9-135">Länka ditt Azure storage-konto och SQL Data Warehouse tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-135">Link your Azure storage account and SQL Data Warehouse tooyour data factory.</span></span>  

1. <span data-ttu-id="c9fe9-136">Först påbörja hello registreringsprocessen genom att klicka på hello ”länkade tjänster” i din data factory och klicka på ”nytt datalager.'</span><span class="sxs-lookup"><span data-stu-id="c9fe9-136">First, begin hello registration process by clicking hello 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="c9fe9-137">Välj ett namn tooregister ditt azure storage under, Välj Azure Storage som typ och ange sedan ditt kontonamn och Kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-137">Choose a name tooregister your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="c9fe9-138">tooregister SQL Data Warehouse navigera toohello 'författare och distribution-avsnittet, väljer Nytt datalager och 'Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-138">tooregister SQL Data Warehouse navigate toohello 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="c9fe9-139">Kopiera och klistra in den här mallen och fyll sedan i din specifika information.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-139">Copy and paste in this template, and then fill in your specific information.</span></span>
   
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

### <a name="step-22-define-hello-dataset"></a><span data-ttu-id="c9fe9-140">Steg 2.2: Definiera hello dataset</span><span class="sxs-lookup"><span data-stu-id="c9fe9-140">Step 2.2: Define hello dataset</span></span>
<span data-ttu-id="c9fe9-141">När du skapar hello länkade tjänster, har vi toodefine hello datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-141">After creating hello linked services, we will have toodefine hello data sets.</span></span>  <span data-ttu-id="c9fe9-142">Det här fallet innebär definierar hello strukturen för hello data som flyttas från ditt lagringsutrymme tooyour data warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-142">Here this means defining hello structure of hello data that is being moved from your storage tooyour data warehouse.</span></span>  <span data-ttu-id="c9fe9-143">Läsa mer om hur du skapar</span><span class="sxs-lookup"><span data-stu-id="c9fe9-143">You can read more about creating</span></span>

1. <span data-ttu-id="c9fe9-144">Starta den här processen genom att gå toohello 'författare och distribution-avsnittet i din data factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-144">Start this process by navigating toohello 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="c9fe9-145">Klicka på ”ny datamängd' Azure Blob storage toolink din lagring tooyour data factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-145">Click 'New dataset' and then 'Azure Blob storage' toolink your storage tooyour data factory.</span></span>  <span data-ttu-id="c9fe9-146">Du kan använda hello nedan skriptet toodefine dina data i Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="c9fe9-146">You can use hello below script toodefine your data in Azure Blob storage:</span></span>
   
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
3. <span data-ttu-id="c9fe9-147">Nu definierar vi vår datauppsättning för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-147">Now we define our dataset for SQL Data Warehouse.</span></span> <span data-ttu-id="c9fe9-148">Vi börjar hello samma sätt genom att klicka på ”ny datamängd' och 'Azure SQL Data Warehouse'.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-148">We start in hello same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>
   
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

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="c9fe9-149">Steg 3: Skapa och kör din pipeline</span><span class="sxs-lookup"><span data-stu-id="c9fe9-149">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="c9fe9-150">Slutligen vi ställer in och kör hello pipeline i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-150">Finally, we set up and run hello pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="c9fe9-151">Detta är hello-åtgärden som slutförts hello faktiska dataflytten.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-151">This is hello operation that completes hello actual data movement.</span></span>  <span data-ttu-id="c9fe9-152">Du hittar en fullständig överblick över hello-åtgärder som du kan utföra med SQL Data Warehouse och Azure Data Factory [här][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-152">You can find a full view of hello operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="c9fe9-153">I hello 'författare och distribution-avsnittet, klickar du på 'Fler kommandon' och 'Ny Pipeline'.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-153">In hello 'Author and Deploy' section, click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="c9fe9-154">När du har skapat hello pipeline, kan du använda hello nedan kod tootransfer hello tooyour data datalager:</span><span class="sxs-lookup"><span data-stu-id="c9fe9-154">After you create hello pipeline, you can use hello below code tootransfer hello data tooyour data warehouse:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="c9fe9-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c9fe9-155">Next steps</span></span>
<span data-ttu-id="c9fe9-156">toolearn starta mer, genom att visa:</span><span class="sxs-lookup"><span data-stu-id="c9fe9-156">toolearn more, start by viewing:</span></span>

* <span data-ttu-id="c9fe9-157">[Utbildningsväg för Azure Data Factory][Azure Data Factory learning path].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-157">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="c9fe9-158">[Azure SQL Data Warehouse-anslutningsapp][Azure SQL Data Warehouse Connector].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-158">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="c9fe9-159">Detta är hello grundläggande referensämnet för att använda Azure Data Factory med Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-159">This is hello core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="c9fe9-160">De här ämnena ger detaljerad information om Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-160">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="c9fe9-161">De behandlar Azure SQL Database eller HDInsight, men hello informationen gäller även tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-161">They discuss Azure SQL Database or HDInsight, but hello information also applies tooAzure SQL Data Warehouse.</span></span>

* <span data-ttu-id="c9fe9-162">[Självstudier: Kom igång med Azure Data Factory] [ Tutorial: Get started with Azure Data Factory] är hello grundläggande självstudierna för databearbetning med Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-162">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is hello core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="c9fe9-163">I kursen får du skapa din första pipeline som använder HDInsight tootransform och analysera webbloggar på månatlig basis.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-163">In this tutorial, you will build your first pipeline that uses HDInsight tootransform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="c9fe9-164">Observera att det inte sker några kopieringsaktiviteter i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-164">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="c9fe9-165">[Självstudier: Kopiera data från Azure Storage Blob tooAzure SQL-databas][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="c9fe9-165">[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span></span> <span data-ttu-id="c9fe9-166">I den här självstudiekursen skapar du en pipeline i Azure Data Factory toocopy data från Azure Storage Blob tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="c9fe9-166">In this tutorial, you create a pipeline in Azure Data Factory toocopy data from Azure Storage Blob tooAzure SQL Database.</span></span>

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
