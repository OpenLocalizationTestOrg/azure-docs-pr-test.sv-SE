---
title: aaaHow toouse PowerShell toomanage Azure File storage | Microsoft Docs
description: "Lär dig toouse PowerShell toomanage Azure File storage."
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: 0e30e8796cf8bbf5f9249b26179d5e0f9077c8fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-powershell-toomanage-azure-file-storage"></a><span data-ttu-id="48c33-103">Hur toouse PowerShell toomanage Azure File storage</span><span class="sxs-lookup"><span data-stu-id="48c33-103">How toouse PowerShell toomanage Azure File storage</span></span>
<span data-ttu-id="48c33-104">Du kan använda Azure PowerShell toocreate och hantera filresurser.</span><span class="sxs-lookup"><span data-stu-id="48c33-104">You can use Azure PowerShell toocreate and manage file shares.</span></span>

## <a name="install-hello-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="48c33-105">Installera hello PowerShell-cmdlets för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="48c33-105">Install hello PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="48c33-106">tooprepare toouse PowerShell, hämta och installera hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="48c33-106">tooprepare toouse PowerShell, download and install hello Azure PowerShell cmdlets.</span></span> <span data-ttu-id="48c33-107">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs) för hello installera installationsplatser och installationsanvisningar.</span><span class="sxs-lookup"><span data-stu-id="48c33-107">See [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for hello install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="48c33-108">Det rekommenderas att du hämtar och installerar eller uppgraderar toohello senaste Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="48c33-108">It's recommended that you download and install or upgrade toohello latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="48c33-109">Öppna ett Azure PowerShell-fönster genom att klicka på **Start** och skriva **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="48c33-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="48c33-110">hello PowerShell-fönstret läser in hello Azure Powershell-modulen för dig.</span><span class="sxs-lookup"><span data-stu-id="48c33-110">hello PowerShell window loads hello Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="48c33-111">Skapa en kontext för ditt lagringskonto och din nyckel</span><span class="sxs-lookup"><span data-stu-id="48c33-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="48c33-112">Skapa hello kontexten för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="48c33-112">Create hello storage account context.</span></span> <span data-ttu-id="48c33-113">hello kontexten innehåller hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="48c33-113">hello context encapsulates hello storage account name and account key.</span></span> <span data-ttu-id="48c33-114">Anvisningar om hur du kopierar din kontonyckel från hello [Azure-portalen](https://portal.azure.com), se [visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="48c33-114">For instructions on copying your account key from hello [Azure portal](https://portal.azure.com), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="48c33-115">Ersätt `storage-account-name` och `storage-account-key` med lagringskontonamn och nyckel i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="48c33-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in hello following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="48c33-116">Skapa en ny filresurs</span><span class="sxs-lookup"><span data-stu-id="48c33-116">Create a new file share</span></span>
<span data-ttu-id="48c33-117">Skapa hello ny resurs med namnet `logs`.</span><span class="sxs-lookup"><span data-stu-id="48c33-117">Create hello new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="48c33-118">Nu har du en filresurs i File Storage.</span><span class="sxs-lookup"><span data-stu-id="48c33-118">You now have a file share in File storage.</span></span> <span data-ttu-id="48c33-119">Härnäst ska vi lägga till en katalog och en fil.</span><span class="sxs-lookup"><span data-stu-id="48c33-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48c33-120">hello namnet på filresursen får bara innehålla gemener.</span><span class="sxs-lookup"><span data-stu-id="48c33-120">hello name of your file share must be all lowercase.</span></span> <span data-ttu-id="48c33-121">Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="48c33-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-hello-file-share"></a><span data-ttu-id="48c33-122">Skapa en katalog i hello filresurs</span><span class="sxs-lookup"><span data-stu-id="48c33-122">Create a directory in hello file share</span></span>
<span data-ttu-id="48c33-123">Skapa en katalog på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="48c33-123">Create a directory in hello share.</span></span> <span data-ttu-id="48c33-124">I följande exempel hello, hello directory heter `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="48c33-124">In hello following example, hello directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in hello share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-toohello-directory"></a><span data-ttu-id="48c33-125">Ladda upp en lokal katalog i toohello</span><span class="sxs-lookup"><span data-stu-id="48c33-125">Upload a local file toohello directory</span></span>
<span data-ttu-id="48c33-126">Ladda upp en lokal fil toohello katalog.</span><span class="sxs-lookup"><span data-stu-id="48c33-126">Now upload a local file toohello directory.</span></span> <span data-ttu-id="48c33-127">hello följande exempel överför en fil från `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="48c33-127">hello following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="48c33-128">Redigera hello sökvägen så att den pekar tooa giltig fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="48c33-128">Edit hello file path so that it points tooa valid file on your local machine.</span></span>

