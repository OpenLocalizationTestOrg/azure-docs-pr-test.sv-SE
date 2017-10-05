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
# <a name="load-data-with-azure-data-factory"></a>Läs in data med Azure Data Factory
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)  
> 
> 

Den här självstudien visar hur du skapar en pipeline i Azure Data Factory för att flytta data från Azure Storage Blob till SQL Data Warehouse. Med följande steg kommer du att:

* Ställa in exempeldata i en Azure Storage Blob.
* Ansluta resurser till Azure Data Factory.
* Skapa en pipeline för att flytta data från Storage-blobbar till SQL Data Warehouse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Innan du börjar
Se [introduktion till Azure Data Factory för att bekanta dig med Azure Data Factory][Introduction to Azure Data Factory].

### <a name="create-or-identify-resources"></a>Skapa eller identifiera resurser
Innan du påbörjar den här självstudien behöver du följande resurser:

* **Azure Storage Blob**: självstudierna använder Azure Storage Blob som datakälla för Azure Data Factory-pipelinen. Du behöver därmed ha en tillgänglig för att lagra exempeldata. Om du inte har ett redan, kan du lära dig hur du [skapar ett lagringskonto][Create a storage account].
* **SQL Data Warehouse**: i självstudierna flyttas data från Azure Storage Blob till SQL Data Warehouse och du behöver därmed ha ett Data Warehouse online där du har läst in AdventureWorksDW-exempeldata. Om du inte redan har ett Data Warehouse, kan du lära dig hur du [etablerar ett][Create a SQL Data Warehouse]. Om du har ett datalager men inte har etablerat det med exempeldata, kan du [läsa in det manuellt][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory slutför den faktiska belastningen så du behöver ha en som du kan använda för att skapa dataflödespipelinen. Om du inte redan har en, kan du läsa hur man skapar en i steg 1 av [Kom igång med Azure Data Factory (Data Factory-redigeraren)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: du behöver AZCopy för att kopiera exempeldata från din lokala klient till din Azure Storage Blob. För installationsanvisningar kan du se [AZCopy-dokumentationen][AZCopy documentation].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Steg 1: Kopiera exempeldata till Azure Storage Blob.
När du väl har alla bitar på plats så är du redo att kopiera exempeldata till din Azure Storage-Blob.

1. [Hämta exempeldata][Download sample data]. Den här datan lägger till ytterligare tre år med försäljningsdata till din AdventureWorksDW-exempeldata.
2. Använd det här AZCopy-kommandot för att kopiera tre års data till din Azure Storage Blob.
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a>Steg 2: Anslut resurser till Azure Data Factory.
Nu när du har dina data på plats, kan vi skapa Azure Data Factory-pipelinen för att flytta data från Azure Blob-lagring till SQL Data Warehouse.

Kom igång genom att öppna [Azure Portal][Azure portal] och välja din Data Factory i den vänstra menyn.

### <a name="step-21-create-linked-service"></a>Steg 2.1: Skapa länkad tjänst
Länka ditt Azure-lagringskonto och SQL Data Warehouse till din Data Factory.  

1. Starta först registreringsprocessen genom att klicka på avsnittet Länkade tjänster i din Data Factory och sedan på Nytt datalager. Välj ett namn att registrera ditt Azure Storage under, välj Azure Storage som typ och ange sedan ditt kontonamn och din kontonyckel.
2. För att registrera SQL Data Warehouse, går du till avsnittet Författare och distribution, väljer Nytt datalager och sedan Azure SQL Data Warehouse. Kopiera och klistra in den här mallen och fyll sedan i din specifika information.
   
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

### <a name="step-22-define-the-dataset"></a>Steg 2.2: Definiera datauppsättningen
När du har skapat de länkade tjänsterna, behöver vi definiera datauppsättningarna.  I det här fallet innebär det att definiera strukturen på de data som flyttas från ditt lager till ditt Data Warehouse.  Läsa mer om hur du skapar

1. Starta processen genom att gå till avsnittet Författare och distribution i din Data Factory.
2. Klicka på Ny datauppsättning och sedan på Azure Blob-lagring för att länka ditt lagringsutrymme till din Data Factory.  Du kan använda skriptet nedan för att definiera dina data i Azure Blob Storage:
   
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
3. Nu definierar vi vår datauppsättning för SQL Data Warehouse. Vi börjar på samma sätt, genom att klicka på Ny datauppsättning och sedan på Azure SQL Data Warehouse.
   
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

## <a name="step-3-create-and-run-your-pipeline"></a>Steg 3: Skapa och kör din pipeline
Slutligen ska vi ställa in och köra pipelinen i Azure Data Factory.  Det här är åtgärden som kommer att slutföra den faktiska dataflytten.  Du hittar en fullständig överblick över åtgärderna som du kan utföra med SQL Data Warehouse och Azure Data Factory [här][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

I avsnittet skapa och distribuera, klickar du på fler kommandon och sedan på ny pipeline.  Efter att du skapat din pipeline, kan du använda koden nedan för att överföra data till ditt Data Warehouse:

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

## <a name="next-steps"></a>Nästa steg
Lär dig mer genom att se:

* [Utbildningsväg för Azure Data Factory][Azure Data Factory learning path].
* [Azure SQL Data Warehouse-anslutningsapp][Azure SQL Data Warehouse Connector]. Det här är det grundläggande referensämnet för att använda Azure Data Factory med Azure SQL Data Warehouse.

De här ämnena ger detaljerad information om Azure Data Factory. De behandlar Azure SQL Database eller HDinsight, men informationen är också relevant för Azure SQL Data Warehouse.

* [Självstudier: Kom igång med Azure Data Factory][Tutorial: Get started with Azure Data Factory] Det här är de grundläggande självstudierna för databearbetning med Azure Data Factory. I den här självstudien skapar du din första pipeline som använder sig av HDInsight för att transformera och analysera webbloggar på månatlig basis. Observera att det inte sker några kopieringsaktiviteter i självstudierna.
* [Självstudier: Kopiera data från Azure Storage Blob till Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]. I den här självstudien skapar du en pipeline i Azure Data Factory för att kopiera data från Azure Storage Blob till Azure SQL Database.

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
