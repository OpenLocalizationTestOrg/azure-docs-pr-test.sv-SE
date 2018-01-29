---
title: "Använda Queue storage från Python | Microsoft Docs"
description: "Lär dig hur du använder Azure-kötjänsten från Python för att skapa och ta bort köer, infoga, hämta och ta bort meddelanden."
services: storage
documentationcenter: python
author: tamram
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: c7976c01436b1c30880bfd4c57cb97f72a4f48b0
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/29/2017
---
# <a name="how-to-use-queue-storage-from-python"></a>Använda Queue Storage från Python
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur du utför vanliga scenarier med hjälp av Azure Queue storage-tjänst. Exemplen är skrivna i Python och Använd den [Microsoft Azure Storage SDK för Python]. Scenarier som tas upp inkluderar **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt **skapa och ta bort köer**. Mer information om köer finns i avsnittet [nästa steg].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="download-and-install-azure-storage-sdk-for-python"></a>Hämta och installera Azure Storage SDK för Python

Azure Storage-SDK för Python kräver Python 2.7, 3.3, 3.4, 3.5 eller 3,6 och ingår i 4 olika paket: `azure-storage-blob`, `azure-storage-file`, `azure-storage-table` och `azure-storage-queue`. I den här kursen ska vi använda `azure-storage-queue` paketet.
 
### <a name="install-via-pypi"></a>Installera via PyPi

Om du vill installera via Python Package Index (PyPI), skriver du:

```bash
pip install azure-storage-queue
```


> [!NOTE]
> Om du uppgraderar från Azure Storage SDK för Python version 0.36 eller tidigare, måste du först avinstallera med `pip uninstall azure-storage` som vi inte längre publicerar Storage SDK: N för Python i ett paket.
> 
> 

Alternativa installationsmetoder finns i [Azure Storage SDK för Python på Github](https://github.com/Azure/azure-storage-python/).

## <a name="how-to-create-a-queue"></a>Så här: Skapa en kö
Den **QueueService** objekt kan du arbeta med köer. Följande kod skapar en **QueueService** objekt. Lägg till följande längst upp i en Python-fil som du vill komma åt Azure Storage:

```python
from azure.storage.queue import QueueService
```

Följande kod skapar en **QueueService** objekt med lagringskontots namn och åtkomstnyckel. Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Så här: Infoga ett meddelande i en kö
Om du vill infoga ett meddelande i en kö, använda den **placera\_meddelandet** metoden för att skapa ett nytt meddelande och lägga till den i kön.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-the-next-message"></a>Så här: Granska nästa meddelande
Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa den **titt\_meddelanden** metod. Som standard **titt\_meddelanden** peeks på ett enda meddelande.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Så här: Status Created meddelanden
Koden tar bort ett meddelande från en kö i två steg. När du anropar **hämta\_meddelanden**, du får nästa meddelande i en kö som standard. Ett meddelande som returneras från **hämta\_meddelanden** blir osynligt för andra koder som läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder. Om du vill slutföra borttagningen av meddelandet från kön, måste du också anropa **ta bort\_meddelandet**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att när din kod inte kan bearbeta ett meddelande på grund av maskinvara eller programvara, kan en annan instans av koden hämta samma meddelande och försök igen. Koden anropar **ta bort\_meddelandet** direkt efter att meddelandet har bearbetats.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.
För det första kan du hämta en grupp med meddelanden (upp till 32). För det andra kan du ange en längre eller kortare tidsgräns för osynlighet för att ge koden mer eller mindre tid att bearbeta klart varje meddelande. Följande kodexempel används den **hämta\_meddelanden** metod för att hämta 16 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en for-loop. Koden ställer också in tidsgränsen för osynlighet till fem minuter för varje meddelande.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-the-contents-of-a-queued-message"></a>Så här: Ändra innehållet i ett meddelande i kön
Du kan ändra innehållet i ett meddelande direkt i kön. Om meddelandet representerar en arbetsuppgift kan du använda den här funktionen för att uppdatera arbetsuppgiftens status. Koden nedan används den **uppdatera\_meddelandet** metod för att uppdatera ett meddelande. Tidsgränsen för visning har angetts till 0, vilket innebär att meddelandet visas omedelbart och innehållet uppdateras.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-the-queue-length"></a>Så här: Hämta kölängden
Du kan hämta en uppskattning av antalet meddelanden i en kö. Den **hämta\_kön\_metadata** metoden ber kötjänsten att returnera metadata om kön, och **approximate_message_count**. Resultatet är ungefärliga eftersom meddelanden kan läggas till eller tas bort efter kötjänsten svarar på begäran.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Så här: Ta bort en kö
Om du vill ta bort en kö och alla meddelanden som finns i den anropar den **ta bort\_kön** metod.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig grunderna i Queue storage kan du följa dessa länkar om du vill veta mer.

* [Python Developer Center](/develop/python/)
* [REST-API för Azure Storage Services](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Storage Teamblogg]
* [Microsoft Azure Storage SDK för Python]

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python