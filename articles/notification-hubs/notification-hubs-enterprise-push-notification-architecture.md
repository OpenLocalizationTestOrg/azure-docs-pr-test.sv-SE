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
# <a name="enterprise-push-architectural-guidance"></a><span data-ttu-id="43aa6-103">Push-arkitekturvägledning för företag</span><span class="sxs-lookup"><span data-stu-id="43aa6-103">Enterprise push architectural guidance</span></span>
<span data-ttu-id="43aa6-104">Företag befordras idag gradvis mot skapa mobila program för slutanvändarna (externa) eller för hello anställda (intern).</span><span class="sxs-lookup"><span data-stu-id="43aa6-104">Enterprises today are gradually moving towards creating mobile applications for either their end users (external) or for hello employees (internal).</span></span> <span data-ttu-id="43aa6-105">De har befintliga serverdelssystem för att den stordatorer eller vissa LoB-program som integreras i hello mobilprogram arkitektur.</span><span class="sxs-lookup"><span data-stu-id="43aa6-105">They have existing backend systems in place be it mainframes or some LoB applications which must be integrated into hello mobile application architecture.</span></span> <span data-ttu-id="43aa6-106">Den här guiden kommer att tala om bästa toodo integrationen rekommenderar möjlig lösning toocommon scenarier.</span><span class="sxs-lookup"><span data-stu-id="43aa6-106">This guide will talk about how best toodo this integration recommending possible solution toocommon scenarios.</span></span>

<span data-ttu-id="43aa6-107">En ofta krävs för att skicka push notification toohello användare via sina mobila program när en händelse av intresse uppstår i hello serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="43aa6-107">A frequent requirement is for sending push notification toohello users through their mobile application when an event of interest occurs in hello backend systems.</span></span> <span data-ttu-id="43aa6-108">T.ex.</span><span class="sxs-lookup"><span data-stu-id="43aa6-108">E.g.</span></span> <span data-ttu-id="43aa6-109">en bank som har hello bank bank appen på sin iPhone kunden toobe meddelas när en debitering görs över en viss mängd från sitt konto eller ett intranätscenario där en medarbetare från ekonomiavdelningen som har en budget godkännande appen på sin Windows Phone vill toobe ett meddelande när han hämtar en begäran om godkännande.</span><span class="sxs-lookup"><span data-stu-id="43aa6-109">a bank customer who has hello bank's banking app on her iPhone wants toobe notified when a debit is made above a certain amount from her account or an intranet scenario where an employee from finance department who has a budget approval app on his Windows Phone wants toobe notified when he gets an approval request.</span></span>

<span data-ttu-id="43aa6-110">hello konto eller godkännande bearbetning är sannolikt toobe göras i vissa backend-system som måste initiera en push-toohello användare.</span><span class="sxs-lookup"><span data-stu-id="43aa6-110">hello bank account or approval processing is likely toobe done in some backend system which must initiate a push toohello user.</span></span> <span data-ttu-id="43aa6-111">Det kan finnas flera backend som måste alla bygga hello samma typ av logik tooimplement push när en händelse utlöser ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="43aa6-111">There may be multiple such backend systems which must all build hello same kind of logic tooimplement push when an event triggers a notification.</span></span> <span data-ttu-id="43aa6-112">hello komplexitet här ligger i integrera flera serverdelssystem tillsammans med ett enda push-system där hello slutanvändare kan prenumererar toodifferent meddelanden och det kan även vara flera mobila program, t.ex. i hello skiftläget för intranät-Appar ett mobilt program kanske där tooreceive meddelanden från flera sådana backend-system.</span><span class="sxs-lookup"><span data-stu-id="43aa6-112">hello complexity here lies in integrating several backend systems together with a single push system where hello end users may have subscribed toodifferent notifications and there may even be multiple mobile applications e.g. in hello case of intranet mobile apps where one mobile application may want tooreceive notifications from multiple such backend systems.</span></span> <span data-ttu-id="43aa6-113">Hej serverdelssystem inte känner till eller behöver tooknow för push-semantik/teknik för en vanlig lösning har traditionellt toointroduce en komponent som genomsöker hello serverdelssystem för alla händelser och ansvarar för att skicka hello push-meddelanden toohello klient.</span><span class="sxs-lookup"><span data-stu-id="43aa6-113">hello backend systems do not know or need tooknow of push semantics/technology so a common solution here traditionally has been toointroduce a component which polls hello backend systems for any events of interest and is responsible for sending hello push messages toohello client.</span></span>
<span data-ttu-id="43aa6-114">Här kommer vi om en ännu bättre lösning med hjälp av Azure Service Bus - ämne /-prenumeration modellen som minskar komplexiteten hello när hello lösning skalbar.</span><span class="sxs-lookup"><span data-stu-id="43aa6-114">Here we will talk about an even better solution using Azure Service Bus - Topic/Subscription model which will reduce hello complexity while making hello solution scalable.</span></span>

