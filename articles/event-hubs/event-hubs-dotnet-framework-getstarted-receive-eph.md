---
title: "Ta emot händelser från Azure Event Hubs med .NET Framework| Microsoft Docs"
description: "Följ den här självstudien för att ta emot händelser från Azure Event Hubs med .NET Framework."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/12/2017
ms.author: sethm
ms.openlocfilehash: 581af52b3264b1a7c57315c65d5385a372308c27
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="receive-events-from-azure-event-hubs-using-the-net-framework"></a><span data-ttu-id="4174e-103">Ta emot händelser från Azure Event Hubs med .NET Framework</span><span class="sxs-lookup"><span data-stu-id="4174e-103">Receive events from Azure Event Hubs using the .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="4174e-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="4174e-104">Introduction</span></span>

<span data-ttu-id="4174e-105">Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="4174e-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="4174e-106">När du har samlat in data i händelsehubbar kan du lagra dem med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys.</span><span class="sxs-lookup"><span data-stu-id="4174e-106">After you collect data into Event Hubs, you can store the data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="4174e-107">Den här storskaliga händelseinsamlingen och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer som t.ex. sakernas internet.</span><span class="sxs-lookup"><span data-stu-id="4174e-107">This large-scale event collection and processing capability is a key component of modern application architectures including the Internet of Things (IoT).</span></span>

<span data-ttu-id="4174e-108">I den här självstudien får du lära dig att skriva ett .NET Framework-konsolprogram som tar emot meddelanden från en Event Hub med **[värden för händelsebearbetning][EventProcessorHost]**.</span><span class="sxs-lookup"><span data-stu-id="4174e-108">This tutorial shows how to write a .NET Framework console application that receives messages from an event hub using the **[Event Processor Host][EventProcessorHost]**.</span></span> <span data-ttu-id="4174e-109">För att skicka händelser med .NET Framework kan du läsa artikeln [Skicka händelser till Azure Event Hubs med .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) eller klicka på ditt avsändarspråk till vänster i innehållsförteckningen.</span><span class="sxs-lookup"><span data-stu-id="4174e-109">To send events using the .NET Framework, see the [Send events to Azure Event Hubs using the .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) article, or click the appropriate sending language in the left-hand table of contents.</span></span>

<span data-ttu-id="4174e-110">[EventProcessorHost][EventProcessorHost] är en .NET-klass som förenklar mottagandet av händelser från händelsehubbar genom att hantera permanenta kontrollpunkter och parallella mottaganden från händelsehubbar.</span><span class="sxs-lookup"><span data-stu-id="4174e-110">The [Event Processor Host][EventProcessorHost] is a .NET class that simplifies receiving events from event hubs by managing persistent checkpoints and parallel receives from those event hubs.</span></span> <span data-ttu-id="4174e-111">Med hjälp av [EventProcessorHost][Event Processor Host] kan du dela upp händelser över flera olika mottagare, även när de ligger på olika noder.</span><span class="sxs-lookup"><span data-stu-id="4174e-111">Using the [Event Processor Host][Event Processor Host], you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="4174e-112">Det här exemplet visas hur man använder [EventProcessorHost][EventProcessorHost] för en enda mottagare.</span><span class="sxs-lookup"><span data-stu-id="4174e-112">This example shows how to use the [Event Processor Host][EventProcessorHost] for a single receiver.</span></span> <span data-ttu-id="4174e-113">Exemplet på att [skala ut händelsebearbetning][Scale out Event Processing with Event Hubs] visar hur man använder [EventProcessorHost][EventProcessorHost] med flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="4174e-113">The [Scale out event processing][Scale out Event Processing with Event Hubs] sample shows how to use the [Event Processor Host][EventProcessorHost] with multiple receivers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4174e-114">Krav</span><span class="sxs-lookup"><span data-stu-id="4174e-114">Prerequisites</span></span>

<span data-ttu-id="4174e-115">För att slutföra den här självstudien, finns följande förhandskrav:</span><span class="sxs-lookup"><span data-stu-id="4174e-115">To complete this tutorial, you need the following prerequisites:</span></span>

