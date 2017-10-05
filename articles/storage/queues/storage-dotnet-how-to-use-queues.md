---
title: "Komma igång med Azure Queue Storage med hjälp av .NET | Microsoft Docs"
description: "Azure Queues ger tillförlitliga, asynkrona meddelandefunktioner mellan programkomponenter. Med hjälp av molnmeddelanden kan programkomponenter skalas separat."
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
ms.openlocfilehash: aa292c1eb048444f988a641df44183312cf39d28
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a><span data-ttu-id="b5333-104">Komma igång med Azure Queue Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="b5333-104">Get started with Azure Queue storage using .NET</span></span>
[!INCLUDE [storage-selector-queue-include](../../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a><span data-ttu-id="b5333-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="b5333-105">Overview</span></span>
<span data-ttu-id="b5333-106">Azure Queue Storage innehåller molnmeddelandehantering mellan programkomponenter.</span><span class="sxs-lookup"><span data-stu-id="b5333-106">Azure Queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="b5333-107">När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="b5333-107">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="b5333-108">Queue Storage är en asynkron meddelandelösning för kommunikation mellan programkomponenter, oavsett om de körs i molnet, på skrivbordet, på en lokal server eller på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="b5333-108">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in the cloud, on the desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="b5333-109">Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="b5333-109">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="b5333-110">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="b5333-110">About this tutorial</span></span>
<span data-ttu-id="b5333-111">I den här kursen lär du dig hur du skriver .NET-kod för några vanliga scenarier med hjälp av Azure Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="b5333-111">This tutorial shows how to write .NET code for some common scenarios using Azure Queue storage.</span></span> <span data-ttu-id="b5333-112">Du lär dig bland annat hur du skapar och tar bort köer och hur du lägger till, läser och tar bort kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="b5333-112">Scenarios covered include creating and deleting queues and adding, reading, and deleting queue messages.</span></span>

<span data-ttu-id="b5333-113">**Uppskattad tidsåtgång:** 45 minuter</span><span class="sxs-lookup"><span data-stu-id="b5333-113">**Estimated time to complete:** 45 minutes</span></span>

<span data-ttu-id="b5333-114">**Förhandskrav:**</span><span class="sxs-lookup"><span data-stu-id="b5333-114">**Prerequisities:**</span></span>

* [<span data-ttu-id="b5333-115">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b5333-115">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="b5333-116">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="b5333-116">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="b5333-117">Azure Configuration Manager för .NET</span><span class="sxs-lookup"><span data-stu-id="b5333-117">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="b5333-118">Ett [Azure Storage-konto](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="b5333-118">An [Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="b5333-119">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="b5333-119">Add using directives</span></span>
<span data-ttu-id="b5333-120">Lägg till följande `using`-direktiv längst upp i filen `Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="b5333-120">Add the following `using` directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="b5333-121">Parsa anslutningssträngen</span><span class="sxs-lookup"><span data-stu-id="b5333-121">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-queue-service-client"></a><span data-ttu-id="b5333-122">Skapa Queue-tjänstklienten</span><span class="sxs-lookup"><span data-stu-id="b5333-122">Create the Queue service client</span></span>
<span data-ttu-id="b5333-123">Med hjälp av **CloudQueueClient**-klassen kan du hämta köer som lagras i Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="b5333-123">The **CloudQueueClient** class enables you to retrieve queues stored in Queue storage.</span></span> <span data-ttu-id="b5333-124">Här är ett sätt att skapa tjänstklienten:</span><span class="sxs-lookup"><span data-stu-id="b5333-124">Here's one way to create the service client:</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
<span data-ttu-id="b5333-125">Nu är det dags att skriva kod som läser data från och skriver data till Queue Storage.</span><span class="sxs-lookup"><span data-stu-id="b5333-125">Now you are ready to write code that reads data from and writes data to Queue storage.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="b5333-126">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="b5333-126">Create a queue</span></span>
<span data-ttu-id="b5333-127">Det här exemplet illustrerar hur du skapar en kö om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="b5333-127">This example shows how to create a queue if it does not already exist:</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a container.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist
queue.CreateIfNotExists();
```

## <a name="insert-a-message-into-a-queue"></a><span data-ttu-id="b5333-128">Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="b5333-128">Insert a message into a queue</span></span>
<span data-ttu-id="b5333-129">Om du vill infoga ett meddelande i en befintlig kö börjar du med att skapa ett nytt **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="b5333-129">To insert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="b5333-130">Därefter anropar du **AddMessage**-metoden.</span><span class="sxs-lookup"><span data-stu-id="b5333-130">Next, call the **AddMessage** method.</span></span> <span data-ttu-id="b5333-131">Du kan skapa ett **CloudQueueMessage** antingen från en sträng (i UTF-8-format) eller från en **byte**-matris.</span><span class="sxs-lookup"><span data-stu-id="b5333-131">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a **byte** array.</span></span> <span data-ttu-id="b5333-132">Här är kod som skapar en kö (om den inte finns) och som infogar meddelandet ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="b5333-132">Here is code which creates a queue (if it doesn't exist) and inserts the message 'Hello, World':</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Create the queue if it doesn't already exist.
queue.CreateIfNotExists();

// Create a message and add it to the queue.
CloudQueueMessage message = new CloudQueueMessage("Hello, World");
queue.AddMessage(message);
```

## <a name="peek-at-the-next-message"></a><span data-ttu-id="b5333-133">En titt på nästa meddelande</span><span class="sxs-lookup"><span data-stu-id="b5333-133">Peek at the next message</span></span>
<span data-ttu-id="b5333-134">Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa metoden **PeekMessage**.</span><span class="sxs-lookup"><span data-stu-id="b5333-134">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessage** method.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Peek at the next message
CloudQueueMessage peekedMessage = queue.PeekMessage();

// Display message.
Console.WriteLine(peekedMessage.AsString);
```

## <a name="change-the-contents-of-a-queued-message"></a><span data-ttu-id="b5333-135">Ändra innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="b5333-135">Change the contents of a queued message</span></span>
<span data-ttu-id="b5333-136">Du kan ändra innehållet i ett meddelande direkt i kön.</span><span class="sxs-lookup"><span data-stu-id="b5333-136">You can change the contents of a message in-place in the queue.</span></span> <span data-ttu-id="b5333-137">Om meddelandet representerar en arbetsuppgift kan du använda den här funktionen för att uppdatera arbetsuppgiftens status.</span><span class="sxs-lookup"><span data-stu-id="b5333-137">If the message represents a work task, you could use this feature to update the status of the work task.</span></span> <span data-ttu-id="b5333-138">Följande kod uppdaterar kömeddelandet med nytt innehåll och utökar tidsgränsen för visning med ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b5333-138">The following code updates the queue message with new contents, and sets the visibility timeout to extend another 60 seconds.</span></span> <span data-ttu-id="b5333-139">Koden sparar statusen för arbetsuppgiften som associeras med meddelandet och ger klienten ytterligare en minut att fortsätta arbeta med meddelandet.</span><span class="sxs-lookup"><span data-stu-id="b5333-139">This saves the state of work associated with the message, and gives the client another minute to continue working on the message.</span></span> <span data-ttu-id="b5333-140">Du kan använda den här tekniken för att spåra arbetsflöden med flera steg i kömeddelanden, utan att behöva börja om från början om ett bearbetningssteg misslyckas på grund av maskin- eller programvarufel.</span><span class="sxs-lookup"><span data-stu-id="b5333-140">You could use this technique to track multi-step workflows on queue messages, without having to start over from the beginning if a processing step fails due to hardware or software failure.</span></span> <span data-ttu-id="b5333-141">Normalt räknar du även antalet omförsök och tar bort meddelandet om fler än *n* försök misslyckas.</span><span class="sxs-lookup"><span data-stu-id="b5333-141">Typically, you would keep a retry count as well, and if the message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="b5333-142">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="b5333-142">This protects against a message that triggers an application error each time it is processed.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the message from the queue and update the message contents.
CloudQueueMessage message = queue.GetMessage();
message.SetMessageContent("Updated contents.");
queue.UpdateMessage(message,
    TimeSpan.FromSeconds(60.0),  // Make it invisible for another 60 seconds.
    MessageUpdateFields.Content | MessageUpdateFields.Visibility);
```

## <a name="de-queue-the-next-message"></a><span data-ttu-id="b5333-143">Ta bort nästa meddelande från kön</span><span class="sxs-lookup"><span data-stu-id="b5333-143">De-queue the next message</span></span>
<span data-ttu-id="b5333-144">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="b5333-144">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="b5333-145">När du anropar **GetMessage** hämtas nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="b5333-145">When you call **GetMessage**, you get the next message in a queue.</span></span> <span data-ttu-id="b5333-146">Ett meddelande som returneras från **GetMessage** blir osynligt för andra meddelanden som läser kod i den här kön.</span><span class="sxs-lookup"><span data-stu-id="b5333-146">A message returned from **GetMessage** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="b5333-147">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="b5333-147">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="b5333-148">För att slutföra borttagningen av meddelandet från kön måste du även anropa **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="b5333-148">To finish removing the message from the queue, you must also call **DeleteMessage**.</span></span> <span data-ttu-id="b5333-149">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="b5333-149">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="b5333-150">Koden anropar **DeleteMessage** direkt efter att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="b5333-150">Your code calls **DeleteMessage** right after the message has been processed.</span></span>

```csharp
// Retrieve storage account from connection string
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Get the next message
CloudQueueMessage retrievedMessage = queue.GetMessage();

//Process the message in less than 30 seconds, and then delete the message
queue.DeleteMessage(retrievedMessage);
```

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a><span data-ttu-id="b5333-151">Använda Async-Await-mönstret med vanliga Queue Storage-API:er</span><span class="sxs-lookup"><span data-stu-id="b5333-151">Use Async-Await pattern with common Queue storage APIs</span></span>
<span data-ttu-id="b5333-152">Det här exemplet illustrerar hur du använder Async-Await-mönstret med vanliga Queue Storage-API:er.</span><span class="sxs-lookup"><span data-stu-id="b5333-152">This example shows how to use the Async-Await pattern with common Queue storage APIs.</span></span> <span data-ttu-id="b5333-153">Exemplet anropar den asynkrona versionen av var och en av de angivna metoderna, vilket du ser på *Async*-suffixet för varje metod.</span><span class="sxs-lookup"><span data-stu-id="b5333-153">The sample calls the asynchronous version of each of the given methods, as indicated by the *Async* suffix of each method.</span></span> <span data-ttu-id="b5333-154">När en async-metod används pausar async-await-mönstret den lokala körningen tills anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b5333-154">When an async method is used, the async-await pattern suspends local execution until the call completes.</span></span> <span data-ttu-id="b5333-155">Detta gör att den aktuella tråden kan arbeta med annat, vilket innebär att flaskhalsar kan undvikas samtidigt som programmets svarstider förbättras.</span><span class="sxs-lookup"><span data-stu-id="b5333-155">This behavior allows the current thread to do other work, which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="b5333-156">Mer information om hur du använder Async-Await-mönstret i .NET finns i [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="b5333-156">For more details on using the Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

```csharp
// Create the queue if it doesn't already exist
if(await queue.CreateIfNotExistsAsync())
{
    Console.WriteLine("Queue '{0}' Created", queue.Name);
}
else
{
    Console.WriteLine("Queue '{0}' Exists", queue.Name);
}

// Create a message to put in the queue
CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

// Async enqueue the message
await queue.AddMessageAsync(cloudQueueMessage);
Console.WriteLine("Message added");

// Async dequeue the message
CloudQueueMessage retrievedMessage = await queue.GetMessageAsync();
Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

// Async delete the message
await queue.DeleteMessageAsync(retrievedMessage);
Console.WriteLine("Deleted message");
```
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="b5333-157">Använda fler alternativ för att hämta meddelanden ur kön</span><span class="sxs-lookup"><span data-stu-id="b5333-157">Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="b5333-158">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="b5333-158">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="b5333-159">För det första kan du hämta en grupp med meddelanden (upp till 32).</span><span class="sxs-lookup"><span data-stu-id="b5333-159">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="b5333-160">För det andra kan du ange en längre eller kortare tidsgräns för osynlighet för att ge koden mer eller mindre tid att bearbeta klart varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="b5333-160">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="b5333-161">I följande kodexempel används metoden **GetMessage** för att hämta 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="b5333-161">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="b5333-162">Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop.</span><span class="sxs-lookup"><span data-stu-id="b5333-162">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="b5333-163">Koden ställer också in tidsgränsen för osynlighet till fem minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="b5333-163">It also sets the invisibility timeout to five minutes for each message.</span></span> <span data-ttu-id="b5333-164">Observera att de fem minuterna startar för alla meddelanden samtidigt, vilket betyder att när det här har gått fem minuter sedan anropet till **GetMessage** så blir alla meddelanden som inte har tagits bort synliga igen.</span><span class="sxs-lookup"><span data-stu-id="b5333-164">Note that the 5 minutes starts for all messages at the same time, so after 5 minutes have passed since the call to **GetMessages**, any messages which have not been deleted will become visible again.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

foreach (CloudQueueMessage message in queue.GetMessages(20, TimeSpan.FromMinutes(5)))
{
    // Process all messages in less than 5 minutes, deleting each message after processing.
    queue.DeleteMessage(message);
}
```

## <a name="get-the-queue-length"></a><span data-ttu-id="b5333-165">Hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="b5333-165">Get the queue length</span></span>
<span data-ttu-id="b5333-166">Du kan hämta en uppskattning av antalet meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="b5333-166">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="b5333-167">**FetchAttributes**-metoden ber kötjänsten att hämta köattributen, inklusive antalet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b5333-167">The **FetchAttributes** method asks the Queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="b5333-168">Egenskapen **ApproximateMessageCount** returnerar det sista värdet som hämtas av **FetchAttributes**-metoden, utan att kötjänsten anropas.</span><span class="sxs-lookup"><span data-stu-id="b5333-168">The **ApproximateMessageCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the Queue service.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Fetch the queue attributes.
queue.FetchAttributes();

// Retrieve the cached approximate message count.
int? cachedMessageCount = queue.ApproximateMessageCount;

// Display number of messages.
Console.WriteLine("Number of messages in queue: " + cachedMessageCount);
```

## <a name="delete-a-queue"></a><span data-ttu-id="b5333-169">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="b5333-169">Delete a queue</span></span>
<span data-ttu-id="b5333-170">Om du vill ta bort en kö och alla meddelanden i den anropar du **Delete**-metoden för köobjektet.</span><span class="sxs-lookup"><span data-stu-id="b5333-170">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the queue client.
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();

// Retrieve a reference to a queue.
CloudQueue queue = queueClient.GetQueueReference("myqueue");

// Delete the queue.
queue.Delete();
```
    

## <a name="next-steps"></a><span data-ttu-id="b5333-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5333-171">Next steps</span></span>
<span data-ttu-id="b5333-172">Nu när du har lärt dig grunderna i Queue Storage kan du följa dessa länkar för att lära dig mer om komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5333-172">Now that you've learned the basics of Queue storage, follow these links to learn about more complex storage tasks.</span></span>

* <span data-ttu-id="b5333-173">Fullständig information om tillgängliga API:er finns i referensdokumentationen för kötjänsten:</span><span class="sxs-lookup"><span data-stu-id="b5333-173">View the Queue service reference documentation for complete details about available APIs:</span></span>
  * [<span data-ttu-id="b5333-174">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="b5333-174">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [<span data-ttu-id="b5333-175">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="b5333-175">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="b5333-176">Lär dig hur du förenklar koden du skriver så att den fungerar med Azure Storage genom att använda [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b5333-176">Learn how to simplify the code you write to work with Azure Storage by using the [Azure WebJobs SDK](../../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span>
* <span data-ttu-id="b5333-177">Visa fler funktionsguider och lär dig mer om andra alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="b5333-177">View more feature guides to learn about additional options for storing data in Azure.</span></span>
  * <span data-ttu-id="b5333-178">[Kom igång med Azure Table Storage med hjälp av .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) om du vill lagra strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="b5333-178">[Get started with Azure Table storage using .NET](../../cosmos-db/table-storage-how-to-use-dotnet.md) to store structured data.</span></span>
  * <span data-ttu-id="b5333-179">[Kom igång med Azure Blob Storage med hjälp av .NET](../blobs/storage-dotnet-how-to-use-blobs.md) om du vill lagra ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="b5333-179">[Get started with Azure Blob storage using .NET](../blobs/storage-dotnet-how-to-use-blobs.md) to store unstructured data.</span></span>
  * <span data-ttu-id="b5333-180">[Anslut till SQL Database med hjälp av .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md) för att lagra relationsdata.</span><span class="sxs-lookup"><span data-stu-id="b5333-180">[Connect to SQL Database by using .NET (C#)](../../sql-database/sql-database-connect-query-dotnet-core.md) to store relational data.</span></span>

[Download and install the Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