<span data-ttu-id="43aa6-115">Här är hello allmänna arkitektur hello-lösning (generaliserad med flera mobila appar men lika tillämpliga när det finns endast en mobil app)</span><span class="sxs-lookup"><span data-stu-id="43aa6-115">Here is hello general architecture of hello solution (generalized with multiple mobile apps but equally applicable when there is only one mobile app)</span></span>

## <a name="architecture"></a><span data-ttu-id="43aa6-116">Arkitektur</span><span class="sxs-lookup"><span data-stu-id="43aa6-116">Architecture</span></span>
![][1]

<span data-ttu-id="43aa6-117">hello nyckel i den här Arkitekturdiagram är Azure Service Bus som ger en prenumerationer eller det avsnitt programmeringsmodellen (mer om den på [Service Bus Pub/Sub programmering]).</span><span class="sxs-lookup"><span data-stu-id="43aa6-117">hello key piece in this architectural diagram is Azure Service Bus which provides a topics/subscriptions programming model (more on it at [Service Bus Pub/Sub programming]).</span></span> <span data-ttu-id="43aa6-118">hello mottagare, som i det här fallet är hello mobila serverdel (vanligtvis [Azure-Mobiltjänst], som initierar en push-toohello mobila appar) inte ta emot meddelanden direkt från hello serverdelssystem men i stället har vi ett mellanliggande Abstraktionslager som tillhandahålls av [Azure Service Bus] som gör det möjligt för mobilserverdel tooreceive meddelanden från en eller flera serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="43aa6-118">hello receiver, which in this case, is hello Mobile backend (typically [Azure Mobile Service], which will initiate a push toohello mobile apps) does not receive messages directly from hello backend systems but instead we have an intermediate abstraction layer provided by [Azure Service Bus] which enables mobile backend tooreceive messages from one or more backend systems.</span></span> <span data-ttu-id="43aa6-119">Ett Service Bus-ämne måste toobe skapas för varje hello serverdelssystem t.ex. konto HR, ekonomi som i princip ”intresseområden” som initierar meddelanden toobe skickas som push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="43aa6-119">A Service Bus Topic needs toobe created for each of hello backend systems e.g. Account, HR, Finance which are basically "topics" of interest which will initiate messages toobe sent as push notification.</span></span> <span data-ttu-id="43aa6-120">Hej serverdelssystem skickar meddelanden toothese avsnitt.</span><span class="sxs-lookup"><span data-stu-id="43aa6-120">hello backend systems will send messages toothese topics.</span></span> <span data-ttu-id="43aa6-121">En mobila serverdel kan prenumerera på tooone eller flera sådana avsnitt genom att skapa en Service Bus-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="43aa6-121">A Mobile Backend can subscribe tooone or more such topics by creating a Service Bus subscription.</span></span> <span data-ttu-id="43aa6-122">Detta ger hello mobilserverdel tooreceive ett meddelande från hello motsvarande backend-systemet.</span><span class="sxs-lookup"><span data-stu-id="43aa6-122">This will entitle hello mobile backend tooreceive a notification from hello corresponding backend system.</span></span> <span data-ttu-id="43aa6-123">Mobilserverdel fortsätter toolisten för meddelanden på sina prenumerationer och när ett meddelande tas emot den stängs tillbaka och skickar som meddelandehubben tooits meddelande.</span><span class="sxs-lookup"><span data-stu-id="43aa6-123">Mobile backend continues toolisten for messages on their subscriptions and as soon as a message arrives, it turns back and sends it as notification tooits notification hub.</span></span> <span data-ttu-id="43aa6-124">Notification hubs ger sedan slutligen hello meddelandet toohello mobila appar.</span><span class="sxs-lookup"><span data-stu-id="43aa6-124">Notification hubs then eventually delivers hello message toohello mobile app.</span></span> <span data-ttu-id="43aa6-125">Toosummarize hello viktiga komponenter, har vi:</span><span class="sxs-lookup"><span data-stu-id="43aa6-125">So toosummarize hello key components, we have:</span></span>

