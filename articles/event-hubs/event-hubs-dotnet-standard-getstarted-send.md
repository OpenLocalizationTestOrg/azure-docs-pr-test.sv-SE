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
# <a name="get-started-sending-messages-tooazure-event-hubs-in-net-standard"></a><span data-ttu-id="79d1c-103">Komma igång med att skicka meddelanden tooAzure Händelsehubbar i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="79d1c-103">Get started sending messages tooAzure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="79d1c-104">Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="79d1c-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="79d1c-105">Den här kursen visar hur toowrite ett .NET Core-konsolprogram som skickar en uppsättning meddelanden tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="79d1c-105">This tutorial shows how toowrite a .NET Core console application that sends a set of messages tooan event hub.</span></span> <span data-ttu-id="79d1c-106">Du kan köra hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) lösning som-, ersätter hello `EhConnectionString` och `EhEntityPath` strängar med event hub värdena.</span><span class="sxs-lookup"><span data-stu-id="79d1c-106">You can run hello [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing hello `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="79d1c-107">Eller du kan följa hello stegen i den här självstudiekursen toocreate egna.</span><span class="sxs-lookup"><span data-stu-id="79d1c-107">Or you can follow hello steps in this tutorial toocreate your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79d1c-108">Krav</span><span class="sxs-lookup"><span data-stu-id="79d1c-108">Prerequisites</span></span>

* <span data-ttu-id="79d1c-109">[Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="79d1c-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="79d1c-110">hello exemplen i den här självstudiekursen används Visual Studio 2017, men Visual Studio 2015 stöds också.</span><span class="sxs-lookup"><span data-stu-id="79d1c-110">hello examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="79d1c-111">[.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="79d1c-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="79d1c-112">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="79d1c-112">An Azure subscription.</span></span>
* <span data-ttu-id="79d1c-113">Ett event hub namnområde.</span><span class="sxs-lookup"><span data-stu-id="79d1c-113">An event hub namespace.</span></span>

<span data-ttu-id="79d1c-114">toosend meddelanden tooan händelsehubb, vi använder Visual Studio toowrite ett C#-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="79d1c-114">toosend messages tooan event hub, we will use Visual Studio toowrite a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="79d1c-115">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="79d1c-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="79d1c-116">hello första steget är toouse hello [Azure-portalen](https://portal.azure.com) toocreate ett namnområde för hello händelsetyp till hubben, och få hello autentiseringsuppgifter för hantering att ditt program måste toocommunicate med hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="79d1c-116">hello first step is toouse hello [Azure portal](https://portal.azure.com) toocreate a namespace for hello event hub type, and obtain hello management credentials that your application needs toocommunicate with hello event hub.</span></span> <span data-ttu-id="79d1c-117">toocreate ett namnområde och en händelsehubb proceduren hello i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="79d1c-117">toocreate a namespace and an event hub, follow hello procedure in [this article](event-hubs-create.md), and then proceed with hello following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="79d1c-118">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="79d1c-118">Create a console application</span></span>

<span data-ttu-id="79d1c-119">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="79d1c-119">Start Visual Studio.</span></span> <span data-ttu-id="79d1c-120">Från hello **filen** -menyn klickar du på **ny**, och klicka sedan på **projekt**.</span><span class="sxs-lookup"><span data-stu-id="79d1c-120">From hello **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="79d1c-121">Skapa ett .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="79d1c-121">Create a .NET Core console application.</span></span>

![Nytt projekt][1]

## <a name="add-hello-event-hubs-nuget-package"></a><span data-ttu-id="79d1c-123">Lägg till hello Event Hubs NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="79d1c-123">Add hello Event Hubs NuGet package</span></span>

<span data-ttu-id="79d1c-124">Lägg till hello [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard NuGet-paketet tooyour biblioteksprojekt genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="79d1c-124">Add hello [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package tooyour project by following these steps:</span></span> 

1. <span data-ttu-id="79d1c-125">Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="79d1c-125">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="79d1c-126">Klicka på hello **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj hello **Microsoft.Azure.EventHubs** paketet.</span><span class="sxs-lookup"><span data-stu-id="79d1c-126">Click hello **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select hello **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="79d1c-127">Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="79d1c-127">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>

## <a name="write-some-code-toosend-messages-toohello-event-hub"></a><span data-ttu-id="79d1c-128">Skriva vissa kod toosend meddelanden toohello händelsehubb</span><span class="sxs-lookup"><span data-stu-id="79d1c-128">Write some code toosend messages toohello event hub</span></span>

1. <span data-ttu-id="79d1c-129">Lägg till följande hello `using` instruktioner toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="79d1c-129">Add hello following `using` statements toohello top of hello Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="79d1c-130">Lägg till konstanter toohello `Program` klass för hello Händelsehubbar sträng och entiteten anslutningssökväg (enskilda händelsehubbens namn).</span><span class="sxs-lookup"><span data-stu-id="79d1c-130">Add constants toohello `Program` class for hello Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="79d1c-131">Ersätt hello platshållare inom parentes med hello rätt värden som erhölls när du skapar hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="79d1c-131">Replace hello placeholders in brackets with hello proper values that were obtained when creating hello event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="79d1c-132">Lägg till en ny metod med namnet `MainAsync` toohello `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="79d1c-132">Add a new method named `MainAsync` toohello `Program` class, as follows:</span></span>

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

4. <span data-ttu-id="79d1c-133">Lägg till en ny metod med namnet `SendMessagesToEventHub` toohello `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="79d1c-133">Add a new method named `SendMessagesToEventHub` toohello `Program` class, as follows:</span></span>

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

5. <span data-ttu-id="79d1c-134">Lägg till följande kod toohello hello `Main` metod i hello `Program` klass.</span><span class="sxs-lookup"><span data-stu-id="79d1c-134">Add hello following code toohello `Main` method in hello `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="79d1c-135">Så här bör din Program.cs se ut.</span><span class="sxs-lookup"><span data-stu-id="79d1c-135">Here is what your Program.cs should look like.</span></span>

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

6. <span data-ttu-id="79d1c-136">Kör programmet hello och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="79d1c-136">Run hello program, and ensure that there are no errors.</span></span>

<span data-ttu-id="79d1c-137">Grattis!</span><span class="sxs-lookup"><span data-stu-id="79d1c-137">Congratulations!</span></span> <span data-ttu-id="79d1c-138">Du har nu skickar meddelanden tooan händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="79d1c-138">You have now sent messages tooan event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="79d1c-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79d1c-139">Next steps</span></span>
<span data-ttu-id="79d1c-140">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="79d1c-140">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="79d1c-141">Ta emot händelser från Event Hubs</span><span class="sxs-lookup"><span data-stu-id="79d1c-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="79d1c-142">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="79d1c-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="79d1c-143">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="79d1c-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="79d1c-144">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="79d1c-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
