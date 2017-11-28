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
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a><span data-ttu-id="4fdea-103">Använda hello Azure Import/Export service för offline-tooData Datasjölager</span><span class="sxs-lookup"><span data-stu-id="4fdea-103">Use hello Azure Import/Export service for offline copy of data tooData Lake Store</span></span>
<span data-ttu-id="4fdea-104">I den här artikeln lär du dig hur toocopy stora datauppsättningar (> 200 GB) till ett Azure Data Lake Store med hjälp av offline kopiera metoder som hello [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span><span class="sxs-lookup"><span data-stu-id="4fdea-104">In this article, you'll learn how toocopy huge data sets (>200 GB) into an Azure Data Lake Store by using offline copy methods, like hello [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span></span> <span data-ttu-id="4fdea-105">Hello-fil som används som ett exempel i den här artikeln är särskilt 339,420,860,416 byte eller om 319 GB på disken.</span><span class="sxs-lookup"><span data-stu-id="4fdea-105">Specifically, hello file used as an example in this article is 339,420,860,416 bytes, or about 319 GB on disk.</span></span> <span data-ttu-id="4fdea-106">Vi ska anropa den här filen 319GB.tsv.</span><span class="sxs-lookup"><span data-stu-id="4fdea-106">Let's call this file 319GB.tsv.</span></span>

<span data-ttu-id="4fdea-107">hello Azure Import/Export service hjälper dig att tootransfer stora mängder data på ett säkert sätt tooAzure Blob storage av leverans hårddisk enheter tooan Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="4fdea-107">hello Azure Import/Export service helps you tootransfer large amounts of data more securely tooAzure Blob storage by shipping hard disk drives tooan Azure datacenter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4fdea-108">Krav</span><span class="sxs-lookup"><span data-stu-id="4fdea-108">Prerequisites</span></span>
<span data-ttu-id="4fdea-109">Innan du börjar måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="4fdea-109">Before you begin, you must have hello following:</span></span>

* <span data-ttu-id="4fdea-110">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="4fdea-110">**An Azure subscription**.</span></span> <span data-ttu-id="4fdea-111">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4fdea-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4fdea-112">**Ett Azure storage-konto**.</span><span class="sxs-lookup"><span data-stu-id="4fdea-112">**An Azure storage account**.</span></span>
* <span data-ttu-id="4fdea-113">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="4fdea-113">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="4fdea-114">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4fdea-114">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

## <a name="preparing-hello-data"></a><span data-ttu-id="4fdea-115">Förbereder hello data</span><span class="sxs-lookup"><span data-stu-id="4fdea-115">Preparing hello data</span></span>
<span data-ttu-id="4fdea-116">Innan du använder hello Import/Export service break hello data filen toobe överförs **till kopiorna som är mindre än 200 GB** i storlek.</span><span class="sxs-lookup"><span data-stu-id="4fdea-116">Before using hello Import/Export service, break hello data file toobe transferred **into copies that are less than 200 GB** in size.</span></span> <span data-ttu-id="4fdea-117">hello-Importverktyget fungerar inte med filer som är större än 200 GB.</span><span class="sxs-lookup"><span data-stu-id="4fdea-117">hello import tool does not work with files greater than 200 GB.</span></span> <span data-ttu-id="4fdea-118">I den här självstudiekursen dela vi hello-filen i mängder 100 GB.</span><span class="sxs-lookup"><span data-stu-id="4fdea-118">In this tutorial, we split hello file into chunks of 100 GB each.</span></span> <span data-ttu-id="4fdea-119">Du kan göra detta med hjälp av [Cygwin](https://cygwin.com/install.html).</span><span class="sxs-lookup"><span data-stu-id="4fdea-119">You can do this by using [Cygwin](https://cygwin.com/install.html).</span></span> <span data-ttu-id="4fdea-120">Cygwin har stöd för Linux-kommandon.</span><span class="sxs-lookup"><span data-stu-id="4fdea-120">Cygwin supports Linux commands.</span></span> <span data-ttu-id="4fdea-121">I det här fallet Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4fdea-121">In this case, use hello following command:</span></span>

    split -b 100m 319GB.tsv

<span data-ttu-id="4fdea-122">hello split-åtgärden skapar filer med hello efter namn.</span><span class="sxs-lookup"><span data-stu-id="4fdea-122">hello split operation creates files with hello following names.</span></span>

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a><span data-ttu-id="4fdea-123">Förbereda diskar med data</span><span class="sxs-lookup"><span data-stu-id="4fdea-123">Get disks ready with data</span></span>
<span data-ttu-id="4fdea-124">Följ instruktionerna hello i [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **förbereda dina enheter** avsnitt) tooprepare dina hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="4fdea-124">Follow hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Prepare your drives** section) tooprepare your hard drives.</span></span> <span data-ttu-id="4fdea-125">Här är hello övergripande sekvensen:</span><span class="sxs-lookup"><span data-stu-id="4fdea-125">Here's hello overall sequence:</span></span>

1. <span data-ttu-id="4fdea-126">Skaffa en hårddisk som uppfyller hello krav toobe används för hello Azure Import/Export service.</span><span class="sxs-lookup"><span data-stu-id="4fdea-126">Procure a hard disk that meets hello requirement toobe used for hello Azure Import/Export service.</span></span>
2. <span data-ttu-id="4fdea-127">Identifiera ett Azure storage-konto där hello data kopieras när den inte levererade toohello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="4fdea-127">Identify an Azure storage account where hello data will be copied after it is shipped toohello Azure datacenter.</span></span>
3. <span data-ttu-id="4fdea-128">Använd hello [Azure Import/Export verktyget](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), ett kommandoradsverktyg.</span><span class="sxs-lookup"><span data-stu-id="4fdea-128">Use hello [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), a command-line utility.</span></span> <span data-ttu-id="4fdea-129">Här är ett exempel kodavsnitt som visar hur toouse hello verktyget.</span><span class="sxs-lookup"><span data-stu-id="4fdea-129">Here's a sample snippet that shows how toouse hello tool.</span></span>

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    <span data-ttu-id="4fdea-130">Se [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) för flera exempel kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="4fdea-130">See [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) for more sample snippets.</span></span>
4. <span data-ttu-id="4fdea-131">hello föregående kommando skapar en journal fil vid hello angivna plats.</span><span class="sxs-lookup"><span data-stu-id="4fdea-131">hello preceding command creates a journal file at hello specified location.</span></span> <span data-ttu-id="4fdea-132">Använd den här journalen filen toocreate ett importjobb från hello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4fdea-132">Use this journal file toocreate an import job from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

## <a name="create-an-import-job"></a><span data-ttu-id="4fdea-133">Skapa ett importjobb</span><span class="sxs-lookup"><span data-stu-id="4fdea-133">Create an import job</span></span>
<span data-ttu-id="4fdea-134">Nu kan du skapa ett importjobb med hjälp av hello instruktionerna i [med hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **skapa hello importjobbet** avsnitt).</span><span class="sxs-lookup"><span data-stu-id="4fdea-134">You can now create an import job by using hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Create hello Import job** section).</span></span> <span data-ttu-id="4fdea-135">För det här importjobbet med annan information, tillhandahålla hello journal-fil som skapats under förberedelserna hello diskenheter.</span><span class="sxs-lookup"><span data-stu-id="4fdea-135">For this import job, with other details, also provide hello journal file created while preparing hello disk drives.</span></span>

