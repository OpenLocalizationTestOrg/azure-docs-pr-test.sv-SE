---
title: "aaaiOS Push-meddelanden med Notification Hubs för Xamarin-appar | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooa Xamarin iOS-program."
services: notification-hubs
keywords: "push-meddelanden för ios, push-meddelanden, push-aviseringar, push-avisering"
documentationcenter: xamarin
author: ysxu
manager: erikre
editor: 
ms.assetid: 4d4dfd42-c5a5-4360-9d70-7812f96924d2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8db60338047dd53074b4d3d4bb127aa6d9f13a25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ios-push-notifications-with-notification-hubs-for-xamarin-apps"></a><span data-ttu-id="74c61-104">iOS-pushmeddelanden med Notification Hubs för Xamarin-appar</span><span class="sxs-lookup"><span data-stu-id="74c61-104">iOS Push Notifications with Notification Hubs for Xamarin apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="74c61-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="74c61-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="74c61-106">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="74c61-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="74c61-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="74c61-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="74c61-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="74c61-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-ios-get-started).</span></span>
> 
> 

<span data-ttu-id="74c61-109">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program.</span><span class="sxs-lookup"><span data-stu-id="74c61-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooan iOS application.</span></span>
<span data-ttu-id="74c61-110">Du skapar en tom Xamarin.iOS-app som tar emot push-meddelanden med hjälp av hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span><span class="sxs-lookup"><span data-stu-id="74c61-110">You'll create a blank Xamarin.iOS app that receives push notifications by using hello [Apple Push Notification Service (APNs)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html).</span></span> <span data-ttu-id="74c61-111">När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="74c61-111">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span> <span data-ttu-id="74c61-112">hello färdiga koden finns i hello [NotificationHubs app] [ GitHub] exempel.</span><span class="sxs-lookup"><span data-stu-id="74c61-112">hello finished code is available in hello [NotificationHubs app][GitHub] sample.</span></span>

<span data-ttu-id="74c61-113">Den här kursen visar hello enkla push meddelandescenario för sändning med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="74c61-113">This tutorial demonstrates hello simple push message broadcast scenario with Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74c61-114">Krav</span><span class="sxs-lookup"><span data-stu-id="74c61-114">Prerequisites</span></span>
<span data-ttu-id="74c61-115">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="74c61-115">This tutorial requires hello following:</span></span>

* <span data-ttu-id="74c61-116">[Xcode 6.0][Install Xcode]</span><span class="sxs-lookup"><span data-stu-id="74c61-116">[Xcode 6.0][Install Xcode]</span></span>
* <span data-ttu-id="74c61-117">En enhet som är kompatibel med iOS 7.0 (eller senare version)</span><span class="sxs-lookup"><span data-stu-id="74c61-117">An iOS 7.0 (or later version) compatible device</span></span>
* <span data-ttu-id="74c61-118">Medlemskap i ett utvecklarprogram för iOS</span><span class="sxs-lookup"><span data-stu-id="74c61-118">iOS Developer Program membership</span></span>
* <span data-ttu-id="74c61-119">[Xamarin Studio]</span><span class="sxs-lookup"><span data-stu-id="74c61-119">[Xamarin Studio]</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="74c61-120">På grund av konfigurationskrav för iOS push-meddelanden, måste du distribuera och testa hello exempelprogrammet på en fysisk iOS-enhet (iPhone eller iPad) i stället för i simulatorn hello.</span><span class="sxs-lookup"><span data-stu-id="74c61-120">Because of configuration requirements for iOS push notifications, you must deploy and test hello sample application on a physical iOS device (iPhone or iPad) instead of in hello simulator.</span></span>
  > 
  > 

