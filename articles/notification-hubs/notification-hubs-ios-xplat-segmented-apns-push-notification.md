---
title: "aaaNotification bryter nyheter kurs om Händelsehubbar - iOS"
description: "Lär dig hur toouse Azure Service Bus Notification Hubs toosend bryter nyheter meddelanden tooiOS enheter."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 763b80b5ffed238b351d95bd3d6a96cb914f53cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a>Använda Notification Hubs toosend senaste nytt
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooan iOS-app. När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier. Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.

Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben. När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande. Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg. Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Krav
Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started]. Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Lägg till kategori markeringen toohello app
hello första steget är tooadd hello UI-element tooyour befintliga storyboard som möjliggör hello användaren tooselect kategorier tooregister. hello kategorier som är markerad som en användare som lagras på hello enhet. När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.

1. Lägg till följande komponenter från objektbiblioteket för hello hello i din MainStoryboard_iPhone.storyboard:
   
   * En etikett med ”bryter nyheter” text
   * Etiketter med kategori text ”World”, ”politik”, ”företag”, ”teknik”, ”vetenskap”, ”sport”
   * Sex växlar en per kategori och ställa in varje switch **tillstånd** toobe **av** som standard.
   * En knapp med etiketten ”prenumerera”
     
     Storyboard bör se ut så här:
     
     ![][3]
2. I Redigeraren för hello assistenten skapa uttag för alla hello-växlar och anropa dem ”WorldSwitch”, ”PoliticsSwitch”, ”BusinessSwitch”, ”TechnologySwitch”, ”ScienceSwitch”, ”SportsSwitch”
3. Skapa en åtgärd för en knapp som kallas ”prenumerera”. Din ViewController.h bör innehålla hello följande:
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. Skapa en ny **Cocoa Touch klassen** kallas `Notifications`. Kopiera följande kod i hello-gränssnittet i hello filen Notifications.h för hello:
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. Lägg till hello efter import direktiv tooNotifications.m:
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. Kopiera följande kod i hello implementering i hello filen Notifications.m för hello.
   
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    Den här klassen använder lokal lagring toostore och hämta hello kategorier av nyheter som den här enheten ska ta emot. Den innehåller också en metod tooregister för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registrering.

1. Lägga till en import-sats för Notifications.h i hello AppDelegate.h-filen och Lägg till en egenskap för en instans av hello meddelanden klass:
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. I hello **didFinishLaunchingWithOptions** metod i AppDelegate.m, lägga till hello kod tooinitialize hello meddelanden instans hello början av hello-metoden.  
   
    `HUBNAME`och `HUBLISTENACCESS` (definieras i hubinfo.h) har redan hello `<hub name>` och `<connection string with listen access>` platshållare ersätts med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature*som du fick tidigare
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp. Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden. hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.
   > 
   > 
3. I hello **didRegisterForRemoteNotificationsWithDeviceToken** metod i AppDelegate.m, Ersätt hello koden i hello-metoden med följande kod toopass hello token toohello meddelanden enhetsklass hello. hello meddelanden klassen utför hello registrering för meddelanden med hello kategorier. Om hello användaren ändrar kategori, vi kallar hello `subscribeWithCategories` metod i svaret toohello **prenumerera** knappen tooupdate dem.
   
   > [!NOTE]
   > Eftersom hello enhetstoken som tilldelats av hello Apple Push-aviseringstjänsten (APNS) kan risken när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel. Det här exemplet registrerar för meddelande varje gång hello appen startas. För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves hello categories from local storage and requests a registration for these categories
        // each time hello app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    Observera att då det ska vara någon annan kod i hello **didRegisterForRemoteNotificationsWithDeviceToken** metod.

