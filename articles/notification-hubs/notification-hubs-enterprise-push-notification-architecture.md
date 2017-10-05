---
title: "Notification Hubs – Enterprise Push-arkitektur"
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
ms.openlocfilehash: ae7c1c9644ecfe7fe4ad6e332cc0683a3b5df22f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="4a310-103">Push-arkitekturvägledning för företag</span><span class="sxs-lookup"><span data-stu-id="4a310-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="4a310-104">Företag befordras idag gradvis mot skapa mobila program för slutanvändarna (externa) eller för anställda (intern).</span><span class="sxs-lookup"><span data-stu-id="4a310-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for the employees (internal).</span></span> <span data-ttu-id="4a310-105">De har befintliga serverdelssystem på plats vara den stordatorer eller vissa LoB-program som integreras i arkitektur för mobila program.</span><span class="sxs-lookup"><span data-stu-id="4a310-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into the mobile application architecture.</span></span> <span data-ttu-id="4a310-106">Den här guiden kommer talar om hur bäst att göra den här integreringen rekommenderar möjlig lösning till vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="4a310-106">This guide will talk about how best to do this integration recommending possible solution to common scenarios.</span></span>

<span data-ttu-id="4a310-107">En ofta krävs för att skicka push-meddelanden till användare via sina mobila program när en händelse av intresse inträffar i backend-system.</span><span class="sxs-lookup"><span data-stu-id="4a310-107">A frequent requirement is for sending push notification to the users through their mobile application when an event of interest occurs in the backend systems.</span></span> <span data-ttu-id="4a310-108">T.ex.</span><span class="sxs-lookup"><span data-stu-id="4a310-108">E.g.</span></span> <span data-ttu-id="4a310-109">en bankkund som har den bank bank appen på sin iPhone vill meddelas när en debitering görs över en viss mängd från sitt konto eller ett intranätscenario där en medarbetare från ekonomiavdelningen som har en budget godkännande appen på sin Windows Phone inte vill givna när han hämtar en begäran om godkännande.</span><span class="sxs-lookup"><span data-stu-id="4a310-109">a bank customer who has the bank's banking app on her iPhone wants to be notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants to be notified when he gets an approval request.</span></span>

<span data-ttu-id="4a310-110">Konto eller godkännande bearbetning kommer troligen att göras i vissa backend-system som måste initiera en sändning till användaren.</span><span class="sxs-lookup"><span data-stu-id="4a310-110">The bank account or approval processing is likely to be done in some backend system which must initiate a push to the user.</span></span> <span data-ttu-id="4a310-111">Det kan finnas flera sådana backend-system som skapa samma typ av logik för att implementera push när en händelse utlöser ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="4a310-111">There may be multiple such backend systems which must all build the same kind of logic to implement push when an event triggers a notification.</span></span> <span data-ttu-id="4a310-112">Komplexitet här ligger i integrera flera serverdelssystem tillsammans med ett enda push-system där slutanvändarna kan prenumererar på olika meddelanden och det kan även finnas flera mobila program som t.ex. intranät-Appar där en mobilprogram kanske vill ta emot meddelanden från flera sådana backend-system.</span><span class="sxs-lookup"><span data-stu-id="4a310-112">The complexity here lies in integrating several backend systems together with a single push system where the end users may have subscribed to different notifications and there may even be multiple mobile applications e.g. in the case of intranet mobile apps where one mobile application may want to receive notifications from multiple such backend systems.</span></span> <span data-ttu-id="4a310-113">Backend-system inte känner till eller behöver veta för push-semantik/teknik så att en vanlig lösning har traditionellt att införa en komponent som genomsöker serverdelssystem för alla händelser och ansvarar för att skicka push-meddelanden till den klienten.</span><span class="sxs-lookup"><span data-stu-id="4a310-113">The backend systems do not know or need to know of push semantics/technology so a common solution here traditionally has been to introduce a component which polls the backend systems for any events of interest and is responsible for sending the push messages to the client.</span></span>
<span data-ttu-id="4a310-114">Här kommer vi om en ännu bättre lösning med hjälp av Azure Service Bus - ämne /-prenumeration modellen som minskar komplexiteten när lösningen skalbara.</span><span class="sxs-lookup"><span data-stu-id="4a310-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce the complexity while making the solution scalable.</span></span>

