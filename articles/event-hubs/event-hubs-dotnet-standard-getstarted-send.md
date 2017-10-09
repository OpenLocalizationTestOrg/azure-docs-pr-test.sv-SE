---
title: "aaaSend händelser tooAzure Händelsehubbar med .NET Standard | Microsoft Docs"
description: "Komma igång med att skicka händelser tooEvent hubbar i .NET Standard"
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
ms.openlocfilehash: caa9747a8a72aa8e7aea1348a116f6e4b406460e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a>Komma igång med att skicka meddelanden tooAzure Händelsehubbar i .NET Standard

> [!NOTE]
> Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

Den här kursen visar hur toowrite ett .NET Core-konsolprogram som skickar en uppsättning meddelanden tooan händelsehubb. Du kan köra hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) lösning som-, ersätter hello `EhConnectionString` och `EhEntityPath` strängar med event hub värdena. Eller du kan följa hello stegen i den här självstudiekursen toocreate egna.

## <a name="prerequisites"></a>Krav

* [Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com). hello exemplen i den här självstudiekursen används Visual Studio 2017, men Visual Studio 2015 stöds också.
* [.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).
* En Azure-prenumeration.
* Ett event hub namnområde.

toosend meddelanden tooan händelsehubb, vi använder Visual Studio toowrite ett C#-konsolprogram.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Skapa ett namnområde för Event Hubs och en händelsehubb

hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate ett namnområde för hello händelsetyp till hubben, och få hello autentiseringsuppgifter för hantering att ditt program måste toocommunicate med hello händelsehubb. toocreate ett namnområde och en händelsehubb proceduren hello i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med hello följande steg.

## <a name="create-a-console-application"></a>Skapa ett konsolprogram

Starta Visual Studio. Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**. Skapa ett .NET Core-konsolprogram.

![Nytt projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a>Lägg till hello Event Hubs NuGet-paketet

Lägg till hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard NuGet-paketet tooyour biblioteksprojekt genom att följa dessa steg: 

1. Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.
2. Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj hello **Microsoft.Azure.EventHubs** paketet. Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a>Skriva vissa kod toosend meddelanden toohello händelsehubb

1. Lägg till följande hello `using` instruktioner toohello överkant hello Program.cs-filen.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Lägg till konstanter toohello `Program` klass för hello Händelsehubbar sträng och entiteten anslutningssökväg (enskilda händelsehubbens namn). Ersätt hello platshållare inom parentes med hello rätt värden som erhölls när du skapar hello händelsehubb.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Lägg till en ny metod med namnet `MainAsync` toohello `Program` class, enligt följande:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
        // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
        // we are using hello connection string from hello namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER tooexit.");
        Console.ReadLine();
    }
    ```

4. Lägg till en ny metod med namnet `SendMessagesToEventHub` toohello `Program` class, enligt följande:

    ```csharp
    // Creates an event hub client and sends 100 messages toohello event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Lägg till följande kod toohello hello `Main` metod i hello `Program` klass.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Så här bör din Program.cs se ut.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from hello connection string, and sets hello EntityPath.
                // Typically, hello connection string should have hello entity path in it, but for hello sake of this simple scenario
                // we are using hello connection string from hello namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER tooexit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages toohello event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Kör programmet hello och kontrollera att det inte finns några fel.

Grattis! Du har nu skickar meddelanden tooan händelsehubb.

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Ta emot händelser från Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md)
* [Skapa en Event Hub](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