1. hello följande metoder ska redan finnas i AppDelegate.m från att slutföras hello [Kom igång med Notification Hubs] [ get-started] kursen.  Om du inte lägga till dem.
   
    -(void) MessageBox:(NSString *) rubrik meddelandet:(NSString *) messageText {
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    }
   
   * (void) programmet:(UIApplication *) program didReceiveRemoteNotification: (NSDictionary *) användarinformationen {NSLog (@ ”% @”, användarinformationen);   [self MessageBox:@"Notification” meddelande: [valueForKey:@"alert [användarinformationen objectForKey:@"aps] ””]]; }
   
   Den här metoden hanterar meddelanden när hello app körs genom att visa en enkel **UIAlert**.
2. Lägga till en import-sats för AppDelegate.h och kopiera hello följande kod i hello ViewController.m, XCode-genererade **prenumerera** metod. Den här koden kommer att uppdatera hello notification registrering toouse hello nya kategori etiketter hello användaren har valt i hello-användargränssnittet.
   
       ```
       #import "Notifications.h"
       ```
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
   
   Den här metoden skapar en **NSMutableArray** av kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrerar hello motsvarande taggar med meddelandehubben. När kategorier ändras, återskapas hello registrering med hello nya kategorier.
3. Lägg till följande kod i hello hello i ViewController.m, **viewDidLoad** användargränssnittet för metoden tooset hello baserat på hello sparat tidigare kategorier.

        // This updates hello UI on startup based on hello status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



hello app kan nu lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister med hello notification hub när hello appen startar.  hello-användare kan ändra hello valet av kategorier under körning och klicka på hello **prenumerera** metoden tooupdate hello registrering för hello enhet. Därefter uppdaterar du hello app toosend hello bryta nyheter meddelanden direkt i själva hello appen.

## <a name="optional-sending-tagged-notifications"></a>(valfritt) Skicka taggade meddelanden
Om du inte har åtkomst tooVisual Studio, kan du hoppa över toohello nästa avsnitt och skicka meddelanden från själva hello appen. Du kan också skicka hello rätt mall meddelande från hello [klassiska Azure-portalen] med hello felsökningsfliken för meddelandehubben. 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-hello-device"></a>(valfritt) Skicka meddelanden från hello-enhet
Meddelanden skickas normalt med en serverdelstjänst men du kan skicka senaste nyheterna meddelanden direkt från hello app. toodo detta vi kommer att uppdatera hello `SendNotificationRESTAPI` metod som vi har definierat i hello [Kom igång med Notification Hubs] [ get-started] kursen.

1. I ViewController.m uppdatering hello `SendNotificationRESTAPI` metod som följer så att den accepterar en parameter för hello category med och skickar hello rätt [mallen](notification-hubs-templates-cross-platform-push-messages.md) meddelande.
   
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
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
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
2. I ViewController.m uppdatering hello **skicka meddelande** åtgärd som visas i hello-kod som följer. Så att den ska skicka hello-meddelanden med varje tagg individuellt och skicka toomultiple plattformar.

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send hello message as breaking news for each category tooWNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. Återskapa projektet och kontrollera att du har inga build-fel.

## <a name="run-hello-app-and-generate-notifications"></a>Kör hello app och generera meddelanden
1. Tryck på hello kör knappen toobuild hello projektet och starta hello app. Välj vissa senaste nyheterna alternativ toosubscribe tooand och tryck sedan på hello **prenumerera** knappen. Du bör se en dialogruta som visar hello meddelanden prenumererar på.
   
    ![][1]
   
    När du väljer **prenumerera**hello appkategorierna konverterar hello som valts i taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben.
2. Ange ett meddelande toobe skickas som de senaste nyheterna och tryck sedan på hello **skicka meddelande** knappen. Du kan också köra hello .NET konsolen app toogenerate meddelanden.
   
    ![][2]
3. Varje enhet som prenumererar på toobreaking nyheter visas hello senaste nyheterna meddelanden har skickat.

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori. Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:

* **[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]**
  
    Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[klassiska Azure-portalen]: https://manage.windowsazure.com
