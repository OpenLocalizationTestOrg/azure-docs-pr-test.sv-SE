---
title: "aaaGet igång med Azure Service Bus-ämnen och prenumerationer | Microsoft Docs"
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
ms.openlocfilehash: 619d602599d97ecff2ded0681a383b19f1a8b7ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-service-bus-topics"></a><span data-ttu-id="2aeee-103">Kom igång med Service Bus-ämnen</span><span class="sxs-lookup"><span data-stu-id="2aeee-103">Get started with Service Bus topics</span></span>

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a><span data-ttu-id="2aeee-104">Detta kommer att utföras</span><span class="sxs-lookup"><span data-stu-id="2aeee-104">What will be accomplished</span></span>

<span data-ttu-id="2aeee-105">Den här kursen ingår hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="2aeee-105">This tutorial covers hello following steps:</span></span>

1. <span data-ttu-id="2aeee-106">Skapa ett namnområde för Service Bus använder hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-106">Create a Service Bus namespace, using hello Azure portal.</span></span>
2. <span data-ttu-id="2aeee-107">Skapa en Service Bus-ämne med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-107">Create a Service Bus topic, using hello Azure portal.</span></span>
3. <span data-ttu-id="2aeee-108">Skapa ett Service Bus prenumeration toothat ämne, med hjälp av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-108">Create a Service Bus subscription toothat topic, using hello Azure portal.</span></span>
4. <span data-ttu-id="2aeee-109">Skriv en konsol programmet toosend ett toohello ämne för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-109">Write a console application toosend a message toohello topic.</span></span>
5. <span data-ttu-id="2aeee-110">Skriva en konsol programmet tooreceive meddelandet från hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2aeee-110">Write a console application tooreceive that message from hello subscription.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2aeee-111">Krav</span><span class="sxs-lookup"><span data-stu-id="2aeee-111">Prerequisites</span></span>

