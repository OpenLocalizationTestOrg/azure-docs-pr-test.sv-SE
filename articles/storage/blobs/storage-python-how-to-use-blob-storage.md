---
title: "Använda Azure Blob storage (objektlagring) från Python | Microsoft Docs"
description: Lagra ostrukturerade data i molnet med Azure Blob Storage (objektlagring).
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 0348e360-b24d-41fa-bb12-b8f18990d8bc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 2/24/2017
ms.author: marsma
ms.openlocfilehash: 1cab8407be6fc8932b68e50d0c301e8ea37ea3ac
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-blob-storage-from-python"></a><span data-ttu-id="3429b-103">Använda Azure Blob storage från Python</span><span class="sxs-lookup"><span data-stu-id="3429b-103">How to use Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="3429b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3429b-104">Overview</span></span>
<span data-ttu-id="3429b-105">Azure Blob Storage är en tjänst som lagrar ostrukturerade data i molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="3429b-105">Azure Blob storage is a service that stores unstructured data in the cloud as objects/blobs.</span></span> <span data-ttu-id="3429b-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="3429b-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="3429b-107">Blob Storage kallas även för objektlagring.</span><span class="sxs-lookup"><span data-stu-id="3429b-107">Blob storage is also referred to as object storage.</span></span>

<span data-ttu-id="3429b-108">Den här artikeln visar hur du utför vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="3429b-108">This article will show you how to perform common scenarios using Blob storage.</span></span> <span data-ttu-id="3429b-109">Exemplen är skrivna i Python och Använd den [Microsoft Azure Storage SDK för Python].</span><span class="sxs-lookup"><span data-stu-id="3429b-109">The samples are written in Python and use the [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="3429b-110">Scenarier som tas upp inkluderar överföringen, lista, hämtar och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="3429b-110">The scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="3429b-111">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="3429b-111">Create a container</span></span>
<span data-ttu-id="3429b-112">Baserat på vilken typ av blob som du vill använda kan du skapa en **BlockBlobService**, **AppendBlobService**, eller **PageBlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="3429b-112">Based on the type of blob you would like to use, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="3429b-113">I följande kod används en **BlockBlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="3429b-113">The following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="3429b-114">Lägg till följande längst upp i en Python-fil som du vill komma åt Azure Block Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="3429b-114">Add the following near the top of any Python file in which you wish to programmatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="3429b-115">Följande kod skapar en **BlockBlobService** objekt med lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="3429b-115">The following code creates a **BlockBlobService** object using the storage account name and account key.</span></span>  <span data-ttu-id="3429b-116">Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="3429b-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="3429b-117">I följande kodexempel, kan du använda en **BlockBlobService** objekt att skapa behållaren om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="3429b-117">In the following code example, you can use a **BlockBlobService** object to create the container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="3429b-118">Som standard är den nya behållaren privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) för att ladda ned blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="3429b-118">By default, the new container is private, so you must specify your storage access key (as you did earlier) to download blobs from this container.</span></span> <span data-ttu-id="3429b-119">Du kan skapa behållaren och skicka offentliga åtkomstnivå med följande kod om du vill göra blobbar i behållaren tillgängliga för alla.</span><span class="sxs-lookup"><span data-stu-id="3429b-119">If you want to make the blobs within the container available to everyone, you can create the container and pass the public access level using the following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="3429b-120">Du kan också ändra en behållare när du har skapat den med hjälp av följande kod.</span><span class="sxs-lookup"><span data-stu-id="3429b-120">Alternatively, you can modify a container after you have created it using the following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="3429b-121">När alla på Internet kan se blobbar i en offentlig behållare, men du kan bara ändra eller ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="3429b-121">After this change, anyone on the Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="3429b-122">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="3429b-122">Upload a blob into a container</span></span>
<span data-ttu-id="3429b-123">Du skapar en blockblobb och ladda upp data genom att använda den **skapa\_blob\_från\_sökväg**, **skapa\_blob\_från\_dataströmmen**, **skapa\_blob\_från\_byte** eller **skapa\_blob\_från\_text** -metoderna.</span><span class="sxs-lookup"><span data-stu-id="3429b-123">To create a block blob and upload data, use the **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="3429b-124">De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.</span><span class="sxs-lookup"><span data-stu-id="3429b-124">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="3429b-125">**Skapa\_blob\_från\_sökväg** Överför innehållet i en fil från den angivna sökvägen och **skapa\_blob\_från\_dataströmmen**Överför innehållet från en redan öppnad filström.</span><span class="sxs-lookup"><span data-stu-id="3429b-125">**create\_blob\_from\_path** uploads the contents of a file from the specified path, and **create\_blob\_from\_stream** uploads the contents from an already opened file/stream.</span></span> <span data-ttu-id="3429b-126">**Skapa\_blob\_från\_byte** överför en matris med byte och **skapa\_blob\_från\_text** överför det angivna textvärde med den angivna kodningen (standardvärdet är UTF-8).</span><span class="sxs-lookup"><span data-stu-id="3429b-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads the specified text value using the specified encoding (defaults to UTF-8).</span></span>

<span data-ttu-id="3429b-127">I följande exempel Överför innehållet i den **sunset.png** filen till den **minblobb** blob.</span><span class="sxs-lookup"><span data-stu-id="3429b-127">The following example uploads the contents of the **sunset.png** file into the **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-the-blobs-in-a-container"></a><span data-ttu-id="3429b-128">Visa en lista över blobbarna i en behållare</span><span class="sxs-lookup"><span data-stu-id="3429b-128">List the blobs in a container</span></span>
<span data-ttu-id="3429b-129">Om du vill visa blobbar i en behållare använder den **lista\_blobbar** metod.</span><span class="sxs-lookup"><span data-stu-id="3429b-129">To list the blobs in a container, use the **list\_blobs** method.</span></span> <span data-ttu-id="3429b-130">Den här metoden returnerar en generator.</span><span class="sxs-lookup"><span data-stu-id="3429b-130">This method returns a generator.</span></span> <span data-ttu-id="3429b-131">I följande kod utdata i **namn** för varje blobb i en behållare i konsolen.</span><span class="sxs-lookup"><span data-stu-id="3429b-131">The following code outputs the **name** of each blob in a container to the console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="3429b-132">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="3429b-132">Download blobs</span></span>
<span data-ttu-id="3429b-133">Hämta data från en blob, **hämta\_blob\_till\_sökväg**, **hämta\_blob\_till\_dataströmmen**, **hämta\_blob\_till\_byte**, eller **hämta\_blob\_till\_text**.</span><span class="sxs-lookup"><span data-stu-id="3429b-133">To download data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="3429b-134">De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.</span><span class="sxs-lookup"><span data-stu-id="3429b-134">They are high-level methods that perform the necessary chunking when the size of the data exceeds 64 MB.</span></span>

<span data-ttu-id="3429b-135">I följande exempel visas hur du använder **hämta\_blob\_till\_sökväg** att hämta innehållet i den **minblobb** blob och lagra den till den  **out-sunset.png** fil.</span><span class="sxs-lookup"><span data-stu-id="3429b-135">The following example demonstrates using **get\_blob\_to\_path** to download the contents of the **myblob** blob and store it to the **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="3429b-136">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="3429b-136">Delete a blob</span></span>
<span data-ttu-id="3429b-137">Slutligen, om du vill ta bort en blobb anropa **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="3429b-137">Finally, to delete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-to-an-append-blob"></a><span data-ttu-id="3429b-138">Skriva till en tilläggsblobb</span><span class="sxs-lookup"><span data-stu-id="3429b-138">Writing to an append blob</span></span>
<span data-ttu-id="3429b-139">En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning.</span><span class="sxs-lookup"><span data-stu-id="3429b-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="3429b-140">Precis som en blockblobb består en tilläggsblobb av block, men när du lägger till ett nytt block till en tilläggsblobb läggs det alltid till sist i blobben.</span><span class="sxs-lookup"><span data-stu-id="3429b-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block to an append blob, it is always appended to the end of the blob.</span></span> <span data-ttu-id="3429b-141">Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="3429b-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="3429b-142">En tilläggsblobbs block-ID:n exponeras inte som de gör för en blockblobb.</span><span class="sxs-lookup"><span data-stu-id="3429b-142">The block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="3429b-143">Blocken i en tilläggsblobb kan ha olika storlek, upp till högst 4 MB, och en tilläggsblobb kan innehålla högst 50 000 block.</span><span class="sxs-lookup"><span data-stu-id="3429b-143">Each block in an append blob can be a different size, up to a maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="3429b-144">Den största storleken på en tilläggsblobb är alltså strax över 195 GB (4 MB × 50 000 block).</span><span class="sxs-lookup"><span data-stu-id="3429b-144">The maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="3429b-145">I exemplet nedan skapar vi en ny tilläggsblobb och lägger till vissa data i den för att simulera en enkel loggningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="3429b-145">The example below creates a new append blob and appends some data to it, simulating a simple logging operation.</span></span>

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# The same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a><span data-ttu-id="3429b-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3429b-146">Next steps</span></span>
<span data-ttu-id="3429b-147">Nu när du har lärt dig grunderna om Blob Storage kan du följa dessa länkar om du vill veta mer.</span><span class="sxs-lookup"><span data-stu-id="3429b-147">Now that you've learned the basics of Blob storage, follow these links to learn more.</span></span>

* [<span data-ttu-id="3429b-148">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="3429b-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="3429b-149">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="3429b-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="3429b-150">[Azure Storage Teamblogg]</span><span class="sxs-lookup"><span data-stu-id="3429b-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="3429b-151">[Microsoft Azure Storage SDK för Python]</span><span class="sxs-lookup"><span data-stu-id="3429b-151">[Microsoft Azure Storage SDK for Python]</span></span>

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python
