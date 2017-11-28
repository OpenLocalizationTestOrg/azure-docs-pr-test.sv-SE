---
title: "aaaGet igång med queue storage- och Visual Studio anslutna tjänster (molntjänster) | Microsoft Docs"
description: "Hur tooget igång med Azure Queue storage i ett molntjänstprojekt i Visual Studio när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: da587aac-5e64-4e9a-8405-44cc1924881d
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 1e90eeb826131cadca90dcb720c931fff5fedcb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="624cb-103">Komma igång med Azure Queue storage och Visual Studio anslutna tjänster (cloud services-projekt)</span><span class="sxs-lookup"><span data-stu-id="624cb-103">Getting started with Azure Queue storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a><span data-ttu-id="624cb-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="624cb-104">Overview</span></span>
<span data-ttu-id="624cb-105">Den här artikeln beskriver hur tooget igång med Azure Queue storage i Visual Studio när du har skapat eller refererar till ett Azure storage-konto i cloud services-projekt med hjälp av hello Visual Studio **Lägg till anslutna tjänster** dialogrutan .</span><span class="sxs-lookup"><span data-stu-id="624cb-105">This article describes how tooget started using Azure Queue storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello  Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="624cb-106">Får du lära dig hur toocreate en kö i koden.</span><span class="sxs-lookup"><span data-stu-id="624cb-106">We'll show you how toocreate a queue in code.</span></span> <span data-ttu-id="624cb-107">Vi också lära dig hur tooperform basic kö åtgärder, till exempel lägga till, ändra, läsa och tar bort Kömeddelanden.</span><span class="sxs-lookup"><span data-stu-id="624cb-107">We'll also show you how tooperform basic queue operations, such as adding, modifying, reading and removing queue messages.</span></span> <span data-ttu-id="624cb-108">hello exemplen är skrivna i C#-kod och använder hello [Microsoft Azure Storage-klientbibliotek för .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="624cb-108">hello samples are written in C# code and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="624cb-109">Hej **Lägg till anslutna tjänster** åtgärden installerar hello lämpligt NuGet-paket tooaccess Azure-lagring i ditt projekt och lägger till hello anslutningssträngen för hello lagring kontot tooyour konfigurationsfiler för projektet.</span><span class="sxs-lookup"><span data-stu-id="624cb-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

