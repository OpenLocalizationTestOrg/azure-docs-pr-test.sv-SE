---
title: "aaaDevelop för Azure File storage med Python | Microsoft Docs"
description: "Lär dig hur toodevelop Python program och tjänster som använder Azure File storage toostore fildata."
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
ms.openlocfilehash: 45623e6dbec6f140cedc4e58e56a93fb4af9054e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a><span data-ttu-id="29d99-103">Utveckla för Azure File storage med Python</span><span class="sxs-lookup"><span data-stu-id="29d99-103">Develop for Azure File storage with Python</span></span>
[!INCLUDE [storage-selector-file-include](../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a><span data-ttu-id="29d99-104">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="29d99-104">About this tutorial</span></span>
<span data-ttu-id="29d99-105">Den här kursen visar hello grunderna i Python toodevelop program eller tjänster som använder Azure File storage toostore fildata.</span><span class="sxs-lookup"><span data-stu-id="29d99-105">This tutorial will demonstrate hello basics of using Python toodevelop applications or services that use Azure File storage toostore file data.</span></span> <span data-ttu-id="29d99-106">I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med Python och Azure File storage:</span><span class="sxs-lookup"><span data-stu-id="29d99-106">In this tutorial, we will create a simple console application and show how tooperform basic actions with Python and Azure File storage:</span></span>

* <span data-ttu-id="29d99-107">Skapa Azure-filresurser</span><span class="sxs-lookup"><span data-stu-id="29d99-107">Create Azure File shares</span></span>
* <span data-ttu-id="29d99-108">Skapa kataloger</span><span class="sxs-lookup"><span data-stu-id="29d99-108">Create directories</span></span>
* <span data-ttu-id="29d99-109">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="29d99-109">Enumerate files and directories in an Azure File share</span></span>
* <span data-ttu-id="29d99-110">Ladda upp, hämta och ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="29d99-110">Upload, download, and delete a file</span></span>

> [!Note]  
> <span data-ttu-id="29d99-111">Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard Python-i/o-klasser och -funktioner.</span><span class="sxs-lookup"><span data-stu-id="29d99-111">Because Azure File storage may be accessed over SMB, it is possible toowrite simple applications that access hello Azure File share using hello standard Python I/O classes and functions.</span></span> <span data-ttu-id="29d99-112">Den här artikeln beskriver hur toowrite program som använder hello Azure Storage Python SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span><span class="sxs-lookup"><span data-stu-id="29d99-112">This article will describe how toowrite applications that use hello Azure Storage Python SDK, which uses hello [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.</span></span>

### <a name="set-up-your-application-toouse-azure-file-storage"></a><span data-ttu-id="29d99-113">Konfigurera ditt program toouse Azure File storage</span><span class="sxs-lookup"><span data-stu-id="29d99-113">Set up your application toouse Azure File storage</span></span>
<span data-ttu-id="29d99-114">Lägg till följande hello hello övre delen av alla Python-källfilen som du vill tooprogrammatically åtkomst till Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="29d99-114">Add hello following near hello top of any Python source file in which you wish tooprogrammatically access Azure Storage.</span></span>

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a><span data-ttu-id="29d99-115">Konfigurera en anslutning tooAzure File storage</span><span class="sxs-lookup"><span data-stu-id="29d99-115">Set up a connection tooAzure File storage</span></span> 
<span data-ttu-id="29d99-116">Hej `FileService` objektet kan du arbeta med resurser, kataloger och filer.</span><span class="sxs-lookup"><span data-stu-id="29d99-116">hello `FileService` object lets you work with shares, directories and files.</span></span> <span data-ttu-id="29d99-117">hello följande kod skapar en `FileService` objektet med hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="29d99-117">hello following code creates a `FileService` object using hello storage account name and account key.</span></span> <span data-ttu-id="29d99-118">Ersätt `<myaccount>` och `<mykey>` med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="29d99-118">Replace `<myaccount>` and `<mykey>` with your account name and key.</span></span>

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a><span data-ttu-id="29d99-119">Skapa en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="29d99-119">Create an Azure File share</span></span>
<span data-ttu-id="29d99-120">I följande kodexempel hello, kan du använda en `FileService` objektet toocreate hello resursen om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="29d99-120">In hello following code example, you can use a `FileService` object toocreate hello share if it doesn't exist.</span></span>

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a><span data-ttu-id="29d99-121">Skapa en katalog</span><span class="sxs-lookup"><span data-stu-id="29d99-121">Create a directory</span></span>
<span data-ttu-id="29d99-122">Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="29d99-122">You can also organize storage by putting files inside sub-directories instead of having all of them in hello root directory.</span></span> <span data-ttu-id="29d99-123">Azure File storage kan toocreate som många kataloger som ditt konto kommer tillåta.</span><span class="sxs-lookup"><span data-stu-id="29d99-123">Azure File storage allows you toocreate as many directories as your account will allow.</span></span> <span data-ttu-id="29d99-124">hello koden nedan skapar en underkatalog med namnet **sampledir** under hello rotkatalog.</span><span class="sxs-lookup"><span data-stu-id="29d99-124">hello code below will create a sub-directory named **sampledir** under hello root directory.</span></span>

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a><span data-ttu-id="29d99-125">Räkna upp filer och kataloger i en filresurs på Azure</span><span class="sxs-lookup"><span data-stu-id="29d99-125">Enumerate files and directories in an Azure File share</span></span>
<span data-ttu-id="29d99-126">toolist hello filer och kataloger i en resurs, använda hello **lista\_kataloger\_och\_filer** metod.</span><span class="sxs-lookup"><span data-stu-id="29d99-126">toolist hello files and directories in a share, use hello **list\_directories\_and\_files** method.</span></span> <span data-ttu-id="29d99-127">Den här metoden returnerar en generator.</span><span class="sxs-lookup"><span data-stu-id="29d99-127">This method returns a generator.</span></span> <span data-ttu-id="29d99-128">hello följande kod visar hello **namn** av varje fil- och i en resurs toohello konsol.</span><span class="sxs-lookup"><span data-stu-id="29d99-128">hello following code outputs hello **name** of each file and directory in a share toohello console.</span></span>

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a><span data-ttu-id="29d99-129">Överför en fil</span><span class="sxs-lookup"><span data-stu-id="29d99-129">Upload a file</span></span> 
<span data-ttu-id="29d99-130">Azure File share innehåller vid hello mycket minst, en rotkatalog där filer kan finnas.</span><span class="sxs-lookup"><span data-stu-id="29d99-130">Azure File share contains at hello very least, a root directory where files can reside.</span></span> <span data-ttu-id="29d99-131">I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.</span><span class="sxs-lookup"><span data-stu-id="29d99-131">In this section, you'll learn how tooupload a file from local storage onto hello root directory of a share.</span></span>

<span data-ttu-id="29d99-132">toocreate en fil och överföringsdata använder hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` eller `create_file_from_text` metoder.</span><span class="sxs-lookup"><span data-stu-id="29d99-132">toocreate a file and upload data, use hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` or `create_file_from_text` methods.</span></span> <span data-ttu-id="29d99-133">De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.</span><span class="sxs-lookup"><span data-stu-id="29d99-133">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="29d99-134">`create_file_from_path`överföringar hello innehållet i en fil från hello angivna sökvägen och `create_file_from_stream` överföringar hello innehåll från en redan öppnad filström.</span><span class="sxs-lookup"><span data-stu-id="29d99-134">`create_file_from_path` uploads hello contents of a file from hello specified path, and `create_file_from_stream` uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="29d99-135">`create_file_from_bytes`Överför en matris med byte och `create_file_from_text` överföringar hello anges textvärdet med hello angetts kodning (standard tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="29d99-135">`create_file_from_bytes` uploads an array of bytes, and `create_file_from_text` uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="29d99-136">hello följande exempel överför hello innehållet i hello **sunset.png** filen till hello **minfil** fil.</span><span class="sxs-lookup"><span data-stu-id="29d99-136">hello following example uploads hello contents of hello **sunset.png** file into hello **myfile** file.</span></span>

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a><span data-ttu-id="29d99-137">Hämta en fil</span><span class="sxs-lookup"><span data-stu-id="29d99-137">Download a file</span></span>
<span data-ttu-id="29d99-138">toodownload data från en fil, använder `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, eller `get_file_to_text`.</span><span class="sxs-lookup"><span data-stu-id="29d99-138">toodownload data from a file, use `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, or `get_file_to_text`.</span></span> <span data-ttu-id="29d99-139">De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.</span><span class="sxs-lookup"><span data-stu-id="29d99-139">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="29d99-140">hello följande exempel visas hur du använder `get_file_to_path` toodownload hello innehållet i hello **minfil** filen och spara den toohello **out sunset.png** fil.</span><span class="sxs-lookup"><span data-stu-id="29d99-140">hello following example demonstrates using `get_file_to_path` toodownload hello contents of hello **myfile** file and store it toohello **out-sunset.png** file.</span></span>

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a><span data-ttu-id="29d99-141">Ta bort en fil</span><span class="sxs-lookup"><span data-stu-id="29d99-141">Delete a file</span></span>
<span data-ttu-id="29d99-142">Slutligen toodelete en fil, anropa `delete_file`.</span><span class="sxs-lookup"><span data-stu-id="29d99-142">Finally, toodelete a file, call `delete_file`.</span></span>

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a><span data-ttu-id="29d99-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29d99-143">Next steps</span></span>
<span data-ttu-id="29d99-144">Nu när du har lärt dig hur toomanipulate Azure File storage med Python, följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="29d99-144">Now that you've learned how toomanipulate Azure File storage with Python, follow these links toolearn more.</span></span>

* [<span data-ttu-id="29d99-145">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="29d99-145">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="29d99-146">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="29d99-146">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* [<span data-ttu-id="29d99-147">Microsoft Azure Storage SDK för Python</span><span class="sxs-lookup"><span data-stu-id="29d99-147">Microsoft Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)