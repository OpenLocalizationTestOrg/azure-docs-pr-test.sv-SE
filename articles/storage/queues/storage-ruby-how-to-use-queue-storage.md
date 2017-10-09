---
title: "aaaHow toouse Queue storage från Ruby | Microsoft Docs"
description: "Lär dig hur toouse hello Azure Queue service toocreate och ta bort köer och infoga, hämta och ta bort meddelanden. Exempel som skrivits i Ruby."
services: storage
documentationcenter: ruby
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 59c2d81b-db9c-46ee-ade2-2f0caae6b1e6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: robinsh
ms.openlocfilehash: c8eacac058442419cb9e8fe62cb69ad7ef1e2fc4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a><span data-ttu-id="8471f-104">Hur toouse Queue storage från Ruby</span><span class="sxs-lookup"><span data-stu-id="8471f-104">How toouse Queue storage from Ruby</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="8471f-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="8471f-105">Overview</span></span>
<span data-ttu-id="8471f-106">Den här guiden visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure Queue Storage-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="8471f-106">This guide shows you how tooperform common scenarios using hello Microsoft Azure Queue Storage service.</span></span> <span data-ttu-id="8471f-107">hello exempel skrivs med hello Ruby Azure API.</span><span class="sxs-lookup"><span data-stu-id="8471f-107">hello samples are written using hello Ruby Azure API.</span></span>
<span data-ttu-id="8471f-108">hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.</span><span class="sxs-lookup"><span data-stu-id="8471f-108">hello scenarios covered include **inserting**, **peeking**, **getting**, and **deleting** queue messages, as well as **creating and deleting queues**.</span></span>

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="8471f-109">Skapa ett Ruby-program</span><span class="sxs-lookup"><span data-stu-id="8471f-109">Create a Ruby Application</span></span>
<span data-ttu-id="8471f-110">Skapa ett Ruby program.</span><span class="sxs-lookup"><span data-stu-id="8471f-110">Create a Ruby application.</span></span> <span data-ttu-id="8471f-111">Instruktioner finns i [Ruby på spår webbprogram på en Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="8471f-111">For instructions, see [Ruby on Rails Web application on an Azure VM](../../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="8471f-112">Konfigurera ditt program tooAccess lagring</span><span class="sxs-lookup"><span data-stu-id="8471f-112">Configure Your Application tooAccess Storage</span></span>
<span data-ttu-id="8471f-113">toouse Azure storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="8471f-113">toouse Azure storage, you need toodownload and use hello Ruby azure package, which includes a set of convenience libraries that communicate with hello storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="8471f-114">Använd RubyGems tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="8471f-114">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="8471f-115">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="8471f-115">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="8471f-116">Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.</span><span class="sxs-lookup"><span data-stu-id="8471f-116">Type "gem install azure" in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="8471f-117">Importera hello-paket</span><span class="sxs-lookup"><span data-stu-id="8471f-117">Import hello package</span></span>
<span data-ttu-id="8471f-118">Använd valfri textredigerare, lägga till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:</span><span class="sxs-lookup"><span data-stu-id="8471f-118">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse storage:</span></span>

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a><span data-ttu-id="8471f-119">Skapa ett Azure Storage-anslutning</span><span class="sxs-lookup"><span data-stu-id="8471f-119">Setup an Azure Storage Connection</span></span>
<span data-ttu-id="8471f-120">hello azure modulen läser hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_ACCESS_KEY** för information krävs tooconnect tooyour Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="8471f-120">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS_KEY** for information required tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="8471f-121">Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::QueueService** med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="8471f-121">If these environment variables are not set, you must specify hello account information before using **Azure::QueueService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

<span data-ttu-id="8471f-122">tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="8471f-122">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="8471f-123">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8471f-123">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8471f-124">Navigera toohello storage-konto som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="8471f-124">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="8471f-125">I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="8471f-125">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="8471f-126">I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2.</span><span class="sxs-lookup"><span data-stu-id="8471f-126">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="8471f-127">Du kan använda någon av dessa.</span><span class="sxs-lookup"><span data-stu-id="8471f-127">You can use either of these.</span></span> 
5. <span data-ttu-id="8471f-128">Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.</span><span class="sxs-lookup"><span data-stu-id="8471f-128">Click hello copy icon toocopy hello key toohello clipboard.</span></span> 

## <a name="how-to-create-a-queue"></a><span data-ttu-id="8471f-129">Så här: Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="8471f-129">How To: Create a Queue</span></span>
<span data-ttu-id="8471f-130">hello följande kod skapar en **Azure::QueueService** -objektet, vilket gör att du toowork med köer.</span><span class="sxs-lookup"><span data-stu-id="8471f-130">hello following code creates a **Azure::QueueService** object, which enables you toowork with queues.</span></span>

```ruby
azure_queue_service = Azure::QueueService.new
```

<span data-ttu-id="8471f-131">Använd hello **create_queue()** metoden toocreate en kö med hello angett namn.</span><span class="sxs-lookup"><span data-stu-id="8471f-131">Use hello **create_queue()** method toocreate a queue with hello specified name.</span></span>

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a><span data-ttu-id="8471f-132">Så här: Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="8471f-132">How To: Insert a Message into a Queue</span></span>
<span data-ttu-id="8471f-133">tooinsert ett meddelande i en kö, Använd hello **create_message()** metoden toocreate ett nytt meddelande och Lägg till den toohello kön.</span><span class="sxs-lookup"><span data-stu-id="8471f-133">tooinsert a message into a queue, use hello **create_message()** method toocreate a new message and add it toohello queue.</span></span>

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a><span data-ttu-id="8471f-134">Så här: Granska hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="8471f-134">How To: Peek at hello Next Message</span></span>
<span data-ttu-id="8471f-135">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **titt\_messages()** metod.</span><span class="sxs-lookup"><span data-stu-id="8471f-135">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **peek\_messages()** method.</span></span> <span data-ttu-id="8471f-136">Som standard **titt\_messages()** peeks på ett enda meddelande.</span><span class="sxs-lookup"><span data-stu-id="8471f-136">By default, **peek\_messages()** peeks at a single message.</span></span> <span data-ttu-id="8471f-137">Du kan även ange hur många meddelanden du vill toopeek.</span><span class="sxs-lookup"><span data-stu-id="8471f-137">You can also specify how many messages you want toopeek.</span></span>

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a><span data-ttu-id="8471f-138">Så här: Status Created hello nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="8471f-138">How To: Dequeue hello Next Message</span></span>
<span data-ttu-id="8471f-139">Du kan ta bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="8471f-139">You can remove a message from a queue in two steps.</span></span>

1. <span data-ttu-id="8471f-140">När du anropar **lista\_messages()**, du får hello nästa meddelande i en kö som standard.</span><span class="sxs-lookup"><span data-stu-id="8471f-140">When you call **list\_messages()**, you get hello next message in a queue by default.</span></span> <span data-ttu-id="8471f-141">Du kan även ange hur många meddelanden du vill tooget.</span><span class="sxs-lookup"><span data-stu-id="8471f-141">You can also specify how many messages you want tooget.</span></span> <span data-ttu-id="8471f-142">Hej meddelanden som returneras från **lista\_messages()** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="8471f-142">hello messages returned from **list\_messages()** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="8471f-143">Du skickar i hello synlighet tidsgränsen i sekunder som en parameter.</span><span class="sxs-lookup"><span data-stu-id="8471f-143">You pass in hello visibility timeout in seconds as a parameter.</span></span>
2. <span data-ttu-id="8471f-144">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **delete_message()**.</span><span class="sxs-lookup"><span data-stu-id="8471f-144">toofinish removing hello message from hello queue, you must also call **delete_message()**.</span></span>

<span data-ttu-id="8471f-145">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer när din kod misslyckas tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="8471f-145">This two-step process of removing a message assures that when your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="8471f-146">Koden anropar **ta bort\_message()** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="8471f-146">Your code calls **delete\_message()** right after hello message has been processed.</span></span>

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a><span data-ttu-id="8471f-147">Så här: Ändra hello innehållet i meddelandet i kön</span><span class="sxs-lookup"><span data-stu-id="8471f-147">How To: Change hello Contents of a Queued Message</span></span>
<span data-ttu-id="8471f-148">Du kan ändra hello innehållet i ett meddelande direkt i hello kö.</span><span class="sxs-lookup"><span data-stu-id="8471f-148">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="8471f-149">hello koden nedan använder hello **update_message()** metoden tooupdate ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="8471f-149">hello code below uses hello **update_message()** method tooupdate a message.</span></span> <span data-ttu-id="8471f-150">hello-metoden returnerar en tuppel som innehåller hello pop mottagit hello kömeddelande och ett UTC-datum tid-värde som representerar när hello-meddelande kommer att vara synliga i hello kön.</span><span class="sxs-lookup"><span data-stu-id="8471f-150">hello method will return a tuple which contains hello pop receipt of hello queue message and a UTC date time value that represents when hello message will be visible on hello queue.</span></span>

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a><span data-ttu-id="8471f-151">Så här: Ytterligare alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="8471f-151">How To: Additional Options for Dequeuing Messages</span></span>
<span data-ttu-id="8471f-152">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="8471f-152">There are two ways you can customize message retrieval from a queue.</span></span>

1. <span data-ttu-id="8471f-153">Du kan få en batch med meddelandet.</span><span class="sxs-lookup"><span data-stu-id="8471f-153">You can get a batch of message.</span></span>
2. <span data-ttu-id="8471f-154">Du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="8471f-154">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span>

<span data-ttu-id="8471f-155">hello följande kodexempel används hello **lista\_messages()** metoden tooget 15 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="8471f-155">hello following code example uses hello **list\_messages()** method tooget 15 messages in one call.</span></span> <span data-ttu-id="8471f-156">Sedan skriver ut och tar bort varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="8471f-156">Then it prints and deletes each message.</span></span> <span data-ttu-id="8471f-157">Den anger också hello osynlighet timeout toofive minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="8471f-157">It also sets hello invisibility timeout toofive minutes for each message.</span></span>

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a><span data-ttu-id="8471f-158">Så här: Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="8471f-158">How To: Get hello Queue Length</span></span>
<span data-ttu-id="8471f-159">Du kan få en uppskattning av hello antal meddelanden i kö för hello.</span><span class="sxs-lookup"><span data-stu-id="8471f-159">You can get an estimation of hello number of messages in hello queue.</span></span> <span data-ttu-id="8471f-160">Hej **hämta\_kön\_metadata()** metoden ber hello kön service tooreturn hello ungefärliga meddelandemängd och metadata om hello kö.</span><span class="sxs-lookup"><span data-stu-id="8471f-160">hello **get\_queue\_metadata()** method asks hello queue service tooreturn hello approximate message count and metadata about hello queue.</span></span>

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a><span data-ttu-id="8471f-161">Så här: Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="8471f-161">How To: Delete a Queue</span></span>
<span data-ttu-id="8471f-162">toodelete en kö och alla hälsningsmeddelande som ingår i den anropet hello **ta bort\_queue()** metod för hello köobjekt.</span><span class="sxs-lookup"><span data-stu-id="8471f-162">toodelete a queue and all hello messages contained in it, call hello **delete\_queue()** method on hello queue object.</span></span>

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a><span data-ttu-id="8471f-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8471f-163">Next Steps</span></span>
<span data-ttu-id="8471f-164">Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8471f-164">Now that you've learned hello basics of queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="8471f-165">Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)</span><span class="sxs-lookup"><span data-stu-id="8471f-165">Visit hello [Azure Storage Team Blog](http://blogs.msdn.com/b/windowsazurestorage/)</span></span>
* <span data-ttu-id="8471f-166">Besök hello [Azure SDK för Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub</span><span class="sxs-lookup"><span data-stu-id="8471f-166">Visit hello [Azure SDK for Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

<span data-ttu-id="8471f-167">En jämförelse mellan hello Azure-kötjänsten som beskrivs i den här artikeln och Azure Service Bus-köer som beskrivs i hello [hur toouse Service Bus-köer](/develop/ruby/how-to-guides/service-bus-queues/) artikel, se [Azure köer och Service Bus-köer - jämfört med och Skillnad från](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span><span class="sxs-lookup"><span data-stu-id="8471f-167">For a comparison between hello Azure Queue Service discussed in this article and Azure Service Bus Queues discussed in hello [How toouse Service Bus Queues](/develop/ruby/how-to-guides/service-bus-queues/) article, see [Azure Queues and Service Bus Queues - Compared and Contrasted](../../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)</span></span>
