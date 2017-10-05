---
title: "Notification Hubs lokaliserade bryta nyheter kursen för iOS"
description: "Lär dig hur du använder Azure Service Bus Notification Hubs för att skicka lokaliserade senaste nyheterna meddelanden (iOS)."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 484914b5-e081-4a05-a84a-798bbd89d428
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: fd2b7d9dfd4f432bbcbaa3ed76f8bec0b9677e17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news-to-ios-devices"></a><span data-ttu-id="6b3fb-103">Använda Notification Hubs för att skicka lokaliserade senaste nyheterna till iOS-enheter</span><span class="sxs-lookup"><span data-stu-id="6b3fb-103">Use Notification Hubs to send localized breaking news to iOS devices</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6b3fb-104">Windows Store C#</span><span class="sxs-lookup"><span data-stu-id="6b3fb-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="6b3fb-105">iOS</span><span class="sxs-lookup"><span data-stu-id="6b3fb-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="6b3fb-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="6b3fb-106">Overview</span></span>
<span data-ttu-id="6b3fb-107">Det här avsnittet visar hur du använder den [mallar](notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs för att sända senaste nyheterna meddelanden som har översatts av språk och enheten.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-107">This topic shows you how to use the [templates](notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs to broadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="6b3fb-108">I den här kursen börjar du med iOS-app som skapats i [använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="6b3fb-108">In this tutorial you start with the iOS app created in [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="6b3fb-109">När du är klar kommer du att kunna registrera för kategorier som du vill, ange ett språk som ska ta emot meddelanden och får endast push-meddelanden i valda kategorier på det språket.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-109">When complete, you will be able to register for categories you are interested in, specify a language in which to receive the notifications, and receive only push notifications for the selected categories in that language.</span></span>

<span data-ttu-id="6b3fb-110">Det finns två delar i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-110">There are two parts to this scenario:</span></span>

* <span data-ttu-id="6b3fb-111">iOS-app kan klienten enheter för att ange ett språk och prenumerera på olika senaste nyheterna kategorier.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-111">iOS app allows client devices to specify a language, and to subscribe to different breaking news categories;</span></span>
* <span data-ttu-id="6b3fb-112">backend-skickar meddelanden med hjälp av den **taggen** och **mallen** feautres i Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-112">the back-end broadcasts the notifications, using the **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b3fb-113">Krav</span><span class="sxs-lookup"><span data-stu-id="6b3fb-113">Prerequisites</span></span>
<span data-ttu-id="6b3fb-114">Du måste redan har slutfört den [använda Notification Hubs för att skicka de senaste nyheterna] självstudier och har kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-114">You must have already completed the [Use Notification Hubs to send breaking news] tutorial and have the code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="6b3fb-115">Visual Studio 2012 eller senare är valfritt.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-115">Visual Studio 2012 or later is optional.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="6b3fb-116">Mall-begrepp</span><span class="sxs-lookup"><span data-stu-id="6b3fb-116">Template concepts</span></span>
<span data-ttu-id="6b3fb-117">I [använda Notification Hubs för att skicka de senaste nyheterna] du skapat en app som används för **taggar** att prenumerera på meddelanden om nyheterna i olika kategorier.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-117">In [Use Notification Hubs to send breaking news] you built an app that used **tags** to subscribe to notifications for different news categories.</span></span>
<span data-ttu-id="6b3fb-118">Många appar dock mål flera marknader och kräver lokalisering.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="6b3fb-119">Detta innebär att innehållet i meddelandena själva måste lokaliserade och levereras till rätt uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-119">This means that the content of the notifications themselves have to be localized and delivered to the correct set of devices.</span></span>
<span data-ttu-id="6b3fb-120">I det här avsnittet visar vi hur du använder den **mallen** funktion i Notification Hubs för att leverera enkelt lokaliserade senaste nyheterna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-120">In this topic we will show how to use the **template** feature of Notification Hubs to easily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="6b3fb-121">Obs: ett sätt att skicka lokaliserade meddelanden är att skapa flera versioner av varje tagg.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-121">Note: one way to send localized notifications is to create multiple versions of each tag.</span></span> <span data-ttu-id="6b3fb-122">Till exempel för att stödja engelska, franska och Mandarin, vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-122">For instance, to support English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="6b3fb-123">Vi har sedan skicka en lokaliserad version av world nyheter till var och en av dessa taggar.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-123">We would then have to send a localized version of the world news to each of these tags.</span></span> <span data-ttu-id="6b3fb-124">Vi använder mallar för att undvika alltför många taggar och kravet flera meddelanden i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-124">In this topic we use templates to avoid the proliferation of tags and the requirement of sending multiple messages.</span></span>

<span data-ttu-id="6b3fb-125">Mallar är ett sätt att ange hur en specifik enhet bör få ett meddelande på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-125">At a high level, templates are a way to specify how a specific device should receive a notification.</span></span> <span data-ttu-id="6b3fb-126">Mallen anger exakt nyttolastformatet genom att referera till egenskaper som ingår i meddelandet som skickas av din appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-126">The template specifies the exact payload format by referring to properties that are part of the message sent by your app back-end.</span></span> <span data-ttu-id="6b3fb-127">I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="6b3fb-128">Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar till egenskapen korrekt.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-128">Then we will ensure that devices register with a template that refers to the correct property.</span></span> <span data-ttu-id="6b3fb-129">Exempelvis kan registrera en iOS-app som du vill registrera för franska nyheter följande:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-129">For instance,  an iOS app that wants to register for French news will register the following:</span></span>

    {
        aps:{
            alert: "$(News_French)"
        }
    }

<span data-ttu-id="6b3fb-130">Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span>

## <a name="the-app-user-interface"></a><span data-ttu-id="6b3fb-131">Användargränssnittet för app</span><span class="sxs-lookup"><span data-stu-id="6b3fb-131">The app user interface</span></span>
<span data-ttu-id="6b3fb-132">Vi kommer nu att ändra bryta nyheter appen som du skapade i avsnittet [använda Notification Hubs för att skicka de senaste nyheterna] att skicka lokaliserade senaste nytt med hjälp av mallar.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-132">We will now modify the Breaking News app that you created in the topic [Use Notification Hubs to send breaking news] to send localized breaking news using templates.</span></span>

<span data-ttu-id="6b3fb-133">Lägga till en segmenterade kontroll med de tre språken som vi stöder din MainStoryboard_iPhone.storyboard: engelska, franska och Mandarin.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-133">In your MainStoryboard_iPhone.storyboard, add a Segmented Control with the three languages which we will support: English, French, and Mandarin.</span></span>

![][13]

<span data-ttu-id="6b3fb-134">Kontrollera sedan att lägga till en IBOutlet i din ViewController.h enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-134">Then make sure to add an IBOutlet in your ViewController.h as shown below:</span></span>

![][14]

## <a name="building-the-ios-app"></a><span data-ttu-id="6b3fb-135">Skapa iOS-app</span><span class="sxs-lookup"><span data-stu-id="6b3fb-135">Building the iOS app</span></span>
1. <span data-ttu-id="6b3fb-136">Lägg till i din Notification.h den *retrieveLocale* -metoden och ändra arkivet och prenumerera metoder som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-136">In your Notification.h add the *retrieveLocale* method, and modify the store and subscribe methods as shown below:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    <span data-ttu-id="6b3fb-137">Ändra i din Notification.m den *storeCategoriesAndSubscribe* metoden genom att lägga till parametern språk och lagrar den i användarens standardinställningar:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-137">In your Notification.m, modify the *storeCategoriesAndSubscribe* method, by adding the locale parameter and storing it in the user defaults:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    <span data-ttu-id="6b3fb-138">Ändrar den *prenumerera* metod för att inkludera språkinställningen:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-138">Then modify the *subscribe* method to include the locale:</span></span>
   
        - (void) subscribeWithLocale: (int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion{
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:@"<connection string>" notificationHubPath:@"<hub name>"];
   
            NSString* localeString;
            switch (locale) {
                case 0:
                    localeString = @"English";
                    break;
                case 1:
                    localeString = @"French";
                    break;
                case 2:
                    localeString = @"Mandarin";
                    break;
            }
   
            NSString* template = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"$(News_%@)\"},\"inAppMessage\":\"$(News_%@)\"}", localeString, localeString];
   
            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"localizednewsTemplate" jsonBodyTemplate:template expiryTemplate:@"0" tags:categories completion:completion];
        }
   
    <span data-ttu-id="6b3fb-139">Observera hur vi använder metoden *registerTemplateWithDeviceToken*, i stället för *registerNativeWithDeviceToken*.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-139">Note how we are now using the method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*.</span></span> <span data-ttu-id="6b3fb-140">När vi registrerar för en mall behöver vi json-mall och ett namn för mallen (som vår app kanske vill registrera olika mallar).</span><span class="sxs-lookup"><span data-stu-id="6b3fb-140">When we register for a template we have to provide the json template and also a name for the template (as our app might want to register different templates).</span></span> <span data-ttu-id="6b3fb-141">Se till att registrera din kategorier som taggar, som vi vill se till att ta emot notifciations för dessa nyheter.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-141">Make sure to register your categories as tags, as we want to make sure to receive the notifciations for those news.</span></span>
   
    <span data-ttu-id="6b3fb-142">Lägg till en metod för att hämta språkinställningen från standardinställningar för användare:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-142">Add a method to retrieve the locale from the user default settings:</span></span>
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. <span data-ttu-id="6b3fb-143">Nu när vi ändrade vår meddelanden klass har vi se till att vår ViewController gör användning av den nya UISegmentControl.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-143">Now that we modified our Notifications class, we have to make sure that our ViewController makes use of the new UISegmentControl.</span></span> <span data-ttu-id="6b3fb-144">Lägg till följande rad i den *viewDidLoad* metod för att se till att visa det språk som är markerade:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-144">Add the following line in the *viewDidLoad* method to make sure to show the locale that is currently selected:</span></span>
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    <span data-ttu-id="6b3fb-145">I din *prenumerera* metod, ändra anrop till den *storeCategoriesAndSubscribe* till följande:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-145">Then, in your *subscribe* method, change your call to the *storeCategoriesAndSubscribe* to the following:</span></span>
   
        [notifications storeCategoriesAndSubscribeWithLocale: self.Locale.selectedSegmentIndex categories:[NSSet setWithArray:categories] completion: ^(NSError* error) {
            if (!error) {
                UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:
                                      @"Subscribed!" delegate:nil cancelButtonTitle:
                                      @"OK" otherButtonTitles:nil, nil];
                [alert show];
            } else {
                NSLog(@"Error subscribing: %@", error);
            }
        }];