<span data-ttu-id="74c61-121">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Xamarin-iOS-appar.</span><span class="sxs-lookup"><span data-stu-id="74c61-121">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Xamarin iOS apps.</span></span>

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub"></a><span data-ttu-id="74c61-122">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="74c61-122">Configure your notification hub</span></span>
<span data-ttu-id="74c61-123">Det här avsnittet vägleder dig genom att skapa en ny meddelandehubb och konfigurerar autentisering med APNS med hello **.p12** push-certifikat som du skapade.</span><span class="sxs-lookup"><span data-stu-id="74c61-123">This section walks you through creating a new notification hub and configuring authentication with APNS using hello **.p12** push certificate that you created.</span></span> <span data-ttu-id="74c61-124">Om du vill toouse en meddelandehubb som du redan har skapat kan du hoppa över toostep 5.</span><span class="sxs-lookup"><span data-stu-id="74c61-124">If you want toouse a notification hub that you have already created, you can skip toostep 5.</span></span>

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="7">

<li>

<p><span data-ttu-id="74c61-125">Vi vill tooconfigure hello APN-anslutningen i hello Azure-portalen, öppna inställningarna för Meddelandehubben, hubs och klicka på <b>Notification Services</b>, och klicka sedan på hello <b>Apple (APNS)</b> objekt i listan om hello.</span><span class="sxs-lookup"><span data-stu-id="74c61-125">As we want tooconfigure hello APNS connection, in hello Azure Portal, open your Notification Hub settings, ande click on <b>Notification Services</b>, and then click hello <b>Apple (APNS)</b> item in hello list.</span></span> <span data-ttu-id="74c61-126">När du har gjort, klicka på <b>överför certifikat</b> och välj hello <b>.p12</b> certifikat som du exporterade tidigare, samt hello lösenordet för hello certifikatet.</span><span class="sxs-lookup"><span data-stu-id="74c61-126">Once done, click on <b>Upload Certificate</b> and select hello <b>.p12</b> certificate that you exported earlier, as well as hello password for hello certificate.</span></span></p>

<p><span data-ttu-id="74c61-127">Se till att tooselect <b>Sandbox</b> läge eftersom du kommer att skicka push-meddelanden i en utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="74c61-127">Make sure tooselect <b>Sandbox</b> mode since you will be sending push messages in a development environment.</span></span> <span data-ttu-id="74c61-128">Använd bara hello <b>produktion</b> inställningen om du vill toosend push-meddelanden toousers som redan har köpt din app hello butiken.</span><span class="sxs-lookup"><span data-stu-id="74c61-128">Only use hello <b>Production</b> setting if you want toosend push notifications toousers who already purchased your app from hello store.</span></span></p>
</li>
</ol>
<span data-ttu-id="74c61-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span><span class="sxs-lookup"><span data-stu-id="74c61-129">&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-apns.png)</span></span>

&emsp;&emsp;![](./media/notification-hubs-ios-get-started/notification-hubs-sandbox.png)

<span data-ttu-id="74c61-130">Din meddelandehubb är nu konfigurerad toowork med APNS och du har hello anslutning strängar tooregister din app och skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="74c61-130">Your notification hub is now configured toowork with APNS, and you have hello connection strings tooregister your app and send push notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="74c61-131">Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="74c61-131">Connect your app toohello notification hub</span></span>
#### <a name="create-a-new-project"></a><span data-ttu-id="74c61-132">Skapa ett nytt projekt</span><span class="sxs-lookup"><span data-stu-id="74c61-132">Create a new project</span></span>
1. <span data-ttu-id="74c61-133">Skapa ett nytt iOS-projekt i Xamarin Studio och välj hello **enhetligt API** > **program enkel vy** mall.</span><span class="sxs-lookup"><span data-stu-id="74c61-133">In Xamarin Studio, create a new iOS project and select hello **Unified API** > **Single View Application** template.</span></span>
   
     ![Xamarin Studio – Välj apptyp][31]
2. <span data-ttu-id="74c61-135">Lägg till en referens toohello Azure Messaging komponent.</span><span class="sxs-lookup"><span data-stu-id="74c61-135">Add a reference toohello Azure Messaging component.</span></span> <span data-ttu-id="74c61-136">I hello vyn lösning högerklickar du på hello **komponenter** för ditt projekt och välj **få fler komponenter**.</span><span class="sxs-lookup"><span data-stu-id="74c61-136">In hello Solution view, right-click hello **Components** folder for your project and choose **Get More Components**.</span></span> <span data-ttu-id="74c61-137">Sök efter hello **Azure Messaging** komponenten och Lägg till hello komponenten tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="74c61-137">Search for hello **Azure Messaging** component and add hello component tooyour project.</span></span>
3. <span data-ttu-id="74c61-138">I **AppDelegate.cs**, lägga till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="74c61-138">In **AppDelegate.cs**, add hello following using statement:</span></span>
   
        using WindowsAzure.Messaging;
