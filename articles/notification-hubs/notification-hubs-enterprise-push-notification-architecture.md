---
title: aaaNotification NAV - Enterprise Push-arkitektur
description: "Anvisningar om hur du använder Azure Notification Hubs i en företagsmiljö"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 903023e9-9347-442a-924b-663af85e05c6
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: c3afb83de1ba0882bf99e10f38cca40cb42d07a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-push-architectural-guidance"></a>Push-arkitekturvägledning för företag
Företag befordras idag gradvis mot skapa mobila program för slutanvändarna (externa) eller för hello anställda (intern). De har befintliga serverdelssystem för att den stordatorer eller vissa LoB-program som integreras i hello mobilprogram arkitektur. Den här guiden kommer att tala om bästa toodo integrationen rekommenderar möjlig lösning toocommon scenarier.

En ofta krävs för att skicka push notification toohello användare via sina mobila program när en händelse av intresse uppstår i hello serverdelssystem. T.ex. en bank som har hello bank bank appen på sin iPhone kunden toobe meddelas när en debitering görs över en viss mängd från sitt konto eller ett intranätscenario där en medarbetare från ekonomiavdelningen som har en budget godkännande appen på sin Windows Phone vill toobe ett meddelande när han hämtar en begäran om godkännande.

hello konto eller godkännande bearbetning är sannolikt toobe göras i vissa backend-system som måste initiera en push-toohello användare. Det kan finnas flera backend som måste alla bygga hello samma typ av logik tooimplement push när en händelse utlöser ett meddelande. hello komplexitet här ligger i integrera flera serverdelssystem tillsammans med ett enda push-system där hello slutanvändare kan prenumererar toodifferent meddelanden och det kan även vara flera mobila program, t.ex. i hello skiftläget för intranät-Appar ett mobilt program kanske där tooreceive meddelanden från flera sådana backend-system. Hej serverdelssystem inte känner till eller behöver tooknow för push-semantik/teknik för en vanlig lösning har traditionellt toointroduce en komponent som genomsöker hello serverdelssystem för alla händelser och ansvarar för att skicka hello push-meddelanden toohello klient.
Här kommer vi om en ännu bättre lösning med hjälp av Azure Service Bus - ämne /-prenumeration modellen som minskar komplexiteten hello när hello lösning skalbar.

Här är hello allmänna arkitektur hello-lösning (generaliserad med flera mobila appar men lika tillämpliga när det finns endast en mobil app)

## <a name="architecture"></a>Arkitektur
![][1]

hello nyckel i den här Arkitekturdiagram är Azure Service Bus som ger en prenumerationer eller det avsnitt programmeringsmodellen (mer om den på [Service Bus Pub/Sub programmering]). hello mottagare, som i det här fallet är hello mobila serverdel (vanligtvis [Azure-Mobiltjänst], som initierar en push-toohello mobila appar) inte ta emot meddelanden direkt från hello serverdelssystem men i stället har vi ett mellanliggande Abstraktionslager som tillhandahålls av [Azure Service Bus] som gör det möjligt för mobilserverdel tooreceive meddelanden från en eller flera serverdelssystem. Ett Service Bus-ämne måste toobe skapas för varje hello serverdelssystem t.ex. konto HR, ekonomi som i princip ”intresseområden” som initierar meddelanden toobe skickas som push-meddelande. Hej serverdelssystem skickar meddelanden toothese avsnitt. En mobila serverdel kan prenumerera på tooone eller flera sådana avsnitt genom att skapa en Service Bus-prenumeration. Detta ger hello mobilserverdel tooreceive ett meddelande från hello motsvarande backend-systemet. Mobilserverdel fortsätter toolisten för meddelanden på sina prenumerationer och när ett meddelande tas emot den stängs tillbaka och skickar som meddelandehubben tooits meddelande. Notification hubs ger sedan slutligen hello meddelandet toohello mobila appar. Toosummarize hello viktiga komponenter, har vi:

1. Serverdelssystem (LoB/äldre system)
   * Skapar Service Bus-ämne
   * Skickar meddelandet
2. Mobil serverdel
   * Skapar en prenumeration på tjänsten
   * Tar emot meddelandet (från Backend-system)
   * Skickar meddelandet tooclients (via Azure Notification Hub)
3. Mobila program
   * Tar emot och visa meddelandet

### <a name="benefits"></a>Fördelar:
1. hello sambandet mellan hello mottagare (app/Mobiltjänst via Notification Hub) och avsändaren (serverdelssystem) gör att ytterligare serverdelssystem integreras med minimal förändring.
2. Det gör även hello scenario där flera mobila appar som kan tooreceive händelser från en eller flera serverdelssystem.  

## <a name="sample"></a>Exempel:
### <a name="prerequisites"></a>Krav
Du bör utföra hello följande kurser toofamiliarize hello koncept samt vanliga åtgärder för skapande och konfiguration:

1. [Service Bus Pub/Sub programmering] -det förklarar hello detaljer att arbeta med prenumerationer/Service Bus avsnitt, hur toocreate ett namnområde toocontain avsnitt/prenumerationer, hur toosend & ta emot meddelanden från dem.
2. [Notification Hubs – självstudiekurs för Windows Universal] -det förklarar hur tooset upp en Windows Store-app och använda Notification Hubs tooregister och ta emot meddelanden.

### <a name="sample-code"></a>Exempelkod
hello fullständig exempelkod finns på [Notification Hub exempel]. Den är uppdelad i tre komponenter:

