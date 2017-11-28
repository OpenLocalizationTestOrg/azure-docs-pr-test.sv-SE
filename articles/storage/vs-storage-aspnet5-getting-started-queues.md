---
title: "aaaGet igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET Core) | Microsoft Docs"
description: "Hur tooget igång med Azure queue storage i ASP.NET Core-projekt i Visual Studio"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 75adb7163827ab17ad89707051ff0e48dbae9c3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="3e400-103">Kom igång med queue storage- och Visual Studio anslutna tjänster (ASP.NET kärnor)</span><span class="sxs-lookup"><span data-stu-id="3e400-103">Get started with queue storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="3e400-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="3e400-104">Overview</span></span>
<span data-ttu-id="3e400-105">Den här artikeln beskriver hur tooget igång med Azure Queue storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i ASP.NET Core-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="3e400-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="3e400-106">Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.</span><span class="sxs-lookup"><span data-stu-id="3e400-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="3e400-107">Azure queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3e400-107">Azure queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="3e400-108">Ett enda kömeddelande kan vara upp too64 kilobyte (KB) i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3e400-108">A single queue message can be up too64 kilobytes (KB) in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

<span data-ttu-id="3e400-109">tooget igång, måste du först toocreate en Azure-kö på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3e400-109">tooget started, you first need toocreate an Azure queue in your storage account.</span></span> <span data-ttu-id="3e400-110">Får du lära dig hur toocreate en kö i koden.</span><span class="sxs-lookup"><span data-stu-id="3e400-110">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="3e400-111">Vi också lära dig hur tooperform basic kö åtgärder, till exempel lägga till, ändra, läsa och tar bort Kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="3e400-111">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading, and removing queue messages.</span></span> <span data-ttu-id="3e400-112">hello prover är skrivna i C\# code och använda hello Azure Storage-klientbibliotek för .NET.</span><span class="sxs-lookup"><span data-stu-id="3e400-112">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span> <span data-ttu-id="3e400-113">Läs mer om ASP.NET [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="3e400-113">For more information about ASP.NET, see [ASP.NET](http://www.asp.net).</span></span>

