---
title: "Ta emot händelser från Azure Event Hubs med .NET Standard | Microsoft Docs"
description: "Börja ta emot meddelanden med EventProcessorHost i .NET Standard"
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
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a><span data-ttu-id="af897-103">Börja ta emot meddelanden med den värd för händelsebearbetning i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="af897-103">Get started receiving messages with the Event Processor Host in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="af897-104">Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span><span class="sxs-lookup"><span data-stu-id="af897-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).</span></span>

<span data-ttu-id="af897-105">Den här kursen visar hur du skriver ett .NET Core-konsolprogram som tar emot meddelanden från en händelsehubb med hjälp av **EventProcessorHost**.</span><span class="sxs-lookup"><span data-stu-id="af897-105">This tutorial shows how to write a .NET Core console application that receives messages from an event hub by using **EventProcessorHost**.</span></span> <span data-ttu-id="af897-106">Du kan köra den [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) lösning som-är ersätta strängarna med händelsen NAV- och kontovärden.</span><span class="sxs-lookup"><span data-stu-id="af897-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) solution as-is, replacing the strings with your event hub and storage account values.</span></span> <span data-ttu-id="af897-107">Eller du kan följa stegen i den här kursen hjälper dig att skapa en egen.</span><span class="sxs-lookup"><span data-stu-id="af897-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="af897-108">Krav</span><span class="sxs-lookup"><span data-stu-id="af897-108">Prerequisites</span></span>

