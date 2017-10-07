---
title: aaaHow toouse queue storage (C++) | Microsoft Docs
description: "Lär dig hur toouse hello queue storage-tjänst i Azure. Exempel är skrivna i C++."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: jahogg
editor: tysonn
ms.assetid: c8a36365-29f6-404d-8fd1-858a7f33b50a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: cpp
ms.topic: article
ms.date: 05/11/2017
ms.author: cbrooksmsft
ms.openlocfilehash: 755380824890ad83774e14d258975915e10cfede
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-queue-storage-from-c"></a>Hur toouse Queue Storage från C++
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här guiden visar hur tooperform vanliga scenarier med hjälp av hello Azure Queue storage-tjänst. hello exemplen är skrivna i C++ och använda hello [Azure Storage-klientbibliotek för C++](http://github.com/Azure/azure-storage-cpp/blob/master/README.md). hello beskrivs scenarier där **infoga**, **granskning**, **komma**, och **bort** kö meddelanden, samt  **Skapa och ta bort köer**.

> [!NOTE]
> Den här guiden riktad mot hello Azure Storage-klientbibliotek för C++ version 1.0.0 och senare. hello rekommenderas version är Storage-klientbibliotek 2.2.0, som är tillgängliga via [NuGet](http://www.nuget.org/packages/wastorage) eller [GitHub](http://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>Skapa ett C++-program
I den här guiden använder lagringsfunktioner som kan köras i ett C++-program.

toodo så behöver du tooinstall hello Azure Storage-klientbibliotek för C++ och skapa ett Azure storage-konto i din Azure-prenumeration.

tooinstall hello Azure Storage-klientbibliotek för C++ kan du använda hello följande metoder:

* **Linux:** Följ instruktionerna i hello hello [Azure Storage-klientbibliotek för C++ viktigt](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sidan.
* **Windows:** i Visual Studio klickar du på **Verktyg > NuGet Package Manager > Package Manager-konsolen**. Typen hello följande kommando i hello [NuGet Package Manager-konsolen](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) och tryck på **RETUR**.

```  
Install-Package wastorage
```

## <a name="configure-your-application-tooaccess-queue-storage"></a>Konfigurera ditt program tooaccess Queue Storage
Lägga till följande hello innehålla instruktioner toohello överkant hello C++ filen där du vill toouse hello Azure storage-API: er tooaccess köer:  

```cpp
#include <was/storage_account.h>
#include <was/queue.h>
```

## <a name="set-up-an-azure-storage-connection-string"></a>Ställ in en anslutningssträng för Azure storage
Ett Azure storage-klienten använder en lagring sträng toostore slutpunkter och autentiseringsuppgifter för åtkomst till data management services. När den körs i ett klientprogram, du måste ange hello lagringsanslutningssträngen i hello följande format, med hello namnet på ditt konto och hello lagring lagringsåtkomstnyckel för hello storage-konto som anges i hello [Azure Portal](https://portal.azure.com)för hello *AccountName* och *AccountKey* värden. Information om lagringskonton och snabbtangenterna finns [om Azure Lagringskonton](storage-create-storage-account.md). Det här exemplet visar hur du kan deklarera en anslutningssträng för statiska fält toohold hello:  

```cpp
// Define hello connection-string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

tootest ditt program i din lokala Windows-dator kan du använda hello Microsoft Azure [lagringsemulatorn](storage-use-emulator.md) som installeras med hello [Azure SDK](https://azure.microsoft.com/downloads/). hello storage-emulatorn är ett verktyg som simulerar hello Blob, köer och tabellen tjänster som är tillgängliga i Azure på utvecklingsdatorn lokala. hello följande exempel visas hur du deklarera ett statiskt fält toohold hello anslutning sträng tooyour lokala storage-emulatorn:  

```cpp
// Define hello connection-string with Azure Storage Emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

toostart hello Azure storage-emulatorn väljer hello **starta** eller tryck på knappen hello **Windows** nyckel. Börja skriva **Azure Storage-emulatorn**, och välj **Microsoft Azure Storage-emulatorn** hello listan med program.

hello förutsätter följande exempel att du har använt någon av dessa två metoder tooget hello lagringsanslutningssträng.

## <a name="retrieve-your-connection-string"></a>Hämta anslutningssträngen
Du kan använda hello **cloud_storage_account** klassen toorepresent kontoinformationen för lagring. tooretrieve lagringen kontoinformation från hello anslutningssträngen för lagring kan du använda hello **parsa** metod.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

## <a name="how-to-create-a-queue"></a>Så här: skapa en kö
En **cloud_queue_client** objekt kan du få referensobjekt för köer. hello följande kod skapar en **cloud_queue_client** objekt.

```cpp
// Retrieve storage account from connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create a queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();
```

Använd hello **cloud_queue_client** objekt tooget en referens toohello kö som du vill toouse. Du kan skapa hello kö om den inte finns.

```cpp
// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
 queue.create_if_not_exists();  
```

## <a name="how-to-insert-a-message-into-a-queue"></a>Så här: Infoga ett meddelande i en kö
tooinsert ett meddelande i en befintlig kö först skapa en ny **cloud_queue_message**. Därefter anropar hello **add_message** metod. En **cloud_queue_message** kan skapas från antingen en sträng eller en **byte** matris. Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”:

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Create hello queue if it doesn't already exist.
queue.create_if_not_exists();

// Create a message and add it toohello queue.
azure::storage::cloud_queue_message message1(U("Hello, World"));
queue.add_message(message1);  
```

## <a name="how-to-peek-at-hello-next-message"></a>Så här: granska nästa hello-meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **peek_message** metod.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Peek at hello next message.
azure::storage::cloud_queue_message peeked_message = queue.peek_message();

// Output hello message content.
std::wcout << U("Peeked message content: ") << peeked_message.content_as_string() << std::endl;
```

## <a name="how-to-change-hello-contents-of-a-queued-message"></a>Så här: ändra hello innehållet i ett meddelande i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö. Om hello-meddelande representerar en arbetsuppgift, kan du använda den här funktionen tooupdate hello hello arbete aktivitetens status. hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder. Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande. Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel. Normalt ska du behålla ett återförsöksvärde och om hello-meddelande försöks mer än n gånger, vill du ta bort den. Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_conection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello message from hello queue and update hello message contents.
// hello visibility timeout "0" means make it visible immediately.
// hello visibility timeout "60" means hello client can get another minute toocontinue
// working on hello message.
azure::storage::cloud_queue_message changed_message = queue.get_message();

changed_message.set_content(U("Changed message"));
queue.update_message(changed_message, std::chrono::seconds(60), true);

// Output hello message content.
std::wcout << U("Changed message content: ") << changed_message.content_as_string() << std::endl;  
```

## <a name="how-to-de-queue-hello-next-message"></a>Så här: Frigör kö nästa hello-meddelande
Koden tar bort ett meddelande från en kö i två steg. När du anropar **get_message**, du får hello nästa meddelande i en kö. Ett meddelande som returneras från **get_message** blir osynligt tooany annan kod läsa meddelanden från den här kön. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **delete_message**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **delete_message** direkt efter hello-meddelande har bearbetats.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Get hello next message.
azure::storage::cloud_queue_message dequeued_message = queue.get_message();
std::wcout << U("Dequeued message: ") << dequeued_message.content_as_string() << std::endl;

// Delete hello message.
queue.delete_message(dequeued_message);
```

## <a name="how-to-leverage-additional-options-for-de-queuing-messages"></a>Så här: använda fler alternativ för meddelanden ur kön
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö. Först får du en grupp med meddelanden (upp too32). Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande. hello följande kodexempel används hello **get_messages** metoden tooget 20 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **för** loop. Den anger också hello osynlighet timeout toofive minuter för varje meddelande. Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**get_messages**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Dequeue some queue messages (maximum 32 at a time) and set their visibility timeout to
// 5 minutes (300 seconds).
azure::storage::queue_request_options options;
azure::storage::operation_context context;

// Retrieve 20 messages from hello queue with a visibility timeout of 300 seconds.
std::vector<azure::storage::cloud_queue_message> messages = queue.get_messages(20, std::chrono::seconds(300), options, context);

for (auto it = messages.cbegin(); it != messages.cend(); ++it)
{
    // Display hello contents of hello message.
    std::wcout << U("Get: ") << it->content_as_string() << std::endl;
}
```

## <a name="how-to-get-hello-queue-length"></a>Så här: hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Hej **download_attributes** metoden ber hello kön service tooretrieve hello köattributen, inklusive antal för hello-meddelande. Hej **approximate_message_count** metoden hämtar hello Ungefärligt antal meddelanden i kö för hello.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// Fetch hello queue attributes.
queue.download_attributes();

// Retrieve hello cached approximate message count.
int cachedMessageCount = queue.approximate_message_count();

// Display number of messages.
std::wcout << U("Number of messages in queue: ") << cachedMessageCount << std::endl;  
```

## <a name="how-to-delete-a-queue"></a>Så här: ta bort en kö
en kö och alla hälsningsmeddelande som ingår i den anropet hello toodelete **delete_queue_if_exists** metod för hello köobjekt.

```cpp
// Retrieve storage account from connection-string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create hello queue client.
azure::storage::cloud_queue_client queue_client = storage_account.create_cloud_queue_client();

// Retrieve a reference tooa queue.
azure::storage::cloud_queue queue = queue_client.get_queue_reference(U("my-sample-queue"));

// If hello queue exists and delete it.
queue.delete_queue_if_exists();  
```

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i Queue storage kan du följa dessa länkar toolearn mer om Azure Storage.

* [Hur toouse Blob Storage från C++](storage-c-plus-plus-how-to-use-blobs.md)
* [Hur toouse Table Storage från C++](storage-c-plus-plus-how-to-use-tables.md)
* [Lista över Azure Storage-resurser i C++](storage-c-plus-plus-enumeration.md)
* [Storage-klientbibliotek för C++-referens](http://azure.github.io/azure-storage-cpp)
* [Azure Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/)