---
title: "aaaReceive händelser från Azure Event Hubs använder hello .NET Framework | Microsoft Docs"
description: "Följ den här självstudiekursen tooreceive händelser från Azure Event Hubs med hello .NET Framework."
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
ms.openlocfilehash: a88c3feeacfd3de9622dbb86e25222e861750204
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a><span data-ttu-id="7372c-103">Ta emot händelser från Azure Event Hubs med hello .NET Framework</span><span class="sxs-lookup"><span data-stu-id="7372c-103">Receive events from Azure Event Hubs using hello .NET Framework</span></span>

## <a name="introduction"></a><span data-ttu-id="7372c-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="7372c-104">Introduction</span></span>

<span data-ttu-id="7372c-105">Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="7372c-105">Event Hubs is a service that processes large amounts of event data (telemetry) from connected devices and applications.</span></span> <span data-ttu-id="7372c-106">När du samlar in data i Händelsehubbar kan du lagra hello data med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys.</span><span class="sxs-lookup"><span data-stu-id="7372c-106">After you collect data into Event Hubs, you can store hello data using a storage cluster or transform it using a real-time analytics provider.</span></span> <span data-ttu-id="7372c-107">Den här storskaliga händelse och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer inklusive hello Sakernas Internet (IoT).</span><span class="sxs-lookup"><span data-stu-id="7372c-107">This large-scale event collection and processing capability is a key component of modern application architectures including hello Internet of Things (IoT).</span></span>

<span data-ttu-id="7372c-108">Den här kursen visar hur toowrite ett .NET Framework-konsolapp som tar emot meddelanden från en händelsehubb med hjälp av hello  **[värd för händelsebearbetning][EventProcessorHost]**.</span><span class="sxs-lookup"><span data-stu-id="7372c-108">This tutorial shows how toowrite a .NET Framework console application that receives messages from an event hub using hello **[Event Processor Host][EventProcessorHost]**.</span></span> <span data-ttu-id="7372c-109">toosend händelser med hjälp av hello .NET Framework finns hello [skicka händelser tooAzure Händelsehubbar med hello .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) artikel, eller klicka på hello skicka språket i hello vänstra innehållsförteckning.</span><span class="sxs-lookup"><span data-stu-id="7372c-109">toosend events using hello .NET Framework, see hello [Send events tooAzure Event Hubs using hello .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) article, or click hello appropriate sending language in hello left-hand table of contents.</span></span>

<span data-ttu-id="7372c-110">Hej [värd för händelsebearbetning] [ EventProcessorHost] är en .NET-klass som förenklar mottagandet av händelser från event hubs genom att hantera permanenta kontrollpunkter och parallella mottaganden från event hubs.</span><span class="sxs-lookup"><span data-stu-id="7372c-110">hello [Event Processor Host][EventProcessorHost] is a .NET class that simplifies receiving events from event hubs by managing persistent checkpoints and parallel receives from those event hubs.</span></span> <span data-ttu-id="7372c-111">Med hjälp av hello [värd för händelsebearbetning][Event Processor Host], du kan dela upp händelser på flera olika mottagare, även när de ligger på olika noder.</span><span class="sxs-lookup"><span data-stu-id="7372c-111">Using hello [Event Processor Host][Event Processor Host], you can split events across multiple receivers, even when hosted in different nodes.</span></span> <span data-ttu-id="7372c-112">Det här exemplet illustrerar hur toouse hello [värd för händelsebearbetning] [ EventProcessorHost] för en enda mottagare.</span><span class="sxs-lookup"><span data-stu-id="7372c-112">This example shows how toouse hello [Event Processor Host][EventProcessorHost] for a single receiver.</span></span> <span data-ttu-id="7372c-113">Hej [skala ut händelsebearbetning] [ Scale out Event Processing with Event Hubs] exempel visar hur toouse hello [värd för händelsebearbetning] [ EventProcessorHost] med flera mottagare.</span><span class="sxs-lookup"><span data-stu-id="7372c-113">hello [Scale out event processing][Scale out Event Processing with Event Hubs] sample shows how toouse hello [Event Processor Host][EventProcessorHost] with multiple receivers.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7372c-114">Krav</span><span class="sxs-lookup"><span data-stu-id="7372c-114">Prerequisites</span></span>