3. <span data-ttu-id="6b3fb-146">Slutligen kan du behöva uppdatera den *didRegisterForRemoteNotificationsWithDeviceToken* metod i din AppDelegate.m så att registreringen kan uppdateras korrekt när appen startar.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-146">Finally, you have to update the *didRegisterForRemoteNotificationsWithDeviceToken* method in your AppDelegate.m, so that you can correctly refresh your registration when your app starts.</span></span> <span data-ttu-id="6b3fb-147">Ändra anrop till den *prenumerera* metod för meddelanden med följande:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-147">Change your call to the *subscribe* method of notifications with the following:</span></span>
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a><span data-ttu-id="6b3fb-148">(valfritt) Skicka meddelanden om lokaliserade mallar från .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-148">(optional) Send localized template notifications from .NET console app.</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-the-device"></a><span data-ttu-id="6b3fb-149">(valfritt) Skicka meddelanden om lokaliserade mallar från enheten</span><span class="sxs-lookup"><span data-stu-id="6b3fb-149">(optional) Send localized template notifications from the device</span></span>
<span data-ttu-id="6b3fb-150">Om du inte har åtkomst till Visual Studio eller bara testa skicka lokaliserade mall-meddelanden direkt från appen på enheten.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-150">If you don't have access to Visual Studio, or want to just test sending the localized template notifications directly from the app on the device.</span></span>  <span data-ttu-id="6b3fb-151">Du kan enkelt lägga till lokaliserade mallens parametrar för att den `SendNotificationRESTAPI` metod som du definierade i föregående kursen.</span><span class="sxs-lookup"><span data-stu-id="6b3fb-151">You can simple add the localized template parameters to the `SendNotificationRESTAPI` method you defined in the previous tutorial.</span></span>

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];

            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\","
                    \"News_English\":\"Breaking %@ News in English : %@\","
                    \"News_French\":\"Breaking %@ News in French : %@\","
                    \"News_Mandarin\":\"Breaking %@ News in Mandarin : %@\","
                    categoryTag, self.notificationMessage.text,
                    categoryTag, self.notificationMessage.text,  // insert English localized news here
                    categoryTag, self.notificationMessage.text,  // insert French localized news here
                    categoryTag, self.notificationMessage.text]; // insert Mandarin localized news here

            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];

            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];

            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];

            [dataTask resume];
        }