4. <span data-ttu-id="74c61-139">Deklarera en instans av **SBNotificationHub**:</span><span class="sxs-lookup"><span data-stu-id="74c61-139">Declare an instance of **SBNotificationHub**:</span></span>
   
        private SBNotificationHub Hub { get; set; }
5. <span data-ttu-id="74c61-140">Skapa en **Constants.cs** klass med hello följande variabler:</span><span class="sxs-lookup"><span data-stu-id="74c61-140">Create a **Constants.cs** class with hello following variables:</span></span>
   
        // Azure app-specific connection string and hub path
        public const string ConnectionString = "<Azure connection string>";
        public const string NotificationHubPath = "<Azure hub path>";
6. <span data-ttu-id="74c61-141">I **AppDelegate.cs**, uppdatera **FinishedLaunching()** toomatch hello följande:</span><span class="sxs-lookup"><span data-stu-id="74c61-141">In **AppDelegate.cs**, update **FinishedLaunching()** toomatch hello following:</span></span>
   
        public override bool FinishedLaunching(UIApplication application, NSDictionary launchOptions)
        {
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());
   
                UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
                UIApplication.SharedApplication.RegisterForRemoteNotifications ();
            } else {
                UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
                UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
            }
   
            return true;
        }
7. <span data-ttu-id="74c61-142">Åsidosätt hello **RegisteredForRemoteNotifications()** metod i **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="74c61-142">Override hello **RegisteredForRemoteNotifications()** method in **AppDelegate.cs**:</span></span>
   
        public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
        {
            Hub = new SBNotificationHub(Constants.ConnectionString, Constants.NotificationHubPath);
   
            Hub.UnregisterAllAsync (deviceToken, (error) => {
                if (error != null)
                {
                    Console.WriteLine("Error calling Unregister: {0}", error.ToString());
                    return;
                }
   
                NSSet tags = null; // create tags if you want
                Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) => {
                    if (errorCallback != null)
                        Console.WriteLine("RegisterNativeAsync error: " + errorCallback.ToString());
                });
            });
        }
8. <span data-ttu-id="74c61-143">Åsidosätt hello **ReceivedRemoteNotification()** metod i **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="74c61-143">Override hello **ReceivedRemoteNotification()** method in **AppDelegate.cs**:</span></span>
   
        public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
        {
            ProcessNotification(userInfo, false);
        }
9. <span data-ttu-id="74c61-144">Skapa följande hello **ProcessNotification()** metod i **AppDelegate.cs**:</span><span class="sxs-lookup"><span data-stu-id="74c61-144">Create hello following **ProcessNotification()** method in **AppDelegate.cs**:</span></span>
   
        void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
        {
            // Check toosee if hello dictionary has hello aps key.  This is hello notification payload you would have sent
            if (null != options && options.ContainsKey(new NSString("aps")))
            {
                //Get hello aps dictionary
                NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
   
                string alert = string.Empty;
   
                //Extract hello alert text
                // NOTE: If you're using hello simple alert by just specifying
                // "  aps:{alert:"alert msg here"}  ", this will work fine.
                // But if you're using a complex alert with Localization keys, etc.,
                // your "alert" object from hello aps dictionary will be another NSDictionary.
                // Basically hello JSON gets dumped right into a NSDictionary,
                // so keep that in mind.
                if (aps.ContainsKey(new NSString("alert")))
                    alert = (aps [new NSString("alert")] as NSString).ToString();
   
                //If this came from hello ReceivedRemoteNotification while hello app was running,
                // we of course need toomanually process things like hello sound, badge, and alert.
                if (!fromFinishedLaunching)
                {
                    //Manually show an alert
                    if (!string.IsNullOrEmpty(alert))
                    {
                        UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                        avAlert.Show();
                    }
                }
            }
        }
   
   > [!NOTE]
   > <span data-ttu-id="74c61-145">Du kan välja toooverride **FailedToRegisterForRemoteNotifications()** toohandle situationer, till exempel någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="74c61-145">You can choose toooverride **FailedToRegisterForRemoteNotifications()** toohandle situations such as no network connection.</span></span> <span data-ttu-id="74c61-146">Detta är särskilt viktigt där hello användaren kan starta appen i offline-läge (t.ex. Flygplansläge) och du vill toohandle push-meddelanden scenarier specifika tooyour app.</span><span class="sxs-lookup"><span data-stu-id="74c61-146">This is especially important where hello user might start your application in offline mode (e.g. Airplane) and you want toohandle push messaging scenarios specific tooyour app.</span></span>
   > 
   > 
