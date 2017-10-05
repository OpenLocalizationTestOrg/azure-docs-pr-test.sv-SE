---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "Läs in data med Azure Data Factory | Microsoft Docs"
description: "Lär dig hur man läser in data med Azure Data Factory"
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
ms.openlocfilehash: 0b01c77c916b616974545fc3da442e72e5336399
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-with-azure-data-factory"></a><span data-ttu-id="ed24e-103">Läs in data med Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="ed24e-103">Load Data with Azure Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ed24e-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="ed24e-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="ed24e-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="ed24e-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="ed24e-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="ed24e-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="ed24e-107">BCP</span><span class="sxs-lookup"><span data-stu-id="ed24e-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)  
> 
> 

<span data-ttu-id="ed24e-108">Den här självstudien visar hur du skapar en pipeline i Azure Data Factory för att flytta data från Azure Storage Blob till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-108">This tutorial shows you how to create a pipeline in Azure Data Factory to move data from Azure Storage Blob to Azure SQL Data Warehouse.</span></span> <span data-ttu-id="ed24e-109">Med följande steg kommer du att:</span><span class="sxs-lookup"><span data-stu-id="ed24e-109">With the following steps you will:</span></span>

* <span data-ttu-id="ed24e-110">Ställa in exempeldata i en Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="ed24e-110">Set up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="ed24e-111">Ansluta resurser till Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-111">Connect resources to Azure Data Factory.</span></span>
* <span data-ttu-id="ed24e-112">Skapa en pipeline för att flytta data från Storage-blobbar till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-112">Create a pipeline to move data from Storage Blobs to SQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="ed24e-113">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ed24e-113">Before you begin</span></span>
<span data-ttu-id="ed24e-114">Se [introduktion till Azure Data Factory för att bekanta dig med Azure Data Factory][Introduction to Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="ed24e-114">To familiarize yourself with Azure Data Factory, see [Introduction to Azure Data Factory][Introduction to Azure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="ed24e-115">Skapa eller identifiera resurser</span><span class="sxs-lookup"><span data-stu-id="ed24e-115">Create or identify resources</span></span>
<span data-ttu-id="ed24e-116">Innan du påbörjar den här självstudien behöver du följande resurser:</span><span class="sxs-lookup"><span data-stu-id="ed24e-116">Before starting this tutorial, you need to have the following resources:</span></span>

* <span data-ttu-id="ed24e-117">**Azure Storage Blob**: självstudierna använder Azure Storage Blob som datakälla för Azure Data Factory-pipelinen. Du behöver därmed ha en tillgänglig för att lagra exempeldata.</span><span class="sxs-lookup"><span data-stu-id="ed24e-117">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as the data source for the Azure Data Factory pipeline, and so you need to have one available to store the sample data.</span></span> <span data-ttu-id="ed24e-118">Om du inte har ett redan, kan du lära dig hur du [skapar ett lagringskonto][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="ed24e-118">If you don't have one already, learn how to [Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="ed24e-119">**SQL Data Warehouse**: i självstudierna flyttas data från Azure Storage Blob till SQL Data Warehouse och du behöver därmed ha ett Data Warehouse online där du har läst in AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="ed24e-119">**SQL Data Warehouse**: This tutorial moves the data from Azure Storage Blob to  SQL Data Warehouse and so need to have a data warehouse online that is loaded with the AdventureWorksDW sample data.</span></span> <span data-ttu-id="ed24e-120">Om du inte redan har ett Data Warehouse, kan du lära dig hur du [etablerar ett][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ed24e-120">If you do not already have a data warehouse, learn how to [provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="ed24e-121">Om du har ett datalager men inte har etablerat det med exempeldata, kan du [läsa in det manuellt][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ed24e-121">If you have a data warehouse but didn't provision it with the sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="ed24e-122">**Azure Data Factory**: Azure Data Factory slutför den faktiska belastningen så du behöver ha en som du kan använda för att skapa dataflödespipelinen.</span><span class="sxs-lookup"><span data-stu-id="ed24e-122">**Azure Data Factory**: Azure Data Factory completes the actual load and so you need to have one that you can use to build the data movement pipeline.</span></span> <span data-ttu-id="ed24e-123">Om du inte redan har en, kan du läsa hur man skapar en i steg 1 av [Kom igång med Azure Data Factory (Data Factory-redigeraren)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="ed24e-123">If you don't have one already, learn how to create one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="ed24e-124">**AZCopy**: du behöver AZCopy för att kopiera exempeldata från din lokala klient till din Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="ed24e-124">**AZCopy**: You need AZCopy to copy the sample data from your local client to your Azure Storage Blob.</span></span> <span data-ttu-id="ed24e-125">För installationsanvisningar kan du se [AZCopy-dokumentationen][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="ed24e-125">For install instructions, see the [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a><span data-ttu-id="ed24e-126">Steg 1: Kopiera exempeldata till Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="ed24e-126">Step 1: Copy sample data to Azure Storage Blob</span></span>
<span data-ttu-id="ed24e-127">När du väl har alla bitar på plats så är du redo att kopiera exempeldata till din Azure Storage-Blob.</span><span class="sxs-lookup"><span data-stu-id="ed24e-127">Once you have all the pieces ready, you are ready to copy sample data to your Azure Storage Blob.</span></span>

1. <span data-ttu-id="ed24e-128">[Hämta exempeldata][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="ed24e-128">[Download sample data][Download sample data].</span></span> <span data-ttu-id="ed24e-129">Den här datan lägger till ytterligare tre år med försäljningsdata till din AdventureWorksDW-exempeldata.</span><span class="sxs-lookup"><span data-stu-id="ed24e-129">This data adds another three years of sales data to your AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="ed24e-130">Använd det här AZCopy-kommandot för att kopiera tre års data till din Azure Storage Blob.</span><span class="sxs-lookup"><span data-stu-id="ed24e-130">Use this AZCopy command to copy the three years of data to your Azure Storage Blob.</span></span>
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a><span data-ttu-id="ed24e-131">Steg 2: Anslut resurser till Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-131">Step 2: Connect resources to Azure Data Factory</span></span>
<span data-ttu-id="ed24e-132">Nu när du har dina data på plats, kan vi skapa Azure Data Factory-pipelinen för att flytta data från Azure Blob-lagring till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-132">Now that the data is in place we can create the Azure Data Factory pipeline to move the data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="ed24e-133">Kom igång genom att öppna [Azure Portal][Azure portal] och välja din Data Factory i den vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="ed24e-133">To get started, open the [Azure portal][Azure portal] and select your data factory from the left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="ed24e-134">Steg 2.1: Skapa länkad tjänst</span><span class="sxs-lookup"><span data-stu-id="ed24e-134">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="ed24e-135">Länka ditt Azure-lagringskonto och SQL Data Warehouse till din Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-135">Link your Azure storage account and SQL Data Warehouse to your data factory.</span></span>  

1. <span data-ttu-id="ed24e-136">Starta först registreringsprocessen genom att klicka på avsnittet Länkade tjänster i din Data Factory och sedan på Nytt datalager.</span><span class="sxs-lookup"><span data-stu-id="ed24e-136">First, begin the registration process by clicking the 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="ed24e-137">Välj ett namn att registrera ditt Azure Storage under, välj Azure Storage som typ och ange sedan ditt kontonamn och din kontonyckel.</span><span class="sxs-lookup"><span data-stu-id="ed24e-137">Choose a name to register your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="ed24e-138">För att registrera SQL Data Warehouse, går du till avsnittet Författare och distribution, väljer Nytt datalager och sedan Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-138">To register SQL Data Warehouse navigate to the 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="ed24e-139">Kopiera och klistra in den här mallen och fyll sedan i din specifika information.</span><span class="sxs-lookup"><span data-stu-id="ed24e-139">Copy and paste in this template, and then fill in your specific information.</span></span>
   
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

### <a name="step-22-define-the-dataset"></a><span data-ttu-id="ed24e-140">Steg 2.2: Definiera datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="ed24e-140">Step 2.2: Define the dataset</span></span>
<span data-ttu-id="ed24e-141">När du har skapat de länkade tjänsterna, behöver vi definiera datauppsättningarna.</span><span class="sxs-lookup"><span data-stu-id="ed24e-141">After creating the linked services, we will have to define the data sets.</span></span>  <span data-ttu-id="ed24e-142">I det här fallet innebär det att definiera strukturen på de data som flyttas från ditt lager till ditt Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-142">Here this means defining the structure of the data that is being moved from your storage to your data warehouse.</span></span>  <span data-ttu-id="ed24e-143">Läsa mer om hur du skapar</span><span class="sxs-lookup"><span data-stu-id="ed24e-143">You can read more about creating</span></span>

1. <span data-ttu-id="ed24e-144">Starta processen genom att gå till avsnittet Författare och distribution i din Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-144">Start this process by navigating to the 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="ed24e-145">Klicka på Ny datauppsättning och sedan på Azure Blob-lagring för att länka ditt lagringsutrymme till din Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-145">Click 'New dataset' and then 'Azure Blob storage' to link your storage to your data factory.</span></span>  <span data-ttu-id="ed24e-146">Du kan använda skriptet nedan för att definiera dina data i Azure Blob Storage:</span><span class="sxs-lookup"><span data-stu-id="ed24e-146">You can use the below script to define your data in Azure Blob storage:</span></span>
   
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
3. <span data-ttu-id="ed24e-147">Nu definierar vi vår datauppsättning för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-147">Now we define our dataset for SQL Data Warehouse.</span></span> <span data-ttu-id="ed24e-148">Vi börjar på samma sätt, genom att klicka på Ny datauppsättning och sedan på Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-148">We start in the same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>
   
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

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="ed24e-149">Steg 3: Skapa och kör din pipeline</span><span class="sxs-lookup"><span data-stu-id="ed24e-149">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="ed24e-150">Slutligen ska vi ställa in och köra pipelinen i Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-150">Finally, we set up and run the pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="ed24e-151">Det här är åtgärden som kommer att slutföra den faktiska dataflytten.</span><span class="sxs-lookup"><span data-stu-id="ed24e-151">This is the operation that completes the actual data movement.</span></span>  <span data-ttu-id="ed24e-152">Du hittar en fullständig överblick över åtgärderna som du kan utföra med SQL Data Warehouse och Azure Data Factory [här][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="ed24e-152">You can find a full view of the operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="ed24e-153">I avsnittet skapa och distribuera, klickar du på fler kommandon och sedan på ny pipeline.</span><span class="sxs-lookup"><span data-stu-id="ed24e-153">In the 'Author and Deploy' section, click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="ed24e-154">Efter att du skapat din pipeline, kan du använda koden nedan för att överföra data till ditt Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="ed24e-154">After you create the pipeline, you can use the below code to transfer the data to your data warehouse:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="ed24e-155">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ed24e-155">Next steps</span></span>
<span data-ttu-id="ed24e-156">Lär dig mer genom att se:</span><span class="sxs-lookup"><span data-stu-id="ed24e-156">To learn more, start by viewing:</span></span>

* <span data-ttu-id="ed24e-157">[Utbildningsväg för Azure Data Factory][Azure Data Factory learning path].</span><span class="sxs-lookup"><span data-stu-id="ed24e-157">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="ed24e-158">[Azure SQL Data Warehouse-anslutningsapp][Azure SQL Data Warehouse Connector].</span><span class="sxs-lookup"><span data-stu-id="ed24e-158">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="ed24e-159">Det här är det grundläggande referensämnet för att använda Azure Data Factory med Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-159">This is the core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="ed24e-160">De här ämnena ger detaljerad information om Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-160">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="ed24e-161">De behandlar Azure SQL Database eller HDinsight, men informationen är också relevant för Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="ed24e-161">They discuss Azure SQL Database or HDInsight, but the information also applies to Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="ed24e-162">[Självstudier: Kom igång med Azure Data Factory][Tutorial: Get started with Azure Data Factory] Det här är de grundläggande självstudierna för databearbetning med Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="ed24e-162">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is the core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="ed24e-163">I den här självstudien skapar du din första pipeline som använder sig av HDInsight för att transformera och analysera webbloggar på månatlig basis.</span><span class="sxs-lookup"><span data-stu-id="ed24e-163">In this tutorial, you will build your first pipeline that uses HDInsight to transform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="ed24e-164">Observera att det inte sker några kopieringsaktiviteter i självstudierna.</span><span class="sxs-lookup"><span data-stu-id="ed24e-164">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="ed24e-165">[Självstudier: Kopiera data från Azure Storage Blob till Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="ed24e-165">[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span></span> <span data-ttu-id="ed24e-166">I den här självstudien skapar du en pipeline i Azure Data Factory för att kopiera data från Azure Storage Blob till Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="ed24e-166">In this tutorial, you create a pipeline in Azure Data Factory to copy data from Azure Storage Blob to Azure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction to Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
