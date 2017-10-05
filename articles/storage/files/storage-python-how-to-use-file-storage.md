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
# <a name="develop-for-azure-file-storage-with-python"></a>Utveckla för Azure File storage med Python
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar grunderna i Python för att utveckla program eller tjänster som använder Azure File storage för att lagra fildata. I den här kursen ska vi skapa ett enkelt konsolprogram och visar hur du utför grundläggande åtgärder med Python och Azure File storage:

* Skapa Azure-filresurser
* Skapa kataloger
* Räkna upp filer och kataloger i en filresurs på Azure
* Ladda upp, hämta och ta bort en fil

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt att skriva enkla program som har åtkomst till Azure filresursen med standard Python-i/o-klasser och funktioner. Den här artikeln beskriver hur du skriver program som använder Azure Storage Python SDK, som använder den [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tala med Azure File storage.

### <a name="set-up-your-application-to-use-azure-file-storage"></a>Konfigurera din app att använda Azure File storage
Lägg till följande längst upp i alla Python-källfilen som du vill komma åt Azure Storage.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-to-azure-file-storage"></a>Konfigurera en anslutning till Azure File storage 
Den `FileService` objektet kan du arbeta med resurser, kataloger och filer. Följande kod skapar en `FileService` objekt med lagringskontots namn och åtkomstnyckel. Ersätt `<myaccount>` och `<mykey>` med kontonamnet och nyckeln.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Skapa en filresurs på Azure
I följande kodexempel, kan du använda en `FileService` objekt att skapa resursen om den inte finns.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Skapa en katalog
Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i rotkatalogen. Azure File storage kan du skapa så många kataloger som ditt konto tillåter. Koden nedan för att skapa en underkatalog med namnet **sampledir** under rotkatalogen.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Räkna upp filer och kataloger i en filresurs på Azure
Om du vill visa en lista över filer och kataloger på en resurs, Använd den **lista\_kataloger\_och\_filer** metod. Den här metoden returnerar en generator. I följande kod utdata i **namn** av varje fil- och på en resurs i konsolen.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Överför en fil 
Azure File resursen innehåller minst, en rotkatalog där filer kan finnas. I det här avsnittet lär du dig hur du överför en fil från lokal lagring till rotkatalogen för en resurs.

Om du vill skapa en fil och överför data använder den `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` eller `create_file_from_text` metoder. De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.

`create_file_from_path`Överför innehållet i en fil från den angivna sökvägen och `create_file_from_stream` Överför innehållet från en redan öppnad filström. `create_file_from_bytes`Överför en matris med byte och `create_file_from_text` överför det angivna textvärdet med den angivna kodningen (standardvärdet är UTF-8).

I följande exempel Överför innehållet i den **sunset.png** filen till den **minfil** fil.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want to create this blob in the root directory, so we specify None for the directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Hämta en fil
Hämta data från en fil, `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, eller `get_file_to_text`. De är övergripande metoder som utför nödvändiga högoptimerat när storleken på data överskrider 64 MB.

I följande exempel visas hur du använder `get_file_to_path` att hämta innehållet i den **minfil** filen och spara den till den **out sunset.png** fil.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Ta bort en fil
Slutligen, om du vill ta bort en fil, anropa `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hur du hanterar Azure File storage med Python, kan du följa dessa länkar om du vill veta mer.

* [Python Developer Center](/develop/python/)
* [REST-API för Azure Storage Services](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python)