10. <span data-ttu-id="74c61-147">Kör hello app på enheten.</span><span class="sxs-lookup"><span data-stu-id="74c61-147">Run hello app on your device.</span></span>

## <a name="sending-push-notifications"></a><span data-ttu-id="74c61-148">Skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="74c61-148">Sending Push Notifications</span></span>
<span data-ttu-id="74c61-149">Du kan testa att ta emot push-meddelanden i appen genom att skicka meddelanden i hello [Azure Portal] via hello **prova att skicka** funktionen hello **felsökning** verktygsuppsättningen, höger hello notification hub på sidan som visas i hello-skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="74c61-149">You can test receiving push notifications in your app by sending notifications in hello [Azure Portal] via hello **Test Send** capability in hello **Troubleshooting** toolset, right in hello notification hub page, as shown in hello screen below.</span></span>

![](./media/notification-hubs-ios-get-started/notification-hubs-test-send.png)

<span data-ttu-id="74c61-150">Push-meddelanden skickas vanligtvis via en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="74c61-150">Push notifications are normally sent through a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="74c61-151">Du kan också använda hello REST API direkt toosend push-meddelanden om ett bibliotek inte är tillgängligt i ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="74c61-151">You can also use hello REST API directly toosend push messages if a library is not available in your scenario.</span></span> 

