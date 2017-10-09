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
# <a name="receive-events-from-azure-event-hubs-using-hello-net-framework"></a>Ta emot händelser från Azure Event Hubs med hello .NET Framework

## <a name="introduction"></a>Introduktion

Händelsehubbar är en tjänst som bearbetar stora mängder händelsedata (telemetri) från anslutna enheter och program. När du samlar in data i Händelsehubbar kan du lagra hello data med ett lagringskluster eller omvandla dem med hjälp av en leverantör av realtidsanalys. Den här storskaliga händelse och bearbetningsfunktionen är en viktig komponent inom moderna programarkitekturer inklusive hello Sakernas Internet (IoT).

Den här kursen visar hur toowrite ett .NET Framework-konsolapp som tar emot meddelanden från en händelsehubb med hjälp av hello  **[värd för händelsebearbetning][EventProcessorHost]**. toosend händelser med hjälp av hello .NET Framework finns hello [skicka händelser tooAzure Händelsehubbar med hello .NET Framework](event-hubs-dotnet-framework-getstarted-send.md) artikel, eller klicka på hello skicka språket i hello vänstra innehållsförteckning.

Hej [värd för händelsebearbetning] [ EventProcessorHost] är en .NET-klass som förenklar mottagandet av händelser från event hubs genom att hantera permanenta kontrollpunkter och parallella mottaganden från event hubs. Med hjälp av hello [värd för händelsebearbetning][Event Processor Host], du kan dela upp händelser på flera olika mottagare, även när de ligger på olika noder. Det här exemplet illustrerar hur toouse hello [värd för händelsebearbetning] [ EventProcessorHost] för en enda mottagare. Hej [skala ut händelsebearbetning] [ Scale out Event Processing with Event Hubs] exempel visar hur toouse hello [värd för händelsebearbetning] [ EventProcessorHost] med flera mottagare.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen behöver du hello följande krav:

* [Microsoft Visual Studio 2015 eller senare](http://visualstudio.com). hello skärmdumpar i den här självstudiekursen använder Visual Studio 2017.
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett utan kostnad på ett par minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Skapa ett namnområde för Event Hubs och en händelsehubb

hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate en namnrymd Skriv Händelsehubbar och hämta hello management-autentiseringsuppgifter som programmet behöver toocommunicate med hello händelsehubb. toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), fortsätt sedan med hello följa stegen i den här självstudiekursen.

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure Storage-konto

toouse hello [värd för händelsebearbetning][EventProcessorHost], måste du ha en [Azure Storage-konto][Azure Storage account]:

1. Logga in toohello [Azure-portalen][Azure portal], och klicka på **ny** på hello upp till vänster i hello-skärmen.
2. Klicka på **Lagring** och sedan på **Lagringskonto**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. I hello **skapa lagringskonto** bladet, ange ett namn för hello storage-konto. Välj en Azure-prenumeration, resursgrupp och plats i vilken toocreate hello-resurs. Klicka sedan på **Skapa**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Hello listan med lagringskonton, på hello nyligen skapade lagringskontot.
5. I hello lagring-kontoblad klickar du på **åtkomstnycklar**. Kopiera hello värdet för **key1** toouse senare i den här kursen.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

## <a name="create-a-receiver-console-application"></a>Skapa ett mottagarkonsolprogram

1. I Visual Studio skapar du ett nytt Visual C#-Skrivbordsapprojekt-projekt med hello **konsolprogram** projektmall. Namnet hello projektet **mottagare**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
2. I Solution Explorer högerklickar du på hello **mottagare** projektet och klicka sedan på **hantera NuGet-paket för lösningen**.
3. Klicka på hello **Bläddra** fliken, och sök sedan efter `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Klicka på **installera**, och Godkänn hello villkor för användning.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Visual Studio hämtar, installerar och lägger till en referens toohello [Azure Service Bus Event Hub - EventProcessorHost NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost), med dess beroenden.
4. Högerklicka på hello **mottagare** projektet, klicka på **Lägg till**, och klicka sedan på **klassen**. Namnge hello nya klassen **SimpleEventProcessor**, och klicka sedan på **Lägg till** toocreate hello-klassen.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
5. Lägg till följande instruktioner överst hello i hello SimpleEventProcessor.cs filen hello:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  using System.Diagnostics;
  ```
    
  Ersätt sedan hello följande kod för hello hello klass:
    
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
    
  Den här klassen anropas av hello **EventProcessorHost** tooprocess händelser som tagits emot från hello händelsehubb. Hej `SimpleEventProcessor` klassen använder ett stoppur tooperiodically hello kontrollpunkt telefonsamtalsmetoden på hello **EventProcessorHost** kontext. Denna bearbetning gör att, om hello mottagaren startas förlorar högst fem minuters bearbetningsarbete.
6. I hello **programmet** klassen och Lägg till följande hello `using` instruktionen hello överst i filen hello:
    
  ```csharp
  using Microsoft.ServiceBus.Messaging;
  ```
    
  Ersätt sedan hello `Main` metod i hello `Program` klassen med följande kod hello, ersätter hello händelsehubbens namn och hello namnområdesnivå anslutning sträng som du sparade tidigare och hello lagringskontot och nyckeln som du kopierade i hello föregående avsnitt. 
    
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

7. Kör programmet hello och kontrollera att det inte finns några fel.
  
Grattis! Du har nu fått meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.


> [!NOTE]
> Den här guiden använder en enda instans av [EventProcessorHost][EventProcessorHost]. tooincrease genomströmning rekommenderas att du kör flera instanser av [EventProcessorHost][EventProcessorHost]som visas i hello [skala ut händelsebearbetning] [skala ut händelsebearbetning] exempel. I sådana fall sinsemellan hello olika instanserna automatiskt tooload saldo hello emot händelser. Om du vill att flera mottagare tooeach processen *alla* hello händelser, måste du använda hello **ConsumerGroup** begrepp. När du tar emot händelser från olika datorer, kan det vara användbart toospecify namn för [EventProcessorHost] [ EventProcessorHost] instanser baserat på hello datorer (eller roller) i som de har distribuerats. Mer information om de här ämnena finns hello [översikt av Händelsehubbar] [ Event Hubs overview] och hello [Programmeringsguide för Händelsehubbar] [ Event Hubs Programming Guide] avsnitt.
> 
> 

## <a name="next-steps"></a>Nästa steg

Nu när du har skapat ett fungerande program som skapar en händelsehubb och skickar och tar emot data, kan du lära dig mer genom att besöka hello följande länkar:

* [Värd för händelsebearbetning][Event Processor Host]
* [Översikt av händelsehubbar][Event Hubs overview]
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)

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