* <span data-ttu-id="4174e-116">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="4174e-116">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="4174e-117">För skärmdumparna i de här självstudierna används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4174e-117">The screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="4174e-118">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="4174e-118">An active Azure account.</span></span> <span data-ttu-id="4174e-119">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="4174e-119">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="4174e-120">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="4174e-120">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="4174e-121">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="4174e-121">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="4174e-122">Det första steget är att använda [Azure Portal](https://portal.azure.com) till att skapa ett namnområde av typen Event Hubs och hämta de autentiseringsuppgifter för hantering som programmet behöver för att kommunicera med händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="4174e-122">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace of type Event Hubs, and obtain the management credentials your application needs to communicate with the event hub.</span></span> <span data-ttu-id="4174e-123">Om du vill skapa ett namnområde och en händelsehubb följer du anvisningarna i [den här artikeln](event-hubs-create.md) och fortsätter sedan enligt följande steg i den här självstudien.</span><span class="sxs-lookup"><span data-stu-id="4174e-123">To create a namespace and event hub, follow the procedure in [this article](event-hubs-create.md), then proceed with the following steps in this tutorial.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="4174e-124">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="4174e-124">Create an Azure Storage account</span></span>

<span data-ttu-id="4174e-125">För att kunna använda [EventProcessorHost][EventProcessorHost] behöver du ett [Azure Storage-konto][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="4174e-125">To use the [Event Processor Host][EventProcessorHost], you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="4174e-126">Logga in på [Azure Portal][Azure portal] och klicka på **Ny** högst upp till vänster på skärmen.</span><span class="sxs-lookup"><span data-stu-id="4174e-126">Log on to the [Azure portal][Azure portal], and click **New** at the top left of the screen.</span></span>
2. <span data-ttu-id="4174e-127">Klicka på **Lagring** och sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="4174e-127">Click **Storage**, then click **Storage account**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. <span data-ttu-id="4174e-128">På bladet **Skapa lagringskonto** anger du ett namn för lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="4174e-128">In the **Create storage account** blade, type a name for the storage account.</span></span> <span data-ttu-id="4174e-129">Välj en Azure-prenumeration, resursgrupp och plats där du vill skapa resursen.</span><span class="sxs-lookup"><span data-stu-id="4174e-129">Choose an Azure subscription, resource group, and location in which to create the resource.</span></span> <span data-ttu-id="4174e-130">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4174e-130">Then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. <span data-ttu-id="4174e-131">Klicka på det nyligen skapade lagringskontot i listan över lagringskonton.</span><span class="sxs-lookup"><span data-stu-id="4174e-131">In the list of storage accounts, click the newly created storage account.</span></span>
5. <span data-ttu-id="4174e-132">På bladet för lagringskontot klickar du på **Åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="4174e-132">In the storage account blade, click **Access keys**.</span></span> <span data-ttu-id="4174e-133">Kopiera värdet för **nyckel1** och använd det senare i de här självstudierna.</span><span class="sxs-lookup"><span data-stu-id="4174e-133">Copy the value of **key1** to use later in this tutorial.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a><span data-ttu-id="4174e-134">Skapa ett mottagarkonsolprogram</span><span class="sxs-lookup"><span data-stu-id="4174e-134">Create a receiver console application</span></span>

1. <span data-ttu-id="4174e-135">I Visual Studio skapar du ett nytt Visual C#-skrivbordsapprojekt med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="4174e-135">In Visual Studio, create a new Visual C# Desktop App project using the **Console  Application** project template.</span></span> <span data-ttu-id="4174e-136">Namnge projektet **Mottagare**.</span><span class="sxs-lookup"><span data-stu-id="4174e-136">Name the project **Receiver**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. <span data-ttu-id="4174e-137">Högerklicka på projektet **Mottagare** i Solution Explorer och klicka sedan på **Hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="4174e-137">In Solution Explorer, right-click the **Receiver** project, and then click **Manage NuGet Packages for Solution**.</span></span>
3. <span data-ttu-id="4174e-138">Klicka på **Bläddra**-fliken och sök sedan efter `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span><span class="sxs-lookup"><span data-stu-id="4174e-138">Click the **Browse** tab, then search for `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span></span> <span data-ttu-id="4174e-139">Klicka på **Installera** och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="4174e-139">Click **Install**, and accept the terms of use.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    <span data-ttu-id="4174e-140">Visual Studio laddar ned, installerar och lägger till en referens till [Azure Service Bus Event Hub –EventProcessorHost NuGet-paket ](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), med alla sina beroenden.</span><span class="sxs-lookup"><span data-stu-id="4174e-140">Visual Studio downloads, installs, and adds a reference to the [Azure Service Bus Event Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), with all its dependencies.</span></span>
4. <span data-ttu-id="4174e-141">Högerklicka på **Mottagare**-projektet, klicka på **Lägg till** och klicka sedan på **Klass**.</span><span class="sxs-lookup"><span data-stu-id="4174e-141">Right-click the **Receiver** project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="4174e-142">Kalla den nya klassen för **SimpleEventProcessor** och klicka sedan på **Lägg till** för att skapa klassen.</span><span class="sxs-lookup"><span data-stu-id="4174e-142">Name the new class **SimpleEventProcessor**, and then click **Add** to create the class.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. <span data-ttu-id="4174e-143">Lägg till följande uttryck överst i filen SimpleEventProcessor.cs:</span><span class="sxs-lookup"><span data-stu-id="4174e-143">Add the following statements at the top of the SimpleEventProcessor.cs file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  <span data-ttu-id="4174e-144">Ersätt sedan följande kod för innehållet i klassen:</span><span class="sxs-lookup"><span data-stu-id="4174e-144">Then, substitute the following code for the body of the class:</span></span>
    
  ```csharp
  class SimpleEventProcessor : IEventProcessor
  {
    Stopwatch checkpointStopWatch;
    
    async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
    {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        if (reason == CloseReason.Shutdown)
        {
            await context.CheckpointAsync();
        }
    }
    
    Task IEventProcessor.OpenAsync(PartitionContext context)
    {
        Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
        this.checkpointStopWatch = new Stopwatch();
        this.checkpointStopWatch.Start();
        return Task.FromResult<object>(null);
    }
    
    async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
    {
        foreach (EventData eventData in messages)
        {
            string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
            Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                context.Lease.PartitionId, data));
        }
    
        //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
        if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
        {
            await context.CheckpointAsync();
            this.checkpointStopWatch.Restart();
        }
    }
  }
  ```
    
  <span data-ttu-id="4174e-145">Den här klassen kommer att anropas av **EventProcessorHost** för att bearbeta händelser som tagits emot från händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="4174e-145">This class is called by the **EventProcessorHost** to process events received from the event hub.</span></span> <span data-ttu-id="4174e-146">Observera att `SimpleEventProcessor`-klassen använder sig av ett stoppur för att regelbundet anropa kontrollpunktsmetoden i **EventProcessorHost**-kontexten.</span><span class="sxs-lookup"><span data-stu-id="4174e-146">The `SimpleEventProcessor` class uses a stopwatch to periodically call the checkpoint method on the **EventProcessorHost** context.</span></span> <span data-ttu-id="4174e-147">Detta garanterar att högst fem minuters bearbetningsarbete försvinner om mottagaren startas om.</span><span class="sxs-lookup"><span data-stu-id="4174e-147">This processing ensures that, if the receiver is restarted, it loses no more than five minutes of processing work.</span></span>
6. <span data-ttu-id="4174e-148">I **Program**-klassen lägger du till följande `using`-uttryck överst i filen:</span><span class="sxs-lookup"><span data-stu-id="4174e-148">In the **Program** class, add the following `using` statement at the top of the file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  <span data-ttu-id="4174e-149">Ersätt sedan metoden `Main` i klassen `Program` med följande kod, och ersätt namnet på händelsehubben och anslutningssträngen på namnområdesnivå som du sparade tidigare, samt lagringskontot och nyckeln som du kopierade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4174e-149">Then, replace the `Main` method in the `Program` class with the following code, substituting the event hub name and the namespace-level connection string that you saved previously, and the storage account and key that you copied in the previous sections.</span></span> 
    
  ```csharp
  static void Main(string[] args)
  {
    string eventHubConnectionString = "{Event Hubs namespace connection string}";
    string eventHubName = "{Event Hub name}";
    string storageAccountName = "{storage account name}";
    string storageAccountKey = "{storage account key}";
    string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
    string eventProcessorHostName = Guid.NewGuid().ToString();
    EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
    Console.WriteLine("Registering EventProcessor...");
    var options = new EventProcessorOptions();
    options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
    eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
    Console.WriteLine("Receiving. Press enter key to stop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. <span data-ttu-id="4174e-150">Kör programmet och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="4174e-150">Run the program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="4174e-151">Grattis!</span><span class="sxs-lookup"><span data-stu-id="4174e-151">Congratulations!</span></span> <span data-ttu-id="4174e-152">Du har nu fått meddelanden från en händelsehubb med värden för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="4174e-152">You have now received messages from an event hub using the Event Processor Host.</span></span>


> [!NOTE]
> <span data-ttu-id="4174e-153">Den här guiden använder en enda instans av [EventProcessorHost][EventProcessorHost].</span><span class="sxs-lookup"><span data-stu-id="4174e-153">This tutorial uses a single instance of [EventProcessorHost][EventProcessorHost].</span></span> <span data-ttu-id="4174e-154">För att öka dataflödet rekommenderas att du kör flera instanser av [EventProcessorHost][EventProcessorHost], enligt exemplet [Utskalad händelsebearbetning][Utskalad händelsebearbetning].</span><span class="sxs-lookup"><span data-stu-id="4174e-154">To increase throughput, it is recommended that you run multiple instances of [EventProcessorHost][EventProcessorHost], as shown in the [Scaled out event processing][Scaled out event processing] sample.</span></span> <span data-ttu-id="4174e-155">I de fallen koordineras de olika instanserna automatiskt sinsemellan för att kunna belastningsutjämna de mottagna händelserna.</span><span class="sxs-lookup"><span data-stu-id="4174e-155">In those cases, the various instances automatically coordinate with each other to load balance the received events.</span></span> <span data-ttu-id="4174e-156">Om du vill att flera mottagare bearbetar *alla* händelser, måste du använda konceptet **ConsumerGroup**.</span><span class="sxs-lookup"><span data-stu-id="4174e-156">If you want multiple receivers to each process *all* the events, you must use the **ConsumerGroup** concept.</span></span> <span data-ttu-id="4174e-157">När du tar emot händelser från olika datorer, kan det vara praktiskt att ange namn för [EventProcessorHost][EventProcessorHost]-instanser baserat på de datorer (eller roller) som de har distribuerats i.</span><span class="sxs-lookup"><span data-stu-id="4174e-157">When receiving events from different machines, it might be useful to specify names for [EventProcessorHost][EventProcessorHost] instances based on the machines (or roles) in which they are deployed.</span></span> <span data-ttu-id="4174e-158">Mer information om de här ämnena finns i [Översikt över Event Hubs][Event Hubs overview] och [Programmeringsguide för Event Hubs][Event Hubs Programming Guide].</span><span class="sxs-lookup"><span data-stu-id="4174e-158">For more information about these topics, see the [Event Hubs overview][Event Hubs overview] and the [Event Hubs programming guide][Event Hubs Programming Guide] topics.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="4174e-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4174e-159">Next steps</span></span>

<span data-ttu-id="4174e-160">Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar och tar emot data kan du lära dig mer genom att besöka följande länkar:</span><span class="sxs-lookup"><span data-stu-id="4174e-160">Now that you've built a working application that creates an event hub and sends and receives data, you can learn more by visiting the following links:</span></span>

* <span data-ttu-id="4174e-161">[Värd för händelsebearbetning][Event Processor Host]</span><span class="sxs-lookup"><span data-stu-id="4174e-161">[Event Processor Host][Event Processor Host]</span></span>
* <span data-ttu-id="4174e-162">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="4174e-162">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="4174e-163">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="4174e-163">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[EventProcessorHost]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]:../storage/common/storage-create-storage-account.md
[Event Processor Host]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com
