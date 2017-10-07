---
title: "aaaGet igång med Azure Queue storage med hjälp av .NET | Microsoft Docs"
description: "Azure Queues ger tillförlitliga, asynkrona meddelandefunktioner mellan programkomponenter. Moln meddelanden gör det möjligt tooscale för komponenter dina program oberoende av varandra."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: c0f82537-a613-4f01-b2ed-fc82e5eea2a7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: robinsh
ms.openlocfilehash: 0cf6a71392b2fe859c7c9a9898c1ec84bcab4b19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a>Komma igång med Azure Queue Storage med hjälp av .NET
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a>Översikt
Azure Queue Storage innehåller molnmeddelandehantering mellan programkomponenter. När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra. Queue storage ger asynkrona meddelanden för kommunikation mellan programkomponenter, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil enhet. Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.

### <a name="about-this-tutorial"></a>Om den här självstudiekursen
Den här kursen visar hur toowrite .NET kod för några vanliga scenarier med hjälp av Azure Queue storage. Du lär dig bland annat hur du skapar och tar bort köer och hur du lägger till, läser och tar bort kömeddelanden.

**Uppskattad tid toocomplete:** 45 minuter

**Förhandskrav:**

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-klientbibliotek för .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager för .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* Ett [Azure Storage-konto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Lägga till med hjälp av direktiv
Lägg till följande hello `using` direktiven toohello överkant hello `Program.cs` fil:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a>Parsa anslutningssträngen för hello
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a>Skapa hello Queue-tjänstklienten
Hej **CloudQueueClient** klassen kan du tooretrieve köer som lagras i Queue storage. Här är ett sätt toocreate hello-klienten:

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
Nu är du redo toowrite kod som läser data från och skriver tooQueue datalagring.

## <a name="create-a-queue"></a>Skapa en kö
Det här exemplet illustrerar hur toocreate en kö om den inte redan finns:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a>Infoga ett meddelande i en kö
tooinsert ett meddelande i en befintlig kö först skapa en ny **CloudQueueMessage**. Därefter anropar hello **AddMessage** metod. Du kan skapa ett **CloudQueueMessage** antingen från en sträng (i UTF-8-format) eller från en **byte**-matris. Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create hello queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it toohello queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-hello-next-message"></a>Granska nästa hello-meddelande
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessage** metod.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at hello next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-hello-contents-of-a-queued-message"></a>Ändra hello innehållet i ett meddelande i kön
Du kan ändra hello innehållet i ett meddelande direkt i hello kö. Om meddelandet representerar en arbetsuppgift kan använda du den här funktionen tooupdate statusen för arbetsuppgiften som hello. hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder. Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande. Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel. Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den. Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello message from hello queue and update hello message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-hello-next-message"></a>Frigör kö nästa hello-meddelande
Koden tar bort ett meddelande från en kö i två steg. När du anropar **GetMessage**, du får hello nästa meddelande i en kö. Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder. toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **DeleteMessage**. Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. Koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get hello next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process hello message in less than 30 seconds, and then delete hello message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a>Använda Async-Await-mönstret med vanliga Queue Storage-API:er
Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga Queue storage-API: er. hello exemplet anropar hello asynkrona versionen av var och en av hello anges metoder som anges av hello *asynkrona* suffixet för varje metod. När en asynkron metod används hello async-await mönster pausar lokala körningen tills hello anropet har slutförts. Det här innebär att hello aktuella tråden toodo andra arbetet, vilket hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet. Mer information om hur du använder hello Async-Await-mönstret i .NET finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

```csharp
// Create hello queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message tooput in hello queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue hello message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue hello message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete hello message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a>Använda fler alternativ för att hämta meddelanden ur kön
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.
Först får du en grupp med meddelanden (upp too32). Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande. hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop. Den anger också hello osynlighet timeout toofive minuter för varje meddelande. Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**GetMessage**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-hello-queue-length"></a>Hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande. Hej **ApproximateMessageCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch hello queue attributes.
queue.FetchAttributes();

// Retrieve hello cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a>Ta bort en kö
toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort** metoden för köobjektet hello.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference tooa queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete hello queue.
queue.Delete();
```
    

## <a name="next-steps"></a>Nästa steg
Nu när du har lärt dig hello grunderna i Queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.

* Visa hello kön referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er:
  * [Storage-klientbibliotek för .NET-referens](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [REST API-referens](http://msdn.microsoft.com/library/azure/dd179355)
* Lär dig hur toosimplify hello koden du skriver toowork med Azure Storage med hjälp av hello [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).
* Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.
  * [Komma igång med Azure Table storage med hjälp av .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) toostore strukturerade data.
  * [Komma igång med Azure Blob storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md) toostore Ostrukturerade data.
  * [Ansluta tooSQL databasen med hjälp av .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md) toostore relationella data.

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