<span data-ttu-id="4a310-115">Här är lösningen allmänna arkitektur (generaliserad med flera mobila appar men lika tillämpliga när det finns endast en mobil app)</span><span class="sxs-lookup"><span data-stu-id="4a310-115">Here is the general architecture of the solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="4a310-116">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="4a310-116">Architecture</span></span>
![][1]

<span data-ttu-id="4a310-117">Den nyckel i den här Arkitekturdiagram är Azure Service Bus som ger en prenumerationer eller det avsnitt programmeringsmodellen (mer om den på [Service Bus Pub/Sub programmering]).</span><span class="sxs-lookup"><span data-stu-id="4a310-117">The key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="4a310-118">Mottagaren som i det här fallet är mobila serverdel (vanligtvis [Azure-Mobiltjänst], som initierar en push för mobila appar) inte ta emot meddelanden direkt från backend-system, men i stället har vi ett mellanliggande Abstraktionslager som tillhandahålls av [Azure Service Bus] som gör det möjligt för mobila serverdel ta emot meddelanden från en eller flera serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="4a310-118">The receiver, which in this case, is the Mobile backend (typically [Azure Mobile Service], which will initiate a push to the mobile apps) does not receive messages directly from the backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend to receive messages from one or more backend systems.</span></span> <span data-ttu-id="4a310-119">Ett Service Bus-ämne måste skapas för varje serverdelssystem t.ex. konto HR, ekonomi som i princip ”intresseområden” som initierar meddelanden ska skickas som push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4a310-119">A Service Bus Topic needs to be created for each of the backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages to be sent as push notification.</span></span> <span data-ttu-id="4a310-120">Backend-system ska skicka meddelanden till det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="4a310-120">The backend systems will send messages to these topics.</span></span> <span data-ttu-id="4a310-121">En mobila serverdel kan prenumerera på en eller flera ämnen genom att skapa en Service Bus-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4a310-121">A Mobile Backend can subscribe to one or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="4a310-122">Detta ger mobilserverdel tar emot ett meddelande från motsvarande backend-systemet.</span><span class="sxs-lookup"><span data-stu-id="4a310-122">This will entitle the mobile backend to receive a notification from the corresponding backend system.</span></span> <span data-ttu-id="4a310-123">Mobilserverdel fortsätter att lyssnar efter meddelanden på sina prenumerationer och när ett meddelande tas emot den stängs tillbaka och skickar det som meddelande till dess meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="4a310-123">Mobile backend continues to listen for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification to its notification hub.</span></span> <span data-ttu-id="4a310-124">Meddelandehubbar sedan slutligen levereras till mobilappen.</span><span class="sxs-lookup"><span data-stu-id="4a310-124">Notification hubs then eventually delivers the message to the mobile app.</span></span> <span data-ttu-id="4a310-125">Om du vill sammanfatta nyckelkomponenterna har vi:</span><span class="sxs-lookup"><span data-stu-id="4a310-125">So to summarize the key components, we have:</span></span>

1. <span data-ttu-id="4a310-126">Serverdelssystem (LoB/äldre system)</span><span class="sxs-lookup"><span data-stu-id="4a310-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="4a310-127">Skapar Service Bus-ämne</span><span class="sxs-lookup"><span data-stu-id="4a310-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="4a310-128">Skickar meddelandet</span><span class="sxs-lookup"><span data-stu-id="4a310-128">Sends Message</span></span>
2. <span data-ttu-id="4a310-129">Mobil serverdel</span><span class="sxs-lookup"><span data-stu-id="4a310-129">Mobile backend</span></span>
   * <span data-ttu-id="4a310-130">Skapar en prenumeration på tjänsten</span><span class="sxs-lookup"><span data-stu-id="4a310-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="4a310-131">Tar emot meddelandet (från Backend-system)</span><span class="sxs-lookup"><span data-stu-id="4a310-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="4a310-132">Skickar meddelandet till klienter (via Azure Notification Hub)</span><span class="sxs-lookup"><span data-stu-id="4a310-132">Sends notification to clients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="4a310-133">Mobila program</span><span class="sxs-lookup"><span data-stu-id="4a310-133">Mobile Application</span></span>
   * <span data-ttu-id="4a310-134">Tar emot och visa meddelandet</span><span class="sxs-lookup"><span data-stu-id="4a310-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="4a310-135">Fördelar:</span><span class="sxs-lookup"><span data-stu-id="4a310-135">Benefits:</span></span>
