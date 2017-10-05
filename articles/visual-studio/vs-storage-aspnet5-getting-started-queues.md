---
title: "Kom igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur du kommer igång med Azure queue storage i ASP.NET Core-projekt i Visual Studio"
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
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="88d9f-103">Kom igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET kärnor)</span><span class="sxs-lookup"><span data-stu-id="88d9f-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="88d9f-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="88d9f-104">Overview</span></span>
<span data-ttu-id="88d9f-105">Den här artikeln beskriver hur du kommer igång med Azure Queue storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av Visual Studio **Lägg till anslutna tjänster** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="88d9f-105">This article describes how to get started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="88d9f-106">Den **Lägg till anslutna tjänster** åtgärden installerar lämpligt NuGet-paket för att komma åt Azure-lagring i ditt projekt och lägger till anslutningssträngen för lagringskontot konfigurationsfilerna projektet.</span><span class="sxs-lookup"><span data-stu-id="88d9f-106">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="88d9f-107">Azure queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="88d9f-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in the world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="88d9f-108">Ett enda kömeddelande kan vara upp till 64 kilobyte (KB) och en kö kan innehålla miljontals meddelanden, upp till den totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="88d9f-108">A single queue message can be up to 64 kilobytes (KB) in size, and a queue can contain millions of messages, up to the total capacity limit of a storage account.</span></span>

<span data-ttu-id="88d9f-109">Om du vill komma igång, måste du först skapa en Azure-kö på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="88d9f-109">To get started, you first need to create an Azure queue in your storage account.</span></span> <span data-ttu-id="88d9f-110">Vi ska visa dig hur du skapar en kö i koden.</span><span class="sxs-lookup"><span data-stu-id="88d9f-110">We'll show you how to create a queue in code.</span></span> <span data-ttu-id="88d9f-111">Vi också lära dig hur du utför grundläggande kön åtgärder, till exempel att lägga till, ändra, läsa och tar bort Kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="88d9f-111">We'll also show you how to perform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="88d9f-112">Exemplen är skrivna i C\# code och använda Azure Storage-klientbiblioteket för .NET.</span><span class="sxs-lookup"><span data-stu-id="88d9f-112">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="88d9f-113">Läs mer om ASP.NET [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="88d9f-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="88d9f-114">**Obs:** några av de API: er som utför anrop till Azure-lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="88d9f-114">**NOTE:** Some of the APIs that perform calls to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="88d9f-115">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="88d9f-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="88d9f-116">Koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="88d9f-116">The code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="88d9f-117">Se [komma igång med Azure Queue storage med hjälp av .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) mer information om programmässigt manipulering köer.</span><span class="sxs-lookup"><span data-stu-id="88d9f-117">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="88d9f-118">Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="88d9f-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="88d9f-119">Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="88d9f-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="88d9f-120">Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="88d9f-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="88d9f-121">Ha åtkomst till serviceköer i koden</span><span class="sxs-lookup"><span data-stu-id="88d9f-121">Access queues in code</span></span>
<span data-ttu-id="88d9f-122">Om du vill komma åt köer i ASP.NET Core projekt måste du inkludera följande objekt till en C# som har åtkomst till Azure queue storage.</span><span class="sxs-lookup"><span data-stu-id="88d9f-122">To access queues in ASP.NET Core projects, you need to include the following items to any C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="88d9f-123">Se till att namnrymdsdeklarationer överst i C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="88d9f-123">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="88d9f-124">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="88d9f-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="88d9f-125">Använd följande kod för att få den dina anslutningssträngen för lagring och information om lagringskonto från Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="88d9f-125">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="88d9f-126">Hämta en **CloudQueueClient** objektet att referera till köobjekt i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="88d9f-126">Get a **CloudQueueClient** object to reference the queue objects in your storage account.</span></span>  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="88d9f-127">Hämta en **CloudQueue** objektet att referera till en särskild kö.</span><span class="sxs-lookup"><span data-stu-id="88d9f-127">Get a **CloudQueue** object to reference a specific queue.</span></span>
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="88d9f-128">**Obs:** använda alla koden ovan framför koden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="88d9f-128">**NOTE:** Use all of the above code in front of the code in the following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="88d9f-129">Skapa en kö i koden</span><span class="sxs-lookup"><span data-stu-id="88d9f-129">Create a queue in code</span></span>
<span data-ttu-id="88d9f-130">Om du vill skapa Azure kön i koden, Lägg till ett anrop till **CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="88d9f-130">To create the Azure queue in code, just add a call to **CreateIfNotExistsAsync**.</span></span>

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a><span data-ttu-id="88d9f-131">Lägga till ett meddelande till en kö</span><span class="sxs-lookup"><span data-stu-id="88d9f-131">Add a message to a queue</span></span>
<span data-ttu-id="88d9f-132">Om du vill infoga ett meddelande i en befintlig kö, skapa en ny **CloudQueueMessage** objekt och sedan anropa den **AddMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="88d9f-132">To insert a message into an existing queue, create a new **CloudQueueMessage** object, then call the **AddMessageAsync** method.</span></span>

