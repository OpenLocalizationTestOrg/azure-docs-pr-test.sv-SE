---
title: "Flytta Data till och från Azure Blob Storage med hjälp av AzCopy | Microsoft Docs"
description: "Flytta data till och från Azure Blob Storage med AzCopy"
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
ms.openlocfilehash: a41ccdd5739a5b10cef201910abd639ae3126c02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azcopy"></a><span data-ttu-id="a344e-103">Flytta data till och från Azure Blob Storage med hjälp av AzCopy</span><span class="sxs-lookup"><span data-stu-id="a344e-103">Move data to and from Azure Blob Storage using AzCopy</span></span>
<span data-ttu-id="a344e-104">AzCopy är ett kommandoradsverktyg som utformats för att ladda upp, hämtar och kopiering av data till och från Microsoft Azure blob-, fil- och tabellagring.</span><span class="sxs-lookup"><span data-stu-id="a344e-104">AzCopy is a command-line utility designed for uploading, downloading, and copying data to and from Microsoft Azure blob, file, and table storage.</span></span>

<span data-ttu-id="a344e-105">Anvisningar om hur du installerar AzCopy och ytterligare information om hur du använder det med Azure-plattformen finns [komma igång med kommandoradsverktyget Azcopy](../storage/common/storage-use-azcopy.md).</span><span class="sxs-lookup"><span data-stu-id="a344e-105">For instructions on installing AzCopy and additional information on using it with the Azure platform, see [Getting Started with the AzCopy Command-Line Utility](../storage/common/storage-use-azcopy.md).</span></span>

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> <span data-ttu-id="a344e-106">Om du använder en virtuell dator som har konfigurerats med skript som tillhandahålls av [datavetenskap virtuella datorer i Azure](machine-learning-data-science-virtual-machines.md), och sedan AzCopy är redan installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a344e-106">If you are using VM that was set up with the scripts provided by [Data Science Virtual machines in Azure](machine-learning-data-science-virtual-machines.md), then AzCopy is already installed on the VM.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="a344e-107">En fullständig introduktion till Azure blob storage finns i [grunderna i Azure Blob](../storage/blobs/storage-dotnet-how-to-use-blobs.md) och [Azure Blob-tjänsten](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span><span class="sxs-lookup"><span data-stu-id="a344e-107">For a complete introduction to Azure blob storage, refer to [Azure Blob Basics](../storage/blobs/storage-dotnet-how-to-use-blobs.md) and to [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a344e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a344e-108">Prerequisites</span></span>
<span data-ttu-id="a344e-109">Det här dokumentet förutsätter att du har en Azure-prenumeration, ett lagringskonto och motsvarande lagringsnyckel för det kontot.</span><span class="sxs-lookup"><span data-stu-id="a344e-109">This document assumes that you have an Azure subscription, a storage account and the corresponding storage key for that account.</span></span> <span data-ttu-id="a344e-110">Innan du laddar upp/hämtar data, måste du känna till din Azure storage-konto och nyckel.</span><span class="sxs-lookup"><span data-stu-id="a344e-110">Before uploading/downloading data, you must know your Azure storage account name and account key.</span></span>

