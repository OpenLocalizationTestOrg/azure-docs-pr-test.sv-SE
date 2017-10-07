---
title: "aaaReceive händelser från Händelsehubbar i Azure med hjälp av .NET Standard | Microsoft Docs"
description: "Börja ta emot meddelanden med hello EventProcessorHost i .NET Standard"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: c3983f2668ac8f65522e44a1609dfd2eed31b7d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a><span data-ttu-id="5583c-103">Börja ta emot meddelanden med hello värd för händelsebearbetning i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="5583c-103">Get started receiving messages with hello Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="5583c-104">Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="5583c-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="5583c-105">Den här kursen visar hur toowrite en .NET Core konsolen program som tar emot meddelanden från en händelsehubb med hjälp av **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="5583c-105">This tutorial shows how toowrite a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="5583c-106">Du kan köra hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) lösning som-, ersätter hello strängar med händelsen NAV- och storage-konto värdena.</span><span class="sxs-lookup"><span data-stu-id="5583c-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing hello strings with your event hub and storage account values.</span></span> <span data-ttu-id="5583c-107">Eller du kan följa hello stegen i den här självstudiekursen toocreate egna.</span><span class="sxs-lookup"><span data-stu-id="5583c-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5583c-108">Krav</span><span class="sxs-lookup"><span data-stu-id="5583c-108">Prerequisites</span></span>