1. <span data-ttu-id="43aa6-126">Serverdelssystem (LoB/äldre system)</span><span class="sxs-lookup"><span data-stu-id="43aa6-126">Backend systems (LoB/Legacy systems)</span></span>
   * <span data-ttu-id="43aa6-127">Skapar Service Bus-ämne</span><span class="sxs-lookup"><span data-stu-id="43aa6-127">Creates Service Bus Topic</span></span>
   * <span data-ttu-id="43aa6-128">Skickar meddelandet</span><span class="sxs-lookup"><span data-stu-id="43aa6-128">Sends Message</span></span>
2. <span data-ttu-id="43aa6-129">Mobil serverdel</span><span class="sxs-lookup"><span data-stu-id="43aa6-129">Mobile backend</span></span>
   * <span data-ttu-id="43aa6-130">Skapar en prenumeration på tjänsten</span><span class="sxs-lookup"><span data-stu-id="43aa6-130">Creates Service Subscription</span></span>
   * <span data-ttu-id="43aa6-131">Tar emot meddelandet (från Backend-system)</span><span class="sxs-lookup"><span data-stu-id="43aa6-131">Receives Message (from Backend system)</span></span>
   * <span data-ttu-id="43aa6-132">Skickar meddelandet tooclients (via Azure Notification Hub)</span><span class="sxs-lookup"><span data-stu-id="43aa6-132">Sends notification tooclients (via Azure Notification Hub)</span></span>
3. <span data-ttu-id="43aa6-133">Mobila program</span><span class="sxs-lookup"><span data-stu-id="43aa6-133">Mobile Application</span></span>
   * <span data-ttu-id="43aa6-134">Tar emot och visa meddelandet</span><span class="sxs-lookup"><span data-stu-id="43aa6-134">Receives and display notification</span></span>

### <a name="benefits"></a><span data-ttu-id="43aa6-135">Fördelar:</span><span class="sxs-lookup"><span data-stu-id="43aa6-135">Benefits:</span></span>
1. <span data-ttu-id="43aa6-136">hello sambandet mellan hello mottagare (app/Mobiltjänst via Notification Hub) och avsändaren (serverdelssystem) gör att ytterligare serverdelssystem integreras med minimal förändring.</span><span class="sxs-lookup"><span data-stu-id="43aa6-136">hello decoupling between hello receiver (mobile app/service via Notification Hub) and sender (backend systems) enables additional backend systems being integrated with minimal change.</span></span>
2. <span data-ttu-id="43aa6-137">Det gör även hello scenario där flera mobila appar som kan tooreceive händelser från en eller flera serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="43aa6-137">This also makes hello scenario of multiple mobile apps being able tooreceive events from one or more backend systems.</span></span>  

## <a name="sample"></a><span data-ttu-id="43aa6-138">Exempel:</span><span class="sxs-lookup"><span data-stu-id="43aa6-138">Sample:</span></span>
### <a name="prerequisites"></a><span data-ttu-id="43aa6-139">Krav</span><span class="sxs-lookup"><span data-stu-id="43aa6-139">Prerequisites</span></span>
<span data-ttu-id="43aa6-140">Du bör utföra hello följande kurser toofamiliarize hello koncept samt vanliga åtgärder för skapande och konfiguration:</span><span class="sxs-lookup"><span data-stu-id="43aa6-140">You should complete hello following tutorials toofamiliarize with hello concepts as well as common creation & configuration steps:</span></span>

1. <span data-ttu-id="43aa6-141">[Service Bus Pub/Sub programmering] -det förklarar hello detaljer att arbeta med prenumerationer/Service Bus avsnitt, hur toocreate ett namnområde toocontain avsnitt/prenumerationer, hur toosend & ta emot meddelanden från dem.</span><span class="sxs-lookup"><span data-stu-id="43aa6-141">[Service Bus Pub/Sub programming] - This explains hello details of working with Service Bus Topics/Subscriptions, how toocreate a namespace toocontain topics/subscriptions, how toosend & receive messages from them.</span></span>
2. <span data-ttu-id="43aa6-142">[Notification Hubs – självstudiekurs för Windows Universal] -det förklarar hur tooset upp en Windows Store-app och använda Notification Hubs tooregister och ta emot meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43aa6-142">[Notification Hubs - Windows Universal tutorial] - This explains how tooset up a Windows Store app and use Notification Hubs tooregister and then receive notifications.</span></span>

