---
title: "aaaMove Data tooand från Azure Blob Storage med hjälp av Python | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage med hjälp av Python"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 24276252-b3dd-4edf-9e5d-f6803f8ccccc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: c2be9600e0d6cb05bcf4109a8d554db522704ecb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a><span data-ttu-id="52aa0-103">Flytta data tooand från Azure Blob Storage med hjälp av Python</span><span class="sxs-lookup"><span data-stu-id="52aa0-103">Move data tooand from Azure Blob Storage using Python</span></span>
<span data-ttu-id="52aa0-104">Det här avsnittet beskrivs hur toolist, ladda upp och hämta blobar med hjälp av hello Python-API.</span><span class="sxs-lookup"><span data-stu-id="52aa0-104">This topic describes how toolist, upload, and download blobs using hello Python API.</span></span> <span data-ttu-id="52aa0-105">Med hello Python-API finns i Azure SDK, kan du:</span><span class="sxs-lookup"><span data-stu-id="52aa0-105">With hello Python API provided in Azure SDK, you can:</span></span>

* <span data-ttu-id="52aa0-106">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="52aa0-106">Create a container</span></span>
* <span data-ttu-id="52aa0-107">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="52aa0-107">Upload a blob into a container</span></span>
* <span data-ttu-id="52aa0-108">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="52aa0-108">Download blobs</span></span>
* <span data-ttu-id="52aa0-109">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="52aa0-109">List hello blobs in a container</span></span>
* <span data-ttu-id="52aa0-110">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="52aa0-110">Delete a blob</span></span>

