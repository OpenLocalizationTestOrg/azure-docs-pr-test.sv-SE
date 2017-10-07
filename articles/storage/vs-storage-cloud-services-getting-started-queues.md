---
title: "aaaGet igång med queue storage- och Visual Studio anslutna tjänster (molntjänster) | Microsoft Docs"
description: "Hur tooget igång med Azure Queue storage i ett molntjänstprojekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 8ba3d830cb83e3d75102b8a09363f1dc200ff1c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Komma igång med Azure Queue storage och Visual Studio anslutna tjänster (cloud services-projekt)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Översikt
Den här artikeln beskriver hur tooget igång med Azure Queue storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i cloud services-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan .

Får du lära dig hur toocreate en kö i koden. Vi också lära dig hur tooperform basic kö åtgärder, till exempel lägga till, ändra, läsa och tar bort Kömeddelanden. hello exemplen är skrivna i C#-kod och använder hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.

* Se [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md) mer information om manipulering köer i kod.
* Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.
* Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.
* Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.

Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS. Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.

## <a name="access-queues-in-code"></a>Ha åtkomst till serviceköer i koden
tooaccess köer i molntjänster i Visual Studio-projekt, behöver du tooinclude hello följande objekt tooany C#-källfilen som har åtkomst till Azure Queue storage.

1. Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring. Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Hämta en **CloudQueueClient** objekt tooreference hello köobjekt i ditt lagringskonto.  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Hämta en **CloudQueue** objekt tooreference en särskild kö.
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Obs:** använda alla hello ovan kod framför hello koden i hello följande exempel.

## <a name="create-a-queue-in-code"></a>Skapa en kö i koden
toocreate hello kön i koden, Lägg till ett anrop för**CreateIfNotExists**.

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a>Lägg till en meddelandekö tooa
tooinsert ett meddelande i en befintlig kö, skapa en ny **CloudQueueMessage** objektet och sedan anropa hello **AddMessage** metod.

En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.

Här är ett exempel som infogar hello-meddelande ”Hello World”.

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a>Läs ett meddelande i en kö
Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessage** metod.

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a>Läsa och ta bort ett meddelande i en kö
Koden kan ta bort (Frigör kö) ett meddelande från en kö i två steg.

1. Anropa **GetMessage** tooget hello nästa meddelande i en kö. Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön. Som standard är det här meddelandet osynligt i 30 sekunder.
2. toofinish ta bort hello-meddelande från hello kö, anropet **DeleteMessage**.

Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen. hello följande koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a>Använda ytterligare alternativ tooprocess och ta bort Kömeddelanden
Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.

* Du kan hämta en grupp med meddelanden (upp too32).
* Du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande. hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop. Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop. Den anger också hello osynlighet timeout toofive minuter för varje meddelande. Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**GetMessage**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.

Här är ett exempel:

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a>Hämta hello Kölängd
Du kan få en uppskattning av hello antal meddelanden i en kö. Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande. Hej **ApproximateMethodCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a>Använda hello Async-Await-mönstret med vanliga Azure kön API: er
Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga Azure kön API: er. hello exemplet anropar hello asynkrona versionen av var och en av hello anges metoder, detta kan ses av hello **asynkrona** efter korrigering för varje metod. När en asynkron metod är används hello async-await mönster pausar lokala körningen tills hello anropet har slutförts. Det här innebär att hello aktuella tråden toodo annat arbete som hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet. Mer information om hur du använder hello Async-Await-mönstret i .NET finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message tooput in hello queue
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Add hello message asynchronously
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue hello message
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Delete hello message asynchronously
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");

## <a name="delete-a-queue"></a>Ta bort en kö
en kö och alla hälsningsmeddelande som ingår i den anropet hello toodelete **ta bort** metoden för köobjektet hello.

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