* <span data-ttu-id="af897-109">[Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="af897-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="af897-110">Exemplen i den här självstudiekursen används Visual Studio 2017 men Visual Studio 2015 stöds också.</span><span class="sxs-lookup"><span data-stu-id="af897-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="af897-111">[.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="af897-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="af897-112">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="af897-112">An Azure subscription.</span></span>
* <span data-ttu-id="af897-113">Ett namnområde för Händelsehubbar i Azure.</span><span class="sxs-lookup"><span data-stu-id="af897-113">An Azure Event Hubs namespace.</span></span>
* <span data-ttu-id="af897-114">Ett Azure-lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="af897-114">An Azure storage account.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="af897-115">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="af897-115">Create an Event Hubs namespace and an event hub</span></span>  

<span data-ttu-id="af897-116">Det första steget är att använda den [Azure-portalen](https://portal.azure.com) att skapa ett namnområde för typen Event Hubs och skaffa autentiseringsuppgifter för hantering som behövs i programmet för att kommunicera med händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="af897-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the Event Hubs type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="af897-117">Om du vill skapa ett namnområde och händelsen NAV, följer du proceduren i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med följande steg.</span><span class="sxs-lookup"><span data-stu-id="af897-117">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>  

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="af897-118">Skapa ett Azure-lagringskonto</span><span class="sxs-lookup"><span data-stu-id="af897-118">Create an Azure storage account</span></span>  

1. <span data-ttu-id="af897-119">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af897-119">Sign in to the [Azure portal](https://portal.azure.com).</span></span>  
2. <span data-ttu-id="af897-120">I det vänstra navigeringsfönstret i portalen klickar du på **ny**, klickar du på **lagring**, och klicka sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="af897-120">In the left navigation pane of the portal, click **New**, click **Storage**, and then click **Storage Account**.</span></span>  
3. <span data-ttu-id="af897-121">Fyll i fälten i bladet storage-konto och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="af897-121">Complete the fields in the storage account blade, and then click **Create**.</span></span>

    ![Skapa lagringskonto][1]

4. <span data-ttu-id="af897-123">Efter att du ser den **distributioner lyckades** klickar du på namnet på det nya lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="af897-123">After you see the **Deployments Succeeded** message, click the name of the new storage account.</span></span> <span data-ttu-id="af897-124">I den **Essentials** bladet, klickar du på **Blobbar**.</span><span class="sxs-lookup"><span data-stu-id="af897-124">In the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="af897-125">När den **Blob-tjänst** blad öppnas, klickar du på **+ behållare** längst upp.</span><span class="sxs-lookup"><span data-stu-id="af897-125">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="af897-126">Namnge behållaren och stäng sedan den **Blob-tjänst** bladet.</span><span class="sxs-lookup"><span data-stu-id="af897-126">Give the container a name, and then close the **Blob service** blade.</span></span>  
5. <span data-ttu-id="af897-127">Klicka på **åtkomstnycklar** i den vänstra bladet och kopiera namnet på lagringsbehållaren, storage-konto och värdet för **key1**.</span><span class="sxs-lookup"><span data-stu-id="af897-127">Click **Access keys** in the left blade and copy the name of the storage container, the storage account, and the value of **key1**.</span></span> <span data-ttu-id="af897-128">Spara dessa värden i anteckningar eller en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="af897-128">Save these values to Notepad or some other temporary location.</span></span>  

## <a name="create-a-console-application"></a><span data-ttu-id="af897-129">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="af897-129">Create a console application</span></span>

<span data-ttu-id="af897-130">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="af897-130">Start Visual Studio.</span></span> <span data-ttu-id="af897-131">Klicka på **Nytt** i **Arkiv**-menyn och klicka sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="af897-131">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="af897-132">Skapa ett .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="af897-132">Create a .NET Core console application.</span></span>

![Nytt projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="af897-134">Lägg till Event Hubs NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="af897-134">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="af897-135">Lägg till den [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) och [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET standardbibliotek NuGet-paket i projektet genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="af897-135">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) and [`Microsoft.Azure.EventHubs.Processor`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard library NuGet packages to your project by following these steps:</span></span> 

1. <span data-ttu-id="af897-136">Högerklicka på det nyskapade projektet och välj **Hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="af897-136">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="af897-137">Klicka på den **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj sedan den **Microsoft.Azure.EventHubs** paketet.</span><span class="sxs-lookup"><span data-stu-id="af897-137">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="af897-138">Klicka på **Installera** för att slutföra installationen och stäng sedan den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="af897-138">Click **Install** to complete the installation, then close this dialog box.</span></span>
3. <span data-ttu-id="af897-139">Upprepa steg 1 och 2 och installera den **Microsoft.Azure.EventHubs.Processor** paketet.</span><span class="sxs-lookup"><span data-stu-id="af897-139">Repeat steps 1 and 2, and install the **Microsoft.Azure.EventHubs.Processor** package.</span></span>

## <a name="implement-the-ieventprocessor-interface"></a><span data-ttu-id="af897-140">Implementera gränssnittet IEventProcessor</span><span class="sxs-lookup"><span data-stu-id="af897-140">Implement the IEventProcessor interface</span></span>

1. <span data-ttu-id="af897-141">Högerklicka på projektet i Solution Explorer, klicka på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="af897-141">In Solution Explorer, right-click the project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="af897-142">Kalla den nya klassen **SimpleEventProcessor**.</span><span class="sxs-lookup"><span data-stu-id="af897-142">Name the new class **SimpleEventProcessor**.</span></span>

2. <span data-ttu-id="af897-143">Öppna filen SimpleEventProcessor.cs och Lägg till följande `using` instruktioner överst i filen.</span><span class="sxs-lookup"><span data-stu-id="af897-143">Open the SimpleEventProcessor.cs file and add the following `using` statements to the top of the file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. <span data-ttu-id="af897-144">Implementera den `IEventProcessor` gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="af897-144">Implement the `IEventProcessor` interface.</span></span> <span data-ttu-id="af897-145">Ersätt hela innehållet i den `SimpleEventProcessor` klassen med följande kod:</span><span class="sxs-lookup"><span data-stu-id="af897-145">Replace the entire contents of the `SimpleEventProcessor` class with the following code:</span></span>

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

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a><span data-ttu-id="af897-146">Skriva en huvudkonsolen-metod som använder SimpleEventProcessor-klassen för att ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="af897-146">Write a main console method that uses the SimpleEventProcessor class to receive messages</span></span>

1. <span data-ttu-id="af897-147">Lägg till följande `using`-instruktioner överst i Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="af897-147">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="af897-148">Lägg till konstanter till den `Program` klass för anslutningssträngen för event hub, händelsehubbens namn, behållare för lagringskontonamnet, lagringskontonamnet och lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="af897-148">Add constants to the `Program` class for the event hub connection string, event hub name, storage account container name, storage account name, and storage account key.</span></span> <span data-ttu-id="af897-149">Lägg till följande kod, Ersätt platshållarna med motsvarande värden.</span><span class="sxs-lookup"><span data-stu-id="af897-149">Add the following code, replacing the placeholders with their corresponding values.</span></span>

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. <span data-ttu-id="af897-150">Lägg till en ny metod med namnet `MainAsync` till den `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="af897-150">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

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

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. <span data-ttu-id="af897-151">Lägg till följande kod för att den `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="af897-151">Add the following line of code to the `Main` method:</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    <span data-ttu-id="af897-152">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="af897-152">Here is what your Program.cs file should look like:</span></span>

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

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. <span data-ttu-id="af897-153">Kör programmet och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="af897-153">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="af897-154">Grattis!</span><span class="sxs-lookup"><span data-stu-id="af897-154">Congratulations!</span></span> <span data-ttu-id="af897-155">Du har nu fått meddelanden från en händelsehubb med hjälp av den värd för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="af897-155">You have now received messages from an event hub by using the Event Processor Host.</span></span>

## <a name="next-steps"></a><span data-ttu-id="af897-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="af897-156">Next steps</span></span>
<span data-ttu-id="af897-157">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="af897-157">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="af897-158">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="af897-158">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="af897-159">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="af897-159">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="af897-160">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="af897-160">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