### <a name="sample-code"></a><span data-ttu-id="43aa6-143">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="43aa6-143">Sample code</span></span>
<span data-ttu-id="43aa6-144">hello fullständig exempelkod finns på [Notification Hub exempel].</span><span class="sxs-lookup"><span data-stu-id="43aa6-144">hello full sample code is available at [Notification Hub Samples].</span></span> <span data-ttu-id="43aa6-145">Den är uppdelad i tre komponenter:</span><span class="sxs-lookup"><span data-stu-id="43aa6-145">It is split into three components:</span></span>

1. <span data-ttu-id="43aa6-146">**EnterprisePushBackendSystem**</span><span class="sxs-lookup"><span data-stu-id="43aa6-146">**EnterprisePushBackendSystem**</span></span>
   
    <span data-ttu-id="43aa6-147">a.</span><span class="sxs-lookup"><span data-stu-id="43aa6-147">a.</span></span> <span data-ttu-id="43aa6-148">Det här projektet använder hello *WindowsAzure.ServiceBus* Nuget-paketet och baseras på [Service Bus Pub/Sub programmering].</span><span class="sxs-lookup"><span data-stu-id="43aa6-148">This project uses hello *WindowsAzure.ServiceBus* Nuget package and is  based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="43aa6-149">b.</span><span class="sxs-lookup"><span data-stu-id="43aa6-149">b.</span></span> <span data-ttu-id="43aa6-150">Det här är en enkel C#-konsolen app toosimulate LoB-systemet som startar hello meddelandet toobe levereras toohello mobila appar.</span><span class="sxs-lookup"><span data-stu-id="43aa6-150">This is a simple C# console app toosimulate an LoB system which initiates hello message toobe delivered toohello mobile app.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello topic where we will send notifications
            CreateTopic(connectionString);
   
            // Send message
            SendMessage(connectionString);
        }
   
    <span data-ttu-id="43aa6-151">c.</span><span class="sxs-lookup"><span data-stu-id="43aa6-151">c.</span></span> <span data-ttu-id="43aa6-152">`CreateTopic`är används toocreate hello Service Bus-ämne där vi ska skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43aa6-152">`CreateTopic` is used toocreate hello Service Bus topic where we will send messages.</span></span>
   
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
   
    <span data-ttu-id="43aa6-153">d.</span><span class="sxs-lookup"><span data-stu-id="43aa6-153">d.</span></span> <span data-ttu-id="43aa6-154">`SendMessage`är används toosend hello meddelanden toothis Service Bus-ämne.</span><span class="sxs-lookup"><span data-stu-id="43aa6-154">`SendMessage` is used toosend hello messages toothis Service Bus Topic.</span></span> <span data-ttu-id="43aa6-155">Här skickar vi helt enkelt en uppsättning slumpmässiga meddelanden toohello avsnittet med jämna mellanrum för hello syftet hello exempel.</span><span class="sxs-lookup"><span data-stu-id="43aa6-155">Here we are simply sending a set of random messages toohello topic periodically for hello purpose of hello sample.</span></span> <span data-ttu-id="43aa6-156">Normalt kan det finnas en backend-system som ska skicka meddelanden när en händelse inträffar.</span><span class="sxs-lookup"><span data-stu-id="43aa6-156">Normally there will be a backend system which will send messages when an event occurs.</span></span>
   
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
2. <span data-ttu-id="43aa6-157">**ReceiveAndSendNotification**</span><span class="sxs-lookup"><span data-stu-id="43aa6-157">**ReceiveAndSendNotification**</span></span>
   
    <span data-ttu-id="43aa6-158">a.</span><span class="sxs-lookup"><span data-stu-id="43aa6-158">a.</span></span> <span data-ttu-id="43aa6-159">Det här projektet använder hello *WindowsAzure.ServiceBus* och *Microsoft.Web.WebJobs.Publish* Nuget-paket och baseras på [Service Bus Pub/Sub programmering].</span><span class="sxs-lookup"><span data-stu-id="43aa6-159">This project uses hello *WindowsAzure.ServiceBus* and *Microsoft.Web.WebJobs.Publish* Nuget packages and is based on [Service Bus Pub/Sub programming].</span></span>
   
    <span data-ttu-id="43aa6-160">b.</span><span class="sxs-lookup"><span data-stu-id="43aa6-160">b.</span></span> <span data-ttu-id="43aa6-161">Det här är ett annat C#-konsolapp som vi ska köras som en [Azure Webjobs] eftersom den har toorun kontinuerligt toolisten för meddelanden från hello LoB/serverdelssystem.</span><span class="sxs-lookup"><span data-stu-id="43aa6-161">This is another C# console app which we will run as an [Azure WebJob] since it has toorun continuously toolisten for messages from hello LoB/backend systems.</span></span> <span data-ttu-id="43aa6-162">Det här är en del av din mobila serverdel.</span><span class="sxs-lookup"><span data-stu-id="43aa6-162">This will be part of your Mobile backend.</span></span>
   
        static void Main(string[] args)
        {
            string connectionString =
                     CloudConfigurationManager.GetSetting("Microsoft.ServiceBus.ConnectionString");
   
            // Create hello subscription which will receive messages
            CreateSubscription(connectionString);
   
            // Receive message
            ReceiveMessageAndSendNotification(connectionString);
        }
   
    <span data-ttu-id="43aa6-163">c.</span><span class="sxs-lookup"><span data-stu-id="43aa6-163">c.</span></span> <span data-ttu-id="43aa6-164">`CreateSubscription`är används toocreate en Service Bus-prenumeration för hello avsnittet där hello backend-systemet ska skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43aa6-164">`CreateSubscription` is used toocreate a Service Bus subscription for hello topic where hello backend system will send messages.</span></span> <span data-ttu-id="43aa6-165">Beroende på hello affärsscenario skapar den här komponenten en eller flera prenumerationer toocorresponding avsnitt (t.ex. vissa kan få meddelanden från HR-system, vissa från ekonomi system och så vidare)</span><span class="sxs-lookup"><span data-stu-id="43aa6-165">Depending on hello business scenario, this component will create one or more subscriptions toocorresponding topics (e.g. some may be receiving messages from HR system, some from Finance system, and so on)</span></span>
   
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
   
    <span data-ttu-id="43aa6-166">d.</span><span class="sxs-lookup"><span data-stu-id="43aa6-166">d.</span></span> <span data-ttu-id="43aa6-167">ReceiveMessageAndSendNotification är används tooread hello-meddelande från hello avsnitt med prenumerationen och hello läsa har lyckats sedan skapa ett meddelande (i hello är ett exempel på ett Windows interna popup-meddelande) toobe skickas toohello mobila programmet som använder Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="43aa6-167">ReceiveMessageAndSendNotification is used tooread hello message from hello topic using its subscription and if hello read is successful then craft a notification (in hello sample scenario a Windows native toast notification) toobe sent toohello mobile application using Azure Notification Hubs.</span></span>
   
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
   
    <span data-ttu-id="43aa6-168">e.</span><span class="sxs-lookup"><span data-stu-id="43aa6-168">e.</span></span> <span data-ttu-id="43aa6-169">För att publicera som en **Webbjobb**, högerklicka på hello lösningen i Visual Studio och välj **Publicera som Webbjobbet**</span><span class="sxs-lookup"><span data-stu-id="43aa6-169">For publishing this as a **WebJob**, right click on hello solution in Visual Studio and select **Publish as WebJob**</span></span>
   
    ![][2]
   
    <span data-ttu-id="43aa6-170">f.</span><span class="sxs-lookup"><span data-stu-id="43aa6-170">f.</span></span> <span data-ttu-id="43aa6-171">Välj din publiceringsprofil och skapa en ny Azure-webbplats om den inte finns redan som ska vara värd för den här Webbjobb och när du har hello webbplats sedan **publicera**.</span><span class="sxs-lookup"><span data-stu-id="43aa6-171">Select your publishing profile and create a new Azure WebSite if it doesnt exist already which will host this WebJob and once you have hello WebSite then **Publish**.</span></span>
   
    ![][3]
   
    <span data-ttu-id="43aa6-172">g.</span><span class="sxs-lookup"><span data-stu-id="43aa6-172">g.</span></span> <span data-ttu-id="43aa6-173">Konfigurera hello jobbet toobe ”kör kontinuerligt” så att när du loggar in i toohello [klassiska Azure-portalen] du bör se något liknande hello följande:</span><span class="sxs-lookup"><span data-stu-id="43aa6-173">Configure hello job toobe "Run Continuously" so that when you log in toohello [Azure Classic Portal] you should see something like hello following:</span></span>
   
    ![][4]