1. <span data-ttu-id="4a310-136">Frikoppling mellan mottagaren (app/Mobiltjänst via Notification Hub) och avsändaren (serverdelssystem) gör att ytterligare serverdelssystem integreras med minimal förändring.</span><span class="sxs-lookup"><span data-stu-id="4a310-136">The decoupling between the receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="4a310-137">Det gör även scenariot för att kunna ta emot händelser från en eller flera serverdelssystem för flera mobila appar.</span><span class="sxs-lookup"><span data-stu-id="4a310-137">This also makes the scenario of multiple mobile apps being able to receive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="4a310-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="4a310-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="4a310-139">Krav</span><span class="sxs-lookup"><span data-stu-id="4a310-139">Prerequisites</span></span>
<span data-ttu-id="4a310-140">Du bör genomföra följande kurser att bekanta med begrepp som vanliga Skapa & konfigurationssteg:</span><span class="sxs-lookup"><span data-stu-id="4a310-140">You should complete the following tutorials to familiarize with the concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="4a310-141">[Service Bus Pub/Sub programmering] -detta beskriver hur du arbetar med Service Bus avsnitt/prenumerationer, hur du skapar ett namnområde för att innehålla avsnitt/prenumerationer, så skicka och ta emot meddelanden från dem.</span><span class="sxs-lookup"><span data-stu-id="4a310-141">[Service Bus Pub/Sub programming] - This explains the details of working with Service Bus Topics/Subscriptions, how to create a namespace to contain topics/subscriptions, how to send & receive messages from them.</span></span>
2. <span data-ttu-id="4a310-142">[Notification Hubs – självstudiekurs för Windows Universal] -här beskrivs hur du ställer in en app för Windows Store och använda Notification Hubs för att registrera och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4a310-142">[Notification Hubs - Windows Universal tutorial] - This explains how to set up a Windows Store app and use Notification Hubs to register and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="4a310-143">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="4a310-143">Sample code</span></span>
<span data-ttu-id="4a310-144">Fullständig exempelkod finns på [Notification Hub exempel].</span><span class="sxs-lookup"><span data-stu-id="4a310-144">The full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="4a310-145">Den är uppdelad i tre komponenter:</span><span class="sxs-lookup"><span data-stu-id="4a310-145">It is split into three components:</span></span>

