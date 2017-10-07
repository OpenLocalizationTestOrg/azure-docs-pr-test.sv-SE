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
ms.openlocfilehash: 2adc5aac2765b98a8022ab1f706c1fcdbca1b43c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="develop-for-azure-file-storage-with-python"></a>Utveckla för Azure File storage med Python
[!INCLUDE [storage-selector-file-include](../../../includes/storage-selector-file-include.md)]

[!INCLUDE [storage-try-azure-tools-files](../../../includes/storage-try-azure-tools-files.md)]

## <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar hello grunderna i Python toodevelop program eller tjänster som använder Azure File storage toostore fildata. I den här kursen ska vi skapa ett enkelt konsolprogram och visa hur tooperform grundläggande åtgärder med Python och Azure File storage:

* Skapa Azure-filresurser
* Skapa kataloger
* Räkna upp filer och kataloger i en filresurs på Azure
* Ladda upp, hämta och ta bort en fil

> [!Note]  
> Eftersom Azure File storage kan nås över SMB, är det möjligt toowrite enkla program som har åtkomst till hello Azure filresurs med hjälp av hello standard Python-i/o-klasser och -funktioner. Den här artikeln beskriver hur toowrite program som använder hello Azure Storage Python SDK, som använder hello [Azure File storage REST API](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/file-service-rest-api) tootalk tooAzure File storage.

### <a name="set-up-your-application-toouse-azure-file-storage"></a>Konfigurera ditt program toouse Azure File storage
Lägg till följande hello hello övre delen av alla Python-källfilen som du vill tooprogrammatically åtkomst till Azure Storage.

```python
from azure.storage.file import FileService
```

### <a name="set-up-a-connection-tooazure-file-storage"></a>Konfigurera en anslutning tooAzure File storage 
Hej `FileService` objektet kan du arbeta med resurser, kataloger och filer. hello följande kod skapar en `FileService` objektet med hello lagringskontots namn och åtkomstnyckel. Ersätt `<myaccount>` och `<mykey>` med kontonamnet och nyckeln.

```python
file_service = FileService(account_name='myaccount', account_key='mykey')
```

### <a name="create-an-azure-file-share"></a>Skapa en filresurs på Azure
I följande kodexempel hello, kan du använda en `FileService` objektet toocreate hello resursen om den inte finns.

```python
file_service.create_share('myshare')
```

### <a name="create-a-directory"></a>Skapa en katalog
Du kan även sortera lagring genom att lägga till filer i underkataloger i stället för att alla i hello rotkatalog. Azure File storage kan toocreate som många kataloger som ditt konto kommer tillåta. hello koden nedan skapar en underkatalog med namnet **sampledir** under hello rotkatalog.

```python
file_service.create_directory('myshare', 'sampledir')
```

### <a name="enumerate-files-and-directories-in-an-azure-file-share"></a>Räkna upp filer och kataloger i en filresurs på Azure
toolist hello filer och kataloger i en resurs, använda hello **lista\_kataloger\_och\_filer** metod. Den här metoden returnerar en generator. hello följande kod visar hello **namn** av varje fil- och i en resurs toohello konsol.

```python
generator = file_service.list_directories_and_files('myshare')
for file_or_dir in generator:
    print(file_or_dir.name)
```

### <a name="upload-a-file"></a>Överför en fil 
Azure File share innehåller vid hello mycket minst, en rotkatalog där filer kan finnas. I det här avsnittet lär du dig hur tooupload en fil från lokal lagring på hello rotkatalogen för en resurs.

toocreate en fil och överföringsdata använder hello `create_file_from_path`, `create_file_from_stream`, `create_file_from_bytes` eller `create_file_from_text` metoder. De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.

`create_file_from_path`överföringar hello innehållet i en fil från hello angivna sökvägen och `create_file_from_stream` överföringar hello innehåll från en redan öppnad filström. `create_file_from_bytes`Överför en matris med byte och `create_file_from_text` överföringar hello anges textvärdet med hello angetts kodning (standard tooUTF-8).

hello följande exempel överför hello innehållet i hello **sunset.png** filen till hello **minfil** fil.

```python
from azure.storage.file import ContentSettings
file_service.create_file_from_path(
    'myshare',
    None, # We want toocreate this blob in hello root directory, so we specify None for hello directory_name
    'myfile',
    'sunset.png',
    content_settings=ContentSettings(content_type='image/png'))
```

### <a name="download-a-file"></a>Hämta en fil
toodownload data från en fil, använder `get_file_to_path`, `get_file_to_stream`, `get_file_to_bytes`, eller `get_file_to_text`. De är övergripande metoder som utför hello nödvändiga högoptimerat när hello hello överskrider data 64 MB.

hello följande exempel visas hur du använder `get_file_to_path` toodownload hello innehållet i hello **minfil** filen och spara den toohello **out sunset.png** fil.

```python
file_service.get_file_to_path('myshare', None, 'myfile', 'out-sunset.png')
```

### <a name="delete-a-file"></a>Ta bort en fil
Slutligen toodelete en fil, anropa `delete_file`.

```python
file_service.delete_file('myshare', None, 'myfile')
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hur toomanipulate Azure File storage med Python, följa dessa länkar toolearn mer.

* [Python Developer Center](/develop/python/)
* [REST-API för Azure Storage Services](http://msdn.microsoft.com/library/azure/dd179355)
* [Microsoft Azure Storage SDK för Python](https://github.com/Azure/azure-storage-python)