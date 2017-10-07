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
# <a name="get-started-receiving-messages-with-hello-event-processor-host-in-net-standard"></a>Börja ta emot meddelanden med hello värd för händelsebearbetning i .NET Standard

> [!NOTE]
> Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Den här kursen visar hur toowrite en .NET Core konsolen program som tar emot meddelanden från en händelsehubb med hjälp av **EventProcessorHost**. Du kan köra hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) lösning som-, ersätter hello strängar med händelsen NAV- och storage-konto värdena. Eller du kan följa hello stegen i den här självstudiekursen toocreate egna.

## <a name="prerequisites"></a>Krav

* [Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com). hello exemplen i den här självstudiekursen används Visual Studio 2017, men Visual Studio 2015 stöds också.
* [.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).
* En Azure-prenumeration.
* Ett namnområde för Händelsehubbar i Azure.
* Ett Azure-lagringskonto.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Skapa ett namnområde för Event Hubs och en händelsehubb  

hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate ett namnområde för hello Händelsehubbar Skriv och hämta hello autentiseringsuppgifter för hantering att ditt program måste toocommunicate med hello händelsehubb. toocreate ett namnområde och händelsehubb, följer du proceduren hello i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med hello följande steg.  

## <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto  

1. Logga in toohello [Azure-portalen](https://portal.azure.com).  
2. Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klickar du på **lagring**, och klicka sedan på **Lagringskonto**.  
3. Slutför hello fälten i hello lagring-bladet för kontot och klicka sedan på **skapa**.

    ![Skapa lagringskonto][1]

4. Efter att du ser hello **distributioner lyckades** klickar du på hello namnet på hello nytt lagringskonto. I hello **Essentials** bladet, klickar du på **Blobbar**. När hello **Blob-tjänst** blad öppnas, klickar du på **+ behållare** hello överst. Namnge hello behållare och stäng sedan hello **Blob-tjänst** bladet.  
5. Klicka på **åtkomstnycklar** i hello vänstra bladet och kopiera hello namn hello lagringsbehållaren, hello storage-konto och hello värdet av **key1**. Spara dessa värden tooNotepad eller en tillfällig plats.  

## <a name="create-a-console-application"></a>Skapa ett konsolprogram

Starta Visual Studio. Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**. Skapa ett .NET Core-konsolprogram.

![Nytt projekt][2]

## <a name="add-hello-event-hubs-nuget-package"></a>Lägg till hello Event Hubs NuGet-paketet

Lägg till hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) och [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) .NET Standard NuGet-paket tooyour biblioteksprojekt genom att följa dessa steg: 

1. Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.
2. Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj hello **Microsoft.Azure.EventHubs** paketet. Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.
3. Upprepa steg 1 och 2 och installera hello **Microsoft.Azure.EventHubs.Processor** paketet.

## <a name="implement-hello-ieventprocessor-interface"></a>Implementera hello IEventProcessor-gränssnittet

1. I Solution Explorer, högerklicka på hello projektet klickar du på **Lägg till**, och klicka sedan på **klassen**. Namnge hello nya klassen **SimpleEventProcessor**.

2. Öppna hello SimpleEventProcessor.cs filen och Lägg till följande hello `using` instruktioner toohello överkant hello-filen.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementera hello `IEventProcessor` gränssnitt. Ersätt hello hela innehållet i hello `SimpleEventProcessor` klassen med följande kod hello:

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

## <a name="write-a-main-console-method-that-uses-hello-simpleeventprocessor-class-tooreceive-messages"></a>Skriv en huvudkonsolen-metod som använder tooreceive hälsningsmeddelande SimpleEventProcessor klass

1. Lägg till följande hello `using` instruktioner toohello överkant hello Program.cs-filen.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Lägg till konstanter toohello `Program` klass för anslutningssträngen för hello event hub, händelsehubbens namn, behållare för lagringskontonamnet, lagringskontonamnet och lagringskontonyckel. Lägg till hello följande kod, ersätta hello platshållarna med motsvarande värden.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Lägg till en ny metod med namnet `MainAsync` toohello `Program` class, enligt följande:

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

3. Lägg till följande rad med kod toohello hello `Main` metoden:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Så här bör din Program.cs-fil se ut:

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

4. Kör programmet hello och kontrollera att det inte finns några fel.

Grattis! Du har nu fått meddelanden från en händelsehubb med hjälp av hello värd för händelsebearbetning.

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en Event Hub](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
