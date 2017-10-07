---
title: "aaaHow toouse Azure Blob storage (objektlagring) från Python | Microsoft Docs"
description: Lagra Ostrukturerade data i hello moln med Azure Blob storage (objektlagring).
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
ms.openlocfilehash: 8f9ca93e52b030384e28a739d2f1c6b610be094a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a><span data-ttu-id="bc5e8-103">Hur toouse Azure Blob storage från Python</span><span class="sxs-lookup"><span data-stu-id="bc5e8-103">How toouse Azure Blob storage from Python</span></span>
[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="bc5e8-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="bc5e8-104">Overview</span></span>
<span data-ttu-id="bc5e8-105">Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-105">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="bc5e8-106">Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="bc5e8-107">BLOB storage är också hänvisade tooas objektlagring.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="bc5e8-108">Den här artikeln visar hur tooperform vanliga scenarier med Blob storage.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-108">This article will show you how tooperform common scenarios using Blob storage.</span></span> <span data-ttu-id="bc5e8-109">hello exemplen är skrivna i Python och använder hello [Microsoft Azure Storage SDK för Python].</span><span class="sxs-lookup"><span data-stu-id="bc5e8-109">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="bc5e8-110">hello-scenarier som tas upp inkluderar överföringen, lista, hämtar och tar bort blobbar.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-110">hello scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a><span data-ttu-id="bc5e8-111">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="bc5e8-111">Create a container</span></span>
<span data-ttu-id="bc5e8-112">Baserat på hello blob som toouse, skapa en **BlockBlobService**, **AppendBlobService**, eller **PageBlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-112">Based on hello type of blob you would like toouse, create a **BlockBlobService**, **AppendBlobService**, or **PageBlobService** object.</span></span> <span data-ttu-id="bc5e8-113">hello följande kod används en **BlockBlobService** objekt.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-113">hello following code uses a **BlockBlobService** object.</span></span> <span data-ttu-id="bc5e8-114">Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically åtkomst Azure Block Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-114">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Block Blob Storage.</span></span>

```python
from azure.storage.blob import BlockBlobService
```

<span data-ttu-id="bc5e8-115">hello följande kod skapar en **BlockBlobService** objekt med hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-115">hello following code creates a **BlockBlobService** object using hello storage account name and account key.</span></span>  <span data-ttu-id="bc5e8-116">Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-116">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="bc5e8-117">I följande kodexempel hello, kan du använda en **BlockBlobService** toocreate hello behållare om den inte finns.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-117">In hello following code example, you can use a **BlockBlobService** object toocreate hello container if it doesn't exist.</span></span>

```python
block_blob_service.create_container('mycontainer')
```

<span data-ttu-id="bc5e8-118">Som standard hello ny behållare är privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) toodownload blobbar från den här behållaren.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-118">By default, hello new container is private, so you must specify your storage access key (as you did earlier) toodownload blobs from this container.</span></span> <span data-ttu-id="bc5e8-119">Om du vill toomake hello blobbar i hello behållaren tillgängliga tooeveryone kan du skapa hello behållare och skicka hello offentliga åtkomstnivå med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-119">If you want toomake hello blobs within hello container available tooeveryone, you can create hello container and pass hello public access level using hello following code.</span></span>

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="bc5e8-120">Du kan också ändra en behållare när du har skapat den med hjälp av hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-120">Alternatively, you can modify a container after you have created it using hello following code.</span></span>

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

<span data-ttu-id="bc5e8-121">När alla på hello Internet kan se blobbar i en offentlig behållare, men du kan bara ändra eller ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-121">After this change, anyone on hello Internet can see blobs in a public container, but only you can modify or delete them.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="bc5e8-122">Ladda upp en blobb till en behållare</span><span class="sxs-lookup"><span data-stu-id="bc5e8-122">Upload a blob into a container</span></span>
<span data-ttu-id="bc5e8-123">toocreate en blockblobb och överföringsdata använder hello **skapa\_blob\_från\_sökväg**, **skapa\_blob\_från\_dataströmmen**, **skapa\_blob\_från\_byte** eller **skapa\_blob\_från\_text** -metoderna.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-123">toocreate a block blob and upload data, use hello **create\_blob\_from\_path**, **create\_blob\_from\_stream**, **create\_blob\_from\_bytes** or **create\_blob\_from\_text** methods.</span></span> <span data-ttu-id="bc5e8-124">De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-124">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="bc5e8-125">**Skapa\_blob\_från\_sökväg** överföringar hello innehållet i en fil från hello angivna sökvägen och **skapa\_blob\_från\_dataströmmen** överföringar hello innehåll från en redan öppnad filström.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-125">**create\_blob\_from\_path** uploads hello contents of a file from hello specified path, and **create\_blob\_from\_stream** uploads hello contents from an already opened file/stream.</span></span> <span data-ttu-id="bc5e8-126">**Skapa\_blob\_från\_byte** överför en matris med byte och **skapa\_blob\_från\_text** överför hello angetts textvärde med hello angetts kodning (standard tooUTF-8).</span><span class="sxs-lookup"><span data-stu-id="bc5e8-126">**create\_blob\_from\_bytes** uploads an array of bytes, and **create\_blob\_from\_text** uploads hello specified text value using hello specified encoding (defaults tooUTF-8).</span></span>