1. <span data-ttu-id="4a310-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="4a310-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="4a310-147">a.</span><span class="sxs-lookup"><span data-stu-id="4a310-147">a.</span></span> <span data-ttu-id="4a310-148">Det här projektet använder den *WindowsAzure.ServiceBus* Nuget-paketet och baseras på [Service Bus Pub/Sub programmering].</span><span class="sxs-lookup"><span data-stu-id="4a310-148">This project uses the *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="4a310-149">b.</span><span class="sxs-lookup"><span data-stu-id="4a310-149">b.</span></span> <span data-ttu-id="4a310-150">Det här är en enkel C#-konsolapp att simulera en LoB-systemet som startar meddelandet som ska levereras till mobilappen.</span><span class="sxs-lookup"><span data-stu-id="4a310-150">This is a simple C# console app to simulate an LoB system which initiates the message to be delivered to the mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="4a310-151">c.</span><span class="sxs-lookup"><span data-stu-id="4a310-151">c.</span></span> <span data-ttu-id="4a310-152">`CreateTopic`används för att skapa Service Bus-ämne där vi ska skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4a310-152">`CreateTopic` is used to create the Service Bus topic where we will send messages.</span></span>
   
        public static void CreateTopic(string connectionString)
        {
            // Create the topic if it does not exist already
   
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.TopicExists(sampleTopic))
            {
                namespaceManager.CreateTopic(sampleTopic);
            }
        }
   
    <span data-ttu-id="4a310-153">d.</span><span class="sxs-lookup"><span data-stu-id="4a310-153">d.</span></span> <span data-ttu-id="4a310-154">`SendMessage`används för att skicka meddelanden till den här Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="4a310-154">`SendMessage` is used to send the messages to this Service Bus Topic.</span></span> <span data-ttu-id="4a310-155">Här skickar vi bara slumpmässiga meddelanden till avsnittet regelbundet i exemplet.</span><span class="sxs-lookup"><span data-stu-id="4a310-155">Here we are simply sending a set of random messages to the topic periodically for the purpose of the sample.</span></span> <span data-ttu-id="4a310-156">Normalt kan det finnas en backend-system som ska skicka meddelanden när en händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="4a310-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
        public static void SendMessage(string connectionString)
        {
            TopicClient client =
                TopicClient.CreateFromConnectionString(connectionString, sampleTopic);
   
            // Sends random messages every 10 seconds to the topic
            string[] messages =
            {
                "Employee Id '{0}' has joined.",
                "Employee Id '{0}' has left.",
                "Employee Id '{0}' has switched to a different team."
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
2. <span data-ttu-id="4a310-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="4a310-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="4a310-158">a.</span><span class="sxs-lookup"><span data-stu-id="4a310-158">a.</span></span> <span data-ttu-id="4a310-159">Det här projektet använder den *WindowsAzure.ServiceBus* och *Microsoft.Web.WebJobs.Publish* Nuget-paket och baseras på [Service Bus Pub/Sub programmering].</span><span class="sxs-lookup"><span data-stu-id="4a310-159">This project uses the *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="4a310-160">b.</span><span class="sxs-lookup"><span data-stu-id="4a310-160">b.</span></span> <span data-ttu-id="4a310-161">Det här är ett annat C#-konsolapp som vi ska köras som en [Azure Webjobs] eftersom den har ska köras för att lyssna efter meddelanden från LoB-/ serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="4a310-161">This is another C# console app which we will run as an [Azure WebJob] since it has to run continuously to listen for messages from the LoB/backend systems.</span></span> <span data-ttu-id="4a310-162">Det här är en del av din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="4a310-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create the subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="4a310-163">c.</span><span class="sxs-lookup"><span data-stu-id="4a310-163">c.</span></span> <span data-ttu-id="4a310-164">`CreateSubscription`används för att skapa en Service Bus-prenumeration för avsnittet där backend-systemet ska skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4a310-164">`CreateSubscription` is used to create a Service Bus subscription for the topic where the backend system will send messages.</span></span> <span data-ttu-id="4a310-165">Den här komponenten kommer beroende på affärsscenario skapa en eller flera prenumerationer till motsvarande avsnitt (t.ex. vissa kan få meddelanden från HR-system, vissa från ekonomi system och så vidare)</span><span class="sxs-lookup"><span data-stu-id="4a310-165">Depending on the business scenario, this component will create one or more subscriptions to corresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
        static void CreateSubscription(string connectionString)
        {
            // Create the subscription if it does not exist already
            var namespaceManager =
                NamespaceManager.CreateFromConnectionString(connectionString);
   
            if (!namespaceManager.SubscriptionExists(sampleTopic, sampleSubscription))
            {
                namespaceManager.CreateSubscription(sampleTopic, sampleSubscription);
            }
        }
   
    <span data-ttu-id="4a310-166">d.</span><span class="sxs-lookup"><span data-stu-id="4a310-166">d.</span></span> <span data-ttu-id="4a310-167">ReceiveMessageAndSendNotification används för att läsa meddelandet från avsnittet med hjälp av prenumerationen och om Läs lyckas skapa en avisering (i Exempelscenario ett Windows interna popup-meddelande) för att skickas till den mobila program med hjälp av Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="4a310-167">ReceiveMessageAndSendNotification is used to read the message from the topic using its subscription and if the read is successful then craft a notification (in the sample scenario a Windows native toast notification) to be sent to the mobile application using Azure Notification Hubs.</span></span>
   
        static void ReceiveMessageAndSendNotification(string connectionString)
        {
            // Initialize the Notification Hub
            string hubConnectionString = CloudConfigurationManager.GetSetting
                    ("Microsoft.NotificationHub.ConnectionString");
            hub = NotificationHubClient.CreateClientFromConnectionString
                    (hubConnectionString, "enterprisepushservicehub");
   
            SubscriptionClient Client =
                SubscriptionClient.CreateFromConnectionString
                        (connectionString, sampleTopic, sampleSubscription);
   
            Client.Receive();
   
            // Continuously process messages received from the subscription
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
   
    <span data-ttu-id="4a310-168">e.</span><span class="sxs-lookup"><span data-stu-id="4a310-168">e.</span></span> <span data-ttu-id="4a310-169">För att publicera som en **Webbjobb**, högerklicka på lösningen i Visual Studio och välj **Publicera som Webbjobbet**</span><span class="sxs-lookup"><span data-stu-id="4a310-169">For publishing this as a **WebJob**, right click on the solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="4a310-170">f.</span><span class="sxs-lookup"><span data-stu-id="4a310-170">f.</span></span> <span data-ttu-id="4a310-171">Välj din publiceringsprofil och skapa en ny Azure-webbplats om den inte finns redan som ska vara värd för den här Webbjobb och när du har webbplatsen sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="4a310-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have the WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="4a310-172">g.</span><span class="sxs-lookup"><span data-stu-id="4a310-172">g.</span></span> <span data-ttu-id="4a310-173">Konfigurera jobbet för att vara ”kör kontinuerligt” så att när du loggar in på den [klassiska Azure-portalen] bör du se något som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="4a310-173">Configure the job to be "Run Continuously" so that when you log in to the [Azure Classic Portal] you should see something like the following:</span></span>
   
    ![][4]
3. <span data-ttu-id="4a310-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="4a310-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="4a310-175">a.</span><span class="sxs-lookup"><span data-stu-id="4a310-175">a.</span></span> <span data-ttu-id="4a310-176">Det här är en Windows Store-programmet som ska ta emot popup-meddelanden från Webbjobb som körs som en del av din mobila serverdel och visa den.</span><span class="sxs-lookup"><span data-stu-id="4a310-176">This is a Windows Store application which will receive toast notifications from the WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="4a310-177">Detta baseras på [Notification Hubs – självstudiekurs för Windows Universal].</span><span class="sxs-lookup"><span data-stu-id="4a310-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="4a310-178">b.</span><span class="sxs-lookup"><span data-stu-id="4a310-178">b.</span></span> <span data-ttu-id="4a310-179">Kontrollera att programmet är aktiverat för att ta emot popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="4a310-179">Ensure that your application is enabled to receive toast notifications.</span></span>
   
    <span data-ttu-id="4a310-180">c.</span><span class="sxs-lookup"><span data-stu-id="4a310-180">c.</span></span> <span data-ttu-id="4a310-181">Se till att följande Notification Hubs registration kod kallas vid appen startar (när du ersätter den *HubName* och *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="4a310-181">Ensure that the following Notification Hubs registration code is being called at the App start up (after replacing the *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("[HubName]", "[DefaultListenSharedAccessSignature]");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

### <a name="running-sample"></a><span data-ttu-id="4a310-182">Köra exemplet:</span><span class="sxs-lookup"><span data-stu-id="4a310-182">Running sample:</span></span>
1. <span data-ttu-id="4a310-183">Kontrollera att din Webbjobbet har körts och schemalagd att ”kör kontinuerligt”.</span><span class="sxs-lookup"><span data-stu-id="4a310-183">Ensure that your WebJob is running successfully and scheduled to "Run Continuously".</span></span>
2. <span data-ttu-id="4a310-184">Kör den **EnterprisePushMobileApp** som startar Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="4a310-184">Run the **EnterprisePushMobileApp** which will start the Windows Store app.</span></span>
3. <span data-ttu-id="4a310-185">Kör den **EnterprisePushBackendSystem** konsolprogram som simulerar LoB-serverdelen och kommer att börja skicka meddelanden och du bör se popup-meddelanden som visas på följande:</span><span class="sxs-lookup"><span data-stu-id="4a310-185">Run the **EnterprisePushBackendSystem** console application which will simulate the LoB backend and will start sending messages and you should see toast notifications appearing like the following:</span></span>
   
    ![][5]
4. <span data-ttu-id="4a310-186">Meddelandena som har ursprungligen skickats till Service Bus-ämnen som övervakades med Service Bus prenumerationer i ditt webb-jobb.</span><span class="sxs-lookup"><span data-stu-id="4a310-186">The messages were originally sent to Service Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="4a310-187">När ett meddelande togs emot ett meddelande skapades och skickas till mobilappen.</span><span class="sxs-lookup"><span data-stu-id="4a310-187">Once a message was received, a notification was created and sent to the mobile app.</span></span> <span data-ttu-id="4a310-188">Du kan titta igenom Webbjobb loggfilerna för att bekräfta bearbetningen när du går till länken loggar i [klassiska Azure-portalen] för Web-jobb:</span><span class="sxs-lookup"><span data-stu-id="4a310-188">You can look through the WebJob logs to confirm the processing when you go to the Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
    ![][6]

<!-- Images -->
[1]: ./media/notification-hubs-enterprise-push-architecture/architecture.png
[2]: ./media/notification-hubs-enterprise-push-architecture/WebJobsContextMenu.png
[3]: ./media/notification-hubs-enterprise-push-architecture/PublishAsWebJob.png
[4]: ./media/notification-hubs-enterprise-push-architecture/WebJob.png
[5]: ./media/notification-hubs-enterprise-push-architecture/Notifications.png
[6]: ./media/notification-hubs-enterprise-push-architecture/WebJobsLog.png

<!-- Links -->
<span data-ttu-id="4a310-189">[Notification Hub exempel]: https://github.com/Azure/azure-notificationhubs-samples</span><span class="sxs-lookup"><span data-stu-id="4a310-189">[Notification Hub Samples]: https://github.com/Azure/azure-notificationhubs-samples</span></span>
<span data-ttu-id="4a310-190">[Azure-Mobiltjänst]: http://azure.microsoft.com/documentation/services/mobile-services/</span><span class="sxs-lookup"><span data-stu-id="4a310-190">[Azure Mobile Service]: http://azure.microsoft.com/documentation/services/mobile-services/</span></span>
<span data-ttu-id="4a310-191">[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span><span class="sxs-lookup"><span data-stu-id="4a310-191">[Azure Service Bus]: http://azure.microsoft.com/documentation/articles/fundamentals-service-bus-hybrid-solutions/</span></span>
<span data-ttu-id="4a310-192">[Service Bus Pub/Sub programmering]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span><span class="sxs-lookup"><span data-stu-id="4a310-192">[Service Bus Pub/Sub programming]: http://azure.microsoft.com/documentation/articles/service-bus-dotnet-how-to-use-topics-subscriptions/</span></span>
<span data-ttu-id="4a310-193">[Azure Webjobs]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span><span class="sxs-lookup"><span data-stu-id="4a310-193">[Azure WebJob]: http://azure.microsoft.com/documentation/articles/web-sites-create-web-jobs/</span></span>
<span data-ttu-id="4a310-194">[Notification Hubs – självstudiekurs för Windows Universal]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span><span class="sxs-lookup"><span data-stu-id="4a310-194">[Notification Hubs - Windows Universal tutorial]: http://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started/</span></span>
<span data-ttu-id="4a310-195">[klassiska Azure-portalen]: https://manage.windowsazure.com/</span><span class="sxs-lookup"><span data-stu-id="4a310-195">[Azure Classic Portal]: https://manage.windowsazure.com/</span></span>