* <span data-ttu-id="624cb-110">Se [komma igång med Azure Queue storage med hjälp av .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) mer information om manipulering köer i kod.</span><span class="sxs-lookup"><span data-stu-id="624cb-110">See [Get started with Azure Queue storage using .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) for more information on manipulating queues in code.</span></span>
* <span data-ttu-id="624cb-111">Se [Storage-dokumentation](https://azure.microsoft.com/documentation/services/storage/) allmän information om Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="624cb-111">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="624cb-112">Se [molntjänster dokumentationen](https://azure.microsoft.com/documentation/services/cloud-services/) allmän information om Azure-molntjänster.</span><span class="sxs-lookup"><span data-stu-id="624cb-112">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="624cb-113">Se [ASP.NET](http://www.asp.net) för mer information om programmering i ASP.NET-program.</span><span class="sxs-lookup"><span data-stu-id="624cb-113">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

<span data-ttu-id="624cb-114">Azure Queue storage är en tjänst för att lagra stora mängder meddelanden som kan nås från var som helst i Hej världen via autentiserade anrop med HTTP eller HTTPS.</span><span class="sxs-lookup"><span data-stu-id="624cb-114">Azure Queue storage is a service for storing large numbers of messages that can be accessed from anywhere in hello world via authenticated calls using HTTP or HTTPS.</span></span> <span data-ttu-id="624cb-115">Ett enda kömeddelande kan vara upp too64 KB i storlek och en kö kan innehålla miljontals meddelanden, upp toohello totala kapacitetsgränsen för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="624cb-115">A single queue message can be up too64 KB in size, and a queue can contain millions of messages, up toohello total capacity limit of a storage account.</span></span>

## <a name="access-queues-in-code"></a><span data-ttu-id="624cb-116">Ha åtkomst till serviceköer i koden</span><span class="sxs-lookup"><span data-stu-id="624cb-116">Access queues in code</span></span>
<span data-ttu-id="624cb-117">tooaccess köer i molntjänster i Visual Studio-projekt, behöver du tooinclude hello följande objekt tooany C#-källfilen som har åtkomst till Azure Queue storage.</span><span class="sxs-lookup"><span data-stu-id="624cb-117">tooaccess queues in Visual Studio Cloud Services projects, you need tooinclude hello following items tooany C# source file that access Azure Queue storage.</span></span>

1. <span data-ttu-id="624cb-118">Se till att hello namnrymdsdeklarationer överst hello i hello C#-filen med dessa **med** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="624cb-118">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
2. <span data-ttu-id="624cb-119">Hämta en **CloudStorageAccount** objekt som representerar din kontoinformation för lagring.</span><span class="sxs-lookup"><span data-stu-id="624cb-119">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="624cb-120">Använd hello följande kod tooget hello dina anslutningssträngen för lagring och information om lagringskonto från hello Azure tjänstkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="624cb-120">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. <span data-ttu-id="624cb-121">Hämta en **CloudQueueClient** objekt tooreference hello köobjekt i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="624cb-121">Get a **CloudQueueClient** object tooreference hello queue objects in your storage account.</span></span>  
   
        // Create hello queue client.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. <span data-ttu-id="624cb-122">Hämta en **CloudQueue** objekt tooreference en särskild kö.</span><span class="sxs-lookup"><span data-stu-id="624cb-122">Get a **CloudQueue** object tooreference a specific queue.</span></span>
   
        // Get a reference tooa queue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

<span data-ttu-id="624cb-123">**Obs:** använda alla hello ovan kod framför hello koden i hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="624cb-123">**NOTE:** Use all of hello above code in front of hello code in hello following samples.</span></span>

## <a name="create-a-queue-in-code"></a><span data-ttu-id="624cb-124">Skapa en kö i koden</span><span class="sxs-lookup"><span data-stu-id="624cb-124">Create a queue in code</span></span>
<span data-ttu-id="624cb-125">toocreate hello kön i koden, Lägg till ett anrop för**CreateIfNotExists**.</span><span class="sxs-lookup"><span data-stu-id="624cb-125">toocreate hello queue in code, just add a call too**CreateIfNotExists**.</span></span>

    // Create hello CloudQueue if it does not exist
    messageQueue.CreateIfNotExists();

## <a name="add-a-message-tooa-queue"></a><span data-ttu-id="624cb-126">Lägg till en meddelandekö tooa</span><span class="sxs-lookup"><span data-stu-id="624cb-126">Add a message tooa queue</span></span>
<span data-ttu-id="624cb-127">tooinsert ett meddelande i en befintlig kö, skapa en ny **CloudQueueMessage** objektet och sedan anropa hello **AddMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="624cb-127">tooinsert a message into an existing queue, create a new **CloudQueueMessage** object, then call hello **AddMessage** method.</span></span>

<span data-ttu-id="624cb-128">En **CloudQueueMessage** objekt kan skapas från en sträng (i UTF-8-format) eller en byte-matris.</span><span class="sxs-lookup"><span data-stu-id="624cb-128">A **CloudQueueMessage** object can be created from either a string (in UTF-8 format) or a byte array.</span></span>

<span data-ttu-id="624cb-129">Här är ett exempel som infogar hello-meddelande ”Hello World”.</span><span class="sxs-lookup"><span data-stu-id="624cb-129">Here is an example which inserts hello message 'Hello, World'.</span></span>

    // Create a message and add it toohello queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    messageQueue.AddMessage(message);

## <a name="read-a-message-in-a-queue"></a><span data-ttu-id="624cb-130">Läs ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="624cb-130">Read a message in a queue</span></span>
<span data-ttu-id="624cb-131">Du kan kika på hello-meddelande i hello framför en kö utan att ta bort den från hello kö genom att anropa hello **PeekMessage** metod.</span><span class="sxs-lookup"><span data-stu-id="624cb-131">You can peek at hello message in hello front of a queue without removing it from hello queue by calling hello **PeekMessage** method.</span></span>

    // Peek at hello next message
    CloudQueueMessage peekedMessage = messageQueue.PeekMessage();

## <a name="read-and-remove-a-message-in-a-queue"></a><span data-ttu-id="624cb-132">Läsa och ta bort ett meddelande i en kö</span><span class="sxs-lookup"><span data-stu-id="624cb-132">Read and remove a message in a queue</span></span>
<span data-ttu-id="624cb-133">Koden kan ta bort (Frigör kö) ett meddelande från en kö i två steg.</span><span class="sxs-lookup"><span data-stu-id="624cb-133">Your code can remove (de-queue) a message from a queue in two steps.</span></span>

1. <span data-ttu-id="624cb-134">Anropa **GetMessage** tooget hello nästa meddelande i en kö.</span><span class="sxs-lookup"><span data-stu-id="624cb-134">Call **GetMessage** tooget hello next message in a queue.</span></span> <span data-ttu-id="624cb-135">Ett meddelande som returneras från **GetMessage** blir osynligt tooany annan kod läsa meddelanden från den här kön.</span><span class="sxs-lookup"><span data-stu-id="624cb-135">A message returned from **GetMessage** becomes invisible tooany other code reading messages from this queue.</span></span> <span data-ttu-id="624cb-136">Som standard är det här meddelandet osynligt i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="624cb-136">By default, this message stays invisible for 30 seconds.</span></span>
2. <span data-ttu-id="624cb-137">toofinish ta bort hello-meddelande från hello kö, anropet **DeleteMessage**.</span><span class="sxs-lookup"><span data-stu-id="624cb-137">toofinish removing hello message from hello queue, call **DeleteMessage**.</span></span>

<span data-ttu-id="624cb-138">Den här tvåstegsprocessen för att ta bort ett meddelande säkerställer som om din kod inte tooprocess får ett meddelande på grund av fel toohardware eller programvara, en annan instans av koden hello samma meddelande och försök igen.</span><span class="sxs-lookup"><span data-stu-id="624cb-138">This two-step process of removing a message assures that if your code fails tooprocess a message due toohardware or software failure, another instance of your code can get hello same message and try again.</span></span> <span data-ttu-id="624cb-139">hello följande koden anropar **DeleteMessage** direkt efter hello-meddelande har bearbetats.</span><span class="sxs-lookup"><span data-stu-id="624cb-139">hello following code calls **DeleteMessage** right after hello message has been processed.</span></span>

    // Get hello next message in hello queue.
    CloudQueueMessage retrievedMessage = messageQueue.GetMessage();

    // Process hello message in less than 30 seconds

    // Then delete hello message.
    await messageQueue.DeleteMessage(retrievedMessage);


## <a name="use-additional-options-tooprocess-and-remove-queue-messages"></a><span data-ttu-id="624cb-140">Använda ytterligare alternativ tooprocess och ta bort Kömeddelanden</span><span class="sxs-lookup"><span data-stu-id="624cb-140">Use additional options tooprocess and remove queue messages</span></span>
<span data-ttu-id="624cb-141">Det finns två metoder som du kan använda för att anpassa meddelandehämtningen från en kö.</span><span class="sxs-lookup"><span data-stu-id="624cb-141">There are two ways you can customize message retrieval from a queue.</span></span>

* <span data-ttu-id="624cb-142">Du kan hämta en grupp med meddelanden (upp too32).</span><span class="sxs-lookup"><span data-stu-id="624cb-142">You can get a batch of messages (up too32).</span></span>
* <span data-ttu-id="624cb-143">Du kan ange en tidsgräns för osynlighet längre eller kortare för att ge koden mer eller mindre tid toofully bearbeta varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="624cb-143">You can set a longer or shorter invisibility timeout, allowing your code more or less time toofully process each message.</span></span> <span data-ttu-id="624cb-144">hello följande kodexempel används den **GetMessage** metoden tooget 20 meddelanden i ett anrop.</span><span class="sxs-lookup"><span data-stu-id="624cb-144">hello following code example uses the **GetMessages** method tooget 20 messages in one call.</span></span> <span data-ttu-id="624cb-145">Sedan bearbetas varje meddelande med hjälp av en **foreach**-loop.</span><span class="sxs-lookup"><span data-stu-id="624cb-145">Then it processes each message using a **foreach** loop.</span></span> <span data-ttu-id="624cb-146">Den anger också hello osynlighet timeout toofive minuter för varje meddelande.</span><span class="sxs-lookup"><span data-stu-id="624cb-146">It also sets hello invisibility timeout toofive minutes for each message.</span></span> <span data-ttu-id="624cb-147">Observera att hello 5 minuter startar för alla meddelanden med hello samma tid, så efter 5 minuter har gått sedan hello anropet för**GetMessage**, alla meddelanden som inte har tagits bort kommer att bli synliga igen.</span><span class="sxs-lookup"><span data-stu-id="624cb-147">Note that hello 5 minutes starts for all messages at hello same time, so after 5 minutes have passed since hello call too**GetMessages**, any messages which have not been deleted will become visible again.</span></span>

<span data-ttu-id="624cb-148">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="624cb-148">Here's an example:</span></span>

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.

        // Then delete hello message after processing
        messageQueue.DeleteMessage(message);

    }