<span data-ttu-id="52aa0-111">Mer information om hur du använder hello Python-API finns [hur tooUse hello tjänsten för Blob Storage från Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="52aa0-111">For more information about using hello Python API, see [How tooUse hello Blob Storage Service from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="52aa0-112">Om du använder en virtuell dator som har konfigurerats med hello-skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan AzCopy är redan installerad på hello VM.</span><span class="sxs-lookup"><span data-stu-id="52aa0-112">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="52aa0-113">Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="52aa0-113">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="52aa0-114">Krav</span><span class="sxs-lookup"><span data-stu-id="52aa0-114">Prerequisites</span></span>
<span data-ttu-id="52aa0-115">Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot.</span><span class="sxs-lookup"><span data-stu-id="52aa0-115">This document assumes that you have an Azure subscription, a storage account, and hello corresponding storage key for that account.</span></span> <span data-ttu-id="52aa0-116">Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="52aa0-116">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="52aa0-117">tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="52aa0-117">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="52aa0-118">Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="52aa0-118">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="upload-data-tooblob"></a><span data-ttu-id="52aa0-119">Ladda upp Data tooBlob</span><span class="sxs-lookup"><span data-stu-id="52aa0-119">Upload Data tooBlob</span></span>
<span data-ttu-id="52aa0-120">Lägg till hello följande fragment hello övre delen av alla Python-kod som du vill tooprogrammatically åtkomst till Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="52aa0-120">Add hello following snippet near hello top of any Python code in which you wish tooprogrammatically access Azure Storage:</span></span>

    from azure.storage.blob import BlobService

<span data-ttu-id="52aa0-121">Hej **BlobService** objekt kan du arbeta med behållare och blobbar.</span><span class="sxs-lookup"><span data-stu-id="52aa0-121">hello **BlobService** object lets you work with containers and blobs.</span></span> <span data-ttu-id="52aa0-122">hello följande kod skapar ett BlobService-objekt med hjälp av hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="52aa0-122">hello following code creates a BlobService object using hello storage account name and account key.</span></span> <span data-ttu-id="52aa0-123">Ersätt kontonamnet och kontonyckel med din verkliga konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="52aa0-123">Replace account name and account key with your real account and key.</span></span>

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

<span data-ttu-id="52aa0-124">Använd följande metoder tooupload data tooa blob hello:</span><span class="sxs-lookup"><span data-stu-id="52aa0-124">Use hello following methods tooupload data tooa blob:</span></span>

1. <span data-ttu-id="52aa0-125">Placera\_block\_blob\_från\_sökväg (Överför hello innehållet i en fil från hello angiven sökväg)</span><span class="sxs-lookup"><span data-stu-id="52aa0-125">put\_block\_blob\_from\_path (uploads hello contents of a file from hello specified path)</span></span>
2. <span data-ttu-id="52aa0-126">Placera\_block_blob\_från\_fil (Överför hello innehåll från en redan öppnad filström)</span><span class="sxs-lookup"><span data-stu-id="52aa0-126">put\_block_blob\_from\_file (uploads hello contents from an already opened file/stream)</span></span>
3. <span data-ttu-id="52aa0-127">Placera\_block\_blob\_från\_byte (överföringar en byte-matris)</span><span class="sxs-lookup"><span data-stu-id="52aa0-127">put\_block\_blob\_from\_bytes (uploads an array of bytes)</span></span>
4. <span data-ttu-id="52aa0-128">Placera\_block\_blob\_från\_text (Överför hello angivna textvärdet med hello angiven kodning)</span><span class="sxs-lookup"><span data-stu-id="52aa0-128">put\_block\_blob\_from\_text (uploads hello specified text value using hello specified encoding)</span></span>

<span data-ttu-id="52aa0-129">Följande exempelkod hello laddar upp en lokal fil tooa behållare:</span><span class="sxs-lookup"><span data-stu-id="52aa0-129">hello following sample code uploads a local file tooa container:</span></span>

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="52aa0-130">hello överför följande exempelkod alla hello-filer (exklusive kataloger) i en lokal katalog tooblob lagring:</span><span class="sxs-lookup"><span data-stu-id="52aa0-130">hello following sample code uploads all hello files (excluding directories) in a local directory tooblob storage:</span></span>

    from azure.storage.blob import BlobService
    from os import listdir
    from os.path import isfile, join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)
    # find all files in hello LOCAL_DIRECT (excluding directory)
    local_file_list = [f for f in listdir(LOCAL_DIRECT) if isfile(join(LOCAL_DIRECT, f))]

    file_num = len(local_file_list)
    for i in range(file_num):
        local_file = join(LOCAL_DIRECT, local_file_list[i])
        blob_name = local_file_list[i]
        try:
            blob_service.put_block_blob_from_path(CONTAINER_NAME, blob_name, local_file)
        except:
            print "something wrong happened when uploading hello data %s"%blob_name


## <a name="download-data-from-blob"></a><span data-ttu-id="52aa0-131">Hämta Data från Blob</span><span class="sxs-lookup"><span data-stu-id="52aa0-131">Download Data from Blob</span></span>
<span data-ttu-id="52aa0-132">Använd följande metoder toodownload data från en blob hello:</span><span class="sxs-lookup"><span data-stu-id="52aa0-132">Use hello following methods toodownload data from a blob:</span></span>

1. <span data-ttu-id="52aa0-133">Hämta\_blob\_till\_sökväg</span><span class="sxs-lookup"><span data-stu-id="52aa0-133">get\_blob\_to\_path</span></span>
2. <span data-ttu-id="52aa0-134">Hämta\_blob\_till\_fil</span><span class="sxs-lookup"><span data-stu-id="52aa0-134">get\_blob\_to\_file</span></span>
3. <span data-ttu-id="52aa0-135">Hämta\_blob\_till\_byte</span><span class="sxs-lookup"><span data-stu-id="52aa0-135">get\_blob\_to\_bytes</span></span>
4. <span data-ttu-id="52aa0-136">Hämta\_blob\_till\_text</span><span class="sxs-lookup"><span data-stu-id="52aa0-136">get\_blob\_to\_text</span></span>

<span data-ttu-id="52aa0-137">Dessa metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.</span><span class="sxs-lookup"><span data-stu-id="52aa0-137">These methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="52aa0-138">hello hämtar följande exempelkod hello innehållet i en blobb i behållaren tooa lokala filer:</span><span class="sxs-lookup"><span data-stu-id="52aa0-138">hello following sample code downloads hello contents of a blob in a container tooa local file:</span></span>

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

<span data-ttu-id="52aa0-139">hello följande exempelkod laddar ned alla blobbar från en behållare.</span><span class="sxs-lookup"><span data-stu-id="52aa0-139">hello following sample code downloads all blobs from a container.</span></span> <span data-ttu-id="52aa0-140">Den använder listan\_tooget hello lista över tillgängliga blobbar i behållaren hello-blobbar och hämtar dem tooa lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="52aa0-140">It uses list\_blobs tooget hello list of available blobs in hello container and downloads them tooa local directory.</span></span>

    from azure.storage.blob import BlobService
    from os.path import join

    # Set parameters here
    ACCOUNT_NAME = "<your_account_name>"
    ACCOUNT_KEY = "<your_account_key>"
    CONTAINER_NAME = "<your_container_name>"
    LOCAL_DIRECT = "<your_local_directory>"        

    blob_service = BlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

    # List all blobs and download them one by one
    blobs = blob_service.list_blobs(CONTAINER_NAME)
    for blob in blobs:
        local_file = join(LOCAL_DIRECT, blob.name)
        try:
            blob_service.get_blob_to_path(CONTAINER_NAME, blob.name, local_file)
        except:
            print "something wrong happened when downloading hello data %s"%blob.name