* <span data-ttu-id="a344e-111">Om du vill konfigurera en Azure-prenumeration finns [kostnadsfri utvärderingsversion för en månad](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a344e-111">To set up an Azure subscription, see [Free one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a344e-112">Anvisningar om hur du skapar ett lagringskonto och för att hämta konto och viktig information, se [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="a344e-112">For instructions on creating a storage account and for getting account and key information, see [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

## <a name="run-azcopy-commands"></a><span data-ttu-id="a344e-113">Kör kommandon för AzCopy</span><span class="sxs-lookup"><span data-stu-id="a344e-113">Run AzCopy commands</span></span>
<span data-ttu-id="a344e-114">För att köra AzCopy kommandon, öppna ett kommandofönster och gå till installationskatalogen för AzCopy på datorn där AzCopy.exe körbara finns.</span><span class="sxs-lookup"><span data-stu-id="a344e-114">To run AzCopy commands, open a command window and navigate to the AzCopy installation directory on your computer, where the AzCopy.exe executable is located.</span></span> 

<span data-ttu-id="a344e-115">Den grundläggande syntaxen för AzCopy kommandon är:</span><span class="sxs-lookup"><span data-stu-id="a344e-115">The basic syntax for AzCopy commands is:</span></span>

    AzCopy /Source:<source> /Dest:<destination> [Options]

> [!NOTE]
> <span data-ttu-id="a344e-116">Du kan lägga till installationsplatsen AzCopy i systemsökvägen och kör kommandon från en katalog.</span><span class="sxs-lookup"><span data-stu-id="a344e-116">You can add the AzCopy installation location to your system path and then run the commands from any directory.</span></span> <span data-ttu-id="a344e-117">Som standard installeras AzCopy på *% ProgramFiles (x86) %\Microsoft SDKs\Azure\AzCopy* eller *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span><span class="sxs-lookup"><span data-stu-id="a344e-117">By default, AzCopy is installed to *%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy* or *%ProgramFiles%\Microsoft SDKs\Azure\AzCopy*.</span></span>
> 
> 

## <a name="upload-files-to-an-azure-blob"></a><span data-ttu-id="a344e-118">Ladda upp filer till en Azure blob</span><span class="sxs-lookup"><span data-stu-id="a344e-118">Upload files to an Azure blob</span></span>
<span data-ttu-id="a344e-119">Om du vill överföra en fil, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a344e-119">To upload a file, use the following command:</span></span>

    # Upload from local file system
    AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## <a name="download-files-from-an-azure-blob"></a><span data-ttu-id="a344e-120">Hämta filer från en Azure blob</span><span class="sxs-lookup"><span data-stu-id="a344e-120">Download files from an Azure blob</span></span>
<span data-ttu-id="a344e-121">Om du vill hämta en fil från en Azure blob, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a344e-121">To download a file from an Azure blob, use the following command:</span></span>

    # Downloading blobs to local file system
    AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## <a name="transfer-blobs-between-azure-containers"></a><span data-ttu-id="a344e-122">Överföra blobbar mellan Azure-behållare</span><span class="sxs-lookup"><span data-stu-id="a344e-122">Transfer blobs between Azure containers</span></span>
<span data-ttu-id="a344e-123">Om du vill överföra blobbar mellan Azure-behållare, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a344e-123">To transfer blobs between Azure containers, use the following command:</span></span>

    # Transferring blobs between Azure containers
    AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

    <your_account_name>: your storage account name
    <your_account_key>: your storage account key
    <your_container_name>: your container name
    <your_sub_directory_at_blob>: the sub directory in the container
    <your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
    <file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## <a name="tips-for-using-azcopy"></a><span data-ttu-id="a344e-124">Tips om att använda AzCopy</span><span class="sxs-lookup"><span data-stu-id="a344e-124">Tips for using AzCopy</span></span>
> [!TIP]
> 1. <span data-ttu-id="a344e-125">När **överför** filer, */S* Överför filer rekursivt.</span><span class="sxs-lookup"><span data-stu-id="a344e-125">When **uploading** files, */S* uploads files recursively.</span></span> <span data-ttu-id="a344e-126">Utan den här parametern överförs inte filer i underkataloger.</span><span class="sxs-lookup"><span data-stu-id="a344e-126">Without this parameter, files in subdirectories are not uploaded.</span></span>  
> 2. <span data-ttu-id="a344e-127">När **hämtar** filen */S* söker behållaren rekursivt tills alla filer i den angivna katalogen och dess underkataloger eller alla filer som matchar det angivna mönstret i den angivna katalogen och dess underkataloger, hämtas.</span><span class="sxs-lookup"><span data-stu-id="a344e-127">When **downloading** file, */S* searches the container recursively until all files in the specified directory and its subdirectories, or all files that match the specified pattern in the given directory and its subdirectories, are downloaded.</span></span>  
> 3. <span data-ttu-id="a344e-128">Du kan inte ange en **specifika blob-fil** att hämta med hjälp av den */Source* parameter.</span><span class="sxs-lookup"><span data-stu-id="a344e-128">You cannot specify a **specific blob file** to download using the */Source* parameter.</span></span> <span data-ttu-id="a344e-129">Om du vill hämta en viss fil, ange filnamnet blob för att hämta med hjälp av den */mönstret* parameter.</span><span class="sxs-lookup"><span data-stu-id="a344e-129">To download a specific file, specify the blob file name to download using the */Pattern* parameter.</span></span> <span data-ttu-id="a344e-130">**/S** parametern kan användas för att leta efter en fil namn mönster rekursivt AzCopy.</span><span class="sxs-lookup"><span data-stu-id="a344e-130">**/S** parameter can be used to have AzCopy look for a file name pattern recursively.</span></span> <span data-ttu-id="a344e-131">Laddar ned alla filer i katalogen utan mönsterparametern AzCopy.</span><span class="sxs-lookup"><span data-stu-id="a344e-131">Without the pattern parameter, AzCopy downloads all files in that directory.</span></span>
> 
> 