## <a name="get-hello-queue-length"></a><span data-ttu-id="624cb-149">Hämta hello Kölängd</span><span class="sxs-lookup"><span data-stu-id="624cb-149">Get hello queue length</span></span>
<span data-ttu-id="624cb-150">Du kan få en uppskattning av hello antal meddelanden i en kö.</span><span class="sxs-lookup"><span data-stu-id="624cb-150">You can get an estimate of hello number of messages in a queue.</span></span> <span data-ttu-id="624cb-151">Den **FetchAttributes** metoden ber hello kötjänsten att hämta hello köattributen, inklusive antal för hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="624cb-151">The **FetchAttributes** method asks hello Queue service to retrieve hello queue attributes, including hello message count.</span></span> <span data-ttu-id="624cb-152">Hej **ApproximateMethodCount** -egenskap returnerar hello senaste värdet som hämtas av den **FetchAttributes** metoden, utan att kötjänsten hello.</span><span class="sxs-lookup"><span data-stu-id="624cb-152">hello **ApproximateMethodCount** property returns hello last value retrieved by the **FetchAttributes** method, without calling hello Queue service.</span></span>

    // Fetch hello queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve hello cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-hello-async-await-pattern-with-common-azure-queue-apis"></a><span data-ttu-id="624cb-153">Använda hello Async-Await-mönstret med vanliga Azure kön API: er</span><span class="sxs-lookup"><span data-stu-id="624cb-153">Use hello Async-Await Pattern with common Azure Queue APIs</span></span>
