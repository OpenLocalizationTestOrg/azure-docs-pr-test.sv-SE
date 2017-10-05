---
title: "Komma igång med Azure Service Bus-köer | Microsoft Docs"
description: "Skriv ett C#-konsolprogram som använder meddelandeköer i Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.openlocfilehash: 99a377db6341d90d263b98e14227db61dd9beabd
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-service-bus-queues"></a><span data-ttu-id="9aaed-103">Komma igång med Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="9aaed-103">Get started with Service Bus queues</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="9aaed-104">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="9aaed-104">What will be accomplished</span></span>
<span data-ttu-id="9aaed-105">Den här självstudien omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="9aaed-105">This tutorial covers the following steps:</span></span>

1. <span data-ttu-id="9aaed-106">Skapa ett Service Bus-namnområde med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9aaed-106">Create a Service Bus namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="9aaed-107">Skapa en Service Bus-kö med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="9aaed-107">Create a Service Bus queue, using the Azure portal.</span></span>
3. <span data-ttu-id="9aaed-108">Skriv ett konsolprogram för att skicka ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="9aaed-108">Write a console application to send a message.</span></span>
4. <span data-ttu-id="9aaed-109">Skriv ett konsolprogram som hämtar meddelandet som skickades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="9aaed-109">Write a console application to receive the messages sent in the previous step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9aaed-110">Krav</span><span class="sxs-lookup"><span data-stu-id="9aaed-110">Prerequisites</span></span>
1. <span data-ttu-id="9aaed-111">[Visual Studio 2015 eller senare](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="9aaed-111">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="9aaed-112">I exemplen i den här självstudiekursen används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="9aaed-112">The examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="9aaed-113">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="9aaed-113">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="9aaed-114">1. Skapa ett namnområde med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9aaed-114">1. Create a namespace using the Azure portal</span></span>
<span data-ttu-id="9aaed-115">Om du redan har skapat ett namnområde för Service Bus-meddelanden går du vidare till avsnittet [Skapa en kö med hjälp av Azure Portal](#2-create-a-queue-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="9aaed-115">If you've already created a Service Bus Messaging namespace, jump to the [Create a queue using the Azure portal](#2-create-a-queue-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a><span data-ttu-id="9aaed-116">2. Skapa en kö med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9aaed-116">2. Create a queue using the Azure portal</span></span>
<span data-ttu-id="9aaed-117">Om du redan har skapat en Service Bus-kö går du vidare till avsnittet [Skicka meddelanden till kön](#3-send-messages-to-the-queue).</span><span class="sxs-lookup"><span data-stu-id="9aaed-117">If you have already created a Service Bus queue, jump to the [Send messages to the queue](#3-send-messages-to-the-queue) section.</span></span>

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a><span data-ttu-id="9aaed-118">3. Skicka meddelanden till kön</span><span class="sxs-lookup"><span data-stu-id="9aaed-118">3. Send messages to the queue</span></span>
<span data-ttu-id="9aaed-119">Vi skriver ett C#-konsolprogram med Visual Studio för att skicka meddelanden till kön.</span><span class="sxs-lookup"><span data-stu-id="9aaed-119">To send messages to the queue, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="9aaed-120">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="9aaed-120">Create a console application</span></span>

<span data-ttu-id="9aaed-121">Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="9aaed-121">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-the-service-bus-nuget-package"></a><span data-ttu-id="9aaed-122">Lägga till Service Bus-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="9aaed-122">Add the Service Bus NuGet package</span></span>
1. <span data-ttu-id="9aaed-123">Högerklicka på det nyskapade projektet och välj **Hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="9aaed-123">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="9aaed-124">Klicka på fliken **Bläddra**, sök efter **Microsoft Azure Service Bus** och markera posten **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="9aaed-124">Click the **Browse** tab, search for **Microsoft Azure Service Bus**, and then select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="9aaed-125">Klicka på **Installera** för att slutföra installationen och stäng sedan den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="9aaed-125">Click **Install** to complete the installation, then close this dialog box.</span></span>
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a><span data-ttu-id="9aaed-127">Skriva kod för att skicka ett meddelande till kön</span><span class="sxs-lookup"><span data-stu-id="9aaed-127">Write some code to send a message to the queue</span></span>
1. <span data-ttu-id="9aaed-128">Lägg till följande `using`-instruktion högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="9aaed-128">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="9aaed-129">Lägg till följande kod i metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="9aaed-129">Add the following code to the `Main` method.</span></span> <span data-ttu-id="9aaed-130">Ställ in variabeln `connectionString` på den anslutningssträng du fick när du skapade namnområdet, och ställ in `queueName` på namnet du använde när du skapade kön.</span><span class="sxs-lookup"><span data-stu-id="9aaed-130">Set the `connectionString` variable to the connection string that you obtained when creating the namespace, and set `queueName` to the queue name that you used when creating the queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="9aaed-131">Så här bör filen Program.cs se ut:</span><span class="sxs-lookup"><span data-stu-id="9aaed-131">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="9aaed-132">Kör programmet och kontrollera Azure Portal: klicka på köns namn på bladet **Översikt** för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="9aaed-132">Run the program, and check the Azure portal: click the name of your queue in the namespace **Overview** blade.</span></span> <span data-ttu-id="9aaed-133">Bladet **Grundläggande** för kön visas.</span><span class="sxs-lookup"><span data-stu-id="9aaed-133">The queue **Essentials** blade is displayed.</span></span> <span data-ttu-id="9aaed-134">Observera att värdet för **Antal aktiva meddelanden** nu bör vara 1.</span><span class="sxs-lookup"><span data-stu-id="9aaed-134">Notice that the **Active Message Count** value should now be 1.</span></span> <span data-ttu-id="9aaed-135">Varje gång du kör sändningsprogrammet utan att hämta meddelanden ökar detta värde med 1.</span><span class="sxs-lookup"><span data-stu-id="9aaed-135">Each time you run the sender application without retrieving the messages, this value increases by 1.</span></span> <span data-ttu-id="9aaed-136">Observera också att den aktuella storleken för kön ökar varje gång programmet lägger till ett meddelande i kön.</span><span class="sxs-lookup"><span data-stu-id="9aaed-136">Also note that the current size of the queue increments each time the app adds a message to the queue.</span></span>
   
      ![Meddelandestorlek][queue-message]

## <a name="4-receive-messages-from-the-queue"></a><span data-ttu-id="9aaed-138">4. Ta emot meddelanden från kön</span><span class="sxs-lookup"><span data-stu-id="9aaed-138">4. Receive messages from the queue</span></span>

1. <span data-ttu-id="9aaed-139">Om du vill ta emot de meddelanden du nyss skickade skapar du ett nytt konsolprogram och lägger till en referens till Service Bus NuGet-paketet, ungefär som i det tidigare sändningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="9aaed-139">To receive the messages you just sent, create a new console application and add a reference to the Service Bus NuGet package, similar to the previous sender application.</span></span>
2. <span data-ttu-id="9aaed-140">Lägg till följande `using`-instruktion högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="9aaed-140">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="9aaed-141">Lägg till följande kod i metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="9aaed-141">Add the following code to the `Main` method.</span></span> <span data-ttu-id="9aaed-142">Ställ in variabeln `connectionString` på den anslutningssträng du fick när du skapade namnområdet, och ställ in `queueName` på namnet du använde när du skapade kön.</span><span class="sxs-lookup"><span data-stu-id="9aaed-142">Set the `connectionString` variable to the connection string that was obtained when creating the namespace, and set `queueName` to the queue name that you used when creating the queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="9aaed-143">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="9aaed-143">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="9aaed-144">Kör programmet och kontrollera portalen igen.</span><span class="sxs-lookup"><span data-stu-id="9aaed-144">Run the program, and check the portal again.</span></span> <span data-ttu-id="9aaed-145">Observera att värdena för **Antal aktiva meddelanden** och **Aktuell** nu är 0.</span><span class="sxs-lookup"><span data-stu-id="9aaed-145">Notice that the **Active Message Count** and **Current** values are now 0.</span></span>
   
    ![Kölängd][queue-message-receive]

<span data-ttu-id="9aaed-147">Grattis!</span><span class="sxs-lookup"><span data-stu-id="9aaed-147">Congratulations!</span></span> <span data-ttu-id="9aaed-148">Nu har du skapat en kö, skickat ett meddelande och tagit emot ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="9aaed-148">You have now created a queue, sent a message, and received a message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9aaed-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9aaed-149">Next steps</span></span>

<span data-ttu-id="9aaed-150">Kolla in våra [GitHub-databaser med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som visar några av de mer avancerade funktionerna i meddelandetjänsten i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="9aaed-150">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of the more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
