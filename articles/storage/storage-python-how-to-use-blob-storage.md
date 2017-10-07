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
ms.openlocfilehash: 6efc61aa89e6d2544b7a18c80ce3546640f90462
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-from-python"></a>Hur toouse Azure Blob storage från Python
[!INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Översikt
Azure Blob storage är en tjänst som lagrar Ostrukturerade data i hello molnet som objekt/blobbar. Blob Storage kan lagra alla slags textdata eller binära data, till exempel ett dokument, en mediefil eller ett installationsprogram. BLOB storage är också hänvisade tooas objektlagring.

Den här artikeln visar hur tooperform vanliga scenarier med Blob storage. hello exemplen är skrivna i Python och använder hello [Microsoft Azure Storage SDK för Python]. hello-scenarier som tas upp inkluderar överföringen, lista, hämtar och tar bort blobbar.

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-container"></a>Skapa en behållare
Baserat på hello blob som toouse, skapa en **BlockBlobService**, **AppendBlobService**, eller **PageBlobService** objekt. hello följande kod används en **BlockBlobService** objekt. Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically åtkomst Azure Block Blob Storage.

```python
from azure.storage.blob import BlockBlobService
```

hello följande kod skapar en **BlockBlobService** objekt med hello lagringskontots namn och åtkomstnyckel.  Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.

```python
block_blob_service = BlockBlobService(account_name='myaccount', account_key='mykey')
```

[!INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

I följande kodexempel hello, kan du använda en **BlockBlobService** toocreate hello behållare om den inte finns.

```python
block_blob_service.create_container('mycontainer')
```

Som standard hello ny behållare är privat, så du måste ange din lagringsåtkomstnyckel (som du gjorde tidigare) toodownload blobbar från den här behållaren. Om du vill toomake hello blobbar i hello behållaren tillgängliga tooeveryone kan du skapa hello behållare och skicka hello offentliga åtkomstnivå med hello följande kod.

```python
from azure.storage.blob import PublicAccess
block_blob_service.create_container('mycontainer', public_access=PublicAccess.Container)
```

Du kan också ändra en behållare när du har skapat den med hjälp av hello följande kod.

```python
block_blob_service.set_container_acl('mycontainer', public_access=PublicAccess.Container)
```

När alla på hello Internet kan se blobbar i en offentlig behållare, men du kan bara ändra eller ta bort dem.

## <a name="upload-a-blob-into-a-container"></a>Ladda upp en blobb till en behållare
toocreate en blockblobb och överföringsdata använder hello **skapa\_blob\_från\_sökväg**, **skapa\_blob\_från\_dataströmmen**, **skapa\_blob\_från\_byte** eller **skapa\_blob\_från\_text** -metoderna. De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.

**Skapa\_blob\_från\_sökväg** överföringar hello innehållet i en fil från hello angivna sökvägen och **skapa\_blob\_från\_dataströmmen** överföringar hello innehåll från en redan öppnad filström. **Skapa\_blob\_från\_byte** överför en matris med byte och **skapa\_blob\_från\_text** överför hello angetts textvärde med hello angetts kodning (standard tooUTF-8).

hello följande exempel överför hello innehållet i hello **sunset.png** filen till hello **minblobb** blob.

```python
from azure.storage.blob import ContentSettings
block_blob_service.create_blob_from_path(
    'mycontainer',
    'myblockblob',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png')
            )
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello blobbar i en behållare
toolist hello blobbar i en behållare använder hello **lista\_blobbar** metod. Den här metoden returnerar en generator. hello följande kod visar hello **namn** för varje blobb i en behållare toohello konsol.

```python
generator = block_blob_service.list_blobs('mycontainer')
for blob in generator:
    print(blob.name)
```

## <a name="download-blobs"></a>Ladda ned blobbar
toodownload data från en blob som använder **hämta\_blob\_till\_sökväg**, **hämta\_blob\_till\_dataströmmen**, **hämta\_blob\_till\_byte**, eller **hämta\_blob\_till\_text**. De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.

hello följande exempel visas hur du använder **hämta\_blob\_till\_sökväg** toodownload hello innehållet i hello **minblobb** blob och lagra den toohello **out sunset.png** fil.

```python
block_blob_service.get_blob_to_path('mycontainer', 'myblockblob', 'out-sunset.png')
```

## <a name="delete-a-blob"></a>Ta bort en blob
Slutligen toodelete blob anropa **delete_blob**.

```python
block_blob_service.delete_blob('mycontainer', 'myblockblob')
```

## <a name="writing-tooan-append-blob"></a>Skrivning tooan lägga till blob
En tilläggsblobb är optimerad för tilläggsåtgärder, t.ex loggning. Som en blockblobb består en tilläggsblobb av block, men när du lägger till en ny tilläggsblobb för block tooan det är alltid tillagda toohello slutet av hello-blob. Du kan inte uppdatera eller ta bort ett befintligt block i en tilläggsblobb. hello block ID för en tilläggsblobb exponeras inte eftersom de är för en blockblob.

Varje block i en tilläggsblobb kan ha olika storlek, upp tooa högst 4 MB och en tilläggsblobb kan innehålla högst 50 000 block. hello maximal storlek på en tilläggsblobb är därför lite över 195 GB (4 MB × 50 000 block).

hello exemplet nedan skapar en ny tilläggsblobb och lägger till vissa data tooit, simulera en enkel loggningsåtgärd.

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

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna hello i Blob storage följa dessa länkar toolearn mer.

* [Python Developer Center](https://azure.microsoft.com/develop/python/)
* [REST-API för Azure Storage Services](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Storage Teamblogg]
* [Microsoft Azure Storage SDK för Python]

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python