<span data-ttu-id="624cb-154">Det här exemplet visar hur toouse hello Async-Await-mönstret med vanliga Azure kön API: er.</span><span class="sxs-lookup"><span data-stu-id="624cb-154">This example shows how toouse hello Async-Await pattern with common Azure Queue APIs.</span></span> <span data-ttu-id="624cb-155">hello exemplet anropar hello asynkrona versionen av var och en av hello anges metoder, detta kan ses av hello **asynkrona** efter korrigering för varje metod.</span><span class="sxs-lookup"><span data-stu-id="624cb-155">hello sample calls hello async version of each of hello given methods, this can be seen by hello **Async** post-fix of each method.</span></span> <span data-ttu-id="624cb-156">När en asynkron metod är används hello async-await mönster pausar lokala körningen tills hello anropet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="624cb-156">When an async method is used hello async-await pattern suspends local execution until hello call completes.</span></span> <span data-ttu-id="624cb-157">Det här innebär att hello aktuella tråden toodo annat arbete som hjälper till att undvika flaskhalsar och förbättrar hello övergripande svarstiden för programmet.</span><span class="sxs-lookup"><span data-stu-id="624cb-157">This behavior allows hello current thread toodo other work which helps avoid performance bottlenecks and improves hello overall responsiveness of your application.</span></span> <span data-ttu-id="624cb-158">Mer information om hur du använder hello Async-Await-mönstret i .NET finns [Async och Await (C# och Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span><span class="sxs-lookup"><span data-stu-id="624cb-158">For more details on using hello Async-Await pattern in .NET see [Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)</span></span>

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

## <a name="delete-a-queue"></a><span data-ttu-id="624cb-159">Ta bort en kö</span><span class="sxs-lookup"><span data-stu-id="624cb-159">Delete a queue</span></span>
<span data-ttu-id="624cb-160">en kö och alla hälsningsmeddelande som ingår i den anropet hello toodelete **ta bort** metoden för köobjektet hello.</span><span class="sxs-lookup"><span data-stu-id="624cb-160">toodelete a queue and all hello messages contained in it, call hello **Delete** method on hello queue object.</span></span>

    // Delete hello queue.
    messageQueue.Delete();

## <a name="next-steps"></a><span data-ttu-id="624cb-161">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="624cb-161">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

