---
title: "aaaMove Data tooand från Azure Blob Storage med hjälp av AzCopy | Microsoft Docs"
description: "Flytta Data tooand från Azure Blob Storage med hjälp av AzCopy"
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: c309ceb2-0e83-4a07-b16d-c997dcd62d5c
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: b381ba004ac16879b6c633950d03d13ad82d5b72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a>Flytta data tooand från Azure Blob Storage med hjälp av AzCopy
AzCopy är ett kommandoradsverktyg som utformats för att ladda upp, hämtar och kopiera data tooand från Microsoft Azure blob-, fil- och tabellagring.

Anvisningar om hur du installerar AzCopy och ytterligare information med hello Azure-plattformen finns [komma igång med kommandoradsverktyget Azcopy hello](../storage/common/storage-use-azcopy.md).

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Om du använder en virtuell dator som har konfigurerats med hello-skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan AzCopy är redan installerad på hello VM.
> 
> [!NOTE]
> Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

## <a name="prerequisites"></a>Krav
Det här dokumentet förutsätts att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot. Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.

* tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).
* Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).

## <a name="run-azcopy-commands"></a>Kör kommandon för AzCopy
toorun AzCopy-kommandon, öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy på datorn där hello AzCopy.exe körbara filen finns. 

grundläggande hello-syntaxen för AzCopy kommandon är:

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> Du kan lägga till hello AzCopy tooyour systemsökvägen installationsplatsen och kör sedan hello kommandon från en katalog. Som standard installeras AzCopy för*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* eller *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.
> 
> 

## <a name="upload-files-tooan-azure-blob"></a>Ladda upp filer tooan Azure blob
tooupload en fil, Använd hello följande kommando:

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a>Hämta filer från en Azure blob
toodownload en fil från en Azure blob, Använd hello följande kommando:

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a>Överföra blobbar mellan Azure-behållare
tootransfer blobbar mellan Azure behållare använder hello följande kommando:

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a>Tips om att använda AzCopy
> [!TIP]
> 1. När **överför** filer, */S* Överför filer rekursivt. Utan den här parametern överförs inte filer i underkataloger.  
> 2. När **hämtar** filen */S* sökningar hello behållaren rekursivt tills alla filer i hello angiven katalog och underkataloger eller alla filer som matchar hello specifikt mönster i hello anges katalogen och dess underkataloger laddas ned.  
> 3. Du kan inte ange en **specifika blob-fil** toodownload med hello */Source* parameter. toodownload en viss fil, ange hello blob-filen namnet toodownload med hello */mönstret* parameter. **/S** parametern kan vara används toohave AzCopy leta efter en fil namn mönster rekursivt. Laddar ned alla filer i katalogen utan hello mönsterparametern AzCopy.
> 
> 