* <span data-ttu-id="5583c-109">[Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="5583c-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="5583c-110">hello exemplen i den här självstudiekursen används Visual Studio 2017, men Visual Studio 2015 stöds också.</span><span class="sxs-lookup"><span data-stu-id="5583c-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="5583c-111">[.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="5583c-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="5583c-112">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5583c-112">An Azure subscription.</span></span>
* <span data-ttu-id="5583c-113">Ett namnområde för Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="5583c-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="5583c-114">Ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5583c-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="5583c-115">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="5583c-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="5583c-116">hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate ett namnområde för hello Händelsehubbar Skriv och hämta hello autentiseringsuppgifter för hantering att ditt program måste toocommunicate med hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="5583c-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello Event Hubs type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="5583c-117">toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="5583c-117">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="5583c-118">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="5583c-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="5583c-119">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5583c-119">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="5583c-120">Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klickar du på **lagring**, och klicka sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="5583c-120">In hello left navigation pane of hello portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="5583c-121">Slutför hello fälten i hello lagring-bladet för kontot och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="5583c-121">Complete hello fields in hello storage account blade, and then click **Create**.</span></span>

    ![Skapa lagringskonto][1]

4. <span data-ttu-id="5583c-123">Efter att du ser hello **distributioner lyckades** klickar du på hello namnet på hello nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="5583c-123">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account.</span></span> <span data-ttu-id="5583c-124">I hello **Essentials** bladet, klickar du på **Blobbar**.</span><span class="sxs-lookup"><span data-stu-id="5583c-124">In hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="5583c-125">När hello **Blob-tjänst** blad öppnas, klickar du på **+ behållare** hello överst.</span><span class="sxs-lookup"><span data-stu-id="5583c-125">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="5583c-126">Namnge hello behållare och stäng sedan hello **Blob-tjänst** bladet.</span><span class="sxs-lookup"><span data-stu-id="5583c-126">Give hello container a name, and then close hello **Blob service** blade.</span></span>  
5. <span data-ttu-id="5583c-127">Klicka på **åtkomstnycklar** i hello vänstra bladet och kopiera hello namn hello lagringsbehållaren, hello storage-konto och hello värdet av **key1**.</span><span class="sxs-lookup"><span data-stu-id="5583c-127">Click **Access keys** in hello left blade and copy hello name of hello storage container, hello storage account, and hello value of **key1**.</span></span> <span data-ttu-id="5583c-128">Spara dessa värden tooNotepad eller en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="5583c-128">Save these values tooNotepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="5583c-129">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="5583c-129">Create a console application</span></span>

<span data-ttu-id="5583c-130">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="5583c-130">Start Visual Studio.</span></span> <span data-ttu-id="5583c-131">Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5583c-131">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="5583c-132">Skapa ett .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="5583c-132">Create a .NET Core console application.</span></span>

![Nytt projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="5583c-134">Lägg till hello Event Hubs NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="5583c-134">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="5583c-135">Lägg till hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) och [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard NuGet-paket tooyour biblioteksprojekt genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="5583c-135">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="5583c-136">Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="5583c-136">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="5583c-137">Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj hello **Microsoft.Azure.EventHubs** paketet.</span><span class="sxs-lookup"><span data-stu-id="5583c-137">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="5583c-138">Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="5583c-138">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
3. <span data-ttu-id="5583c-139">Upprepa steg 1 och 2 och installera hello **Microsoft.Azure.EventHubs.Processor** paketet.</span><span class="sxs-lookup"><span data-stu-id="5583c-139">Repeat steps 1 and 2, and install hello **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-hello-ieventprocessor-interface"></a><span data-ttu-id="5583c-140">Implementera hello IEventProcessor-gränssnittet</span><span class="sxs-lookup"><span data-stu-id="5583c-140">Implement hello IEventProcessor interface</span></span>

1. <span data-ttu-id="5583c-141">I Solution Explorer, högerklicka på hello projektet klickar du på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="5583c-141">In Solution Explorer, right-click hello project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="5583c-142">Namnge hello nya klassen **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="5583c-142">Name hello new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="5583c-143">Öppna hello SimpleEventProcessor.cs filen och Lägg till följande hello `using` instruktioner toohello överkant hello-filen.</span><span class="sxs-lookup"><span data-stu-id="5583c-143">Open hello SimpleEventProcessor.cs file and add hello following `using` statements toohello top of hello file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="5583c-144">Implementera hello `IEventProcessor` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="5583c-144">Implement hello `IEventProcessor` interface.</span></span> <span data-ttu-id="5583c-145">Ersätt hello hela innehållet i hello `SimpleEventProcessor` klassen med följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="5583c-145">Replace hello entire contents of hello `SimpleEventProcessor` class with hello following code:</span></span>

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a><span data-ttu-id="5583c-146">Skriv en huvudkonsolen-metod som använder tooreceive hälsningsmeddelande SimpleEventProcessor klass</span><span class="sxs-lookup"><span data-stu-id="5583c-146">Write a main console method that uses hello SimpleEventProcessor class tooreceive messages</span></span>

1. <span data-ttu-id="5583c-147">Lägg till följande hello `using` instruktioner toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="5583c-147">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="5583c-148">Lägg till konstanter toohello `Program` klass för anslutningssträngen för hello event hub, händelsehubbens namn, behållare för lagringskontonamnet, lagringskontonamnet och lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="5583c-148">Add constants toohello `Program` class for hello event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="5583c-149">Lägg till hello följande kod, ersätta hello platshållarna med motsvarande värden.</span><span class="sxs-lookup"><span data-stu-id="5583c-149">Add hello following code, replacing hello placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="5583c-150">Lägg till en ny metod med namnet `MainAsync` toohello `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="5583c-150">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers hello Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER toostop worker.");
        Console.ReadLine();

        // Disposes of hello Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. <span data-ttu-id="5583c-151">Lägg till följande rad med kod toohello hello `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="5583c-151">Add hello following line of code toohello `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="5583c-152">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="5583c-152">Here is what your Program.cs file should look like:</span></span>

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers hello Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER toostop worker.");
                Console.ReadLine();

                // Disposes of hello Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. <span data-ttu-id="5583c-153">Kör programmet hello och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="5583c-153">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="5583c-154">Grattis!</span><span class="sxs-lookup"><span data-stu-id="5583c-154">Congratulations!</span></span> <span data-ttu-id="5583c-155">Du har nu fått meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="5583c-155">You have now received messages from an event hub by using hello Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5583c-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5583c-156">Next steps</span></span>
<span data-ttu-id="5583c-157">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="5583c-157">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="5583c-158">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="5583c-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="5583c-159">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="5583c-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="5583c-160">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="5583c-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
