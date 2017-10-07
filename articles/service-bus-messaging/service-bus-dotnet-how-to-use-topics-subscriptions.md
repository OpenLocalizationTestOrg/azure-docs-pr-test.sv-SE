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
# <a name="get-started-with-service-bus-topics"></a>Kom igång med Service Bus-ämnen

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras

Den här kursen ingår hello följande steg:

1. Skapa ett namnområde för Service Bus använder hello Azure-portalen.
2. Skapa en Service Bus-ämne med hello Azure-portalen.
3. Skapa ett Service Bus prenumeration toothat ämne, med hjälp av hello Azure-portalen.
4. Skriv en konsol programmet toosend ett toohello ämne för meddelandet.
5. Skriva en konsol programmet tooreceive meddelandet från hello prenumeration.

## <a name="prerequisites"></a>Krav

1. [Visual Studio 2015 eller senare](http://www.visualstudio.com). hello exemplen i den här självstudiekursen använder Visual Studio 2017.
2. En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Skapa ett namnområde med hello Azure-portalen

Om du redan har skapat ett namnområde för Service Bus-meddelanden hoppa toohello [skapar ett ämne med hello Azure-portalen](#2-create-a-topic-using-the-azure-portal) avsnitt.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-hello-azure-portal"></a>2. Skapa ett avsnitt med hello Azure-portalen

1. Logga in toohello [Azure-portalen][azure-portal].
2. Hello vänstra navigeringsfönstret hello-portalen, klicka på **Service Bus** (om du inte ser **Service Bus**, klickar du på **fler tjänster**).
3. Klicka på hello namnområde som du vill att toocreate hello-avsnittet. hello namnområde översikt bladet visas:
   
    ![Skapa ett ämne][createtopic1]
4. I hello **Service Bus-namnrymd** bladet, klickar du på **avsnitt**, klicka på **Lägg till avsnittet**.
   
    ![Välja ämnen][createtopic2]
5. Ange ett namn för ämnet hello och avmarkera hello **aktivera partitionering** alternativet. Lämna hello andra alternativ med sina standardvärden.
   
    ![Välj ny][createtopic3]
6. Hello längst ned på hello-bladet, klickar du på **skapa**.

## <a name="3-create-a-subscription-toohello-topic"></a>3. Skapa en prenumeration toohello ämne

1. I rutan portal resurser hello hello namnområdet som du skapade i steg 1 Klicka på namnet på hello ämne som du skapade i steg 2.
2. Hello överkant hello översikt fönstret klickar du på hello plus logga bredvid för**prenumeration** tooadd en prenumeration toothis avsnittet.

    ![Skapa en prenumeration][createtopic4]

3. Ange ett namn för hello prenumeration. Lämna hello andra alternativ med sina standardvärden.

## <a name="4-send-messages-toohello-topic"></a>4. Skicka meddelanden toohello avsnittet

toosend meddelanden toohello avsnittet vi skriva en C#-konsolapp med Visual Studio.

### <a name="create-a-console-application"></a>Skapa ett konsolprogram

Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.

### <a name="add-hello-service-bus-nuget-package"></a>Lägg till hello Service Bus-NuGet-paketet

1. Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.
2. Klicka på hello **Bläddra** fliken, söka efter **Microsoft Azure Service Bus**, och välj sedan hello **WindowsAzure.ServiceBus** objekt. Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-topic"></a>Skriva vissa kod toosend ett toohello ämne för meddelandet

1. Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Lägg till följande kod toohello hello `Main` metod. Ange hello `connectionString` variabeln toohello anslutningssträngen som du fick när du skapar hello namnområde och ange `topicName` toohello namn som du använde när du skapar hello-avsnittet.
   
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
   
    Så här bör filen Program.cs se ut:
   
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
3. Kör programmet hello och kontrollera hello Azure-portalen: hello namnet på ditt ämne i hello namnområdet **översikt** bladet. hello avsnittet **Essentials** bladet visas. Observera att hello i hello abonnemang visas hello nedre delen av hello-bladet, **Meddelandemängd** värde för varje prenumeration ska nu vara 1. Varje gång du kör hello avsändarprogrammet utan att hämta hälsningsmeddelande (som beskrivs i nästa avsnitt om hello), detta värde ökar med 1. Observera även att hello aktuell storlek för hello avsnittet steg hello **aktuella** värdet på hello **Essentials** bladet varje gång hello app lägger till en message toohello ämne /-prenumeration.
   
      ![Meddelandestorlek][topic-message]

## <a name="5-receive-messages-from-hello-subscription"></a>5. Ta emot meddelanden från hello prenumeration

1. tooreceive hello-meddelande eller meddelanden som du har skickat skapa ett nytt konsolprogram och lägga till en referens toohello Service Bus-NuGet-paketet, liknande toohello tidigare avsändarprogrammet.
2. Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Lägg till följande kod toohello hello `Main` metod. Ange hello `connectionString` variabeln toohello anslutningssträngen du fick när du skapar hello namnområde och ange `topicName` toohello namn som du använde när du skapar hello-avsnittet.
   
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
   
    Så här bör din Program.cs-fil se ut:
   
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
4. Kör programmet hello och kontrollera hello portal igen. Observera att hello **Meddelandemängd** och **aktuella** värden är nu 0.
   
    ![Ämnets längd][topic-message-receive]

Grattis! Du har nu skapat ett ämne och en prenumeration, skickat ett meddelande och tagit emot meddelandet.

## <a name="next-steps"></a>Nästa steg

Kolla in våra [GitHub-lagret med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som demonstrerar några av hello mer avancerade funktioner för Service Bus-meddelanden.

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
