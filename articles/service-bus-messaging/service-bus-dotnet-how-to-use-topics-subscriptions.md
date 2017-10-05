---
title: "Kom igång med ämnen och prenumerationer i Azure Service Bus | Microsoft Docs"
description: "Skriv ett C#-konsolprogram som använder meddelandeämnen och prenumerationer i Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="801fa-103">Kom igång med Service Bus-ämnen</span><span class="sxs-lookup"><span data-stu-id="801fa-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="801fa-104">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="801fa-104">What will be accomplished</span></span>

<span data-ttu-id="801fa-105">Den här självstudien omfattar följande steg:</span><span class="sxs-lookup"><span data-stu-id="801fa-105">This tutorial covers the following steps:</span></span>

1. <span data-ttu-id="801fa-106">Skapa ett Service Bus-namnområde med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="801fa-106">Create a Service Bus namespace, using the Azure portal.</span></span>
2. <span data-ttu-id="801fa-107">Skapa ett Service Bus-ämne med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="801fa-107">Create a Service Bus topic, using the Azure portal.</span></span>
3. <span data-ttu-id="801fa-108">Skapa en Service Bus-prenumeration på ämnet med Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="801fa-108">Create a Service Bus subscription to that topic, using the Azure portal.</span></span>
4. <span data-ttu-id="801fa-109">Skriv ett konsolprogram för att skicka ett meddelande till ämnet.</span><span class="sxs-lookup"><span data-stu-id="801fa-109">Write a console application to send a message to the topic.</span></span>
5. <span data-ttu-id="801fa-110">Skriv ett konsolprogram för att ta emot meddelandet från prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="801fa-110">Write a console application to receive that message from the subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="801fa-111">Krav</span><span class="sxs-lookup"><span data-stu-id="801fa-111">Prerequisites</span></span>

