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
ms.openlocfilehash: 726c7d2f08b2d5938ee5f9dcdc2735e447388856
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-ruby"></a>Hur toouse Queue storage från Ruby
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur hello tooperform vanliga scenarier med hjälp av Microsoft Azure Queue Storage-tjänsten. hello exempel skrivs med hello Ruby Azure API.
hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Skapa ett Ruby-program
Skapa ett Ruby program. Instruktioner finns i [Ruby på spår webbprogram på en Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-tooaccess-storage"></a>Konfigurera ditt program tooAccess lagring
toouse Azure storage, behöver du toodownload och Använd hello Ruby azure-paketet som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med hello storage REST-tjänster.

### <a name="use-rubygems-tooobtain-hello-package"></a>Använd RubyGems tooobtain hello-paket
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix).
2. Skriv ”symbolen installera azure” i hello kommandot fönstret tooinstall hello symbolen och beroenden.

### <a name="import-hello-package"></a>Importera hello-paket
Använd valfri textredigerare, lägga till hello följande toohello överkant hello Ruby filen där du ska toouse lagring:

```ruby
require "azure"
```

## <a name="setup-an-azure-storage-connection"></a>Skapa ett Azure Storage-anslutning
hello azure modulen läser hello miljövariabler **AZURE\_lagring\_konto** och **AZURE\_lagring\_ACCESS_KEY** för information krävs tooconnect tooyour Azure storage-konto. Om de här miljövariablerna inte har angetts måste du ange hello kontoinformation innan du använder **Azure::QueueService** med hello följande kod:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your Azure storage access key>"
```

tooobtain dessa värden från en klassiska eller Resource Manager lagring konto i hello Azure-portalen:

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Navigera toohello storage-konto som du vill toouse.
3. I hello inställningsbladet på hello höger klickar du på **åtkomstnycklar**.
4. I hello åtkomst bladet nycklar som visas, visas hello åtkomstnyckel 1 och åtkomstnyckel 2. Du kan använda någon av dessa. 
5. Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp. 

tooobtain värdena från klassiska lagring konto i hello klassiska Azure-portalen:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello storage-konto som du vill toouse.
3. Klicka på **hantera ÅTKOMSTNYCKLAR** längst hello hello navigeringsfönstret.
4. I hello popup dialogrutan visas hello lagringskontonamn, primärnyckeln och sekundära åtkomstnyckeln. Du kan använda hello primära eller hello sekundära för åtkomst till nyckeln. 
5. Klicka på hello kopiera ikonen toocopy hello viktiga toohello Urklipp.

## <a name="how-to-create-a-queue"></a>Så här: Skapa en kö
hello följande kod skapar en **Azure::QueueService** -objektet, vilket gör att du toowork med köer.

```ruby
azure_queue_service = Azure::QueueService.new
```

Använd hello **create_queue()** metoden toocreate en kö med hello angett namn.

```ruby
begin
  azure_queue_service.create_queue("test-queue")
rescue
  puts $!
end
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Så här: Infoga ett meddelande i en kö
tooinsert ett meddelande i en kö, Använd hello **create_message()** metoden toocreate ett nytt meddelande och Lägg till den toohello kön.

```ruby
azure_queue_service.create_message("test-queue", "test message")
```

## <a name="how-to-peek-at-hello-next-message"></a>Så här: Granska hello nästa meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **titt\_messages()** metod. Som standard **titt\_messages()** peeks på ett enda meddelande. Du kan även ange hur många meddelanden du vill toopeek.

```ruby
result = azure_queue_service.peek_messages("test-queue",
  {:number_of_messages => 10})
```

## <a name="how-to-dequeue-hello-next-message"></a>Så här: Status Created hello nästa meddelande
Du kan ta bort ett meddelande från en kö i två steg.

1. När du anropar **lista\_messages()**, du får hello nästa meddelande i en kö som standard. Du kan även ange hur många meddelanden du vill tooget. Hej meddelanden som returneras från **lista\_messages()** blir osynligt tooany annan kod läsa meddelanden från den här kön. Du skickar i hello synlighet tidsgränsen i sekunder som en parameter.
2. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **delete_message()**.

Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer när din kod misslyckas tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **ta bort\_message()** direkt efter hello-meddelande har bearbetats.

```ruby
messages = azure_queue_service.list_messages("test-queue", 30)
azure_queue_service.delete_message("test-queue", 
  messages[0].id, messages[0].pop_receipt)
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Så här: Ändra hello innehållet i meddelandet i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö. hello koden nedan använder hello **update_message()** metoden tooupdate ett meddelande. hello-metoden returnerar en tuppel som innehåller hello pop mottagit hello kömeddelande och ett UTC-datum tid-värde som representerar när hello-meddelande kommer att vara synliga i hello kön.

```ruby
message = azure_queue_service.list_messages("test-queue", 30)
pop_receipt, time_next_visible = azure_queue_service.update_message(
  "test-queue", message.id, message.pop_receipt, "updated test message", 
  30)
```

## <a name="how-to-additional-options-for-dequeuing-messages"></a>Så här: Ytterligare alternativ för mellan köer meddelanden
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.

1. Du kan få en batch med meddelandet.
2. Du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.

hello följande kodexempel används hello **lista\_messages()** metoden tooget 15 meddelanden i ett anrop. Sedan skriver ut och tar bort varje meddelande. Den anger också hello osynlighet timeout toofive minuter för varje meddelande.

```ruby
azure_queue_service.list_messages("test-queue", 300
  {:number_of_messages => 15}).each do |m|
  puts m.message_text
  azure_queue_service.delete_message("test-queue", m.id, m.pop_receipt)
end
```

## <a name="how-to-get-hello-queue-length"></a>Så här: Hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i kö för hello. Hej **hämta\_kön\_metadata()** metoden ber hello kön service tooreturn hello ungefärliga meddelandemängd och metadata om hello kö.

```ruby
message_count, metadata = azure_queue_service.get_queue_metadata(
  "test-queue")
```

## <a name="how-to-delete-a-queue"></a>Så här: Ta bort en kö
toodelete en kö och alla hälsningsmeddelande som ingår i den anropet hello **ta bort\_queue()** metod för hello köobjekt.

```ruby
azure_queue_service.delete_queue("test-queue")
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* Besök hello [Azure Storage-teamets blogg](http://blogs.msdn.com/b/windowsazurestorage/)
* Besök hello [Azure SDK för Ruby](https://github.com/WindowsAzure/azure-sdk-for-ruby) databasen på GitHub

En jämförelse mellan hello Azure-kötjänsten som beskrivs i den här artikeln och Azure Service Bus-köer som beskrivs i hello [hur toouse Service Bus-köer](/develop/ruby/how-to-guides/service-bus-queues/) artikel, se [Azure köer och Service Bus-köer - jämfört med och Skillnad från](../service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted.md)