## <a name="next-steps"></a><span data-ttu-id="6b3fb-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b3fb-152">Next Steps</span></span>
<span data-ttu-id="6b3fb-153">Mer information om hur du använder mallar finns:</span><span class="sxs-lookup"><span data-stu-id="6b3fb-153">For more information on using templates, see:</span></span>

* <span data-ttu-id="6b3fb-154">[Meddela användare med Notification Hubs: ASP.NET]</span><span class="sxs-lookup"><span data-stu-id="6b3fb-154">[Notify users with Notification Hubs: ASP.NET]</span></span>
* <span data-ttu-id="6b3fb-155">[Meddela användare med Notification Hubs: Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="6b3fb-155">[Notify users with Notification Hubs: Mobile Services]</span></span>

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
<span data-ttu-id="6b3fb-156">[använda Notification Hubs för att skicka de senaste nyheterna]: /manage/services/notification-hubs/breaking-news-ios</span><span class="sxs-lookup"><span data-stu-id="6b3fb-156">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-ios</span></span>
[Mobile Service]: /develop/mobile/tutorials/get-started
<span data-ttu-id="6b3fb-157">[Meddela användare med Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="6b3fb-157">[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet</span></span>
<span data-ttu-id="6b3fb-158">[Meddela användare med Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users</span><span class="sxs-lookup"><span data-stu-id="6b3fb-158">[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users</span></span>
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
