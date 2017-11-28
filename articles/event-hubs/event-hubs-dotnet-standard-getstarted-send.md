---
title: "Skicka händelser till Händelsehubbar i Azure med hjälp av .NET Standard | Microsoft Docs"
description: "Komma igång med att skicka händelser till Händelsehubbar i .NET Standard"
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
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a><span data-ttu-id="54394-103">Komma igång med att skicka meddelanden till Azure Event Hubs i .NET Standard</span><span class="sxs-lookup"><span data-stu-id="54394-103">Get started sending messages to Azure Event Hubs in .NET Standard</span></span>

> [!NOTE]
> <span data-ttu-id="54394-104">Det här exemplet är tillgängligt på [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span><span class="sxs-lookup"><span data-stu-id="54394-104">This sample is available on [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).</span></span>

<span data-ttu-id="54394-105">Den här kursen visar hur du skriver ett .NET Core-konsolprogram som skickar meddelanden till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="54394-105">This tutorial shows how to write a .NET Core console application that sends a set of messages to an event hub.</span></span> <span data-ttu-id="54394-106">Du kan köra den [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) lösning som-, ersätter den `EhConnectionString` och `EhEntityPath` strängar med event hub värdena.</span><span class="sxs-lookup"><span data-stu-id="54394-106">You can run the [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) solution as-is, replacing the `EhConnectionString` and `EhEntityPath` strings with your event hub values.</span></span> <span data-ttu-id="54394-107">Eller du kan följa stegen i den här kursen hjälper dig att skapa en egen.</span><span class="sxs-lookup"><span data-stu-id="54394-107">Or you can follow the steps in this tutorial to create your own.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54394-108">Krav</span><span class="sxs-lookup"><span data-stu-id="54394-108">Prerequisites</span></span>

* <span data-ttu-id="54394-109">[Microsoft Visual Studio 2015 eller 2017](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="54394-109">[Microsoft Visual Studio 2015 or 2017](http://www.visualstudio.com).</span></span> <span data-ttu-id="54394-110">Exemplen i den här självstudiekursen används Visual Studio 2017 men Visual Studio 2015 stöds också.</span><span class="sxs-lookup"><span data-stu-id="54394-110">The examples in this tutorial use Visual Studio 2017, but Visual Studio 2015 is also supported.</span></span>
* <span data-ttu-id="54394-111">[.NET core Visual Studio 2015 eller 2017 verktyg](http://www.microsoft.com/net/core).</span><span class="sxs-lookup"><span data-stu-id="54394-111">[.NET Core Visual Studio 2015 or 2017 tools](http://www.microsoft.com/net/core).</span></span>
* <span data-ttu-id="54394-112">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54394-112">An Azure subscription.</span></span>
* <span data-ttu-id="54394-113">Ett event hub namnområde.</span><span class="sxs-lookup"><span data-stu-id="54394-113">An event hub namespace.</span></span>

<span data-ttu-id="54394-114">Om du vill skicka meddelanden till en händelsehubb, använder vi Visual Studio för att skriva ett C#-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="54394-114">To send messages to an event hub, we will use Visual Studio to write a C# console application.</span></span>

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a><span data-ttu-id="54394-115">Skapa ett namnområde för Event Hubs och en händelsehubb</span><span class="sxs-lookup"><span data-stu-id="54394-115">Create an Event Hubs namespace and an event hub</span></span>

<span data-ttu-id="54394-116">Det första steget är att använda den [Azure-portalen](https://portal.azure.com) att skapa ett namnområde för händelsetypen hubb och skaffa autentiseringsuppgifter för hantering som behövs i programmet för att kommunicera med händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="54394-116">The first step is to use the [Azure portal](https://portal.azure.com) to create a namespace for the event hub type, and obtain the management credentials that your application needs to communicate with the event hub.</span></span> <span data-ttu-id="54394-117">Om du vill skapa ett namnområde och en händelsehubb, följer du proceduren i [i den här artikeln](event-hubs-create.md), och fortsätt sedan med följande steg.</span><span class="sxs-lookup"><span data-stu-id="54394-117">To create a namespace and an event hub, follow the procedure in [this article](event-hubs-create.md), and then proceed with the following steps.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="54394-118">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="54394-118">Create a console application</span></span>

<span data-ttu-id="54394-119">Starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54394-119">Start Visual Studio.</span></span> <span data-ttu-id="54394-120">Klicka på **Nytt** i **Arkiv**-menyn och klicka sedan på **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="54394-120">From the **File** menu, click **New**, and then click **Project**.</span></span> <span data-ttu-id="54394-121">Skapa ett .NET Core-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="54394-121">Create a .NET Core console application.</span></span>

![Nytt projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a><span data-ttu-id="54394-123">Lägg till Event Hubs NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="54394-123">Add the Event Hubs NuGet package</span></span>

<span data-ttu-id="54394-124">Lägg till den [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard bibliotekets NuGet-paket i projektet genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="54394-124">Add the [`Microsoft.Azure.EventHubs`](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard library NuGet package to your project by following these steps:</span></span> 

1. <span data-ttu-id="54394-125">Högerklicka på det nyskapade projektet och välj **Hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="54394-125">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="54394-126">Klicka på den **Bläddra** fliken och Sök efter ”Microsoft.Azure.EventHubs” och välj sedan den **Microsoft.Azure.EventHubs** paketet.</span><span class="sxs-lookup"><span data-stu-id="54394-126">Click the **Browse** tab, then search for "Microsoft.Azure.EventHubs" and select the **Microsoft.Azure.EventHubs** package.</span></span> <span data-ttu-id="54394-127">Klicka på **Installera** för att slutföra installationen och stäng sedan den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="54394-127">Click **Install** to complete the installation, then close this dialog box.</span></span>

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a><span data-ttu-id="54394-128">Skriva kod för att skicka meddelanden till händelsehubben</span><span class="sxs-lookup"><span data-stu-id="54394-128">Write some code to send messages to the event hub</span></span>

1. <span data-ttu-id="54394-129">Lägg till följande `using`-instruktioner överst i Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="54394-129">Add the following `using` statements to the top of the Program.cs file.</span></span>

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. <span data-ttu-id="54394-130">Lägg till konstanter till den `Program` klass för Händelsehubbar sträng och entiteten anslutningssökvägen (enskilda händelsehubbens namn).</span><span class="sxs-lookup"><span data-stu-id="54394-130">Add constants to the `Program` class for the Event Hubs connection string and entity path (individual event hub name).</span></span> <span data-ttu-id="54394-131">Ersätt platshållarna med hakparenteser med rätt värden som erhölls när du skapar händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="54394-131">Replace the placeholders in brackets with the proper values that were obtained when creating the event hub.</span></span>

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. <span data-ttu-id="54394-132">Lägg till en ny metod med namnet `MainAsync` till den `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="54394-132">Add a new method named `MainAsync` to the `Program` class, as follows:</span></span>

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. <span data-ttu-id="54394-133">Lägg till en ny metod med namnet `SendMessagesToEventHub` till den `Program` class, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="54394-133">Add a new method named `SendMessagesToEventHub` to the `Program` class, as follows:</span></span>

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
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

5. <span data-ttu-id="54394-134">Lägg till följande kod i den `Main` metod i den `Program` klass.</span><span class="sxs-lookup"><span data-stu-id="54394-134">Add the following code to the `Main` method in the `Program` class.</span></span>

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   <span data-ttu-id="54394-135">Så här bör din Program.cs se ut.</span><span class="sxs-lookup"><span data-stu-id="54394-135">Here is what your Program.cs should look like.</span></span>

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
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
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

6. <span data-ttu-id="54394-136">Kör programmet och kontrollera att det inte finns några fel.</span><span class="sxs-lookup"><span data-stu-id="54394-136">Run the program, and ensure that there are no errors.</span></span>

<span data-ttu-id="54394-137">Grattis!</span><span class="sxs-lookup"><span data-stu-id="54394-137">Congratulations!</span></span> <span data-ttu-id="54394-138">Du har nu skickat meddelanden till en händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="54394-138">You have now sent messages to an event hub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54394-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54394-139">Next steps</span></span>
<span data-ttu-id="54394-140">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="54394-140">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="54394-141">Ta emot händelser från Event Hubs</span><span class="sxs-lookup"><span data-stu-id="54394-141">Receive events from Event Hubs</span></span>](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [<span data-ttu-id="54394-142">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="54394-142">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md)
* [<span data-ttu-id="54394-143">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="54394-143">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="54394-144">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="54394-144">Event Hubs FAQ</span></span>](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
