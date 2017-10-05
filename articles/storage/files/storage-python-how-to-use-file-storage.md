---
title: "Utveckla för Azure File storage med Python | Microsoft Docs"
description: "Lär dig hur du utvecklar Python-program och tjänster som använder Azure File storage för att lagra fildata."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: 3dd14e8d3ea7d1e50f41633a7920a6d36becf789
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="653a2-103">Utveckla för Azure File storage med Python</span><span class="sxs-lookup"><span data-stu-id="653a2-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="653a2-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="653a2-104">About this tutorial</span></span>
<span data-ttu-id="653a2-105">Den här kursen visar grunderna i Python för att utveckla program eller tjänster som använder Azure File storage för att lagra fildata.</span><span class="sxs-lookup"><span data-stu-id="653a2-105">This tutorial will demonstrate the basics of using Python to develop applications or services that use Azure File storage to store file data.</span></span> <span data-ttu-id="653a2-106">I den här kursen ska vi skapa ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med Python och Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="653a2-106">In this tutorial, we will create a simple console application and show how to perform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="653a2-107">Skapa Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="653a2-107">Create Azure File shares</span></span>
* <span data-ttu-id="653a2-108">Skapa kataloger</span><span class="sxs-lookup"><span data-stu-id="653a2-108">Create directories</span></span>
* <span data-ttu-id="653a2-109">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="653a2-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="653a2-110">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="653a2-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="653a2-111">Eftersom Azure File storage kan nås över SMB, är det möjligt att skriva enkla program som har åtkomst till Azure filresursen med standard Python-i/o-klasser och funktioner.</span><span class="sxs-lookup"><span data-stu-id="653a2-111">Because Azure File storage may be accessed over SMB, it is possible to write simple applications that access the Azure File share using the standard Python I/O classes and functions.</span></span> <span data-ttu-id="653a2-112">Den här artikeln beskriver hur du skriver program som använder Azure Storage Python SDK, som använder den [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tala med Azure File storage.</span><span class="sxs-lookup"><span data-stu-id="653a2-112">This article will describe how to write applications that use the Azure Storage Python SDK, which uses the [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) to talk to Azure File storage.</span></span>

### <a name="set-up-your-application-to-use-azure-file-storage"></a><span data-ttu-id="653a2-113">Konfigurera din app att använda Azure File storage</span><span class="sxs-lookup"><span data-stu-id="653a2-113">Set up your application to use Azure File storage</span></span>
<span data-ttu-id="653a2-114">Lägg till följande längst upp i alla Python-källfilen som du vill komma åt Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="653a2-114">Add the following near the top of any Python source file in which you wish to programmatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a><span data-ttu-id="653a2-115">Konfigurera en anslutning till Azure File storage</span><span class="sxs-lookup"><span data-stu-id="653a2-115">Set up a connection to Azure File storage</span></span> 
<span data-ttu-id="653a2-116">Den `FileService` objektet kan du arbeta med resurser, kataloger och filer.</span><span class="sxs-lookup"><span data-stu-id="653a2-116">The `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="653a2-117">Följande kod skapar en `FileService` objekt med lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="653a2-117">The following code creates a `FileService` object using the storage account name and account key.</span></span> <span data-ttu-id="653a2-118">Ersätt `<myaccount>` och `<mykey>` med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="653a2-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="653a2-119">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="653a2-119">Create an Azure File share</span></span>
<span data-ttu-id="653a2-120">I följande kodexempel, kan du använda en `FileService` objekt att skapa resursen om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="653a2-120">In the following code example, you can use a `FileService` object to create the share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="653a2-121">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="653a2-121">Create a directory</span></span>
<span data-ttu-id="653a2-122">Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="653a2-122">You can also organize storage by putting files inside sub-directories instead of having all of them in the root directory.</span></span> <span data-ttu-id="653a2-123">Azure File storage kan du skapa så många kataloger som ditt konto tillåter.</span><span class="sxs-lookup"><span data-stu-id="653a2-123">Azure File storage allows you to create as many directories as your account will allow.</span></span> <span data-ttu-id="653a2-124">Koden nedan för att skapa en underkatalog med namnet **sampledir** under rotkatalogen.</span><span class="sxs-lookup"><span data-stu-id="653a2-124">The code below will create a sub-directory named **sampledir** under the root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="653a2-125">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="653a2-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="653a2-126">Om du vill visa en lista över filer och kataloger på en resurs, Använd den **lista\_kataloger\_och\_filer** metod.</span><span class="sxs-lookup"><span data-stu-id="653a2-126">To list the files and directories in a share, use the **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="653a2-127">Den här metoden returnerar en generator.</span><span class="sxs-lookup"><span data-stu-id="653a2-127">This method returns a generator.</span></span> <span data-ttu-id="653a2-128">I följande kod utdata i **namn** av varje fil- och på en resurs i konsolen.</span><span class="sxs-lookup"><span data-stu-id="653a2-128">The following code outputs the **name** of each file and directory in a share to the console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="653a2-129">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="653a2-129">Upload a file</span></span> 
<span data-ttu-id="653a2-130">Azure File resursen innehåller minst, en rotkatalog där filer kan finnas.</span><span class="sxs-lookup"><span data-stu-id="653a2-130">Azure File share contains at the very least, a root directory where files can reside.</span></span> <span data-ttu-id="653a2-131">I det här avsnittet lär du dig hur du överför en fil från lokal lagring till rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="653a2-131">In this section, you'll learn how to upload a file from local storage onto the root directory of a share.</span></span>

<span data-ttu-id="653a2-132">Om du vill skapa en fil och överför data använder den `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` eller `create_file_from_text` metoder.</span><span class="sxs-lookup"><span data-stu-id="653a2-132">To create a file and upload data, use the `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="653a2-133">De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.</span><span class="sxs-lookup"><span data-stu-id="653a2-133">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="653a2-134">`create_file_from_path`Överför innehållet i en fil från den angivna sökvägen och `create_file_from_stream` Överför innehållet från en redan öppnad filström.</span><span class="sxs-lookup"><span data-stu-id="653a2-134">`create_file_from_path` uploads the contents of a file from the specified path, and `create_file_from_stream` uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="653a2-135">`create_file_from_bytes`Överför en matris med byte och `create_file_from_text` överför det angivna textvärdet med den angivna kodningen (standardvärdet är UTF-8).</span><span class="sxs-lookup"><span data-stu-id="653a2-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="653a2-136">I följande exempel Överför innehållet i den **sunset.png** filen till den **minfil** fil.</span><span class="sxs-lookup"><span data-stu-id="653a2-136">The following example uploads the contents of the **sunset.png** file into the **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="653a2-137">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="653a2-137">Download a file</span></span>
<span data-ttu-id="653a2-138">Hämta data från en fil, `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, eller `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="653a2-138">To download data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="653a2-139">De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.</span><span class="sxs-lookup"><span data-stu-id="653a2-139">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="653a2-140">I följande exempel visas hur du använder `get_file_to_path` att hämta innehållet i den **minfil** filen och spara den till den **out sunset.png** fil.</span><span class="sxs-lookup"><span data-stu-id="653a2-140">The following example demonstrates using `get_file_to_path` to download the contents of the **myfile** file and store it to the **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="653a2-141">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="653a2-141">Delete a file</span></span>
<span data-ttu-id="653a2-142">Slutligen, om du vill ta bort en fil, anropa `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="653a2-142">Finally, to delete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="653a2-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="653a2-143">Next steps</span></span>
<span data-ttu-id="653a2-144">Nu när du har lärt dig hur du hanterar Azure File storage med Python, kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="653a2-144">Now that you've learned how to manipulate Azure File storage with Python, follow these links to learn more.</span></span>

* [<span data-ttu-id="653a2-145">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="653a2-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="653a2-146">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="653a2-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="653a2-147">Microsoft Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="653a2-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)