1. <span data-ttu-id="2aeee-112">[Visual Studio 2015 eller senare](http://www.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="2aeee-112">[Visual Studio 2015 or higher](http://www.visualstudio.com).</span></span> <span data-ttu-id="2aeee-113">hello exemplen i den här självstudiekursen använder Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2aeee-113">hello examples in this tutorial use Visual Studio 2017.</span></span>
2. <span data-ttu-id="2aeee-114">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2aeee-114">An Azure subscription.</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a><span data-ttu-id="2aeee-115">1. Skapa ett namnområde med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2aeee-115">1. Create a namespace using hello Azure portal</span></span>

<span data-ttu-id="2aeee-116">Om du redan har skapat ett namnområde för Service Bus-meddelanden hoppa toohello [skapar ett ämne med hello Azure-portalen](#2-create-a-topic-using-the-azure-portal) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2aeee-116">If you have already created a Service Bus Messaging namespace, jump toohello [Create a topic using hello Azure portal](#2-create-a-topic-using-the-azure-portal) section.</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a><span data-ttu-id="2aeee-117">2. Skapa ett avsnitt med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2aeee-117">2. Create a topic using hello Azure portal</span></span>

1. <span data-ttu-id="2aeee-118">Logga in toohello [Azure-portalen][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="2aeee-118">Log on toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="2aeee-119">Hello vänstra navigeringsfönstret hello-portalen, klicka på **Service Bus** (om du inte ser **Service Bus**, klickar du på **fler tjänster**).</span><span class="sxs-lookup"><span data-stu-id="2aeee-119">In hello left navigation pane of hello portal, click **Service Bus** (if you don't see **Service Bus**, click **More services**).</span></span>
3. <span data-ttu-id="2aeee-120">Klicka på hello namnområde som du vill att toocreate hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-120">Click hello namespace in which you would like toocreate hello topic.</span></span> <span data-ttu-id="2aeee-121">hello namnområde översikt bladet visas:</span><span class="sxs-lookup"><span data-stu-id="2aeee-121">hello namespace overview blade appears:</span></span>
   
    ![Skapa ett ämne][createtopic1]
4. <span data-ttu-id="2aeee-123">I hello **Service Bus-namnrymd** bladet, klickar du på **avsnitt**, klicka på **Lägg till avsnittet**.</span><span class="sxs-lookup"><span data-stu-id="2aeee-123">In hello **Service Bus namespace** blade, click **Topics**, then click **Add topic**.</span></span>
   
    ![Välja ämnen][createtopic2]
5. <span data-ttu-id="2aeee-125">Ange ett namn för ämnet hello och avmarkera hello **aktivera partitionering** alternativet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-125">Enter a name for hello topic, and uncheck hello **Enable partitioning** option.</span></span> <span data-ttu-id="2aeee-126">Lämna hello andra alternativ med sina standardvärden.</span><span class="sxs-lookup"><span data-stu-id="2aeee-126">Leave hello other options with their default values.</span></span>
   
    ![Välj ny][createtopic3]
6. <span data-ttu-id="2aeee-128">Hello längst ned på hello-bladet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="2aeee-128">At hello bottom of hello blade, click **Create**.</span></span>

## <a name="3-create-a-subscription-toohello-topic"></a><span data-ttu-id="2aeee-129">3. Skapa en prenumeration toohello ämne</span><span class="sxs-lookup"><span data-stu-id="2aeee-129">3. Create a subscription toohello topic</span></span>

1. <span data-ttu-id="2aeee-130">I rutan portal resurser hello hello namnområdet som du skapade i steg 1 Klicka på namnet på hello ämne som du skapade i steg 2.</span><span class="sxs-lookup"><span data-stu-id="2aeee-130">In hello portal resources pane, click hello namespace you created in step 1, then click name of hello topic you created in step 2.</span></span>
2. <span data-ttu-id="2aeee-131">Hello överkant hello översikt fönstret klickar du på hello plus logga bredvid för**prenumeration** tooadd en prenumeration toothis avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-131">A hello top of hello overview pane, click hello plus sign next too**Subscription** tooadd a subscription toothis topic.</span></span>

    ![Skapa en prenumeration][createtopic4]

3. <span data-ttu-id="2aeee-133">Ange ett namn för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2aeee-133">Enter a name for hello subscription.</span></span> <span data-ttu-id="2aeee-134">Lämna hello andra alternativ med sina standardvärden.</span><span class="sxs-lookup"><span data-stu-id="2aeee-134">Leave hello other options with their default values.</span></span>

## <a name="4-send-messages-toohello-topic"></a><span data-ttu-id="2aeee-135">4. Skicka meddelanden toohello avsnittet</span><span class="sxs-lookup"><span data-stu-id="2aeee-135">4. Send messages toohello topic</span></span>

<span data-ttu-id="2aeee-136">toosend meddelanden toohello avsnittet vi skriva en C#-konsolapp med Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2aeee-136">toosend messages toohello topic, we write a C# console application using Visual Studio.</span></span>

### <a name="create-a-console-application"></a><span data-ttu-id="2aeee-137">Skapa ett konsolprogram</span><span class="sxs-lookup"><span data-stu-id="2aeee-137">Create a console application</span></span>

<span data-ttu-id="2aeee-138">Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="2aeee-138">Launch Visual Studio and create a new **Console app (.NET Framework)** project.</span></span>

### <a name="add-hello-service-bus-nuget-package"></a><span data-ttu-id="2aeee-139">Lägg till hello Service Bus-NuGet-paketet</span><span class="sxs-lookup"><span data-stu-id="2aeee-139">Add hello Service Bus NuGet package</span></span>

1. <span data-ttu-id="2aeee-140">Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="2aeee-140">Right-click hello newly created project and select **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="2aeee-141">Klicka på hello **Bläddra** fliken, söka efter **Microsoft Azure Service Bus**, och välj sedan hello **WindowsAzure.ServiceBus** objekt.</span><span class="sxs-lookup"><span data-stu-id="2aeee-141">Click hello **Browse** tab, search for **Microsoft Azure Service Bus**, and then select hello **WindowsAzure.ServiceBus** item.</span></span> <span data-ttu-id="2aeee-142">Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="2aeee-142">Click **Install** toocomplete hello installation, then close this dialog box.</span></span>
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a><span data-ttu-id="2aeee-144">Skriva vissa kod toosend ett toohello ämne för meddelandet</span><span class="sxs-lookup"><span data-stu-id="2aeee-144">Write some code toosend a message toohello topic</span></span>

1. <span data-ttu-id="2aeee-145">Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-145">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. <span data-ttu-id="2aeee-146">Lägg till följande kod toohello hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="2aeee-146">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="2aeee-147">Ange hello `connectionString` variabeln toohello anslutningssträngen som du fick när du skapar hello namnområde och ange `topicName` toohello namn som du använde när du skapar hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-147">Set hello `connectionString` variable toohello connection string that you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="2aeee-148">Så här bör filen Program.cs se ut:</span><span class="sxs-lookup"><span data-stu-id="2aeee-148">Here is what your Program.cs file should look like.</span></span>
   
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

                Console.WriteLine("Message successfully sent! Press ENTER tooexit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. <span data-ttu-id="2aeee-149">Kör programmet hello och kontrollera hello Azure-portalen: hello namnet på ditt ämne i hello namnområdet **översikt** bladet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-149">Run hello program, and check hello Azure portal: click hello name of your topic in hello namespace **Overview** blade.</span></span> <span data-ttu-id="2aeee-150">hello avsnittet **Essentials** bladet visas.</span><span class="sxs-lookup"><span data-stu-id="2aeee-150">hello topic **Essentials** blade is displayed.</span></span> <span data-ttu-id="2aeee-151">Observera att hello i hello abonnemang visas hello nedre delen av hello-bladet, **Meddelandemängd** värde för varje prenumeration ska nu vara 1.</span><span class="sxs-lookup"><span data-stu-id="2aeee-151">In hello subscription(s) listed near hello bottom of hello blade, notice that hello **Message Count** value for each subscription should now be 1.</span></span> <span data-ttu-id="2aeee-152">Varje gång du kör hello avsändarprogrammet utan att hämta hälsningsmeddelande (som beskrivs i nästa avsnitt om hello), detta värde ökar med 1.</span><span class="sxs-lookup"><span data-stu-id="2aeee-152">Each time you run hello sender application without retrieving hello messages (as described in hello next section), this value increases by 1.</span></span> <span data-ttu-id="2aeee-153">Observera även att hello aktuell storlek för hello avsnittet steg hello **aktuella** värdet på hello **Essentials** bladet varje gång hello app lägger till en message toohello ämne /-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2aeee-153">Also note that hello current size of hello topic increments hello **Current** value on hello **Essentials** blade each time hello app adds a message toohello topic/subscription.</span></span>
   
      ![Meddelandestorlek][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a><span data-ttu-id="2aeee-155">5. Ta emot meddelanden från hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="2aeee-155">5. Receive messages from hello subscription</span></span>

1. <span data-ttu-id="2aeee-156">tooreceive hello-meddelande eller meddelanden som du har skickat skapa ett nytt konsolprogram och lägga till en referens toohello Service Bus-NuGet-paketet, liknande toohello tidigare avsändarprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-156">tooreceive hello message or messages you just sent, create a new console application and add a reference toohello Service Bus NuGet package, similar toohello previous sender application.</span></span>
2. <span data-ttu-id="2aeee-157">Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-157">Add hello following `using` statement toohello top of hello Program.cs file.</span></span>
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. <span data-ttu-id="2aeee-158">Lägg till följande kod toohello hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="2aeee-158">Add hello following code toohello `Main` method.</span></span> <span data-ttu-id="2aeee-159">Ange hello `connectionString` variabeln toohello anslutningssträngen du fick när du skapar hello namnområde och ange `topicName` toohello namn som du använde när du skapar hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-159">Set hello `connectionString` variable toohello connection string you obtained when creating hello namespace, and set `topicName` toohello name that you used when creating hello topic.</span></span>
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER tooexit program");
    Console.ReadLine();
    ```
   
    <span data-ttu-id="2aeee-160">Så här bör din Program.cs-fil se ut:</span><span class="sxs-lookup"><span data-stu-id="2aeee-160">Here is what your Program.cs file should look like:</span></span>
   
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

          Console.WriteLine("Press ENTER tooexit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. <span data-ttu-id="2aeee-161">Kör programmet hello och kontrollera hello portal igen.</span><span class="sxs-lookup"><span data-stu-id="2aeee-161">Run hello program, and check hello portal again.</span></span> <span data-ttu-id="2aeee-162">Observera att hello **Meddelandemängd** och **aktuella** värden är nu 0.</span><span class="sxs-lookup"><span data-stu-id="2aeee-162">Notice that hello **Message Count** and **Current** values are now 0.</span></span>
   
    ![Ämnets längd][topic-message-receive]

<span data-ttu-id="2aeee-164">Grattis!</span><span class="sxs-lookup"><span data-stu-id="2aeee-164">Congratulations!</span></span> <span data-ttu-id="2aeee-165">Du har nu skapat ett ämne och en prenumeration, skickat ett meddelande och tagit emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2aeee-165">You have now created a topic and subscription, sent a message, and received that message.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2aeee-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2aeee-166">Next steps</span></span>

<span data-ttu-id="2aeee-167">Kolla in våra [GitHub-lagret med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som demonstrerar några av hello mer avancerade funktioner för Service Bus-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2aeee-167">Check out our [GitHub repository with samples](https://github.com/Azure/azure-service-bus/tree/master/samples) that demonstrate some of hello more advanced features of Service Bus messaging.</span></span>

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
