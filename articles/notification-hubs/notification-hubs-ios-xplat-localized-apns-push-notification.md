---
title: "aaaNotification lokaliserade bryta nyheter kurs om Händelsehubbar för iOS"
description: "Lär dig hur toouse Azure Service Bus Notification Hubs toosend lokaliserade senaste nyheterna meddelanden (iOS)."
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
ms.openlocfilehash: 9fe88c0440e93b72d349574160ddcd85a7ba0be0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a><span data-ttu-id="29eac-103">Använda Notification Hubs toosend lokaliserade senaste nyheterna tooiOS enheter</span><span class="sxs-lookup"><span data-stu-id="29eac-103">Use Notification Hubs toosend localized breaking news tooiOS devices</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29eac-104">Windows Store C#</span><span class="sxs-lookup"><span data-stu-id="29eac-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="29eac-105">iOS</span><span class="sxs-lookup"><span data-stu-id="29eac-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="29eac-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="29eac-106">Overview</span></span>
<span data-ttu-id="29eac-107">Det här avsnittet beskrivs hur du toouse hello [mallar](notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs toobroadcast bryter nyheter meddelanden som har översatts av språk och enheten.</span><span class="sxs-lookup"><span data-stu-id="29eac-107">This topic shows you how toouse hello [templates](notification-hubs-templates-cross-platform-push-messages.md) feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="29eac-108">I den här kursen börjar du med hello iOS-app som skapats i [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="29eac-108">In this tutorial you start with hello iOS app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="29eac-109">När du är klar, kommer du att kunna tooregister kategorier som du är intresserad av att ange ett språk i vilka tooreceive hello meddelanden och ta emot endast push-meddelanden för hello valda kategorier på det språket.</span><span class="sxs-lookup"><span data-stu-id="29eac-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="29eac-110">Det finns två delar toothis scenariot:</span><span class="sxs-lookup"><span data-stu-id="29eac-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="29eac-111">iOS-app kan klienten enheter toospecify ett språk och toosubscribe toodifferent bryter nyhetskategorier.</span><span class="sxs-lookup"><span data-stu-id="29eac-111">iOS app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="29eac-112">hello backend-sänder hello-meddelanden med hjälp av hello **taggen** och **mallen** feautres i Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="29eac-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29eac-113">Krav</span><span class="sxs-lookup"><span data-stu-id="29eac-113">Prerequisites</span></span>
<span data-ttu-id="29eac-114">Du måste redan har slutfört hello [använda Notification Hubs toosend senaste nytt] självstudier och ha hello-kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.</span><span class="sxs-lookup"><span data-stu-id="29eac-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="29eac-115">Visual Studio 2012 eller senare är valfritt.</span><span class="sxs-lookup"><span data-stu-id="29eac-115">Visual Studio 2012 or later is optional.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="29eac-116">Mall-begrepp</span><span class="sxs-lookup"><span data-stu-id="29eac-116">Template concepts</span></span>
<span data-ttu-id="29eac-117">I [använda Notification Hubs toosend senaste nytt] du skapat en app som används för **taggar** toosubscribe toonotifications för olika nyhetskategorier.</span><span class="sxs-lookup"><span data-stu-id="29eac-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="29eac-118">Många appar dock mål flera marknader och kräver lokalisering.</span><span class="sxs-lookup"><span data-stu-id="29eac-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="29eac-119">Detta innebär att hello hello meddelanden själva innehållet har toobe lokaliserade och levererat toohello korrigera uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="29eac-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="29eac-120">I det här avsnittet visas hur toouse hello **mallen** funktion i Notification Hubs tooeasily leverera lokaliserade senaste nyheterna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="29eac-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="29eac-121">Obs: enkelriktade toosend lokaliserade meddelanden är toocreate flera versioner av varje tagg.</span><span class="sxs-lookup"><span data-stu-id="29eac-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="29eac-122">Till exempel toosupport engelska, franska och Mandarin kan vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”.</span><span class="sxs-lookup"><span data-stu-id="29eac-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="29eac-123">Vi har sedan toosend en lokaliserad version av hello world news tooeach taggar.</span><span class="sxs-lookup"><span data-stu-id="29eac-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="29eac-124">I det här avsnittet använder vi mallar tooavoid hello spridning av taggar och hello krav flera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="29eac-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="29eac-125">På en hög nivå mallar är ett sätt toospecify hur en specifik enhet bör få ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="29eac-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="29eac-126">hello mallen anger hello exakt nyttolastformatet genom att referera tooproperties som ingår i hello-meddelande som skickas av din appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="29eac-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="29eac-127">I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="29eac-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="29eac-128">Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar rätt toohello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="29eac-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="29eac-129">Till exempel registrerar en iOS-app som vill tooregister för franska nyheter hello följande:</span><span class="sxs-lookup"><span data-stu-id="29eac-129">For instance,  an iOS app that wants tooregister for French news will register hello following:</span></span>

    {
        aps:{
            alert: "$(News_French)"
        }
    }

<span data-ttu-id="29eac-130">Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="29eac-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span>

## <a name="hello-app-user-interface"></a><span data-ttu-id="29eac-131">användargränssnittet för hello app</span><span class="sxs-lookup"><span data-stu-id="29eac-131">hello app user interface</span></span>
<span data-ttu-id="29eac-132">Vi kommer nu att ändra hello bryta nyheter app som du skapade i avsnittet hello [använda Notification Hubs toosend senaste nytt] toosend lokaliserade senaste nyheterna med hjälp av mallar.</span><span class="sxs-lookup"><span data-stu-id="29eac-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="29eac-133">Lägga till en segmenterade kontroll med hello tre språk som vi stöder din MainStoryboard_iPhone.storyboard: engelska, franska och Mandarin.</span><span class="sxs-lookup"><span data-stu-id="29eac-133">In your MainStoryboard_iPhone.storyboard, add a Segmented Control with hello three languages which we will support: English, French, and Mandarin.</span></span>

![][13]

<span data-ttu-id="29eac-134">Gör att tooadd en IBOutlet i din ViewController.h enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="29eac-134">Then make sure tooadd an IBOutlet in your ViewController.h as shown below:</span></span>

![][14]

## <a name="building-hello-ios-app"></a><span data-ttu-id="29eac-135">Skapa hello iOS-app</span><span class="sxs-lookup"><span data-stu-id="29eac-135">Building hello iOS app</span></span>
1. <span data-ttu-id="29eac-136">Lägg till hello i din Notification.h *retrieveLocale* -metoden och ändra hello store och prenumerera metoder som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="29eac-136">In your Notification.h add hello *retrieveLocale* method, and modify hello store and subscribe methods as shown below:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    <span data-ttu-id="29eac-137">Ändra hello i din Notification.m *storeCategoriesAndSubscribe* metoden genom att lägga till hello språkvariantparameter och lagra den i hello användarens standardinställningar:</span><span class="sxs-lookup"><span data-stu-id="29eac-137">In your Notification.m, modify hello *storeCategoriesAndSubscribe* method, by adding hello locale parameter and storing it in hello user defaults:</span></span>
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    <span data-ttu-id="29eac-138">Ändra hello *prenumerera* metoden tooinclude hello språk:</span><span class="sxs-lookup"><span data-stu-id="29eac-138">Then modify hello *subscribe* method tooinclude hello locale:</span></span>
   
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
   
    <span data-ttu-id="29eac-139">Observera hur vi använder hello metoden *registerTemplateWithDeviceToken*, i stället för *registerNativeWithDeviceToken*.</span><span class="sxs-lookup"><span data-stu-id="29eac-139">Note how we are now using hello method *registerTemplateWithDeviceToken*, instead of *registerNativeWithDeviceToken*.</span></span> <span data-ttu-id="29eac-140">När vi registrerar för en mall har vi tooprovide hello json-mall och ett namn för mallen hello (som vår app kanske vill tooregister olika mallar).</span><span class="sxs-lookup"><span data-stu-id="29eac-140">When we register for a template we have tooprovide hello json template and also a name for hello template (as our app might want tooregister different templates).</span></span> <span data-ttu-id="29eac-141">Se till att tooregister kategorier som taggar, som vi vill toomake att tooreceive hello notifciations för dessa nyheter.</span><span class="sxs-lookup"><span data-stu-id="29eac-141">Make sure tooregister your categories as tags, as we want toomake sure tooreceive hello notifciations for those news.</span></span>
   
    <span data-ttu-id="29eac-142">Lägg till en metod tooretrieve hello språkinställning från hello standardinställningar för användare:</span><span class="sxs-lookup"><span data-stu-id="29eac-142">Add a method tooretrieve hello locale from hello user default settings:</span></span>
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. <span data-ttu-id="29eac-143">Nu när vi ändrade vår meddelanden klass har vi till att vår ViewController gör toomake användning av hello nya UISegmentControl.</span><span class="sxs-lookup"><span data-stu-id="29eac-143">Now that we modified our Notifications class, we have toomake sure that our ViewController makes use of hello new UISegmentControl.</span></span> <span data-ttu-id="29eac-144">Lägg till följande rad i hello hello *viewDidLoad* metoden toomake att tooshow hello språk som är markerade:</span><span class="sxs-lookup"><span data-stu-id="29eac-144">Add hello following line in hello *viewDidLoad* method toomake sure tooshow hello locale that is currently selected:</span></span>
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    <span data-ttu-id="29eac-145">I din *prenumerera* metod, ändra anrop-toohello *storeCategoriesAndSubscribe* toohello följande:</span><span class="sxs-lookup"><span data-stu-id="29eac-145">Then, in your *subscribe* method, change your call toohello *storeCategoriesAndSubscribe* toohello following:</span></span>
   
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
3. <span data-ttu-id="29eac-146">Slutligen har du tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* metod i din AppDelegate.m så att registreringen kan uppdateras korrekt när appen startar.</span><span class="sxs-lookup"><span data-stu-id="29eac-146">Finally, you have tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* method in your AppDelegate.m, so that you can correctly refresh your registration when your app starts.</span></span> <span data-ttu-id="29eac-147">Ändra din anropet toohello *prenumerera* metod för meddelanden med hello följande:</span><span class="sxs-lookup"><span data-stu-id="29eac-147">Change your call toohello *subscribe* method of notifications with hello following:</span></span>
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a><span data-ttu-id="29eac-148">(valfritt) Skicka meddelanden om lokaliserade mallar från .NET-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="29eac-148">(optional) Send localized template notifications from .NET console app.</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a><span data-ttu-id="29eac-149">(valfritt) Skicka meddelanden om lokaliserade mallar från hello enhet</span><span class="sxs-lookup"><span data-stu-id="29eac-149">(optional) Send localized template notifications from hello device</span></span>
<span data-ttu-id="29eac-150">Om du inte har åtkomst tooVisual Studio eller vill toojust testa att skicka meddelanden om hello lokaliserade mallar direkt från hello appen på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="29eac-150">If you don't have access tooVisual Studio, or want toojust test sending hello localized template notifications directly from hello app on hello device.</span></span>  <span data-ttu-id="29eac-151">Du kan enkelt lägga till hello lokaliserade mallen parametrar toohello `SendNotificationRESTAPI` metod som du definierade i hello tidigare kursen.</span><span class="sxs-lookup"><span data-stu-id="29eac-151">You can simple add hello localized template parameters toohello `SendNotificationRESTAPI` method you defined in hello previous tutorial.</span></span>

        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];

            NSString *json;

            // Construct hello messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];

            // Generated hello token toobe used in hello authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];

            //Create hello request tooadd hello template notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];

            // Add hello category as a tag
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

            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];

            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];

            // Send hello REST request
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