## <a name="physically-ship-hello-disks"></a><span data-ttu-id="4fdea-136">Fysiskt leverera hello diskar</span><span class="sxs-lookup"><span data-stu-id="4fdea-136">Physically ship hello disks</span></span>
<span data-ttu-id="4fdea-137">Du kan nu fysiskt leverera hello diskar tooan Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="4fdea-137">You can now physically ship hello disks tooan Azure datacenter.</span></span> <span data-ttu-id="4fdea-138">Det, kopieras hello data över toohello Azure Storage-blobbar som du angav när du skapar hello importjobbet.</span><span class="sxs-lookup"><span data-stu-id="4fdea-138">There, hello data is copied over toohello Azure Storage blobs you provided while creating hello import job.</span></span> <span data-ttu-id="4fdea-139">Dessutom när du skapar hello jobbet går om du har valt tooprovide hello senare spårningsinformation nu du tillbaka tooyour importera jobbet och uppdatera hello spårnings-ID.</span><span class="sxs-lookup"><span data-stu-id="4fdea-139">Also, while creating hello job, if you opted tooprovide hello tracking information later, you can now go back tooyour import job and update hello tracking number.</span></span>

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a><span data-ttu-id="4fdea-140">Kopiera data från Azure Storage BLOB tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4fdea-140">Copy data from Azure Storage blobs tooAzure Data Lake Store</span></span>
<span data-ttu-id="4fdea-141">Efter hello status för hello visar importjobbet att den är klar kan du verifiera om hello data är tillgängliga i hello Azure Storage blob som du har angett.</span><span class="sxs-lookup"><span data-stu-id="4fdea-141">After hello status of hello import job shows that it's completed, you can verify whether hello data is available in hello Azure Storage blobs you had specified.</span></span> <span data-ttu-id="4fdea-142">Du kan sedan använda olika metoder toomove att data från hello-blobbar tooAzure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="4fdea-142">You can then use a variety of methods toomove that data from hello blobs tooAzure Data Lake Store.</span></span> <span data-ttu-id="4fdea-143">Alla hello alternativen för att ladda upp data finns [mata in data i Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="4fdea-143">For all hello available options for uploading data, see [Ingesting data into Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span></span>