1. <span data-ttu-id="801fa-112">[Visual Studio 2015 eller senare](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="801fa-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="801fa-113">I exemplen i den här självstudiekursen används Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="801fa-113">The examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="801fa-114">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="801fa-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a><span data-ttu-id="801fa-115">1. Skapa ett namnområde med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="801fa-115">1. Create a namespace using the Azure portal</span></span>

<span data-ttu-id="801fa-116">Om du redan har skapat ett namnområde för Service Bus-meddelanden går du vidare till avsnittet [Skapa ett ämne med Azure Portal](#2-create-a-topic-using-the-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="801fa-116">If you have already created a Service Bus Messaging namespace, jump to the [Create a topic using the Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a><span data-ttu-id="801fa-117">2. Skapa ett ämne med Azure Portal</span><span class="sxs-lookup"><span data-stu-id="801fa-117">2. Create a topic using the Azure portal</span></span>

1. <span data-ttu-id="801fa-118">Logga in på [Azure portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="801fa-118">Log on to the [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="801fa-119">I det vänstra navigeringsfönstret i portalen klickar du på **Service Bus** (om du inte ser **Service Bus** klickar du på **More services** (Fler tjänster)).</span><span class="sxs-lookup"><span data-stu-id="801fa-119">In the left navigation pane of the portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="801fa-120">Klicka på det namnområde där du vill skapa ämnet.</span><span class="sxs-lookup"><span data-stu-id="801fa-120">Click the namespace in which you would like to create the topic.</span></span> <span data-ttu-id="801fa-121">Bladet med namnområdesöversikten visas:</span><span class="sxs-lookup"><span data-stu-id="801fa-121">The namespace overview blade appears:</span></span>
   
    ![Skapa ett ämne][createtopic1]
4. <span data-ttu-id="801fa-123">På bladet **Service Bus-namnområde** klickar du på **Ämnen** och sedan på **Lägg till ämne**.</span><span class="sxs-lookup"><span data-stu-id="801fa-123">In the **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Välja ämnen][createtopic2]
5. <span data-ttu-id="801fa-125">Ange ett namn för ämnet och avmarkera alternativet **Aktivera partitionering**.</span><span class="sxs-lookup"><span data-stu-id="801fa-125">Enter a name for the topic, and uncheck the **Enable partitioning** option.</span></span> <span data-ttu-id="801fa-126">Lämna standardvärdena för de andra alternativen.</span><span class="sxs-lookup"><span data-stu-id="801fa-126">Leave the other options with their default values.</span></span>
   
    ![Välj ny][createtopic3]
6. <span data-ttu-id="801fa-128">Klicka på **Skapa** längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="801fa-128">At the bottom of the blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-to-the-topic"></a><span data-ttu-id="801fa-129">3. Skapa en prenumeration på ämnet</span><span class="sxs-lookup"><span data-stu-id="801fa-129">3. Create a subscription to the topic</span></span>

1. <span data-ttu-id="801fa-130">Klicka på det namnområde du skapade i steg 1 i fönstret med portalresurser, och klicka sedan på namnet på det ämne du skapade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="801fa-130">In the portal resources pane, click the namespace you created in step 1, then click name of the topic you created in step 2.</span></span>
2. <span data-ttu-id="801fa-131">Klicka på plustecknet bredvid **Prenumeration** längst upp i översiktsfönstret för att lägga till en prenumeration på det här ämnet.</span><span class="sxs-lookup"><span data-stu-id="801fa-131">A the top of the overview pane, click the plus sign next to **Subscription** to add a subscription to this topic.</span></span>

    ![Skapa en prenumeration][createtopic4]

3. <span data-ttu-id="801fa-133">Ange ett namn för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="801fa-133">Enter a name for the subscription.</span></span> <span data-ttu-id="801fa-134">Lämna standardvärdena för de andra alternativen.</span><span class="sxs-lookup"><span data-stu-id="801fa-134">Leave the other options with their default values.</span></span>

## <a name="4-send-messages-to-the-topic"></a><span data-ttu-id="801fa-135">4. Skicka meddelanden till ämnet</span><span class="sxs-lookup"><span data-stu-id="801fa-135">4. Send messages to the topic</span></span>

<span data-ttu-id="801fa-136">För att kunna skicka meddelanden till ämnet skriver vi ett C#-konsolprogram med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="801fa-136">To send messages to the topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="801fa-137">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="801fa-137">Create a console application</span></span>

<span data-ttu-id="801fa-138">Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="801fa-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-the-service-bus-nuget-package"></a><span data-ttu-id="801fa-139">Lägga till Service Bus-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="801fa-139">Add the Service Bus NuGet package</span></span>

1. <span data-ttu-id="801fa-140">Högerklicka på det nyskapade projektet och välj **Hantera Nuget-paket**.</span><span class="sxs-lookup"><span data-stu-id="801fa-140">Right-click the newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="801fa-141">Klicka på fliken **Bläddra**, sök efter **Microsoft Azure Service Bus** och markera posten **WindowsAzure.ServiceBus**.</span><span class="sxs-lookup"><span data-stu-id="801fa-141">Click the **Browse** tab, search for **Microsoft Azure Service Bus**, and then select the **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="801fa-142">Klicka på **Installera** för att slutföra installationen och stäng sedan den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="801fa-142">Click **Install** to complete the installation, then close this dialog box.</span></span>
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a><span data-ttu-id="801fa-144">Skriv kod för att skicka ett meddelande till ämnet</span><span class="sxs-lookup"><span data-stu-id="801fa-144">Write some code to send a message to the topic</span></span>

1. <span data-ttu-id="801fa-145">Lägg till följande `using`-instruktion högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="801fa-145">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="801fa-146">Lägg till följande kod i metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="801fa-146">Add the following code to the `Main` method.</span></span> <span data-ttu-id="801fa-147">Ställ in variabeln `connectionString` på den anslutningssträng du fick när du skapade namnområdet, och ställ in `topicName` på namnet du använde när du skapade ämnet.</span><span class="sxs-lookup"><span data-stu-id="801fa-147">Set the `connectionString` variable to the connection string that you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="801fa-148">Så här bör filen Program.cs se ut:</span><span class="sxs-lookup"><span data-stu-id="801fa-148">Here is what your Program.cs file should look like.</span></span>
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="801fa-149">Kör programmet och kontrollera Azure Portal: klicka på ämnets namn på bladet **Översikt** för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="801fa-149">Run the program, and check the Azure portal: click the name of your topic in the namespace **Overview** blade.</span></span> <span data-ttu-id="801fa-150">Bladet **Grundläggande** för ämnet visas.</span><span class="sxs-lookup"><span data-stu-id="801fa-150">The topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="801fa-151">Lägg märke till att värdet **Antal meddelanden** nu ska vara 1 för prenumerationerna som visas längst ned på bladet.</span><span class="sxs-lookup"><span data-stu-id="801fa-151">In the subscription(s) listed near the bottom of the blade, notice that the **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="801fa-152">Varje gång du kör sändningsprogrammet utan att hämta meddelanden (beskrivs i nästa avsnitt) ökar detta värde med 1.</span><span class="sxs-lookup"><span data-stu-id="801fa-152">Each time you run the sender application without retrieving the messages (as described in the next section), this value increases by 1.</span></span> <span data-ttu-id="801fa-153">Observera också att ämnets aktuella storlek ökar värdet **Aktuell** på bladet **Grundläggande** varje gång programmet lägger till ett meddelande i ämnet/prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="801fa-153">Also note that the current size of the topic increments the **Current** value on the **Essentials** blade each time the app adds a message to the topic/subscription.</span></span>
   
      ![Meddelandestorlek][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a><span data-ttu-id="801fa-155">5. Ta emot meddelanden från prenumerationen</span><span class="sxs-lookup"><span data-stu-id="801fa-155">5. Receive messages from the subscription</span></span>

1. <span data-ttu-id="801fa-156">Om du vill ta emot de meddelanden du nyss skickade skapar du ett nytt konsolprogram och lägger till en referens till Service Bus NuGet-paketet, ungefär som i det tidigare sändningsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="801fa-156">To receive the message or messages you just sent, create a new console application and add a reference to the Service Bus NuGet package, similar to the previous sender application.</span></span>
2. <span data-ttu-id="801fa-157">Lägg till följande `using`-instruktion högst upp i filen Program.cs.</span><span class="sxs-lookup"><span data-stu-id="801fa-157">Add the following `using` statement to the top of the Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="801fa-158">Lägg till följande kod i metoden `Main`.</span><span class="sxs-lookup"><span data-stu-id="801fa-158">Add the following code to the `Main` method.</span></span> <span data-ttu-id="801fa-159">Ställ in variabeln `connectionString` på den anslutningssträng du fick när du skapade namnområdet, och ställ in `topicName` på namnet du använde när du skapade ämnet.</span><span class="sxs-lookup"><span data-stu-id="801fa-159">Set the `connectionString` variable to the connection string you obtained when creating the namespace, and set `topicName` to the name that you used when creating the topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="801fa-160">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="801fa-160">Here is what your Program.cs file should look like:</span></span>
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. <span data-ttu-id="801fa-161">Kör programmet och kontrollera portalen igen.</span><span class="sxs-lookup"><span data-stu-id="801fa-161">Run the program, and check the portal again.</span></span> <span data-ttu-id="801fa-162">Observera att värdena för **Antal meddelanden** och **Aktuell** nu är 0.</span><span class="sxs-lookup"><span data-stu-id="801fa-162">Notice that the **Message Count** and **Current** values are now 0.</span></span>
   
    ![Ämnets längd][topic-message-receive]

<span data-ttu-id="801fa-164">Grattis!</span><span class="sxs-lookup"><span data-stu-id="801fa-164">Congratulations!</span></span> <span data-ttu-id="801fa-165">Du har nu skapat ett ämne och en prenumeration, skickat ett meddelande och tagit emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="801fa-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="801fa-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="801fa-166">Next steps</span></span>

<span data-ttu-id="801fa-167">Kolla in våra [GitHub-databaser med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som visar några av de mer avancerade funktionerna i meddelandetjänsten i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="801fa-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of the more advanced features of Service Bus messaging.</span></span>

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com