1. **EnterprisePushBackendSystem**
   
    a. Det här projektet använder hello *WindowsAzure.ServiceBus* Nuget-paketet och baseras på [Service Bus Pub/Sub programmering].
   
    b. Det här är en enkel C#-konsolen app toosimulate LoB-systemet som startar hello meddelandet toobe levereras toohello mobila appar.
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    c. `CreateTopic`är används toocreate hello Service Bus-ämne där vi ska skicka meddelanden.
   
        public static void CreateTopic(string connectionString)
        {
            // Create hello topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    d. `SendMessage`är används toosend hello meddelanden toothis Service Bus-ämne. Här skickar vi helt enkelt en uppsättning slumpmässiga meddelanden toohello avsnittet med jämna mellanrum för hello syftet hello exempel. Normalt kan det finnas en backend-system som ska skicka meddelanden när en händelse inträffar.
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds toohello topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched tooa different team."
            };
   
            while (true)
            {
                Random rnd = new Random();
                string employeeId = rnd.Next(10000, 99999).ToString();
                string notification = String.Format(messages[rnd.Next(0,messages.Length)], employeeId);
   
                // Send Notification
                BrokeredMessage message = new BrokeredMessage(notification);
                client.Send(message);
   
                Console.WriteLine("{0} Message sent - '{1}'", DateTime.Now, notification);
   
                System.Threading.Thread.Sleep(new TimeSpan(0, 0, 10));
            }
        }
2. **ReceiveAndSendNotification**
   
    a. Det här projektet använder hello *WindowsAzure.ServiceBus* och *Microsoft.Web.WebJobs.Publish* Nuget-paket och baseras på [Service Bus Pub/Sub programmering].
   
    b. Det här är ett annat C#-konsolapp som vi ska köras som en [Azure Webjobs] eftersom den har toorun kontinuerligt toolisten för meddelanden från hello LoB/serverdelssystem. Det här är en del av din mobila serverdel.
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    c. `CreateSubscription`är används toocreate en Service Bus-prenumeration för hello avsnittet där hello backend-systemet ska skicka meddelanden. Beroende på hello affärsscenario skapar den här komponenten en eller flera prenumerationer toocorresponding avsnitt (t.ex. vissa kan få meddelanden från HR-system, vissa från ekonomi system och så vidare)
   
        static void CreateSubscription(string connectionString)
        {
            // Create hello subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    d. ReceiveMessageAndSendNotification är används tooread hello-meddelande från hello avsnitt med prenumerationen och hello läsa har lyckats sedan skapa ett meddelande (i hello är ett exempel på ett Windows interna popup-meddelande) toobe skickas toohello mobila programmet som använder Azure Notification Hubs.
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize hello Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from hello subscription
            while (true)
            {
                BrokeredMessage message = Client.Receive();
                var toastMessage = @"<toast><visual><binding template=""ToastText01""><text id=""1"">{messagepayload}</text></binding></visual></toast>";
   
                if (message != null)
                {
                    try
                    {
                        Console.WriteLine(message.MessageId);
                        Console.WriteLine(message.SequenceNumber);
                        string messageBody = message.GetBody<string>();
                        Console.WriteLine("Body: " + messageBody + "\n");
   
                        toastMessage = toastMessage.Replace("{messagepayload}", messageBody);
                        SendNotificationAsync(toastMessage);
   
                        // Remove message from subscription
                        message.Complete();
                    }
                    catch (Exception)
                    {
                        // Indicate a problem, unlock message in subscription
                        message.Abandon();
                    }
                }
            }
        }
        static async void SendNotificationAsync(string message)
        {
            await hub.SendWindowsNativeNotificationAsync(message);
        }
   
    e. För att publicera som en **Webbjobb**, högerklicka på hello lösningen i Visual Studio och välj **Publicera som Webbjobbet**
   
    ![][2]
   
    f. Välj din publiceringsprofil och skapa en ny Azure-webbplats om den inte finns redan som ska vara värd för den här Webbjobb och när du har hello webbplats sedan **publicera**.
   
    ![][3]
   
    g. Konfigurera hello jobbet toobe ”kör kontinuerligt” så att när du loggar in i toohello [klassiska Azure-portalen] du bör se något liknande hello följande:
   
    ![][4]
3. **EnterprisePushMobileApp**
   
    a. Det här är en Windows Store-programmet som ska ta emot popup-meddelanden från hello Webbjobbet körs som en del av din mobila serverdel och visa den. Detta baseras på [Notification Hubs – självstudiekurs för Windows Universal].  
   
    b. Kontrollera att programmet är aktiverat tooreceive popup-meddelanden.
   
    c. Se till att hello följande Notification Hubs registration kod anropas vid hello appen startar (när du ersätter hello *HubName* och *DefaultListenSharedAccessSignature*:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a>Köra exemplet:
1. Kontrollera att din Webbjobbet har körts och schemalagda för ”kör kontinuerligt”.
2. Kör hello **EnterprisePushMobileApp** som startar hello Windows Store-app.
3. Kör hello **EnterprisePushBackendSystem** konsolprogram som simulerar hello LoB serverdelen och kommer att börja skicka meddelanden och du bör se popup-meddelanden som visas som hello nedan:
   
    ![][5]
4. hälsningsmeddelande skickades ursprungligen tooService Bus-ämnen som övervakades med Service Bus prenumerationer i ditt webb-jobb. När ett meddelande togs emot ett meddelande skapades och skickas toohello mobila appar. Du kan titta igenom hello Webbjobb loggar tooconfirm hello bearbetning när du går toohello loggar länken i [klassiska Azure-portalen] för Web-jobb:
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
[Notification Hub exempel]: https://github.com/Azure/azure-notificationhubs-samples
[Azure-Mobiltjänst]: http://azure.microsoft.com/documentation/services/mobile-services/
[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/
[Service Bus Pub/Sub programmering]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/
[Azure Webjobs]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/
[Notification Hubs – självstudiekurs för Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/
[klassiska Azure-portalen]: https://manage.windowsazure.com/
