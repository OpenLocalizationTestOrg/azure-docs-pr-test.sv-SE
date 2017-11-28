---
title: "Så här använder du PowerShell för att hantera Azure File Storage | Microsoft Docs"
description: "Lär dig hur du använder PowerShell för att hantera Azure File Storage."
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
ms.openlocfilehash: 148375b156c4ae1aa4bf203d215f7ed607a71b89
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="how-to-use-powershell-to-manage-azure-file-storage"></a><span data-ttu-id="0696c-103">Så här använder du PowerShell för att hantera Azure File Storage</span><span class="sxs-lookup"><span data-stu-id="0696c-103">How to use PowerShell to manage Azure File storage</span></span>
<span data-ttu-id="0696c-104">Du kan skapa och hantera filresurser med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0696c-104">You can use Azure PowerShell to create and manage file shares.</span></span>

## <a name="install-the-powershell-cmdlets-for-azure-storage"></a><span data-ttu-id="0696c-105">Installera PowerShell-cmdlets för Azure Storage</span><span class="sxs-lookup"><span data-stu-id="0696c-105">Install the PowerShell cmdlets for Azure Storage</span></span>
<span data-ttu-id="0696c-106">Innan du kan använda PowerShell laddar du ned och installerar Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0696c-106">To prepare to use PowerShell, download and install the Azure PowerShell cmdlets.</span></span> <span data-ttu-id="0696c-107">Information om installationsplatser och installationsanvisningar finns i [Installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="0696c-107">See [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) for the install point and installation instructions.</span></span>

> [!NOTE]
> <span data-ttu-id="0696c-108">Vi rekommenderar att du laddar ned och installerar eller uppgradera till den senaste Azure PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="0696c-108">It's recommended that you download and install or upgrade to the latest Azure PowerShell module.</span></span>
> 
> 

<span data-ttu-id="0696c-109">Öppna ett Azure PowerShell-fönster genom att klicka på **Start** och skriva **Windows PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="0696c-109">Open an Azure PowerShell window by clicking **Start** and typing **Windows PowerShell**.</span></span> <span data-ttu-id="0696c-110">Azure Powershell-modulen läses in i PowerShell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="0696c-110">The PowerShell window loads the Azure Powershell module for you.</span></span>

## <a name="create-a-context-for-your-storage-account-and-key"></a><span data-ttu-id="0696c-111">Skapa en kontext för ditt lagringskonto och din nyckel</span><span class="sxs-lookup"><span data-stu-id="0696c-111">Create a context for your storage account and key</span></span>
<span data-ttu-id="0696c-112">Skapa kontexten för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0696c-112">Create the storage account context.</span></span> <span data-ttu-id="0696c-113">Kontexten innehåller lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="0696c-113">The context encapsulates the storage account name and account key.</span></span> <span data-ttu-id="0696c-114">Anvisningar för hur du kopierar din kontonyckel från [Azure Portal](https://portal.azure.com) finns i [Visa och kopiera åtkomstnycklar för lagring](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="0696c-114">For instructions on copying your account key from the [Azure portal](https://portal.azure.com), see [View and copy storage access keys](storage-create-storage-account.md#view-and-copy-storage-access-keys).</span></span>

<span data-ttu-id="0696c-115">Ersätt `storage-account-name` och `storage-account-key` med lagringskontots namn och nyckel i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="0696c-115">Replace `storage-account-name` and `storage-account-key` with your storage account name and key in the following example.</span></span>

```powershell
# create a context for account and key
$ctx=New-AzureStorageContext storage-account-name storage-account-key
```

## <a name="create-a-new-file-share"></a><span data-ttu-id="0696c-116">Skapa en ny filresurs</span><span class="sxs-lookup"><span data-stu-id="0696c-116">Create a new file share</span></span>
<span data-ttu-id="0696c-117">Skapa en ny resurs med namnet `logs`.</span><span class="sxs-lookup"><span data-stu-id="0696c-117">Create the new share, named `logs`.</span></span>

```powershell
# create a new share
$s = New-AzureStorageShare logs -Context $ctx
```

<span data-ttu-id="0696c-118">Nu har du en filresurs i File Storage.</span><span class="sxs-lookup"><span data-stu-id="0696c-118">You now have a file share in File storage.</span></span> <span data-ttu-id="0696c-119">Härnäst ska vi lägga till en katalog och en fil.</span><span class="sxs-lookup"><span data-stu-id="0696c-119">Next we'll add a directory and a file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0696c-120">Namnet på filresursen får bara innehålla gemener.</span><span class="sxs-lookup"><span data-stu-id="0696c-120">The name of your file share must be all lowercase.</span></span> <span data-ttu-id="0696c-121">Mer information om hur du namnger filresurser och filer finns i [Namnge och referera till resurser, kataloger, filer och Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span><span class="sxs-lookup"><span data-stu-id="0696c-121">For complete details about naming file shares and files, see [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx).</span></span>
> 
> 

## <a name="create-a-directory-in-the-file-share"></a><span data-ttu-id="0696c-122">Skapa en katalog i filresursen</span><span class="sxs-lookup"><span data-stu-id="0696c-122">Create a directory in the file share</span></span>
<span data-ttu-id="0696c-123">Skapa en katalog i resursen.</span><span class="sxs-lookup"><span data-stu-id="0696c-123">Create a directory in the share.</span></span> <span data-ttu-id="0696c-124">I följande exempel heter katalogen `CustomLogs`.</span><span class="sxs-lookup"><span data-stu-id="0696c-124">In the following example, the directory is named `CustomLogs`.</span></span>