<span data-ttu-id="4fdea-144">I det här avsnittet förse vi dig med definitioner av hello JSON som du kan använda toocreate ett Azure Data Factory-pipelinen för att kopiera data.</span><span class="sxs-lookup"><span data-stu-id="4fdea-144">In this section, we provide you with hello JSON definitions that you can use toocreate an Azure Data Factory pipeline for copying data.</span></span> <span data-ttu-id="4fdea-145">Du kan använda dessa JSON-definitioner från hello [Azure-portalen](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), eller [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), eller [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4fdea-145">You can use these JSON definitions from hello [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), or [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

### <a name="source-linked-service-azure-storage-blob"></a><span data-ttu-id="4fdea-146">Källan länkad tjänst (Azure Storage blob)</span><span class="sxs-lookup"><span data-stu-id="4fdea-146">Source linked service (Azure Storage blob)</span></span>
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

### <a name="target-linked-service-azure-data-lake-store"></a><span data-ttu-id="4fdea-147">Rikta länkad tjänst (Azure Data Lake Store)</span><span class="sxs-lookup"><span data-stu-id="4fdea-147">Target linked service (Azure Data Lake Store)</span></span>
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
### <a name="input-data-set"></a><span data-ttu-id="4fdea-148">Inkommande datauppsättning</span><span class="sxs-lookup"><span data-stu-id="4fdea-148">Input data set</span></span>
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
### <a name="output-data-set"></a><span data-ttu-id="4fdea-149">Datamängd för utdata</span><span class="sxs-lookup"><span data-stu-id="4fdea-149">Output data set</span></span>
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
### <a name="pipeline-copy-activity"></a><span data-ttu-id="4fdea-150">Pipeline (kopieringsaktiviteten)</span><span class="sxs-lookup"><span data-stu-id="4fdea-150">Pipeline (copy activity)</span></span>
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
<span data-ttu-id="4fdea-151">Mer information finns i [flytta data från Azure Storage blob tooAzure Data Lake Store med Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span><span class="sxs-lookup"><span data-stu-id="4fdea-151">For more information, see [Move data from Azure Storage blob tooAzure Data Lake Store using Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span></span>

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a><span data-ttu-id="4fdea-152">Rekonstruera hello datafiler i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4fdea-152">Reconstruct hello data files in Azure Data Lake Store</span></span>
<span data-ttu-id="4fdea-153">Vi igång med en fil som tidigare 319 GB och delades det upp till filer med en mindre storlek så att den kan överföras med hjälp av hello Azure Import/Export service.</span><span class="sxs-lookup"><span data-stu-id="4fdea-153">We started with a file that was 319 GB, and broke it down into files of smaller size so that it could be transferred by using hello Azure Import/Export service.</span></span> <span data-ttu-id="4fdea-154">Nu när hello data i Azure Data Lake Store, rekonstruera vi hello tooits ursprungliga filstorlek.</span><span class="sxs-lookup"><span data-stu-id="4fdea-154">Now that hello data is in Azure Data Lake Store, we can reconstruct hello file tooits original size.</span></span> <span data-ttu-id="4fdea-155">Du kan använda följande Azure PowerShell cmldts toodo så hello.</span><span class="sxs-lookup"><span data-stu-id="4fdea-155">You can use hello following Azure PowerShell cmldts toodo so.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="4fdea-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fdea-156">Next steps</span></span>
* [<span data-ttu-id="4fdea-157">Säkra data i Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4fdea-157">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="4fdea-158">Använd Azure Data Lake Analytics med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4fdea-158">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="4fdea-159">Använd Azure HDInsight med Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4fdea-159">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
