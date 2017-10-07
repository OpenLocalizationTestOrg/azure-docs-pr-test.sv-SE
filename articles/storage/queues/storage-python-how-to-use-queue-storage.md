---
title: "aaaHow toouse Queue storage från Python | Microsoft Docs"
description: "Lär dig hur toouse hello Azure-kötjänsten från Python toocreate och ta bort köer och infoga, hämta och ta bort meddelanden."
services: storage
documentationcenter: python
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: cc0d2da2-379a-4b58-a234-8852b4e3d99d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: f4f902a2c314401e5c1768fbc80566c8ba25c058
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-python"></a>Hur toouse Queue storage från Python
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst. hello exemplen är skrivna i Python och använder hello [Microsoft Azure Storage SDK för Python]. hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**. Mer information om köer finns i avsnittet toohello [nästa steg].

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a>Så här: Skapa en kö
Hej **QueueService** objekt kan du arbeta med köer. hello följande kod skapar en **QueueService** objekt. Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically åtkomst till Azure Storage:

```python
from azure.storage.queue import QueueService
```

hello följande kod skapar en **QueueService** objekt med hello lagringskontots namn och åtkomstnyckel. Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Så här: Infoga ett meddelande i en kö
tooinsert ett meddelande i en kö, Använd hello **placera\_meddelandet** metod för att skapa ett nytt meddelande och Lägg till den toohello kön.

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a>Så här: Granska hello nästa meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **titt\_meddelanden** metod. Som standard **titt\_meddelanden** peeks på ett enda meddelande.

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a>Så här: Status Created meddelanden
Koden tar bort ett meddelande från en kö i två steg. När du anropar **hämta\_meddelanden**, du får hello nästa meddelande i en kö som standard. Ett meddelande som returneras från **hämta\_meddelanden** blir osynligt tooany annan kod läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **ta bort\_meddelandet**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att när din kod inte tooprocess ett meddelande på grund av maskinvara eller programvara, kan en annan instans av koden hämta samma meddelande och försök igen. Koden anropar **ta bort\_meddelandet** direkt efter hello-meddelande har bearbetats.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.
Först får du en grupp med meddelanden (upp too32). Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande. hello följande kodexempel används den **hämta\_meddelanden** metoden tooget 16 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en for-loop. Den anger också hello tidsgränsen för osynlighet till fem minuter för varje meddelande.

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Så här: Ändra hello innehållet i meddelandet i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö. Om meddelandet representerar en arbetsuppgift kan använda du den här funktionen tooupdate statusen för arbetsuppgiften som hello. hello koden nedan använder hello **uppdatera\_meddelandet** metoden tooupdate ett meddelande. Tidsgränsen för visning av hello anges too0, vilket innebär att meddelandet visas omedelbart och hello innehållet uppdateras.

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a>Så här: Hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Den **hämta\_kön\_metadata** metoden ber hello kön service tooreturn metadata om hello kön och hello **approximate_message_count**. hello resultatet är bara ungefärliga eftersom meddelanden kan läggas till eller tas bort efter kötjänsten svarar tooyour begäran.

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a>Så här: Ta bort en kö
toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort\_kön** metod.

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i Queue storage följa dessa länkar toolearn mer.

* [Python Developer Center](/develop/python/)
* [REST-API för Azure Storage Services](http://msdn.microsoft.com/library/azure/dd179355)
* [Azure Storage Teamblogg]
* [Microsoft Azure Storage SDK för Python]

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python