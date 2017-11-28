---
title: "aaaGet igång med Azure Service Bus-köer | Microsoft Docs"
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
ms.openlocfilehash: eaa362ab0eabd2427977398c1deab5dc00105ae9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-queues"></a><span data-ttu-id="6589c-103">Komma igång med Service Bus-köer</span><span class="sxs-lookup"><span data-stu-id="6589c-103">Get started with Service Bus queues</span></span>
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="6589c-104">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="6589c-104">What will be accomplished</span></span>
<span data-ttu-id="6589c-105">Den här kursen ingår hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="6589c-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="6589c-106">Skapa ett namnområde för Service Bus använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6589c-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="6589c-107">Skapa en Service Bus-kö med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6589c-107">Create a Service Bus queue, using hello Azure portal.</span></span>
3. <span data-ttu-id="6589c-108">Skriva en konsol programmet toosend ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="6589c-108">Write a console application toosend a message.</span></span>
4. <span data-ttu-id="6589c-109">Skriv en konsol programmet tooreceive hello skickade meddelanden i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="6589c-109">Write a console application tooreceive hello messages sent in hello previous step.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6589c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6589c-110">Prerequisites</span></span>
1. <span data-ttu-id="6589c-111">[Visual Studio 2015 eller senare](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="6589c-111">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="6589c-112">hello exemplen i den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6589c-112">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="6589c-113">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="6589c-113">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="6589c-114">1. Skapa ett namnområde med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6589c-114">1. Create a namespace using hello Azure portal</span></span>
<span data-ttu-id="6589c-115">Om du redan har skapat ett namnområde för Service Bus-meddelanden hoppa toohello [skapar en kö med hello Azure-portalen](#2-create-a-queue-using-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6589c-115">If you've already created a Service Bus Messaging namespace, jump toohello [Create a queue using hello Azure portal](#2-create-a-queue-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a><span data-ttu-id="6589c-116">2. Skapa en kö med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="6589c-116">2. Create a queue using hello Azure portal</span></span>
<span data-ttu-id="6589c-117">Om du redan har skapat en Service Bus-kö hoppa toohello [överföringskön meddelanden toohello](#3-send-messages-to-the-queue) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6589c-117">If you have already created a Service Bus queue, jump toohello [Send messages toohello queue](#3-send-messages-to-the-queue) section.</span></span>

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a><span data-ttu-id="6589c-118">3. Skicka meddelanden toohello kön</span><span class="sxs-lookup"><span data-stu-id="6589c-118">3. Send messages toohello queue</span></span>
<span data-ttu-id="6589c-119">toosend toohello kön, vi skriva en C#-konsolapp med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6589c-119">toosend messages toohello queue, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="6589c-120">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="6589c-120">Create a console application</span></span>

<span data-ttu-id="6589c-121">Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="6589c-121">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="6589c-122">Lägg till hello Service Bus-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="6589c-122">Add hello Service Bus NuGet package</span></span>
1. <span data-ttu-id="6589c-123">Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="6589c-123">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="6589c-124">Klicka på hello **Bläddra** fliken, söka efter **Microsoft Azure Service Bus**, och välj sedan hello **WindowsAzure.ServiceBus** objekt.</span><span class="sxs-lookup"><span data-stu-id="6589c-124">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="6589c-125">Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="6589c-125">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a><span data-ttu-id="6589c-127">Skriva vissa koden toosend en meddelandekö toohello</span><span class="sxs-lookup"><span data-stu-id="6589c-127">Write some code toosend a message toohello queue</span></span>
1. <span data-ttu-id="6589c-128">Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="6589c-128">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="6589c-129">Lägg till följande kod toohello hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="6589c-129">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="6589c-130">Ange hello `connectionString` variabeln toohello anslutningssträngen som du fick när du skapar hello namnområde och ange `queueName` toohello kön namn som du använde när du skapar hello kön.</span><span class="sxs-lookup"><span data-stu-id="6589c-130">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="6589c-131">Så här bör filen Program.cs se ut:</span><span class="sxs-lookup"><span data-stu-id="6589c-131">Here is what your Program.cs file should look like.</span></span>
   
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

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="6589c-132">Kör programmet hello och kontrollera hello Azure-portalen: hello namnet på kön i hello namnområdet **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="6589c-132">Run hello program, and check hello Azure portal: click hello name of your queue in hello namespace **Overview** blade.</span></span> <span data-ttu-id="6589c-133">hello kön **Essentials** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="6589c-133">hello queue **Essentials** blade is displayed.</span></span> <span data-ttu-id="6589c-134">Observera att hello **Active Meddelandemängd** värdet ska nu vara 1.</span><span class="sxs-lookup"><span data-stu-id="6589c-134">Notice that hello **Active Message Count** value should now be 1.</span></span> <span data-ttu-id="6589c-135">Varje gång du kör hello avsändarprogrammet utan att hämta hälsningsmeddelande, ökar detta värde med 1.</span><span class="sxs-lookup"><span data-stu-id="6589c-135">Each time you run hello sender application without retrieving hello messages, this value increases by 1.</span></span> <span data-ttu-id="6589c-136">Observera att hello aktuell storlek för hello kön ökar varje gång hello app lägger också till en meddelandekö toohello.</span><span class="sxs-lookup"><span data-stu-id="6589c-136">Also note that hello current size of hello queue increments each time hello app adds a message toohello queue.</span></span>
   
      ![Meddelandestorlek][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a><span data-ttu-id="6589c-138">4. Ta emot meddelanden från kön hello</span><span class="sxs-lookup"><span data-stu-id="6589c-138">4. Receive messages from hello queue</span></span>

1. <span data-ttu-id="6589c-139">tooreceive hälsningsmeddelande som du har skickat skapa ett nytt konsolprogram och lägga till en referens toohello Service Bus-NuGet-paketet, liknande toohello tidigare avsändarprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6589c-139">tooreceive hello messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="6589c-140">Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="6589c-140">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="6589c-141">Lägg till följande kod toohello hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="6589c-141">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="6589c-142">Ange hello `connectionString` variabeln toohello anslutningssträngen som har fått när du skapar hello namnområde och ange `queueName` toohello kön namn som du använde när du skapar hello kön.</span><span class="sxs-lookup"><span data-stu-id="6589c-142">Set hello `connectionString` variable toohello connection string that was obtained when creating hello namespace, and set `queueName` toohello queue name that you used when creating hello queue.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="6589c-143">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="6589c-143">Here is what your Program.cs file should look like:</span></span>
   
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

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="6589c-144">Kör programmet hello och kontrollera hello portal igen.</span><span class="sxs-lookup"><span data-stu-id="6589c-144">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="6589c-145">Observera att hello **Active Meddelandemängd** och **aktuella** värden är nu 0.</span><span class="sxs-lookup"><span data-stu-id="6589c-145">Notice that hello **Active Message Count** and **Current** values are now 0.</span></span>
   
    ![Kölängd][queue-message-receive]

<span data-ttu-id="6589c-147">Grattis!</span><span class="sxs-lookup"><span data-stu-id="6589c-147">Congratulations!</span></span> <span data-ttu-id="6589c-148">Nu har du skapat en kö, skickat ett meddelande och tagit emot ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="6589c-148">You have now created a queue, sent a message, and received a message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6589c-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6589c-149">Next steps</span></span>

<span data-ttu-id="6589c-150">Kolla in våra [GitHub-lagret med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som demonstrerar några av hello mer avancerade funktioner för Service Bus-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6589c-150">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
