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
ms.openlocfilehash: 36bbb40189a301cddbc2ded92d0623fa5e093eb1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-queue-storage-using-net"></a><span data-ttu-id="60327-104">Komma igång med Azure Queue Storage med hjälp av .NET</span><span class="sxs-lookup"><span data-stu-id="60327-104">Get started with Azure Queue storage using .NET</span></span>
[!INCLUDE [storage-selector-queue-include](../../includes/storage-selector-queue-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../includes/storage-check-out-samples-dotnet.md)]

## <a name="overview"></a><span data-ttu-id="60327-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="60327-105">Overview</span></span>
<span data-ttu-id="60327-106">Azure Queue Storage innehåller molnmeddelandehantering mellan programkomponenter.</span><span class="sxs-lookup"><span data-stu-id="60327-106">Azure Queue storage provides cloud messaging between application components.</span></span> <span data-ttu-id="60327-107">När program utformas för skalning är programkomponenterna ofta fristående, så att de kan skalas oberoende av varandra.</span><span class="sxs-lookup"><span data-stu-id="60327-107">In designing applications for scale, application components are often decoupled, so that they can scale independently.</span></span> <span data-ttu-id="60327-108">Queue storage ger asynkrona meddelanden för kommunikation mellan programkomponenter, oavsett om de körs i hello molnet på hello skrivbordet, på en lokal server eller på en mobil enhet.</span><span class="sxs-lookup"><span data-stu-id="60327-108">Queue storage delivers asynchronous messaging for communication between application components, whether they are running in hello cloud, on hello desktop, on an on-premises server, or on a mobile device.</span></span> <span data-ttu-id="60327-109">Queue Storage har också stöd för hantering av asynkrona åtgärder och utveckling av processarbetsflöden.</span><span class="sxs-lookup"><span data-stu-id="60327-109">Queue storage also supports managing asynchronous tasks and building process work flows.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="60327-110">Om den här självstudiekursen</span><span class="sxs-lookup"><span data-stu-id="60327-110">About this tutorial</span></span>
<span data-ttu-id="60327-111">Den här kursen visar hur toowrite .NET kod för några vanliga scenarier med hjälp av Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="60327-111">This tutorial shows how toowrite .NET code for some common scenarios using Azure Queue storage.</span></span> <span data-ttu-id="60327-112">Du lär dig bland annat hur du skapar och tar bort köer och hur du lägger till, läser och tar bort kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="60327-112">Scenarios covered include creating and deleting queues and adding, reading, and deleting queue messages.</span></span>

<span data-ttu-id="60327-113">**Uppskattad tid toocomplete:** 45 minuter</span><span class="sxs-lookup"><span data-stu-id="60327-113">**Estimated time toocomplete:** 45 minutes</span></span>

<span data-ttu-id="60327-114">**Förhandskrav:**</span><span class="sxs-lookup"><span data-stu-id="60327-114">**Prerequisities:**</span></span>