<span data-ttu-id="bc5e8-127">hello följande exempel överför hello innehållet i hello **sunset.png** filen till hello **minblobb** blob.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-127">hello following example uploads hello contents of hello **sunset.png** file into hello **myblob** blob.</span></span>

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="bc5e8-128">Lista hello blobbar i en behållare</span><span class="sxs-lookup"><span data-stu-id="bc5e8-128">List hello blobs in a container</span></span>
<span data-ttu-id="bc5e8-129">toolist hello blobbar i en behållare använder hello **lista\_blobbar** metod.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-129">toolist hello blobs in a container, use hello **list\_blobs** method.</span></span> <span data-ttu-id="bc5e8-130">Den här metoden returnerar en generator.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-130">This method returns a generator.</span></span> <span data-ttu-id="bc5e8-131">hello följande kod visar hello **namn** för varje blobb i en behållare toohello konsol.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-131">hello following code outputs hello **name** of each blob in a container toohello console.</span></span>

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a><span data-ttu-id="bc5e8-132">Ladda ned blobbar</span><span class="sxs-lookup"><span data-stu-id="bc5e8-132">Download blobs</span></span>
<span data-ttu-id="bc5e8-133">toodownload data från en blob som använder **hämta\_blob\_till\_sökväg**, **hämta\_blob\_till\_dataströmmen**, **hämta\_blob\_till\_byte**, eller **hämta\_blob\_till\_text**.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-133">toodownload data from a blob, use **get\_blob\_to\_path**, **get\_blob\_to\_stream**, **get\_blob\_to\_bytes**, or **get\_blob\_to\_text**.</span></span> <span data-ttu-id="bc5e8-134">De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-134">They are high-level methods that perform hello necessary chunking when hello size of hello data exceeds 64 MB.</span></span>

<span data-ttu-id="bc5e8-135">hello följande exempel visas hur du använder **hämta\_blob\_till\_sökväg** toodownload hello innehållet i hello **minblobb** blob och lagra den toohello **out sunset.png** fil.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-135">hello following example demonstrates using **get\_blob\_to\_path** toodownload hello contents of hello **myblob** blob and store it toohello **out-sunset.png** file.</span></span>

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a><span data-ttu-id="bc5e8-136">Ta bort en blob</span><span class="sxs-lookup"><span data-stu-id="bc5e8-136">Delete a blob</span></span>
<span data-ttu-id="bc5e8-137">Slutligen toodelete blob anropa **delete_blob**.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-137">Finally, toodelete a blob, call **delete_blob**.</span></span>

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="bc5e8-138">Skrivning tooan lägga till blob</span><span class="sxs-lookup"><span data-stu-id="bc5e8-138">Writing tooan append blob</span></span>
<span data-ttu-id="bc5e8-139">En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-139">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="bc5e8-140">Som en blockblobb består en tilläggsblobb av block, men när du lägger till en ny tilläggsblobb för block tooan det är alltid tillagda toohello slutet av hello-blob.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-140">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="bc5e8-141">Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-141">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="bc5e8-142">hello block ID för en tilläggsblobb exponeras inte eftersom de är för en blockblob.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-142">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="bc5e8-143">Varje block i en tilläggsblobb kan ha olika storlek, upp tooa högst 4 MB och en tilläggsblobb kan innehålla högst 50 000 block.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-143">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="bc5e8-144">hello maximal storlek på en tilläggsblobb är därför lite över 195 GB (4 MB × 50 000 block).</span><span class="sxs-lookup"><span data-stu-id="bc5e8-144">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="bc5e8-145">hello exemplet nedan skapar en ny tilläggsblobb och lägger till vissa data tooit, simulera en enkel loggningsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-145">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

```python
from azure.storage.blob import AppendBlobService
append_blob_service = AppendBlobService(account_name='myaccount', account_key='mykey')

# hello same containers can hold all types of blobs
append_blob_service.create_container('mycontainer')

# Append blobs must be created before they are appended to
append_blob_service.create_blob('mycontainer', 'myappendblob')
append_blob_service.append_blob_from_text('mycontainer', 'myappendblob', u'Hello, world!')

append_blob = append_blob_service.get_blob_to_text('mycontainer', 'myappendblob')
```

## <a name="next-steps"></a><span data-ttu-id="bc5e8-146">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bc5e8-146">Next steps</span></span>
<span data-ttu-id="bc5e8-147">Nu när du har lärt dig grunderna hello i Blob storage följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="bc5e8-147">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="bc5e8-148">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="bc5e8-148">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* [<span data-ttu-id="bc5e8-149">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="bc5e8-149">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="bc5e8-150">[Azure Storage Teamblogg]</span><span class="sxs-lookup"><span data-stu-id="bc5e8-150">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="bc5e8-151">[Microsoft Azure Storage SDK för Python]</span><span class="sxs-lookup"><span data-stu-id="bc5e8-151">[Microsoft Azure Storage SDK for Python]</span></span>

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python