3. <span data-ttu-id="43aa6-174">**EnterprisePushMobileApp**</span><span class="sxs-lookup"><span data-stu-id="43aa6-174">**EnterprisePushMobileApp**</span></span>
   
    <span data-ttu-id="43aa6-175">a.</span><span class="sxs-lookup"><span data-stu-id="43aa6-175">a.</span></span> <span data-ttu-id="43aa6-176">Det här är en Windows Store-programmet som ska ta emot popup-meddelanden från hello Webbjobbet körs som en del av din mobila serverdel och visa den.</span><span class="sxs-lookup"><span data-stu-id="43aa6-176">This is a Windows Store application which will receive toast notifications from hello WebJob running as part of your Mobile backend and display it.</span></span> <span data-ttu-id="43aa6-177">Detta baseras på [Notification Hubs – självstudiekurs för Windows Universal].</span><span class="sxs-lookup"><span data-stu-id="43aa6-177">This is based on [Notification Hubs - Windows Universal tutorial].</span></span>  
   
    <span data-ttu-id="43aa6-178">b.</span><span class="sxs-lookup"><span data-stu-id="43aa6-178">b.</span></span> <span data-ttu-id="43aa6-179">Kontrollera att programmet är aktiverat tooreceive popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43aa6-179">Ensure that your application is enabled tooreceive toast notifications.</span></span>
   
    <span data-ttu-id="43aa6-180">c.</span><span class="sxs-lookup"><span data-stu-id="43aa6-180">c.</span></span> <span data-ttu-id="43aa6-181">Se till att hello följande Notification Hubs registration kod anropas vid hello appen startar (när du ersätter hello *HubName* och *DefaultListenSharedAccessSignature*:</span><span class="sxs-lookup"><span data-stu-id="43aa6-181">Ensure that hello following Notification Hubs registration code is being called at hello App start up (after replacing hello *HubName* and *DefaultListenSharedAccessSignature*:</span></span>
   
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

### <a name="running-sample"></a><span data-ttu-id="43aa6-182">Köra exemplet:</span><span class="sxs-lookup"><span data-stu-id="43aa6-182">Running sample:</span></span>
1. <span data-ttu-id="43aa6-183">Kontrollera att din Webbjobbet har körts och schemalagda för ”kör kontinuerligt”.</span><span class="sxs-lookup"><span data-stu-id="43aa6-183">Ensure that your WebJob is running successfully and scheduled too"Run Continuously".</span></span>
2. <span data-ttu-id="43aa6-184">Kör hello **EnterprisePushMobileApp** som startar hello Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="43aa6-184">Run hello **EnterprisePushMobileApp** which will start hello Windows Store app.</span></span>
3. <span data-ttu-id="43aa6-185">Kör hello **EnterprisePushBackendSystem** konsolprogram som simulerar hello LoB serverdelen och kommer att börja skicka meddelanden och du bör se popup-meddelanden som visas som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="43aa6-185">Run hello **EnterprisePushBackendSystem** console application which will simulate hello LoB backend and will start sending messages and you should see toast notifications appearing like hello following:</span></span>
   
    ![][5]
4. <span data-ttu-id="43aa6-186">hälsningsmeddelande skickades ursprungligen tooService Bus-ämnen som övervakades med Service Bus prenumerationer i ditt webb-jobb.</span><span class="sxs-lookup"><span data-stu-id="43aa6-186">hello messages were originally sent tooService Bus topics which was being monitored by Service Bus subscriptions in your Web Job.</span></span> <span data-ttu-id="43aa6-187">När ett meddelande togs emot ett meddelande skapades och skickas toohello mobila appar.</span><span class="sxs-lookup"><span data-stu-id="43aa6-187">Once a message was received, a notification was created and sent toohello mobile app.</span></span> <span data-ttu-id="43aa6-188">Du kan titta igenom hello Webbjobb loggar tooconfirm hello bearbetning när du går toohello loggar länken i [klassiska Azure-portalen] för Web-jobb:</span><span class="sxs-lookup"><span data-stu-id="43aa6-188">You can look through hello WebJob logs tooconfirm hello processing when you go toohello Logs link in [Azure Classic Portal] for your Web Job:</span></span>
   
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
