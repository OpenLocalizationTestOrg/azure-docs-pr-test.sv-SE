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
# <a name="get-started-with-service-bus-queues"></a>Komma igång med Service Bus-köer
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Detta kommer att utföras
Den här kursen ingår hello följande steg:

1. Skapa ett namnområde för Service Bus använder hello Azure-portalen.
2. Skapa en Service Bus-kö med hello Azure-portalen.
3. Skriva en konsol programmet toosend ett meddelande.
4. Skriv en konsol programmet tooreceive hello skickade meddelanden i hello föregående steg.

## <a name="prerequisites"></a>Krav
1. [Visual Studio 2015 eller senare](http://www.visualstudio.com). hello exemplen i den här självstudiekursen använder Visual Studio 2017.
2. En Azure-prenumeration.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-hello-azure-portal"></a>1. Skapa ett namnområde med hello Azure-portalen
Om du redan har skapat ett namnområde för Service Bus-meddelanden hoppa toohello [skapar en kö med hello Azure-portalen](#2-create-a-queue-using-the-azure-portal) avsnitt.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-hello-azure-portal"></a>2. Skapa en kö med hello Azure-portalen
Om du redan har skapat en Service Bus-kö hoppa toohello [överföringskön meddelanden toohello](#3-send-messages-to-the-queue) avsnitt.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-toohello-queue"></a>3. Skicka meddelanden toohello kön
toosend toohello kön, vi skriva en C#-konsolapp med Visual Studio.

### <a name="create-a-console-application"></a>Skapa ett konsolprogram

Starta Visual Studio och skapa ett nytt projekt: **Konsolprogram (.NET Framework)**.

### <a name="add-hello-service-bus-nuget-package"></a>Lägg till hello Service Bus-NuGet-paketet
1. Högerklicka på hello nyskapad projektet och välj **hantera NuGet-paket**.
2. Klicka på hello **Bläddra** fliken, söka efter **Microsoft Azure Service Bus**, och välj sedan hello **WindowsAzure.ServiceBus** objekt. Klicka på **installera** toocomplete hello installationen och sedan stänga den här dialogrutan.
   
    ![Välj ett NuGet-paket][nuget-pkg]

### <a name="write-some-code-toosend-a-message-toohello-queue"></a>Skriva vissa koden toosend en meddelandekö toohello
1. Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Lägg till följande kod toohello hello `Main` metod. Ange hello `connectionString` variabeln toohello anslutningssträngen som du fick när du skapar hello namnområde och ange `queueName` toohello kön namn som du använde när du skapar hello kön.
   
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
   
    Så här bör filen Program.cs se ut:
   
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
3. Kör programmet hello och kontrollera hello Azure-portalen: hello namnet på kön i hello namnområdet **översikt** bladet. hello kön **Essentials** bladet visas. Observera att hello **Active Meddelandemängd** värdet ska nu vara 1. Varje gång du kör hello avsändarprogrammet utan att hämta hälsningsmeddelande, ökar detta värde med 1. Observera att hello aktuell storlek för hello kön ökar varje gång hello app lägger också till en meddelandekö toohello.
   
      ![Meddelandestorlek][queue-message]

## <a name="4-receive-messages-from-hello-queue"></a>4. Ta emot meddelanden från kön hello

1. tooreceive hälsningsmeddelande som du har skickat skapa ett nytt konsolprogram och lägga till en referens toohello Service Bus-NuGet-paketet, liknande toohello tidigare avsändarprogrammet.
2. Lägg till följande hello `using` instruktionen toohello överkant hello Program.cs-filen.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Lägg till följande kod toohello hello `Main` metod. Ange hello `connectionString` variabeln toohello anslutningssträngen som har fått när du skapar hello namnområde och ange `queueName` toohello kön namn som du använde när du skapar hello kön.
   
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
   
    Så här bör din Program.cs-fil se ut:
   
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
4. Kör programmet hello och kontrollera hello portal igen. Observera att hello **Active Meddelandemängd** och **aktuella** värden är nu 0.
   
    ![Kölängd][queue-message-receive]

Grattis! Nu har du skapat en kö, skickat ett meddelande och tagit emot ett meddelande.

## <a name="next-steps"></a>Nästa steg

Kolla in våra [GitHub-lagret med exempel](https://github.com/Azure/azure-service-bus/tree/master/samples) som demonstrerar några av hello mer avancerade funktioner för Service Bus-meddelanden.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