<span data-ttu-id="7372c-115">toocomplete den här kursen behöver du hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="7372c-115">toocomplete this tutorial, you need hello following prerequisites:</span></span>

* <span data-ttu-id="7372c-116">[Microsoft Visual Studio 2015 eller senare](http://visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="7372c-116">[Microsoft Visual Studio 2015 or higher](http://visualstudio.com).</span></span> <span data-ttu-id="7372c-117">hello skärmdumpar i den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="7372c-117">hello screen shots in this tutorial use Visual Studio 2017.</span></span>
* <span data-ttu-id="7372c-118">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="7372c-118">An active Azure account.</span></span> <span data-ttu-id="7372c-119">Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter.</span><span class="sxs-lookup"><span data-stu-id="7372c-119">If you don't have one, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="7372c-120">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="7372c-120">For details, see [Azure Free Trial](https://azure.microsoft.com/free/).</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="7372c-121">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="7372c-121">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="7372c-122">hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate en namnrymd Skriv Händelsehubbar och hämta hello management-autentiseringsuppgifter som programmet behöver toocommunicate med hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="7372c-122">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace of type Event Hubs, and obtain hello management credentials your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="7372c-123">toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), fortsätt sedan med hello följa stegen i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="7372c-123">toocreate a namespace and event hub, follow hello procedure in [this article](event-hubs-create.md), then proceed with hello following steps in this tutorial.</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="7372c-124">Skapa ett Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="7372c-124">Create an Azure Storage account</span></span>

<span data-ttu-id="7372c-125">toouse hello [värd för händelsebearbetning][EventProcessorHost], måste du ha en [Azure Storage-konto][Azure Storage account]:</span><span class="sxs-lookup"><span data-stu-id="7372c-125">toouse hello [Event Processor Host][EventProcessorHost], you must have an [Azure Storage account][Azure Storage account]:</span></span>

1. <span data-ttu-id="7372c-126">Logga in toohello [Azure-portalen][Azure portal], och klicka på **ny** på hello upp till vänster i hello-skärmen.</span><span class="sxs-lookup"><span data-stu-id="7372c-126">Log on toohello [Azure portal][Azure portal], and click **New** at hello top left of hello screen.</span></span>
2. <span data-ttu-id="7372c-127">Klicka på **Lagring** och sedan på **Lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="7372c-127">Click **Storage**, then click **Storage account**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. <span data-ttu-id="7372c-128">I hello **skapa lagringskonto** bladet, ange ett namn för hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="7372c-128">In hello **Create storage account** blade, type a name for hello storage account.</span></span> <span data-ttu-id="7372c-129">Välj en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs.</span><span class="sxs-lookup"><span data-stu-id="7372c-129">Choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="7372c-130">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7372c-130">Then click **Create**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. <span data-ttu-id="7372c-131">Hello listan med lagringskonton, på hello nyligen skapade lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="7372c-131">In hello list of storage accounts, click hello newly created storage account.</span></span>
5. <span data-ttu-id="7372c-132">I hello lagring-kontoblad klickar du på **åtkomstnycklar**.</span><span class="sxs-lookup"><span data-stu-id="7372c-132">In hello storage account blade, click **Access keys**.</span></span> <span data-ttu-id="7372c-133">Kopiera hello värdet för **key1** toouse senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="7372c-133">Copy hello value of **key1** toouse later in this tutorial.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a><span data-ttu-id="7372c-134">Skapa ett mottagarkonsolprogram</span><span class="sxs-lookup"><span data-stu-id="7372c-134">Create a receiver console application</span></span>

1. <span data-ttu-id="7372c-135">I Visual Studio skapar du ett nytt Visual C#-Skrivbordsapprojekt-projekt med hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="7372c-135">In Visual Studio, create a new Visual C# Desktop App project using hello **Console  Application** project template.</span></span> <span data-ttu-id="7372c-136">Namnet hello projektet **mottagare**.</span><span class="sxs-lookup"><span data-stu-id="7372c-136">Name hello project **Receiver**.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. <span data-ttu-id="7372c-137">I Solution Explorer högerklickar du på hello **mottagare** projektet och klicka sedan på **hantera NuGet-paket för lösningen**.</span><span class="sxs-lookup"><span data-stu-id="7372c-137">In Solution Explorer, right-click hello **Receiver** project, and then click **Manage NuGet Packages for Solution**.</span></span>
3. <span data-ttu-id="7372c-138">Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span><span class="sxs-lookup"><span data-stu-id="7372c-138">Click hello **Browse** tab, then search for `Microsoft Azure Service Bus Event Hub - EventProcessorHost`.</span></span> <span data-ttu-id="7372c-139">Klicka på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="7372c-139">Click **Install**, and accept hello terms of use.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    <span data-ttu-id="7372c-140">Visual Studio hämtar, installerar och lägger till en referens toohello [Azure Service Bus Event Hub - EventProcessorHost NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), med dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="7372c-140">Visual Studio downloads, installs, and adds a reference toohello [Azure Service Bus Event Hub - EventProcessorHost NuGet package](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), with all its dependencies.</span></span>
4. <span data-ttu-id="7372c-141">Högerklicka på hello **mottagare** projektet, klicka på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="7372c-141">Right-click hello **Receiver** project, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="7372c-142">Namnge hello nya klassen **SimpleEventProcessor**, och klicka sedan på **Lägg till** toocreate hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="7372c-142">Name hello new class **SimpleEventProcessor**, and then click **Add** toocreate hello class.</span></span>
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. <span data-ttu-id="7372c-143">Lägg till följande instruktioner överst hello i hello SimpleEventProcessor.cs filen hello:</span><span class="sxs-lookup"><span data-stu-id="7372c-143">Add hello following statements at hello top of hello SimpleEventProcessor.cs file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  <span data-ttu-id="7372c-144">Ersätt sedan hello följande kod för hello hello klass:</span><span class="sxs-lookup"><span data-stu-id="7372c-144">Then, substitute hello following code for hello body of hello class:</span></span>
    
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
    
  <span data-ttu-id="7372c-145">Den här klassen anropas av hello **EventProcessorHost** tooprocess händelser som tagits emot från hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="7372c-145">This class is called by hello **EventProcessorHost** tooprocess events received from hello event hub.</span></span> <span data-ttu-id="7372c-146">Hej `SimpleEventProcessor` klassen använder ett stoppur tooperiodically hello kontrollpunkt telefonsamtalsmetoden på hello **EventProcessorHost** kontext.</span><span class="sxs-lookup"><span data-stu-id="7372c-146">hello `SimpleEventProcessor` class uses a stopwatch tooperiodically call hello checkpoint method on hello **EventProcessorHost** context.</span></span> <span data-ttu-id="7372c-147">Denna bearbetning gör att, om hello mottagaren startas förlorar högst fem minuters bearbetningsarbete.</span><span class="sxs-lookup"><span data-stu-id="7372c-147">This processing ensures that, if hello receiver is restarted, it loses no more than five minutes of processing work.</span></span>
6. <span data-ttu-id="7372c-148">I hello **programmet** klassen och Lägg till följande hello `using` instruktionen hello överst i filen hello:</span><span class="sxs-lookup"><span data-stu-id="7372c-148">In hello **Program** class, add hello following `using` statement at hello top of hello file:</span></span>
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  <span data-ttu-id="7372c-149">Ersätt sedan hello `Main` metod i hello `Program` klassen med följande kod hello, ersätter hello händelsehubbens namn och hello namnområdesnivå anslutning sträng som du sparade tidigare och hello lagringskontot och nyckeln som du kopierade i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7372c-149">Then, replace hello `Main` method in hello `Program` class with hello following code, substituting hello event hub name and hello namespace-level connection string that you saved previously, and hello storage account and key that you copied in hello previous sections.</span></span> 
    
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
    
    Console.WriteLine("Receiving. Press enter key toostop worker.");
    Console.ReadLine();
    eventProcessorHost.UnregisterEventProcessorAsync().Wait();
  }
  ```

7. <span data-ttu-id="7372c-150">Kör programmet hello och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="7372c-150">Run hello program, and ensure that there are no errors.</span></span>
  
<span data-ttu-id="7372c-151">Grattis!</span><span class="sxs-lookup"><span data-stu-id="7372c-151">Congratulations!</span></span> <span data-ttu-id="7372c-152">Du har nu fått meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.</span><span class="sxs-lookup"><span data-stu-id="7372c-152">You have now received messages from an event hub using hello Event Processor Host.</span></span>


> [!NOTE]
> <span data-ttu-id="7372c-153">Den här guiden använder en enda instans av [EventProcessorHost][EventProcessorHost].</span><span class="sxs-lookup"><span data-stu-id="7372c-153">This tutorial uses a single instance of [EventProcessorHost][EventProcessorHost].</span></span> <span data-ttu-id="7372c-154">tooincrease genomströmning rekommenderas att du kör flera instanser av [EventProcessorHost][EventProcessorHost]som visas i hello [skala ut händelsebearbetning] [skala ut händelsebearbetning] exempel.</span><span class="sxs-lookup"><span data-stu-id="7372c-154">tooincrease throughput, it is recommended that you run multiple instances of [EventProcessorHost][EventProcessorHost], as shown in hello [Scaled out event processing][Scaled out event processing] sample.</span></span> <span data-ttu-id="7372c-155">I sådana fall sinsemellan hello olika instanserna automatiskt tooload saldo hello emot händelser.</span><span class="sxs-lookup"><span data-stu-id="7372c-155">In those cases, hello various instances automatically coordinate with each other tooload balance hello received events.</span></span> <span data-ttu-id="7372c-156">Om du vill att flera mottagare tooeach processen *alla* hello händelser, måste du använda hello **ConsumerGroup** begrepp.</span><span class="sxs-lookup"><span data-stu-id="7372c-156">If you want multiple receivers tooeach process *all* hello events, you must use hello **ConsumerGroup** concept.</span></span> <span data-ttu-id="7372c-157">När du tar emot händelser från olika datorer, kan det vara användbart toospecify namn för [EventProcessorHost] [ EventProcessorHost] instanser baserat på hello datorer (eller roller) i som de har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="7372c-157">When receiving events from different machines, it might be useful toospecify names for [EventProcessorHost][EventProcessorHost] instances based on hello machines (or roles) in which they are deployed.</span></span> <span data-ttu-id="7372c-158">Mer information om de här ämnena finns hello [översikt av Händelsehubbar] [ Event Hubs overview] och hello [Programmeringsguide för Händelsehubbar] [ Event Hubs Programming Guide] avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7372c-158">For more information about these topics, see hello [Event Hubs overview][Event Hubs overview] and hello [Event Hubs programming guide][Event Hubs Programming Guide] topics.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7372c-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7372c-159">Next steps</span></span>

<span data-ttu-id="7372c-160">Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar och tar emot data, kan du lära dig mer genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="7372c-160">Now that you've built a working application that creates an event hub and sends and receives data, you can learn more by visiting hello following links:</span></span>

* <span data-ttu-id="7372c-161">[Värd för händelsebearbetning][Event Processor Host]</span><span class="sxs-lookup"><span data-stu-id="7372c-161">[Event Processor Host][Event Processor Host]</span></span>
* <span data-ttu-id="7372c-162">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="7372c-162">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="7372c-163">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="7372c-163">Event Hubs FAQ</span></span>](event-hubs-faq.md)

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
