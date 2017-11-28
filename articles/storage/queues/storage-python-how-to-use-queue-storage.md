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
# <a name="how-toouse-queue-storage-from-python"></a><span data-ttu-id="c0fb9-103">Hur toouse Queue storage från Python</span><span class="sxs-lookup"><span data-stu-id="c0fb9-103">How toouse Queue storage from Python</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="c0fb9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c0fb9-104">Overview</span></span>
<span data-ttu-id="c0fb9-105">Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-105">This guide shows you how tooperform common scenarios using hello Azure Queue storage service.</span></span> <span data-ttu-id="c0fb9-106">hello exemplen är skrivna i Python och använder hello [Microsoft Azure Storage SDK för Python].</span><span class="sxs-lookup"><span data-stu-id="c0fb9-106">hello samples are written in Python and use hello [Microsoft Azure Storage SDK for Python].</span></span> <span data-ttu-id="c0fb9-107">hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-107">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span> <span data-ttu-id="c0fb9-108">Mer information om köer finns i avsnittet toohello [nästa steg].</span><span class="sxs-lookup"><span data-stu-id="c0fb9-108">For more information on queues, refer toohello [Next Steps] section.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="how-to-create-a-queue"></a><span data-ttu-id="c0fb9-109">Så här: Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="c0fb9-109">How To: Create a Queue</span></span>
<span data-ttu-id="c0fb9-110">Hej **QueueService** objekt kan du arbeta med köer.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-110">hello **QueueService** object lets you work with queues.</span></span> <span data-ttu-id="c0fb9-111">hello följande kod skapar en **QueueService** objekt.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-111">hello following code creates a **QueueService** object.</span></span> <span data-ttu-id="c0fb9-112">Lägg till följande hello hello övre delen av Python-fil som du vill tooprogrammatically åtkomst till Azure Storage:</span><span class="sxs-lookup"><span data-stu-id="c0fb9-112">Add hello following near hello top of any Python file in which you wish tooprogrammatically access Azure Storage:</span></span>

```python
from azure.storage.queue import QueueService
```

<span data-ttu-id="c0fb9-113">hello följande kod skapar en **QueueService** objekt med hello lagringskontots namn och åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-113">hello following code creates a **QueueService** object using hello storage account name and account key.</span></span> <span data-ttu-id="c0fb9-114">Ersätt ”MITTKONTO' och 'MinNyckel' med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-114">Replace 'myaccount' and 'mykey' with your account name and key.</span></span>

```python
queue_service = QueueService(account_name='myaccount', account_key='mykey')

queue_service.create_queue('taskqueue')
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="c0fb9-115">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="c0fb9-115">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="c0fb9-116">tooinsert ett meddelande i en kö, Använd hello **placera\_meddelandet** metod för att skapa ett nytt meddelande och Lägg till den toohello kön.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-116">tooinsert a message into a queue, use hello **put\_message** method to create a new message and add it toohello queue.</span></span>

```python
queue_service.put_message('taskqueue', u'Hello World')
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="c0fb9-117">Så här: Granska hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="c0fb9-117">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="c0fb9-118">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **titt\_meddelanden** metod.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-118">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages** method.</span></span> <span data-ttu-id="c0fb9-119">Som standard **titt\_meddelanden** peeks på ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-119">By default, **peek\_messages** peeks at a single message.</span></span>

```python
messages = queue_service.peek_messages('taskqueue')
for message in messages:
    print(message.content)
```

