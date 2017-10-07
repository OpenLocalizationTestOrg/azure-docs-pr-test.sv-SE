---
title: "aaaUpload stora mängder data i Data Lake Store med hjälp av offline metoder | Microsoft Docs"
description: "Använd hello AdlCopy verktyget toocopy data från Azure Storage-blobbar tooData Datasjölager"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 42ef75142a26ebfab05d89614782a54c244c4bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a>Använda hello Azure Import/Export service för offline-tooData Datasjölager
I den här artikeln lär du dig hur toocopy stora datauppsättningar (> 200 GB) till ett Azure Data Lake Store med hjälp av offline kopiera metoder som hello [Azure Import/Export service](../storage/common/storage-import-export-service.md). Hello-fil som används som ett exempel i den här artikeln är särskilt 339,420,860,416 byte eller om 319 GB på disken. Vi ska anropa den här filen 319GB.tsv.

hello Azure Import/Export service hjälper dig att tootransfer stora mängder data på ett säkert sätt tooAzure Blob storage av leverans hårddisk enheter tooan Azure-datacenter.

## <a name="prerequisites"></a>Krav
Innan du börjar måste du ha hello följande:

* **En Azure-prenumeration**. Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Ett Azure storage-konto**.
* **Ett Azure Data Lake Store-konto**. Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="preparing-hello-data"></a>Förbereder hello data
Innan du använder hello Import/Export service break hello data filen toobe överförs **till kopiorna som är mindre än 200 GB** i storlek. hello-Importverktyget fungerar inte med filer som är större än 200 GB. I den här självstudiekursen dela vi hello-filen i mängder 100 GB. Du kan göra detta med hjälp av [Cygwin](https://cygwin.com/install.html). Cygwin har stöd för Linux-kommandon. I det här fallet Använd hello följande kommando:

    split -b 100m 319GB.tsv

hello split-åtgärden skapar filer med hello efter namn.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Förbereda diskar med data
Följ instruktionerna hello i [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **förbereda dina enheter** avsnitt) tooprepare dina hårddiskar. Här är hello övergripande sekvensen:

1. Skaffa en hårddisk som uppfyller hello krav toobe används för hello Azure Import/Export service.
2. Identifiera ett Azure storage-konto där hello data kopieras när den inte levererade toohello Azure-datacenter.
3. Använd hello [Azure Import/Export verktyget](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), ett kommandoradsverktyg. Här är ett exempel kodavsnitt som visar hur toouse hello verktyget.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Se [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) för flera exempel kodavsnitt.
4. hello föregående kommando skapar en journal fil vid hello angivna plats. Använd den här journalen filen toocreate ett importjobb från hello [klassiska Azure-portalen](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Skapa ett importjobb
Nu kan du skapa ett importjobb med hjälp av hello instruktionerna i [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **skapa hello importjobbet** avsnitt). För det här importjobbet med annan information, tillhandahålla hello journal-fil som skapats under förberedelserna hello diskenheter.

## <a name="physically-ship-hello-disks"></a>Fysiskt leverera hello diskar
Du kan nu fysiskt leverera hello diskar tooan Azure-datacenter. Det, kopieras hello data över toohello Azure Storage-blobbar som du angav när du skapar hello importjobbet. Dessutom när du skapar hello jobbet går om du har valt tooprovide hello senare spårningsinformation nu du tillbaka tooyour importera jobbet och uppdatera hello spårnings-ID.

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a>Kopiera data från Azure Storage BLOB tooAzure Data Lake Store
Efter hello status för hello visar importjobbet att den är klar kan du verifiera om hello data är tillgängliga i hello Azure Storage blob som du har angett. Du kan sedan använda olika metoder toomove att data från hello-blobbar tooAzure Data Lake Store. Alla hello alternativen för att ladda upp data finns [mata in data i Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

I det här avsnittet förse vi dig med definitioner av hello JSON som du kan använda toocreate ett Azure Data Factory-pipelinen för att kopiera data. Du kan använda dessa JSON-definitioner från hello [Azure-portalen](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), eller [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Källan länkad tjänst (Azure Storage blob)
````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Rikta länkad tjänst (Azure Data Lake Store)
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' tooallow this data factory and hello activities it runs tooaccess this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from hello OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Inkommande datauppsättning
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Datamängd för utdata
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Pipeline (kopieringsaktiviteten)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
Mer information finns i [flytta data från Azure Storage blob tooAzure Data Lake Store med Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a>Rekonstruera hello datafiler i Azure Data Lake Store
Vi igång med en fil som tidigare 319 GB och delades det upp till filer med en mindre storlek så att den kan överföras med hjälp av hello Azure Import/Export service. Nu när hello data i Azure Data Lake Store, rekonstruera vi hello tooits ursprungliga filstorlek. Du kan använda följande Azure PowerShell cmldts toodo så hello.

````
# Login tooour account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch toohello subscription you want toowork with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  hello files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Nästa steg
* [Säkra data i Data Lake Store](data-lake-store-secure-data.md)
* [Använd Azure Data Lake Analytics med Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Använd Azure HDInsight med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