```powershell
# create a directory in the share
New-AzureStorageDirectory -Share $s -Path CustomLogs
```

## <a name="upload-a-local-file-to-the-directory"></a><span data-ttu-id="0696c-125">Ladda upp en lokal fil till katalogen</span><span class="sxs-lookup"><span data-stu-id="0696c-125">Upload a local file to the directory</span></span>
<span data-ttu-id="0696c-126">Nu ska du ladda upp en lokal fil till katalogen.</span><span class="sxs-lookup"><span data-stu-id="0696c-126">Now upload a local file to the directory.</span></span> <span data-ttu-id="0696c-127">I följande exempel laddar vi upp en fil från `C:\temp\Log1.txt`.</span><span class="sxs-lookup"><span data-stu-id="0696c-127">The following example uploads a file from `C:\temp\Log1.txt`.</span></span> <span data-ttu-id="0696c-128">Ändra sökvägen till filen så att den pekar på en giltig fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="0696c-128">Edit the file path so that it points to a valid file on your local machine.</span></span>

```powershell
# upload a local file to the new directory
Set-AzureStorageFileContent -Share $s -Source C:\temp\Log1.txt -Path CustomLogs
```

## <a name="list-the-files-in-the-directory"></a><span data-ttu-id="0696c-129">Visa en lista med filerna i katalogen</span><span class="sxs-lookup"><span data-stu-id="0696c-129">List the files in the directory</span></span>
<span data-ttu-id="0696c-130">Om du vill visa filen i katalogen kan du visa en lista med alla filer i katalogen.</span><span class="sxs-lookup"><span data-stu-id="0696c-130">To see the file in the directory, you can list all of the directory's files.</span></span> <span data-ttu-id="0696c-131">Det här kommandot returnerar filer och underkataloger (om det finns några) i katalogen CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="0696c-131">This command returns the files and subdirectories (if there are any) in the CustomLogs directory.</span></span>

```powershell
# list files in the new directory
Get-AzureStorageFile -Share $s -Path CustomLogs | Get-AzureStorageFile
```

<span data-ttu-id="0696c-132">Get-AzureStorageFile returnerar en lista över filer och kataloger för det katalogobjekt som du har angett.</span><span class="sxs-lookup"><span data-stu-id="0696c-132">Get-AzureStorageFile returns a list of files and directories for whatever directory object is passed in.</span></span> <span data-ttu-id="0696c-133">”Get-AzureStorageFile -Share $s” returnerar en lista över filer och kataloger i rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="0696c-133">"Get-AzureStorageFile -Share $s" returns a list of files and directories in the root directory.</span></span> <span data-ttu-id="0696c-134">Om du vill hämta en lista över filerna i en underkatalog måste du skicka underkatalogen till Get-AzureStorageFile.</span><span class="sxs-lookup"><span data-stu-id="0696c-134">To get a list of files in a subdirectory, you have to pass the subdirectory to Get-AzureStorageFile.</span></span> <span data-ttu-id="0696c-135">Detta händer: den första delen av kommandot fram till pipe returnerar en kataloginstans av underkatalogen CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="0696c-135">That's what this does -- the first part of the command up to the pipe returns a directory instance of the subdirectory CustomLogs.</span></span> <span data-ttu-id="0696c-136">Därefter skickas detta till Get-AzureStorageFile, som returnerar filerna och katalogerna i CustomLogs.</span><span class="sxs-lookup"><span data-stu-id="0696c-136">Then that is passed into Get-AzureStorageFile, which returns the files and directories in CustomLogs.</span></span>

## <a name="copy-files"></a><span data-ttu-id="0696c-137">Kopiera filer</span><span class="sxs-lookup"><span data-stu-id="0696c-137">Copy files</span></span>
<span data-ttu-id="0696c-138">Från och med version 0.9.7 av Azure PowerShell kan du kopiera en fil till en annan fil, en fil till en blobb eller en blobb till en fil.</span><span class="sxs-lookup"><span data-stu-id="0696c-138">Beginning with version 0.9.7 of Azure PowerShell, you can copy a file to another file, a file to a blob, or a blob to a file.</span></span> <span data-ttu-id="0696c-139">Nedan ser du hur du utför dessa kopieringsåtgärder med PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0696c-139">Below we demonstrate how to perform these copy operations using PowerShell cmdlets.</span></span>

```powershell
# copy a file to the new directory
Start-AzureStorageFileCopy -SrcShareName srcshare -SrcFilePath srcdir/hello.txt -DestShareName destshare -DestFilePath destdir/hellocopy.txt -Context $srcCtx -DestContext $destCtx

# copy a blob to a file directory
Start-AzureStorageFileCopy -SrcContainerName srcctn -SrcBlobName hello2.txt -DestShareName hello -DestFilePath hellodir/hello2copy.txt -DestContext $ctx -Context $ctx
```
## <a name="next-steps"></a><span data-ttu-id="0696c-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0696c-140">Next steps</span></span>
<span data-ttu-id="0696c-141">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="0696c-141">See these links for more information about Azure File storage.</span></span>

* [<span data-ttu-id="0696c-142">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="0696c-142">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="0696c-143">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0696c-143">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)