<span data-ttu-id="3e400-114">**Obs:** vissa hello API: er som utför anrop tooAzure lagring i ASP.NET Core är asynkron.</span><span class="sxs-lookup"><span data-stu-id="3e400-114">**NOTE:** Some of hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="3e400-115">Se [asynkron programmering med Async och Await](http://msdn.microsoft.com/library/hh191443.aspx) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3e400-115">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="3e400-116">hello koden nedan förutsätter asynkrona programming metoder som används.</span><span class="sxs-lookup"><span data-stu-id="3e400-116">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="3e400-117">Se [komma igång med Azure Queue storage med hjälp av .NET](storage-dotnet-how-to-use-queues.md) mer information om programmässigt manipulering köer.</span><span class="sxs-lookup"><span data-stu-id="3e400-117">See [Get started with Azure Queue storage using .NET](storage-dotnet-how-to-use-queues.md) for more information on programmatically manipulating queues.</span></span>
* <span data-ttu-id="3e400-118">Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="3e400-118">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="3e400-119">Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="3e400-119">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="3e400-120">Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="3e400-120">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="3e400-121">Ha åtkomst till serviceköer i koden</span><span class="sxs-lookup"><span data-stu-id="3e400-121">Access queues in code</span></span>
<span data-ttu-id="3e400-122">tooaccess köer i ASP.NET Core projekt, behöver du tooinclude hello följande objekt tooany C#-källfilen som har åtkomst till Azure queue storage.</span><span class="sxs-lookup"><span data-stu-id="3e400-122">tooaccess queues in ASP.NET Core projects, you need tooinclude hello following items tooany C# source file that accesses Azure queue storage.</span></span>

1. <span data-ttu-id="3e400-123">Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="3e400-123">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="3e400-124">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="3e400-124">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="3e400-125">Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="3e400-125">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="3e400-126">Hämta en **CloudQueueClient** objekt tooreference hello köobjekt i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="3e400-126">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello CloudQueueClient object for hello storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="3e400-127">Hämta en **CloudQueue** objekt tooreference en särskild kö.</span><span class="sxs-lookup"><span data-stu-id="3e400-127">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference toohello CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="3e400-128">**Obs:** använda alla hello ovan kod framför hello koden i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="3e400-128">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

### <a name="create-a-queue-in-code"></a><span data-ttu-id="3e400-129">Skapa en kö i koden</span><span class="sxs-lookup"><span data-stu-id="3e400-129">Create a queue in code</span></span>
<span data-ttu-id="3e400-130">toocreate hello Azure kön i koden, Lägg till ett anrop för**CreateIfNotExistsAsync**.</span><span class="sxs-lookup"><span data-stu-id="3e400-130">toocreate hello Azure queue in code, just add a call too**CreateIfNotExistsAsync**.</span></span>

    // Create hello CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="3e400-131">Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="3e400-131">Add a message tooa queue</span></span>
<span data-ttu-id="3e400-132">tooinsert ett meddelande i en befintlig kö, skapa en ny **CloudQueueMessage** objektet och sedan anropa hello **AddMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="3e400-132">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessageAsync** method.</span></span>

<span data-ttu-id="3e400-133">En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="3e400-133">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="3e400-134">Här är ett exempel som infogar hello-meddelande ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="3e400-134">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="3e400-135">Läs ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="3e400-135">Read a message in a queue</span></span>
<span data-ttu-id="3e400-136">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessageAsync** metod.</span><span class="sxs-lookup"><span data-stu-id="3e400-136">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessageAsync** method.</span></span>

    // Peek hello next message in hello queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="3e400-137">Läsa och ta bort ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="3e400-137">Read and remove a message in a queue</span></span>
<span data-ttu-id="3e400-138">Koden kan ta bort (status Created) ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="3e400-138">Your code can remove (dequeue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="3e400-139">Anropa **GetMessageAsync** tooget hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="3e400-139">Call **GetMessageAsync** tooget hello next message in a queue.</span></span> <span data-ttu-id="3e400-140">Ett meddelande som returneras från **GetMessageAsync** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="3e400-140">A message returned from **GetMessageAsync** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="3e400-141">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="3e400-141">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="3e400-142">toofinish ta bort hello-meddelande från hello kö, anropet **DeleteMessageAsync**.</span><span class="sxs-lookup"><span data-stu-id="3e400-142">toofinish removing hello message from hello queue, call **DeleteMessageAsync**.</span></span>

<span data-ttu-id="3e400-143">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="3e400-143">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="3e400-144">hello följande koden anropar **DeleteMessageAsync** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="3e400-144">hello following code calls **DeleteMessageAsync** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process hello message in less than 30 seconds.

    // Then delete hello message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a><span data-ttu-id="3e400-145">Använda fler alternativ för mellan köer meddelanden</span><span class="sxs-lookup"><span data-stu-id="3e400-145">Leverage additional options for dequeuing messages</span></span>
<span data-ttu-id="3e400-146">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="3e400-146">There are two ways you can customize message retrieval from a queue.</span></span>
<span data-ttu-id="3e400-147">Först får du en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="3e400-147">First, you can get a batch of messages (up too32).</span></span> <span data-ttu-id="3e400-148">Andra, du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="3e400-148">Second, you can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="3e400-149">hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="3e400-149">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="3e400-150">Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop.</span><span class="sxs-lookup"><span data-stu-id="3e400-150">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="3e400-151">Den anger också hello osynlighet timeout too5 minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="3e400-151">It also sets hello invisibility timeout too5 minutes for each message.</span></span> <span data-ttu-id="3e400-152">Observera att hello 5 minuter start för alla meddelanden på hello samma tid, så efter 5 minuter efter hello-anropet för**GetMessage**, alla meddelanden som inte har tagits bort blir synliga igen.</span><span class="sxs-lookup"><span data-stu-id="3e400-152">Note that hello 5 minutes start for all messages at hello same time, so after 5 minutes have passed after hello call too**GetMessages**, any messages which have not been deleted become visible again.</span></span>

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="3e400-153">Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="3e400-153">Get hello queue length</span></span>
<span data-ttu-id="3e400-154">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="3e400-154">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="3e400-155">Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="3e400-155">The **FetchAttributes** method asks hello queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="3e400-156">Hej **ApproximateMethodCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="3e400-156">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display hello number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-queue-apis"></a><span data-ttu-id="3e400-157">Använd hello Async-Await-mönstret med vanliga queue API: er</span><span class="sxs-lookup"><span data-stu-id="3e400-157">Use hello Async-Await pattern with common queue APIs</span></span>
<span data-ttu-id="3e400-158">Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga queue API: er.</span><span class="sxs-lookup"><span data-stu-id="3e400-158">This example shows how toouse hello Async-Await pattern with common queue APIs.</span></span> <span data-ttu-id="3e400-159">hello exempel anrop hello asynkrona versionen av var och en av hello anges metoder.</span><span class="sxs-lookup"><span data-stu-id="3e400-159">hello sample calls hello async version of each of hello given methods.</span></span> <span data-ttu-id="3e400-160">Detta kan ses av hello asynkrona efter korrigering för varje metod.</span><span class="sxs-lookup"><span data-stu-id="3e400-160">This can be seen by hello Async post-fix of each method.</span></span> <span data-ttu-id="3e400-161">När en asynkron metod används pausar hello Async-Await-mönstret lokala körningen tills hello anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3e400-161">When an async method is used, hello Async-Await pattern suspends local execution until hello call is completed.</span></span> <span data-ttu-id="3e400-162">Det här innebär att hello aktuella tråden toodo annat arbete som hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet.</span><span class="sxs-lookup"><span data-stu-id="3e400-162">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="3e400-163">Mer information om hur du använder hello Async-Await-mönstret i .NET, finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="3e400-163">For more details on using hello Async-Await pattern in .NET, see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

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
## <a name="delete-a-queue"></a><span data-ttu-id="3e400-164">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="3e400-164">Delete a queue</span></span>
<span data-ttu-id="3e400-165">toodelete en kö och alla hälsningsmeddelande finns i den, anropa den **ta bort** metoden för köobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="3e400-165">toodelete a queue and all hello messages contained in it, call the **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();


## <a name="next-steps"></a><span data-ttu-id="3e400-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e400-166">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

