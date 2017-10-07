---
title: "aaaGet igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur tooget igång med Azure queue storage i ASP.NET Core-projekt i Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 90c1ebcb6a2eac6bc4f6b69133bdf3135d359a72
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Kom igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET kärnor)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooget igång med Azure Queue storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan. Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.

Azure queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Ett enda kömeddelande kan vara upp too64 kilobyte (KB) i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.

tooget igång, måste du först toocreate en Azure-kö på ditt lagringskonto. Får du lära dig hur toocreate en kö i koden. Vi också lära dig hur tooperform basic kö åtgärder, till exempel lägga till, ändra, läsa och tar bort Kömeddelanden. hello prover är skrivna i C\# code och använda hello Azure Storage-klientbibliotek för .NET. Läs mer om ASP.NET [ASP.NET](http://www.asp.net).

**Obs:** vissa hello API: er som utför anrop tooAzure lagring i ASP.NET Core är asynkron. Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information. hello koden nedan förutsätter asynkrona programming metoder som används.

* Se [komma igång med Azure Queue storage med hjälp av .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) mer information om programmässigt manipulering köer.
* Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.
* Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.
* Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.

## <a name="access-queues-in-code"></a>Ha åtkomst till serviceköer i koden
tooaccess köer i ASP.NET Core projekt, behöver du tooinclude hello följande objekt tooany C#-källfilen som har åtkomst till Azure queue storage.

1. Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Hämta en **CloudQueueClient** objekt tooreference hello köobjekt i ditt lagringskonto.  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Hämta en **CloudQueue** objekt tooreference en särskild kö.
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Obs:** använda alla hello ovan kod framför hello koden i hello följande exempel.

### <a name="create-a-queue-in-code"></a>Skapa en kö i koden
toocreate hello Azure kön i koden, Lägg till ett anrop för**CreateIfNotExistsAsync**.

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a>Lägg till en meddelandekö tooa
tooinsert ett meddelande i en befintlig kö, skapa en ny **CloudQueueMessage** objektet och sedan anropa hello **AddMessageAsync** metod.

En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.

Här är ett exempel som infogar hello-meddelande ”Hello World”.

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Läs ett meddelande i en kö
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessageAsync** metod.

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Läsa och ta bort ett meddelande i en kö
Koden kan ta bort (status Created) ett meddelande från en kö i två steg.

1. Anropa **GetMessageAsync** tooget hello nästa meddelande i en kö. Ett meddelande som returneras från **GetMessageAsync** blir osynligt tooany annan kod läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder.
2. toofinish ta bort hello-meddelande från hello kö, anropet **DeleteMessageAsync**.

Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. hello följande koden anropar **DeleteMessageAsync** direkt efter hello-meddelande har bearbetats.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Använda fler alternativ för mellan köer meddelanden
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.
Först får du en grupp med meddelanden (upp too32). Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande. hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop. Den anger också hello osynlighet timeout too5 minuter för varje meddelande. Observera att hello 5 minuter start för alla meddelanden på hello samma tid, så efter 5 minuter efter hello-anropet för**GetMessage**, alla meddelanden som inte har tagits bort blir synliga igen.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a>Hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande. Hej **ApproximateMethodCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a>Använd hello Async-Await-mönstret med vanliga queue API: er
Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga queue API: er. hello exempel anrop hello asynkrona versionen av var och en av hello anges metoder. Detta kan ses av hello asynkrona efter korrigering för varje metod. När en asynkron metod används pausar hello Async-Await-mönstret lokala körningen tills hello anropet har slutförts. Det här innebär att hello aktuella tråden toodo annat arbete som hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet. Mer information om hur du använder hello Async-Await-mönstret i .NET, finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message tooadd toohello queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue hello message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Ta bort en kö
toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort** metoden för köobjektet hello.

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Nästa steg
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

