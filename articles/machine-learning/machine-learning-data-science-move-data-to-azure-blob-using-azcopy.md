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
# <a name="move-data-tooand-from-azure-blob-storage-using-azcopy"></a><span data-ttu-id="87bea-103">Flytta data tooand från Azure Blob Storage med hjälp av AzCopy</span><span class="sxs-lookup"><span data-stu-id="87bea-103">Move data tooand from Azure Blob Storage using AzCopy</span></span>
<span data-ttu-id="87bea-104">AzCopy är ett kommandoradsverktyg som utformats för att ladda upp, hämtar och kopiera data tooand från Microsoft Azure blob-, fil- och tabellagring.</span><span class="sxs-lookup"><span data-stu-id="87bea-104">AzCopy is a command-line utility designed for uploading, downloading, and copying data tooand from Microsoft Azure blob, file, and table storage.</span></span>

<span data-ttu-id="87bea-105">Anvisningar om hur du installerar AzCopy och ytterligare information med hello Azure-plattformen finns [komma igång med kommandoradsverktyget Azcopy hello](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="87bea-105">For instructions on installing AzCopy and additional information on using it with hello Azure platform, see [Getting Started with hello AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="87bea-106">Om du använder en virtuell dator som har konfigurerats med hello-skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan AzCopy är redan installerad på hello VM.</span><span class="sxs-lookup"><span data-stu-id="87bea-106">If you are using VM that was set up with hello scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on hello VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="87bea-107">Fullständig presentation tooAzure blob storage finns för[grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och för[Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="87bea-107">For a complete introduction tooAzure blob storage, refer too[Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and too[Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="87bea-108">Krav</span><span class="sxs-lookup"><span data-stu-id="87bea-108">Prerequisites</span></span>
<span data-ttu-id="87bea-109">Det här dokumentet förutsätts att du har en Azure-prenumeration, ett lagringskonto och hello motsvarande lagringsnyckel för det kontot.</span><span class="sxs-lookup"><span data-stu-id="87bea-109">This document assumes that you have an Azure subscription, a storage account and hello corresponding storage key for that account.</span></span> <span data-ttu-id="87bea-110">Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="87bea-110">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="87bea-111">tooset upp en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="87bea-111">tooset up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="87bea-112">Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="87bea-112">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="run-azcopy-commands"></a><span data-ttu-id="87bea-113">Kör kommandon för AzCopy</span><span class="sxs-lookup"><span data-stu-id="87bea-113">Run AzCopy commands</span></span>
<span data-ttu-id="87bea-114">toorun AzCopy-kommandon, öppna ett kommandofönster och gå toohello installationskatalogen för AzCopy på datorn där hello AzCopy.exe körbara filen finns.</span><span class="sxs-lookup"><span data-stu-id="87bea-114">toorun AzCopy commands, open a command window and navigate toohello AzCopy installation directory on your computer, where hello AzCopy.exe executable is located.</span></span> 

<span data-ttu-id="87bea-115">grundläggande hello-syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="87bea-115">hello basic syntax for AzCopy commands is:</span></span>

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> <span data-ttu-id="87bea-116">Du kan lägga till hello AzCopy tooyour systemsökvägen installationsplatsen och kör sedan hello kommandon från en katalog.</span><span class="sxs-lookup"><span data-stu-id="87bea-116">You can add hello AzCopy installation location tooyour system path and then run hello commands from any directory.</span></span> <span data-ttu-id="87bea-117">Som standard installeras AzCopy för*% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* eller *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="87bea-117">By default, AzCopy is installed too*%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* or *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span></span>
> 
> 

## <a name="upload-files-tooan-azure-blob"></a><span data-ttu-id="87bea-118">Ladda upp filer tooan Azure blob</span><span class="sxs-lookup"><span data-stu-id="87bea-118">Upload files tooan Azure blob</span></span>
<span data-ttu-id="87bea-119">tooupload en fil, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="87bea-119">tooupload a file, use hello following command:</span></span>

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a><span data-ttu-id="87bea-120">Hämta filer från en Azure blob</span><span class="sxs-lookup"><span data-stu-id="87bea-120">Download files from an Azure blob</span></span>
<span data-ttu-id="87bea-121">toodownload en fil från en Azure blob, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="87bea-121">toodownload a file from an Azure blob, use hello following command:</span></span>

    # Downloading blobs toolocal file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a><span data-ttu-id="87bea-122">Överföra blobbar mellan Azure-behållare</span><span class="sxs-lookup"><span data-stu-id="87bea-122">Transfer blobs between Azure containers</span></span>
<span data-ttu-id="87bea-123">tootransfer blobbar mellan Azure behållare använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="87bea-123">tootransfer blobs between Azure containers, use hello following command:</span></span>

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: hello sub directory in hello container
    <your_local_directory>: directory of local file system where files toobe uploaded from or hello directory of local file system files toobe downloaded to
    <file_pattern>: pattern of file names toobe transferred. hello standard wildcards are supported


## <a name="tips-for-using-azcopy"></a><span data-ttu-id="87bea-124">Tips om att använda AzCopy</span><span class="sxs-lookup"><span data-stu-id="87bea-124">Tips for using AzCopy</span></span>
> [!TIP]
> 1. <span data-ttu-id="87bea-125">När **överför** filer, */S* Överför filer rekursivt.</span><span class="sxs-lookup"><span data-stu-id="87bea-125">When **uploading** files, */S* uploads files recursively.</span></span> <span data-ttu-id="87bea-126">Utan den här parametern överförs inte filer i underkataloger.</span><span class="sxs-lookup"><span data-stu-id="87bea-126">Without this parameter, files in subdirectories are not uploaded.</span></span>  
> 2. <span data-ttu-id="87bea-127">När **hämtar** filen */S* sökningar hello behållaren rekursivt tills alla filer i hello angiven katalog och underkataloger eller alla filer som matchar hello specifikt mönster i hello anges katalogen och dess underkataloger laddas ned.</span><span class="sxs-lookup"><span data-stu-id="87bea-127">When **downloading** file, */S* searches hello container recursively until all files in hello specified directory and its subdirectories, or all files that match hello specified pattern in hello given directory and its subdirectories, are downloaded.</span></span>  
> 3. <span data-ttu-id="87bea-128">Du kan inte ange en **specifika blob-fil** toodownload med hello */Source* parameter.</span><span class="sxs-lookup"><span data-stu-id="87bea-128">You cannot specify a **specific blob file** toodownload using hello */Source* parameter.</span></span> <span data-ttu-id="87bea-129">toodownload en viss fil, ange hello blob-filen namnet toodownload med hello */mönstret* parameter.</span><span class="sxs-lookup"><span data-stu-id="87bea-129">toodownload a specific file, specify hello blob file name toodownload using hello */Pattern* parameter.</span></span> <span data-ttu-id="87bea-130">**/S** parametern kan vara används toohave AzCopy leta efter en fil namn mönster rekursivt.</span><span class="sxs-lookup"><span data-stu-id="87bea-130">**/S** parameter can be used toohave AzCopy look for a file name pattern recursively.</span></span> <span data-ttu-id="87bea-131">Laddar ned alla filer i katalogen utan hello mönsterparametern AzCopy.</span><span class="sxs-lookup"><span data-stu-id="87bea-131">Without hello pattern parameter, AzCopy downloads all files in that directory.</span></span>
> 
> 

