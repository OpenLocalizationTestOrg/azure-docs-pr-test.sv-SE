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
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Läs in data från Azure blobblagring till Azure SQL Data Warehouse (Azure Data Factory)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 De här självstudierna visar hur toocreate en pipeline i Azure Data Factory toomove data från Azure Storage Blob tooSQL Data Warehouse. Med följande steg hello kommer du att:

* Ställa in exempeldata i en Azure Storage Blob.
* Ansluta resurser tooAzure Data Factory.
* Skapa en pipeline toomove data från Storage-Blobbar tooSQL Data Warehouse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Innan du börjar
toofamiliarize själv med Azure Data Factory finns [introduktion tooAzure Data Factory][Introduction tooAzure Data Factory].

### <a name="create-or-identify-resources"></a>Skapa eller identifiera resurser
Innan du påbörjar de här självstudierna måste toohave hello följande resurser.

* **Azure Storage Blob**: den här kursen används Azure Storage Blob som datakälla för hello för hello Azure Data Factory-pipelinen och måste därför toohave en tillgänglig toostore hello exempeldata. Om du inte har någon redan lär du dig hur för[skapa ett lagringskonto][Create a storage account].
* **SQL Data Warehouse**: den här självstudiekursen flyttar hello data från Azure Storage Blob för SQL Data Warehouse och det måste toohave ett data warehouse online som läses med hello AdventureWorksDW-exempeldata. Om du inte redan har ett data warehouse lär du dig hur för[etablera ett][Create a SQL Data Warehouse]. Om du har ett data warehouse men inte har etablerat det med hello exempeldata, kan du [läsa in det manuellt][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: Azure Data Factory slutförs hello faktiska belastningen och du behöver toohave något som du kan använda toobuild hello data movement pipeline. Om du inte har någon redan lär du dig hur toocreate en i steg 1 av [Kom igång med Azure Data Factory (Data Factory-redigeraren)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: du behöver AZCopy toocopy hello exempeldata från din lokala klient tooyour Azure Storage Blob. Installera instruktioner finns i hello [AZCopy-dokumentationen][AZCopy documentation].

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a>Steg 1: Kopiera exempel data tooAzure Lagringsblob
När du har alla hello bitar på plats är klar toocopy exempel data tooyour Azure Storage Blob.

1. [Hämta exempeldata][Download sample data]. Dessa data lägger till ytterligare tre års säljdata tooyour AdventureWorksDW-exempeldata.
2. Använd den här AZCopy-kommandot toocopy hello tre års data tooyour Azure Storage Blob.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a>Steg 2: Anslut resurser tooAzure Data Factory
Nu när hello data är på plats kan vi skapa hello Azure Data Factory pipeline toomove hello data från Azure blob storage till SQL Data Warehouse.

tooget igång, öppna hello [Azure-portalen] [ Azure portal] och välj din data factory hello vänstra menyn.

### <a name="step-21-create-linked-service"></a>Steg 2.1: Skapa länkad tjänst
Länka ditt Azure storage-konto och SQL Data Warehouse tooyour data factory.  

1. Först påbörja hello registreringsprocessen genom att klicka på hello ”länkade tjänster” i din data factory och klicka på ”nytt datalager.' Välj ett namn tooregister ditt azure storage under, Välj Azure Storage som typ och ange sedan ditt kontonamn och Kontonyckel.
2. tooregister SQL Data Warehouse navigera toohello 'författare och distribution-avsnittet, väljer Nytt datalager och 'Azure SQL Data Warehouse'. Kopiera och klistra in den här mallen och fyll sedan i din specifika information.

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

### <a name="step-22-define-hello-dataset"></a>Steg 2.2: Definiera hello dataset
När du skapar hello länkade tjänster, har vi toodefine hello datauppsättningar.  Det här fallet innebär definierar hello strukturen för hello data som flyttas från ditt lagringsutrymme tooyour data warehouse.  Läsa mer om hur du skapar

1. Starta den här processen genom att gå toohello 'författare och distribution-avsnittet i din data factory.
2. Klicka på ”ny datamängd' Azure Blob storage toolink din lagring tooyour data factory.  Du kan använda hello nedan skriptet toodefine dina data i Azure Blob storage:

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


1. Nu ska vi också definiera datauppsättningarna för SQL Data Warehouse.  Vi börjar hello samma sätt genom att klicka på ”ny datamängd' och 'Azure SQL Data Warehouse'.

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
Slutligen ska vi ställa in och kör hello pipeline i Azure Data Factory.  Detta är hello-åtgärd som kommer att slutföra hello faktiska dataflytten.  Du hittar en fullständig överblick över hello-åtgärder som du kan utföra med SQL Data Warehouse och Azure Data Factory [här][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].

Under hello 'författare och distribution, klicka på 'Fler kommandon' och 'Ny Pipeline'.  När du har skapat hello pipeline, kan du använda hello nedan kod tootransfer hello tooyour data datalager:

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
toolearn starta mer, genom att visa:

* [Utbildningsväg för Azure Data Factory][Azure Data Factory learning path].
* [Azure SQL Data Warehouse-anslutningsapp][Azure SQL Data Warehouse Connector]. Detta är hello grundläggande referensämnet för att använda Azure Data Factory med Azure SQL Data Warehouse.

De här ämnena ger detaljerad information om Azure Data Factory. De behandlar Azure SQL Database eller HDinsight, men hello informationen gäller även tooAzure SQL Data Warehouse.

* [Självstudier: Kom igång med Azure Data Factory] [ Tutorial: Get started with Azure Data Factory] är hello grundläggande självstudierna för databearbetning med Azure Data Factory. I den här självstudiekursen kommer du skapa din första pipeline som använder HDInsight tootransform och analysera webbloggar på månatlig basis. Observera att det inte sker några kopieringsaktiviteter i självstudierna.
* [Självstudier: Kopiera data från Azure Storage Blob tooAzure SQL-databas][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]. I den här självstudiekursen skapar du en pipeline i Azure Data Factory toocopy data från Azure Storage Blob tooAzure SQL-databas.

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