```powershell
# upload a local file toohello new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-hello-files-in-hello-directory"></a><span data-ttu-id="48c33-129">Lista över hello filer i hello directory</span><span class="sxs-lookup"><span data-stu-id="48c33-129">List hello files in hello directory</span></span>
<span data-ttu-id="48c33-130">toosee hello-fil i hello, kan du visa alla hello directory filer.</span><span class="sxs-lookup"><span data-stu-id="48c33-130">toosee hello file in hello directory, you can list all of hello directory's files.</span></span> <span data-ttu-id="48c33-131">Det här kommandot returnerar hello filer och underkataloger (om det finns några) i hello CustomLogs directory.</span><span class="sxs-lookup"><span data-stu-id="48c33-131">This command returns hello files and subdirectories (if there are any) in hello CustomLogs directory.</span></span>

```powershell
# list files in hello new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="48c33-132">Get-AzureStorageFile returnerar en lista över filer och kataloger för det katalogobjekt som du har angett.</span><span class="sxs-lookup"><span data-stu-id="48c33-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="48c33-133">”Get-AzureStorageFile-Share $s” returnerar en lista över filer och kataloger i rotkatalogen för hello.</span><span class="sxs-lookup"><span data-stu-id="48c33-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in hello root directory.</span></span> <span data-ttu-id="48c33-134">tooget en lista över filer i en underkatalog har toopass hello underkatalog tooGet-AzureStorageFile.</span><span class="sxs-lookup"><span data-stu-id="48c33-134">tooget a list of files in a subdirectory, you have toopass hello subdirectory tooGet-AzureStorageFile.</span></span> <span data-ttu-id="48c33-135">Det är det här--hello första delen av kommandot hello toohello pipe returnerar en kataloginstans av hello underkatalogen CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="48c33-135">That's what this does -- hello first part of hello command up toohello pipe returns a directory instance of hello subdirectory CustomLogs.</span></span> <span data-ttu-id="48c33-136">Därefter skickas detta till Get-AzureStorageFile, som returnerar hello filer och kataloger i CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="48c33-136">Then that is passed into Get-AzureStorageFile, which returns hello files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="48c33-137">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="48c33-137">Copy files</span></span>
<span data-ttu-id="48c33-138">Från och med version 0.9.7 av Azure PowerShell kan kopiera du en fil för tooanother, en tooa blob-fil eller en tooa blob-fil.</span><span class="sxs-lookup"><span data-stu-id="48c33-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file tooanother file, a file tooa blob, or a blob tooa file.</span></span> <span data-ttu-id="48c33-139">Nedan ser du hur tooperform dessa kopiera åtgärder med hjälp av PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="48c33-139">Below we demonstrate how tooperform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file toohello new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob tooa file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="48c33-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48c33-140">Next steps</span></span>
<span data-ttu-id="48c33-141">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="48c33-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="48c33-142">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="48c33-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="48c33-143">Felsökning</span><span class="sxs-lookup"><span data-stu-id="48c33-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)