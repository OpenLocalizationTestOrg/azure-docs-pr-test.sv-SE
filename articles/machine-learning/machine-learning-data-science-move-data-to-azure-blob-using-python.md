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
# <a name="move-data-tooand-from-azure-blob-storage-using-python"></a>Flytta data tooand från Azure Blob Storage med hjälp av Python
Det här avsnittet beskrivs hur toolist, ladda upp och hämta blobar med hjälp av hello Python-API. Med hello Python-API finns i Azure SDK, kan du:

* Skapa en behållare
* Ladda upp en blobb till en behållare
* Ladda ned blobbar
* Lista hello blobbar i en behållare
* Ta bort en blob

Mer information om hur du använder hello Python-API finns [hur tooUse hello tjänsten för Blob Storage från Python](../storage/blobs/storage-python-how-to-use-blob-storage.md).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Om du använder en virtuell dator som har konfigurerats med hello-skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan AzCopy är redan installerad på hello VM.
> 
> [!NOTE]
> Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Krav
Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot. Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.

* tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).

## <a name="upload-data-tooblob"></a>Ladda upp Data tooBlob
Lägg till hello följande fragment hello övre delen av alla Python-kod som du vill tooprogrammatically åtkomst till Azure Storage:

    from azure.storage.blob import BlobService

Hej **BlobService** objekt kan du arbeta med behållare och blobbar. hello följande kod skapar ett BlobService-objekt med hjälp av hello lagringskontots namn och åtkomstnyckel. Ersätt kontonamnet och kontonyckel med din verkliga konto och nyckel.

    blob_service = BlobService(account_name="<your_account_name>", account_key="<your_account_key>")

Använd följande metoder tooupload data tooa blob hello:

1. Placera\_block\_blob\_från\_sökväg (Överför hello innehållet i en fil från hello angiven sökväg)
2. Placera\_block_blob\_från\_fil (Överför hello innehåll från en redan öppnad filström)
3. Placera\_block\_blob\_från\_byte (överföringar en byte-matris)
4. Placera\_block\_blob\_från\_text (Överför hello angivna textvärdet med hello angiven kodning)

Följande exempelkod hello laddar upp en lokal fil tooa behållare:

    blob_service.put_block_blob_from_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

hello överför följande exempelkod alla hello-filer (exklusive kataloger) i en lokal katalog tooblob lagring:

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


## <a name="download-data-from-blob"></a>Hämta Data från Blob
Använd följande metoder toodownload data från en blob hello:

1. Hämta\_blob\_till\_sökväg
2. Hämta\_blob\_till\_fil
3. Hämta\_blob\_till\_byte
4. Hämta\_blob\_till\_text

Dessa metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.

hello hämtar följande exempelkod hello innehållet i en blobb i behållaren tooa lokala filer:

    blob_service.get_blob_to_path("<your_container_name>", "<your_blob_name>", "<your_local_file_name>")

hello följande exempelkod laddar ned alla blobbar från en behållare. Den använder listan\_tooget hello lista över tillgängliga blobbar i behållaren hello-blobbar och hämtar dem tooa lokal katalog.

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