## <a name="how-to-dequeue-messages"></a><span data-ttu-id="c0fb9-120">Så här: Status Created meddelanden</span><span class="sxs-lookup"><span data-stu-id="c0fb9-120">How To: Dequeue Messages</span></span>
<span data-ttu-id="c0fb9-121">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-121">Your code removes a message from a queue in two steps.</span></span> <span data-ttu-id="c0fb9-122">När du anropar **hämta\_meddelanden**, du får hello nästa meddelande i en kö som standard.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-122">When you call **get\_messages**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="c0fb9-123">Ett meddelande som returneras från **hämta\_meddelanden** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-123">A message returned from **get\_messages** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="c0fb9-124">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-124">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="c0fb9-125">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **ta bort\_meddelandet**.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-125">toofinish removing hello message from hello queue, you must also call **delete\_message**.</span></span> <span data-ttu-id="c0fb9-126">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att när din kod inte tooprocess ett meddelande på grund av maskinvara eller programvara, kan en annan instans av koden hämta samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-126">This two-step process of removing a message assures that when your code fails tooprocess a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="c0fb9-127">Koden anropar **ta bort\_meddelandet** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-127">Your code calls **delete\_message** right after hello message has been processed.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)
```

<span data-ttu-id="c0fb9-128">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-128">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="c0fb9-129">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="c0fb9-129">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="c0fb9-130">Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-130">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="c0fb9-131">hello följande kodexempel används den **hämta\_meddelanden** metoden tooget 16 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-131">hello following code example uses the **get\_messages** method tooget 16 messages in one call.</span></span> <span data-ttu-id="c0fb9-132">Sedan bearbetas varje meddelande med hjälp av en for-loop.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-132">Then it processes each message using a for loop.</span></span> <span data-ttu-id="c0fb9-133">Den anger också hello tidsgränsen för osynlighet till fem minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-133">It also sets hello invisibility timeout to five minutes for each message.</span></span>

```python
messages = queue_service.get_messages('taskqueue', num_messages=16, visibility_timeout=5*60)
for message in messages:
    print(message.content)
    queue_service.delete_message('taskqueue', message.id, message.pop_receipt)        
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="c0fb9-134">Så här: Ändra hello innehållet i meddelandet i kön</span><span class="sxs-lookup"><span data-stu-id="c0fb9-134">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="c0fb9-135">Du kan ändra hello innehållet i ett meddelande direkt i hello kö.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-135">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="c0fb9-136">Om meddelandet representerar en arbetsuppgift kan använda du den här funktionen tooupdate statusen för arbetsuppgiften som hello.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-136">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="c0fb9-137">hello koden nedan använder hello **uppdatera\_meddelandet** metoden tooupdate ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-137">hello code below uses hello **update\_message** method tooupdate a message.</span></span> <span data-ttu-id="c0fb9-138">Tidsgränsen för visning av hello anges too0, vilket innebär att meddelandet visas omedelbart och hello innehållet uppdateras.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-138">hello visibility timeout is set too0, meaning the message appears immediately and hello content is updated.</span></span>

```python
messages = queue_service.get_messages('taskqueue')
for message in messages:
    queue_service.update_message('taskqueue', message.id, message.pop_receipt, 0, u'Hello World Again')
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="c0fb9-139">Så här: Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="c0fb9-139">How To: Get hello Queue Length</span></span>
<span data-ttu-id="c0fb9-140">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-140">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="c0fb9-141">Den **hämta\_kön\_metadata** metoden ber hello kön service tooreturn metadata om hello kön och hello **approximate_message_count**.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-141">The **get\_queue\_metadata** method asks hello queue service tooreturn metadata about hello queue, and hello **approximate_message_count**.</span></span> <span data-ttu-id="c0fb9-142">hello resultatet är bara ungefärliga eftersom meddelanden kan läggas till eller tas bort efter kötjänsten svarar tooyour begäran.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-142">hello result is only approximate because messages can be added or removed after the queue service responds tooyour request.</span></span>

```python
metadata = queue_service.get_queue_metadata('taskqueue')
count = metadata.approximate_message_count
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="c0fb9-143">Så här: Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="c0fb9-143">How To: Delete a Queue</span></span>
<span data-ttu-id="c0fb9-144">toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort\_kön** metod.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-144">toodelete a queue and all hello messages contained in it, call the **delete\_queue** method.</span></span>

```python
queue_service.delete_queue('taskqueue')
```

## <a name="next-steps"></a><span data-ttu-id="c0fb9-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0fb9-145">Next Steps</span></span>
<span data-ttu-id="c0fb9-146">Nu när du har lärt dig hello grunderna i Queue storage följa dessa länkar toolearn mer.</span><span class="sxs-lookup"><span data-stu-id="c0fb9-146">Now that you've learned hello basics of Queue storage, follow these links toolearn more.</span></span>

* [<span data-ttu-id="c0fb9-147">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="c0fb9-147">Python Developer Center</span></span>](/develop/python/)
* [<span data-ttu-id="c0fb9-148">REST-API för Azure Storage Services</span><span class="sxs-lookup"><span data-stu-id="c0fb9-148">Azure Storage Services REST API</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="c0fb9-149">[Azure Storage Teamblogg]</span><span class="sxs-lookup"><span data-stu-id="c0fb9-149">[Azure Storage Team Blog]</span></span>
* <span data-ttu-id="c0fb9-150">[Microsoft Azure Storage SDK för Python]</span><span class="sxs-lookup"><span data-stu-id="c0fb9-150">[Microsoft Azure Storage SDK for Python]</span></span>

[Azure Storage Teamblogg]: http://blogs.msdn.com/b/windowsazurestorage/
[Microsoft Azure Storage SDK för Python]: https://github.com/Azure/azure-storage-python