## <a name="next-steps"></a><span data-ttu-id="29eac-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29eac-152">Next Steps</span></span>
<span data-ttu-id="29eac-153">Mer information om hur du använder mallar finns:</span><span class="sxs-lookup"><span data-stu-id="29eac-153">For more information on using templates, see:</span></span>

* <span data-ttu-id="29eac-154">[Meddela användare med Notification Hubs: ASP.NET]</span><span class="sxs-lookup"><span data-stu-id="29eac-154">[Notify users with Notification Hubs: ASP.NET]</span></span>
* <span data-ttu-id="29eac-155">[Meddela användare med Notification Hubs: Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="29eac-155">[Notify users with Notification Hubs: Mobile Services]</span></span>

<!-- Images. -->

[13]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized1.png
[14]: ./media/notification-hubs-ios-send-localized-breaking-news/ios_localized2.png






<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[använda Notification Hubs toosend senaste nytt]: /manage/services/notification-hubs/breaking-news-ios
[Mobile Service]: /develop/mobile/tutorials/get-started
[Meddela användare med Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Meddela användare med Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-ios
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-ios
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-ios
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-users-ios
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-ios
[JavaScript and HTML]: ../get-started-with-push-js.md

[Windows Developer Preview registration steps for Mobile Services]: ../mobile-services-windows-developer-preview-registration.md
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