* [<span data-ttu-id="60327-115">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="60327-115">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="60327-116">Azure Storage-klientbibliotek för .NET</span><span class="sxs-lookup"><span data-stu-id="60327-116">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="60327-117">Azure Configuration Manager för .NET</span><span class="sxs-lookup"><span data-stu-id="60327-117">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="60327-118">Ett [Azure Storage-konto](storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="60327-118">An [Azure storage account](storage-create-storage-account.md#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

[!INCLUDE [storage-queue-concepts-include](../../includes/storage-queue-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="60327-119">Lägga till med hjälp av direktiv</span><span class="sxs-lookup"><span data-stu-id="60327-119">Add using directives</span></span>
<span data-ttu-id="60327-120">Lägg till följande hello `using` direktiven toohello överkant hello `Program.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="60327-120">Add hello following `using` directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Queue; // Namespace for Queue storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="60327-121">Parsa anslutningssträngen för hello</span><span class="sxs-lookup"><span data-stu-id="60327-121">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-queue-service-client"></a><span data-ttu-id="60327-122">Skapa hello Queue-tjänstklienten</span><span class="sxs-lookup"><span data-stu-id="60327-122">Create hello Queue service client</span></span>
<span data-ttu-id="60327-123">Hej **CloudQueueClient** klassen kan du tooretrieve köer som lagras i Queue storage.</span><span class="sxs-lookup"><span data-stu-id="60327-123">hello **CloudQueueClient** class enables you tooretrieve queues stored in Queue storage.</span></span> <span data-ttu-id="60327-124">Här är ett sätt toocreate hello-klienten:</span><span class="sxs-lookup"><span data-stu-id="60327-124">Here's one way toocreate hello service client:</span></span>

```csharp
CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
```
    
<span data-ttu-id="60327-125">Nu är du redo toowrite kod som läser data från och skriver tooQueue datalagring.</span><span class="sxs-lookup"><span data-stu-id="60327-125">Now you are ready toowrite code that reads data from and writes data tooQueue storage.</span></span>

## <a name="create-a-queue"></a><span data-ttu-id="60327-126">Skapa en kö</span><span class="sxs-lookup"><span data-stu-id="60327-126">Create a queue</span></span>
<span data-ttu-id="60327-127">Det här exemplet illustrerar hur toocreate en kö om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="60327-127">This example shows how toocreate a queue if it does not already exist:</span></span>

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

## <a name="insert-a-message-into-a-queue"></a><span data-ttu-id="60327-128">Infoga ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="60327-128">Insert a message into a queue</span></span>
<span data-ttu-id="60327-129">tooinsert ett meddelande i en befintlig kö först skapa en ny **CloudQueueMessage**.</span><span class="sxs-lookup"><span data-stu-id="60327-129">tooinsert a message into an existing queue, first create a new **CloudQueueMessage**.</span></span> <span data-ttu-id="60327-130">Därefter anropar hello **AddMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="60327-130">Next, call hello **AddMessage** method.</span></span> <span data-ttu-id="60327-131">Du kan skapa ett **CloudQueueMessage** antingen från en sträng (i UTF-8-format) eller från en **byte**-matris.</span><span class="sxs-lookup"><span data-stu-id="60327-131">A **CloudQueueMessage** can be created from either a string (in UTF-8 format) or a **byte** array.</span></span> <span data-ttu-id="60327-132">Här är kod som skapar en kö (om den inte finns) och infogningar hello-meddelande ”Hello World”:</span><span class="sxs-lookup"><span data-stu-id="60327-132">Here is code which creates a queue (if it doesn't exist) and inserts hello message 'Hello, World':</span></span>

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

## <a name="peek-at-hello-next-message"></a><span data-ttu-id="60327-133">Granska nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="60327-133">Peek at hello next message</span></span>
<span data-ttu-id="60327-134">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="60327-134">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

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

## <a name="change-hello-contents-of-a-queued-message"></a><span data-ttu-id="60327-135">Ändra hello innehållet i ett meddelande i kön</span><span class="sxs-lookup"><span data-stu-id="60327-135">Change hello contents of a queued message</span></span>
<span data-ttu-id="60327-136">Du kan ändra hello innehållet i ett meddelande direkt i hello kö.</span><span class="sxs-lookup"><span data-stu-id="60327-136">You can change hello contents of a message in-place in hello queue.</span></span> <span data-ttu-id="60327-137">Om meddelandet representerar en arbetsuppgift kan använda du den här funktionen tooupdate statusen för arbetsuppgiften som hello.</span><span class="sxs-lookup"><span data-stu-id="60327-137">If the message represents a work task, you could use this feature tooupdate the status of hello work task.</span></span> <span data-ttu-id="60327-138">hello följande kod uppdaterar hello kömeddelandet med nytt innehåll och anger hello synlighet timeout tooextend ytterligare 60 sekunder.</span><span class="sxs-lookup"><span data-stu-id="60327-138">hello following code updates hello queue message with new contents, and sets hello visibility timeout tooextend another 60 seconds.</span></span> <span data-ttu-id="60327-139">Detta sparar hello statusen för arbetsuppgiften som associeras med hello-meddelande och ger hello klienten en annan minut toocontinue som arbetar på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="60327-139">This saves hello state of work associated with hello message, and gives hello client another minute toocontinue working on hello message.</span></span> <span data-ttu-id="60327-140">Du kan använda den här tekniken tootrack arbetsflöden med flera steg i Kömeddelanden, utan att behöva toostart över från hello början om ett bearbetningssteg misslyckas på grund av toohardware eller ett programvarufel.</span><span class="sxs-lookup"><span data-stu-id="60327-140">You could use this technique tootrack multi-step workflows on queue messages, without having toostart over from hello beginning if a processing step fails due toohardware or software failure.</span></span> <span data-ttu-id="60327-141">Normalt ska du behålla ett återförsöksvärde och om hello meddelandet försöks mer än  *n*  tider, tar du bort den.</span><span class="sxs-lookup"><span data-stu-id="60327-141">Typically, you would keep a retry count as well, and if hello message is retried more than *n* times, you would delete it.</span></span> <span data-ttu-id="60327-142">Detta skyddar mot meddelanden som utlöser ett programfel varje gång de bearbetas.</span><span class="sxs-lookup"><span data-stu-id="60327-142">This protects against a message that triggers an application error each time it is processed.</span></span>

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

## <a name="de-queue-hello-next-message"></a><span data-ttu-id="60327-143">Frigör kö nästa hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="60327-143">De-queue hello next message</span></span>
<span data-ttu-id="60327-144">Koden tar bort ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="60327-144">Your code de-queues a message from a queue in two steps.</span></span> <span data-ttu-id="60327-145">När du anropar **GetMessage**, du får hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="60327-145">When you call **GetMessage**, you get hello next message in a queue.</span></span> <span data-ttu-id="60327-146">Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="60327-146">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="60327-147">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="60327-147">By default, this message stays invisible for 30 seconds.</span></span> <span data-ttu-id="60327-148">toofinish att ta bort hello-meddelande från kön hello, måste du också anropa **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="60327-148">toofinish removing hello message from hello queue, you must also call **DeleteMessage**.</span></span> <span data-ttu-id="60327-149">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="60327-149">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="60327-150">Koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="60327-150">Your code calls **DeleteMessage** right after hello message has been processed.</span></span>

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

## <a name="use-async-await-pattern-with-common-queue-storage-apis"></a><span data-ttu-id="60327-151">Använda Async-Await-mönstret med vanliga Queue Storage-API:er</span><span class="sxs-lookup"><span data-stu-id="60327-151">Use Async-Await pattern with common Queue storage APIs</span></span>
<span data-ttu-id="60327-152">Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga Queue storage-API: er.</span><span class="sxs-lookup"><span data-stu-id="60327-152">This example shows how toouse hello Async-Await pattern with common Queue storage APIs.</span></span> <span data-ttu-id="60327-153">hello exemplet anropar hello asynkrona versionen av var och en av hello anges metoder som anges av hello *asynkrona* suffixet för varje metod.</span><span class="sxs-lookup"><span data-stu-id="60327-153">hello sample calls hello asynchronous version of each of hello given methods, as indicated by hello *Async* suffix of each method.</span></span> <span data-ttu-id="60327-154">När en asynkron metod används hello async-await mönster pausar lokala körningen tills hello anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="60327-154">When an async method is used, hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="60327-155">Det här innebär att hello aktuella tråden toodo andra arbetet, vilket hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet.</span><span class="sxs-lookup"><span data-stu-id="60327-155">This behavior allows hello current thread toodo other work, which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="60327-156">Mer information om hur du använder hello Async-Await-mönstret i .NET finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="60327-156">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

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
    
## <a name="leverage-additional-options-for-de-queuing-messages"></a><span data-ttu-id="60327-157">Använda fler alternativ för att hämta meddelanden ur kön</span><span class="sxs-lookup"><span data-stu-id="60327-157">Leverage additional options for de-queuing messages</span></span>
<span data-ttu-id="60327-158">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="60327-158">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="60327-159">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="60327-159">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="60327-160">Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="60327-160">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="60327-161">hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="60327-161">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="60327-162">Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop.</span><span class="sxs-lookup"><span data-stu-id="60327-162">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="60327-163">Den anger också hello osynlighet timeout toofive minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="60327-163">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="60327-164">Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**GetMessage**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="60327-164">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

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

## <a name="get-hello-queue-length"></a><span data-ttu-id="60327-165">Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="60327-165">Get hello queue length</span></span>
<span data-ttu-id="60327-166">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="60327-166">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="60327-167">Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="60327-167">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="60327-168">Hej **ApproximateMessageCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="60327-168">hello **ApproximateMessageCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="60327-169">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="60327-169">Delete a queue</span></span>
<span data-ttu-id="60327-170">toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort** metoden för köobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="60327-170">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

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
    

## <a name="next-steps"></a><span data-ttu-id="60327-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60327-171">Next steps</span></span>
<span data-ttu-id="60327-172">Nu när du har lärt dig hello grunderna i Queue storage kan du följa dessa länkar toolearn om mer komplexa lagringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="60327-172">Now that you've learned hello basics of Queue storage, follow these links toolearn about more complex storage tasks.</span></span>

* <span data-ttu-id="60327-173">Visa hello kön referensdokumentationen för kötjänsten fullständig information om tillgängliga API: er:</span><span class="sxs-lookup"><span data-stu-id="60327-173">View hello Queue service reference documentation for complete details about available APIs:</span></span>
  * [<span data-ttu-id="60327-174">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="60327-174">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
  * [<span data-ttu-id="60327-175">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="60327-175">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="60327-176">Lär dig hur toosimplify hello koden du skriver toowork med Azure Storage med hjälp av hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="60327-176">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md).</span></span>
* <span data-ttu-id="60327-177">Visa mer funktionen guider toolearn om ytterligare alternativ för att lagra data i Azure.</span><span class="sxs-lookup"><span data-stu-id="60327-177">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
  * <span data-ttu-id="60327-178">[Komma igång med Azure Table storage med hjälp av .NET](storage-dotnet-how-to-use-tables.md) toostore strukturerade data.</span><span class="sxs-lookup"><span data-stu-id="60327-178">[Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) toostore structured data.</span></span>
  * <span data-ttu-id="60327-179">[Komma igång med Azure Blob storage med hjälp av .NET](storage-dotnet-how-to-use-blobs.md) toostore Ostrukturerade data.</span><span class="sxs-lookup"><span data-stu-id="60327-179">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
  * <span data-ttu-id="60327-180">[Ansluta tooSQL databasen med hjälp av .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relationella data.</span><span class="sxs-lookup"><span data-stu-id="60327-180">[Connect tooSQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relational data.</span></span>

[Download and install hello Azure SDK for .NET]: /develop/net/
[.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
[Creating a Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[OData]: http://nuget.org/packages/Microsoft.Data.OData/5.0.2
[Edm]: http://nuget.org/packages/Microsoft.Data.Edm/5.0.2
[Spatial]: http://nuget.org/packages/System.Spatial/5.0.2