<span data-ttu-id="88d9f-133">En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="88d9f-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="88d9f-134">Här är ett exempel som infogar meddelandet ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="88d9f-134">Here is an example which inserts the message 'Hello, World'.</span></span>

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="88d9f-135">Läs ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="88d9f-135">Read a message in a queue</span></span>
<span data-ttu-id="88d9f-136">Du kan kika på meddelandet först i en kö utan att ta bort det från kön genom att anropa den **PeekMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="88d9f-136">You can peek at the message in the front of a queue without removing it from the queue by calling the **PeekMessageAsync** method.</span></span>

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="88d9f-137">Läsa och ta bort ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="88d9f-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="88d9f-138">Koden kan ta bort (status Created) ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="88d9f-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="88d9f-139">Anropa **GetMessageAsync** att hämta nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="88d9f-139">Call **GetMessageAsync** to get the next message in a queue.</span></span> <span data-ttu-id="88d9f-140">Ett meddelande som returneras från **GetMessageAsync** blir osynligt för andra koder som läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="88d9f-140">A message returned from **GetMessageAsync** becomes invisible to any other code reading messages from this queue.</span></span> <span data-ttu-id="88d9f-141">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="88d9f-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="88d9f-142">Om du vill slutföra borttagningen av meddelandet från kön, anropa **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="88d9f-142">To finish removing the message from the queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="88d9f-143">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer att om din kod inte kan bearbeta ett meddelande på grund av ett maskin- eller programvarufel så kan en annan instans av koden hämta samma meddelande och försöka igen.</span><span class="sxs-lookup"><span data-stu-id="88d9f-143">This two-step process of removing a message assures that if your code fails to process a message due to hardware or software failure, another instance of your code can get the same message and try again.</span></span> <span data-ttu-id="88d9f-144">Följande koden anropar **DeleteMessageAsync** direkt efter att meddelandet har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="88d9f-144">The following code calls **DeleteMessageAsync** right after the message has been processed.</span></span>

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="88d9f-145">Använda fler alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="88d9f-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="88d9f-146">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="88d9f-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="88d9f-147">För det första kan du hämta en grupp med meddelanden (upp till 32).</span><span class="sxs-lookup"><span data-stu-id="88d9f-147">First, you can get a batch of messages (up to 32).</span></span> <span data-ttu-id="88d9f-148">För det andra kan du ange en längre eller kortare tidsgräns för osynlighet för att ge koden mer eller mindre tid att bearbeta klart varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="88d9f-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time to fully process each message.</span></span> <span data-ttu-id="88d9f-149">I följande kodexempel används metoden **GetMessage** för att hämta 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="88d9f-149">The following code example uses the **GetMessages** method to get 20 messages in one call.</span></span> <span data-ttu-id="88d9f-150">Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop.</span><span class="sxs-lookup"><span data-stu-id="88d9f-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="88d9f-151">Den anger också tidsgränsen för osynlighet till 5 minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="88d9f-151">It also sets the invisibility timeout to 5 minutes for each message.</span></span> <span data-ttu-id="88d9f-152">Observera att 5 minuter startar för alla meddelanden samtidigt, efter 5 minuter har gått efter anropet till **GetMessage**, alla meddelanden som inte har tagits bort blir synliga igen.</span><span class="sxs-lookup"><span data-stu-id="88d9f-152">Note that the 5 minutes start for all messages at the same time, so after 5 minutes have passed after the call to **GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a><span data-ttu-id="88d9f-153">Hämta kölängden</span><span class="sxs-lookup"><span data-stu-id="88d9f-153">Get the queue length</span></span>
<span data-ttu-id="88d9f-154">Du kan hämta en uppskattning av antalet meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="88d9f-154">You can get an estimate of the number of messages in a queue.</span></span> <span data-ttu-id="88d9f-155">Den **FetchAttributes** metoden ber kötjänsten att hämta köattributen, inklusive antalet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="88d9f-155">The **FetchAttributes** method asks the queue service to retrieve the queue attributes, including the message count.</span></span> <span data-ttu-id="88d9f-156">Den **ApproximateMethodCount** egenskapen returnerar det sista värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten.</span><span class="sxs-lookup"><span data-stu-id="88d9f-156">The **ApproximateMethodCount** property returns the last value retrieved by the **FetchAttributes** method, without calling the queue service.</span></span>

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="88d9f-157">Använda Async-Await-mönstret med vanliga queue API: er</span><span class="sxs-lookup"><span data-stu-id="88d9f-157">Use the Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="88d9f-158">Det här exemplet visar hur du använder Async-Await-mönstret med vanliga queue API: er.</span><span class="sxs-lookup"><span data-stu-id="88d9f-158">This example shows how to use the Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="88d9f-159">Exemplet anropar den asynkron versionen av var och en av de angivna metoderna.</span><span class="sxs-lookup"><span data-stu-id="88d9f-159">The sample calls the async version of each of the given methods.</span></span> <span data-ttu-id="88d9f-160">Detta kan ses av asynkrona efter korrigering för varje metod.</span><span class="sxs-lookup"><span data-stu-id="88d9f-160">This can be seen by the Async post-fix of each method.</span></span> <span data-ttu-id="88d9f-161">När en asynkron metod används pausar Async-Await-mönstret lokala körningen tills anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="88d9f-161">When an async method is used, the Async-Await pattern suspends local execution until the call is completed.</span></span> <span data-ttu-id="88d9f-162">Det här innebär att den aktuella tråden utföra annat arbete som hjälper till att undvika flaskhalsar och förbättrar den övergripande svarstiden för programmet.</span><span class="sxs-lookup"><span data-stu-id="88d9f-162">This behavior allows the current thread to do other work which helps avoid performance bottlenecks and improves the overall responsiveness of your application.</span></span> <span data-ttu-id="88d9f-163">Mer information om hur du använder Async-Await-mönstret i .NET finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="88d9f-163">For more details on using the Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a><span data-ttu-id="88d9f-164">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="88d9f-164">Delete a queue</span></span>
<span data-ttu-id="88d9f-165">Om du vill ta bort en kö och alla meddelanden i den anropar du **Delete**-metoden för köobjektet.</span><span class="sxs-lookup"><span data-stu-id="88d9f-165">To delete a queue and all the messages contained in it, call the **Delete** method on the queue object.</span></span>

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="88d9f-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="88d9f-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

