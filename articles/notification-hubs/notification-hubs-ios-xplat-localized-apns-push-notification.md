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
# <a name="use-notification-hubs-toosend-localized-breaking-news-tooios-devices"></a>Använda Notification Hubs toosend lokaliserade senaste nyheterna tooiOS enheter
> [!div class="op_single_selector"]
> * [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse hello [mallar](notification-hubs-templates-cross-platform-push-messages.md) funktion i Azure Notification Hubs toobroadcast bryter nyheter meddelanden som har översatts av språk och enheten. I den här kursen börjar du med hello iOS-app som skapats i [använda Notification Hubs toosend senaste nytt]. När du är klar, kommer du att kunna tooregister kategorier som du är intresserad av att ange ett språk i vilka tooreceive hello meddelanden och ta emot endast push-meddelanden för hello valda kategorier på det språket.

Det finns två delar toothis scenariot:

* iOS-app kan klienten enheter toospecify ett språk och toosubscribe toodifferent bryter nyhetskategorier.
* hello backend-sänder hello-meddelanden med hjälp av hello **taggen** och **mallen** feautres i Azure Notification Hubs.

## <a name="prerequisites"></a>Krav
Du måste redan har slutfört hello [använda Notification Hubs toosend senaste nytt] självstudier och ha hello-kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.

Visual Studio 2012 eller senare är valfritt.

## <a name="template-concepts"></a>Mall-begrepp
I [använda Notification Hubs toosend senaste nytt] du skapat en app som används för **taggar** toosubscribe toonotifications för olika nyhetskategorier.
Många appar dock mål flera marknader och kräver lokalisering. Detta innebär att hello hello meddelanden själva innehållet har toobe lokaliserade och levererat toohello korrigera uppsättning enheter.
I det här avsnittet visas hur toouse hello **mallen** funktion i Notification Hubs tooeasily leverera lokaliserade senaste nyheterna meddelanden.

Obs: enkelriktade toosend lokaliserade meddelanden är toocreate flera versioner av varje tagg. Till exempel toosupport engelska, franska och Mandarin kan vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”. Vi har sedan toosend en lokaliserad version av hello world news tooeach taggar. I det här avsnittet använder vi mallar tooavoid hello spridning av taggar och hello krav flera meddelanden.

På en hög nivå mallar är ett sätt toospecify hur en specifik enhet bör få ett meddelande. hello mallen anger hello exakt nyttolastformatet genom att referera tooproperties som ingår i hello-meddelande som skickas av din appens serverdel. I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar rätt toohello-egenskapen. Till exempel registrerar en iOS-app som vill tooregister för franska nyheter hello följande:

    {
        aps:{
            alert: "$(News_French)"
        }
    }

Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel.

## <a name="hello-app-user-interface"></a>användargränssnittet för hello app
Vi kommer nu att ändra hello bryta nyheter app som du skapade i avsnittet hello [använda Notification Hubs toosend senaste nytt] toosend lokaliserade senaste nyheterna med hjälp av mallar.

Lägga till en segmenterade kontroll med hello tre språk som vi stöder din MainStoryboard_iPhone.storyboard: engelska, franska och Mandarin.

![][13]

Gör att tooadd en IBOutlet i din ViewController.h enligt nedan:

![][14]

## <a name="building-hello-ios-app"></a>Skapa hello iOS-app
1. Lägg till hello i din Notification.h *retrieveLocale* -metoden och ändra hello store och prenumerera metoder som visas nedan:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet*) categories completion: (void (^)(NSError* error))completion;
   
        - (void) subscribeWithLocale:(int) locale categories:(NSSet*) categories completion:(void (^)(NSError *))completion;
   
        - (NSSet*) retrieveCategories;
   
        - (int) retrieveLocale;
   
    Ändra hello i din Notification.m *storeCategoriesAndSubscribe* metoden genom att lägga till hello språkvariantparameter och lagra den i hello användarens standardinställningar:
   
        - (void) storeCategoriesAndSubscribeWithLocale:(int) locale categories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
            [defaults setInteger:locale forKey:@"BreakingNewsLocale"];
            [defaults synchronize];
   
            [self subscribeWithLocale: locale categories:categories completion:completion];
        }
   
    Ändra hello *prenumerera* metoden tooinclude hello språk:
   
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
   
    Observera hur vi använder hello metoden *registerTemplateWithDeviceToken*, i stället för *registerNativeWithDeviceToken*. När vi registrerar för en mall har vi tooprovide hello json-mall och ett namn för mallen hello (som vår app kanske vill tooregister olika mallar). Se till att tooregister kategorier som taggar, som vi vill toomake att tooreceive hello notifciations för dessa nyheter.
   
    Lägg till en metod tooretrieve hello språkinställning från hello standardinställningar för användare:
   
        - (int) retrieveLocale {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
   
            int locale = [defaults integerForKey:@"BreakingNewsLocale"];
   
            return locale < 0?0:locale;
        }
2. Nu när vi ändrade vår meddelanden klass har vi till att vår ViewController gör toomake användning av hello nya UISegmentControl. Lägg till följande rad i hello hello *viewDidLoad* metoden toomake att tooshow hello språk som är markerade:
   
        self.Locale.selectedSegmentIndex = [notifications retrieveLocale];
   
    I din *prenumerera* metod, ändra anrop-toohello *storeCategoriesAndSubscribe* toohello följande:
   
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
3. Slutligen har du tooupdate hello *didRegisterForRemoteNotificationsWithDeviceToken* metod i din AppDelegate.m så att registreringen kan uppdateras korrekt när appen startar. Ändra din anropet toohello *prenumerera* metod för meddelanden med hello följande:
   
        NSSet* categories = [self.notifications retrieveCategories];
        int locale = [self.notifications retrieveLocale];
        [self.notifications subscribeWithLocale: locale categories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

## <a name="optional-send-localized-template-notifications-from-net-console-app"></a>(valfritt) Skicka meddelanden om lokaliserade mallar från .NET-konsolapp.
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

## <a name="optional-send-localized-template-notifications-from-hello-device"></a>(valfritt) Skicka meddelanden om lokaliserade mallar från hello enhet
Om du inte har åtkomst tooVisual Studio eller vill toojust testa att skicka meddelanden om hello lokaliserade mallar direkt från hello appen på hello enhet.  Du kan enkelt lägga till hello lokaliserade mallen parametrar toohello `SendNotificationRESTAPI` metod som du definierade i hello tidigare kursen.

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




## <a name="next-steps"></a>Nästa steg
Mer information om hur du använder mallar finns:

* [Meddela användare med Notification Hubs: ASP.NET]
* [Meddela användare med Notification Hubs: Mobile Services]

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