<span data-ttu-id="74c61-152">I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="74c61-152">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="74c61-153">Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="74c61-153">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="74c61-154">Hello följande metoder kan dock användas för att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="74c61-154">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="74c61-155">**REST-gränssnittet**: du kan använda push-meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="74c61-155">**REST Interface**:  You can support push notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="74c61-156">**Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="74c61-156">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="74c61-157">**Node.js** : [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="74c61-157">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>

<span data-ttu-id="74c61-158">**Mobile Apps**: ett exempel på hur toosend meddelanden från en serverdel för Azure Apptjänst Mobilappar som är integrerad med Notification Hubs finns [Lägg till push-meddelanden tooyour mobilappen](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="74c61-158">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>

* <span data-ttu-id="74c61-159">**Java / PHP**: ett exempel på hur hello toosend push-meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="74c61-159">**Java / PHP**: For an example of how toosend push notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

#### <a name="optional-send-push-notifications-from-a-net-console-app"></a><span data-ttu-id="74c61-160">(Valfritt) Skicka push-meddelanden från en .NET-konsolapp</span><span class="sxs-lookup"><span data-stu-id="74c61-160">(Optional) Send Push Notifications from a .NET Console App</span></span>
<span data-ttu-id="74c61-161">I det här avsnittet skickar du meddelanden med en .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="74c61-161">In this section, we will send push notifications by using a simple .NET console app.</span></span> <span data-ttu-id="74c61-162">Hello enligt det här exemplet växlar vi tooa Windows-utvecklingsmiljö där Visual Studio som redan har installerats.</span><span class="sxs-lookup"><span data-stu-id="74c61-162">For hello purposes of this example, we will switch tooa Windows development environment that has Visual Studio already installed.</span></span>

1. <span data-ttu-id="74c61-163">Skapa en ny Visual C#-konsolapp i Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="74c61-163">In Visual Studio, create a new Visual C# console application:</span></span>
   
       ![Visual Studio - Create a new console application][213]
2. <span data-ttu-id="74c61-164">I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="74c61-164">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="74c61-165">Hej pakethanterarkonsolen bör visas dockad toohello längst ned i Visual Studio-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="74c61-165">hello package manager console should appear docked toohello bottom of your Visual Studio workspace.</span></span>
3. <span data-ttu-id="74c61-166">Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="74c61-166">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="74c61-167">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="74c61-167">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="74c61-168">Öppna hello `Program.cs` och Lägg till följande hello `using` instruktionen, se till att du kan använda Azure-klasser och funktioner i huvudklassen:</span><span class="sxs-lookup"><span data-stu-id="74c61-168">Open hello `Program.cs` file and add hello following `using` statement, ensuring that we can use Azure classes and functions within your main class:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="74c61-169">I din `Program` klassen, Lägg till följande metod hello (Glöm inte tooreplace hello **anslutningssträngen** och **hubbnamn**):</span><span class="sxs-lookup"><span data-stu-id="74c61-169">In your `Program` class, add hello following method (don't forget tooreplace hello **connection string** and **hub name**):</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var alert = "{\"aps\":{\"alert\":\"Hello from .NET!\"}}";
            await hub.SendAppleNativeNotificationAsync(alert);
        }
6. <span data-ttu-id="74c61-170">Lägg till följande rader i hello din `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="74c61-170">Add hello following lines in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="74c61-171">Tryck på hello F5 viktiga toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="74c61-171">Press hello F5 key toorun hello app.</span></span> <span data-ttu-id="74c61-172">Inom några sekunder bör du se ett push-meddelande på enheten.</span><span class="sxs-lookup"><span data-stu-id="74c61-172">Within seconds, you should see a push notification appear on your device.</span></span> <span data-ttu-id="74c61-173">Om du använder Wi-Fi eller ett mobilt nätverk, se till att du har en aktiv Internetanslutning på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="74c61-173">Whether you are using Wi-Fi or a cellular data network, make sure that you have an active internet connection on hello device.</span></span>

<span data-ttu-id="74c61-174">Du hittar alla möjliga nyttolaster för hello i hello Apple [Push Notification Programmeringsguide för lokala och].</span><span class="sxs-lookup"><span data-stu-id="74c61-174">You can find all hello possible payloads in hello Apple [Local and Push Notification Programming Guide].</span></span>

#### <a name="optional-send-notifications-from-a-mobile-service"></a><span data-ttu-id="74c61-175">(Valfritt) Skicka meddelanden med en mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="74c61-175">(Optional) Send Notifications from a Mobile Service</span></span>
<span data-ttu-id="74c61-176">I det här avsnittet skickar du push-meddelanden med en mobiltjänst via ett nodskript.</span><span class="sxs-lookup"><span data-stu-id="74c61-176">In this section, we will send push notifications using a mobile service through a node script.</span></span>

<span data-ttu-id="74c61-177">Följ toosend ett meddelande med hjälp av en Mobiltjänst [komma igång med Mobile Services], och sedan:</span><span class="sxs-lookup"><span data-stu-id="74c61-177">toosend a notification by using a mobile service, follow [Get started with Mobile Services], and then:</span></span>

1. <span data-ttu-id="74c61-178">Logga in toohello [klassiska Azure-portalen], och välj mobiltjänst.</span><span class="sxs-lookup"><span data-stu-id="74c61-178">Sign in toohello [Azure Classic Portal], and select your mobile service.</span></span>
2. <span data-ttu-id="74c61-179">Välj hello **Scheduler** fliken hello längst upp.</span><span class="sxs-lookup"><span data-stu-id="74c61-179">Select hello **Scheduler** tab on hello top.</span></span>
   
       ![Azure Classic Portal - Scheduler][215]
3. <span data-ttu-id="74c61-180">Skapa ett nytt schemalagt jobb, infoga ett namn och välj **På begäran**.</span><span class="sxs-lookup"><span data-stu-id="74c61-180">Create a new scheduled job, insert a name, and select **On demand**.</span></span>
   
       ![Azure Classic Portal - Create new job][216]
4. <span data-ttu-id="74c61-181">Klicka på hello Jobbnamnet när hello jobb skapas.</span><span class="sxs-lookup"><span data-stu-id="74c61-181">When hello job is created, click hello job name.</span></span> <span data-ttu-id="74c61-182">Klicka på hello **skriptet** fliken på hello översta raden.</span><span class="sxs-lookup"><span data-stu-id="74c61-182">Then click hello **Script** tab on hello top bar.</span></span>
5. <span data-ttu-id="74c61-183">Infoga följande skript i schemaläggarfunktionen hello.</span><span class="sxs-lookup"><span data-stu-id="74c61-183">Insert hello following script inside your scheduler function.</span></span> <span data-ttu-id="74c61-184">Se till att tooreplace hello-platshållare med notification hub namn och hello anslutningssträngen för *DefaultFullSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="74c61-184">Make sure tooreplace hello placeholders with your notification hub name and hello connection string for *DefaultFullSharedAccessSignature* that you obtained earlier.</span></span> <span data-ttu-id="74c61-185">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="74c61-185">Click **Save**.</span></span>
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<Hubname>', '<SAS Full access >');
        notificationHubService.apns.send(
            null,
            {"aps":
                {
                  "alert": "Hello from Mobile Services!"
                }
            },
            function (error)
            {
                if (!error) {
                    console.warn("Notification successful");
                }
            }
        );
6. <span data-ttu-id="74c61-186">Klicka på **kör en gång** på hello nedre fältet.</span><span class="sxs-lookup"><span data-stu-id="74c61-186">Click **Run Once** on hello bottom bar.</span></span> <span data-ttu-id="74c61-187">Du bör få en avisering på enheten.</span><span class="sxs-lookup"><span data-stu-id="74c61-187">You should receive an alert on your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="74c61-188">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="74c61-188">Next steps</span></span>
<span data-ttu-id="74c61-189">I det här enkla exemplet skickade du push-meddelanden tooall iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="74c61-189">In this simple example, you broadcasted push notifications tooall your iOS devices.</span></span> <span data-ttu-id="74c61-190">I ordning tootarget specifika användare finns i självstudiekursen toohello [använda Notification Hubs toopush meddelanden toousers].</span><span class="sxs-lookup"><span data-stu-id="74c61-190">In order tootarget specific users, refer toohello tutorial [Use Notification Hubs toopush notifications toousers].</span></span> <span data-ttu-id="74c61-191">Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="74c61-191">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="74c61-192">Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs] och i hello [Meddelandehubbar hur-toofor iOS].</span><span class="sxs-lookup"><span data-stu-id="74c61-192">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance] and in hello [Notification Hubs How-toofor iOS].</span></span>

<!-- Images. -->

[213]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-console-app.png

[215]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler1.png
[216]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-scheduler2.png


[31]: ./media/partner-xamarin-notification-hubs-ios-get-started/notification-hub-create-ios-app.png




<!-- URLs. -->
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[komma igång med Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-ios
[klassiska Azure-portalen]: https://manage.windowsazure.com/
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Meddelandehubbar hur-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[använda Notification Hubs toopush meddelanden toousers]: /manage/services/notification-hubs/notify-users-aspnet
[använda Notification Hubs toosend senaste nytt]: /manage/services/notification-hubs/breaking-news-dotnet

[Push Notification Programmeringsguide för lokala och]:https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/HandlingRemoteNotifications.html#//apple_ref/doc/uid/TP40008194-CH6-SW1
[Apple Push Notification Service]: http://go.microsoft.com/fwlink/p/?LinkId=272584

[Azure Mobile Services Component]: http://components.xamarin.com/view/azure-mobile-services/
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Xamarin Studio]: http://xamarin.com/download
[WindowsAzure.Messaging]: https://github.com/infosupport/WindowsAzure.Messaging.iOS
[Azure Portal]: https://portal.azure.com
