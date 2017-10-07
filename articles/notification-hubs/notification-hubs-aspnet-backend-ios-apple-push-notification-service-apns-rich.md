---
title: aaaAzure Notification Hubs Rich Push
description: "Lär dig hur toosend omfattande push-meddelanden tooan iOS-app från Azure. Kodexempel som skrivits i Objective-C och C#."
documentationcenter: ios
services: notification-hubs
author: ysxu
manager: erikre
editor: 
ms.assetid: 590304df-c0a4-46c5-8ef5-6a6486bb3340
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 5432d8bf47777371bea3521a0c0176ade75fbd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-rich-push"></a>Azure Notification Hubs omfattande Push
## <a name="overview"></a>Översikt
Ett program kanske vill toopush utöver oformaterad text i ordning tooengage användare med snabbmeddelanden omfattande innehållet. Dessa aviseringar främja användarinteraktioner och finns innehåll, till exempel URL: er, ljud, bilder/kuponger och mycket mer. Den här kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) avsnittet, och visar hur toosend push-meddelanden med nyttolaster (till exempel bild).

Den här kursen är kompatibel med iOS 7 och 8.

  ![][IOS1]

På en hög nivå:

1. Hej appserverdelen:
   * Lagrar hello omfattande nyttolasten (i det här fallet bild) i hello backend-databas/lokal lagring
   * Skickar ID för den här omfattande meddelande toohello enhet
2. Appen på hello enhet:
   * Kontakter hello backend begär hello omfattande nyttolast med hello-ID som den tar emot
   * Skickar meddelanden för användarna på hello enhet när datahämtning är klar och visar hello nyttolast omedelbart när användarna trycker på toolearn mer

## <a name="webapi-project"></a>WebAPI-projekt
1. Öppna i Visual Studio hello **AppBackend** projekt som du skapade i hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.
2. Hämta en avbildning som du vill toonotify användare med och placera den i en **img** mappen i projektkatalogen.
3. Klicka på **visa alla filer** hello i Solution Explorer och högerklicka på hello mappen för**inkluderar i projektet**.
4. Med hello bilden är markerad ändra dess Skapa-åtgärd i fönstret Egenskaper för**inbäddad resurs**.
   
    ![][IOS2]
5. I **Notifications.cs**, lägga till hello följande med instruktionen:
   
        using System.Reflection;
6. Uppdatera hello hela **meddelanden** klassen med följande kod hello. Vara säker på att tooreplace hello-platshållare med dina autentiseringsuppgifter för notification hub och bildfilens namn.
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message toodisplay toousers
            public string Message { get; set; }
            // Type of rich payload (developer-defined)
            public string RichType { get; set; }
            public string Payload { get; set; }
            public bool Read { get; set; }
        }
   
        public class Notifications {
            public static Notifications Instance = new Notifications();
   
            private List<Notification> notifications = new List<Notification>();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                // Placeholders: replace with hello connection string (with full access) for your notification hub and hello hub name from hello Azure Classics Portal
                Hub = NotificationHubClient.CreateClientFromConnectionString("{conn string with full access}",  "{hub name}");
            }
   
            public Notification CreateNotification(string message, string richType, string payload) {
                var notification = new Notification() {
                    Id = notifications.Count,
                    Message = message,
                    RichType = richType,
                    Payload = payload,
                    Read = false
                };
   
                notifications.Add(notification);
   
                return notification;
            }
   
            public Stream ReadImage(int id) {
                var assembly = Assembly.GetExecutingAssembly();
                // Placeholder: image file name (for example, logo.png).
                return assembly.GetManifestResourceStream("AppBackend.img.{logo.png}");
            }
        }
   
   > [!NOTE]
   > (valfritt) Se för[hur tooembed och komma åt resurser med hjälp av Visual C#](http://support.microsoft.com/kb/319292) för mer information om hur tooadd och hämta projektresurser.
   > 
   > 
7. I **NotificationsController.cs**, omdefiniera **NotificationsController** med hello följande kodavsnitt. Detta skickar ett inledande tyst omfattande meddelande-id toodevice och tillåter klientsidan hämtning av avbildningen:
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) tooclient
        public async Task<HttpResponseMessage> Post() {
            // Replace hello placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification tooapns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. Nu kommer vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter. Högerklicka på hello **AppBackend** projektet och välj **publicera**.
9. Välj Azure-webbplatsen som publicera-mål. Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna hello **Måladress** egenskap i hello **anslutning** fliken. Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen. Klicka på **Publicera**.

## <a name="modify-hello-ios-project"></a>Ändra hello iOS-projekt
Nu när du har ändrat din app backend toosend bara hello *id* av ett meddelande du och ändrar din iOS-app toohandle detta id och hämta omfattande hello-meddelande från din serverdel.

1. Öppna projektet iOS och aktivera fjärråtkomst-meddelanden genom att gå tooyour huvudsakliga app mål i hello **mål** avsnitt.
2. Klicka på **funktioner**, aktivera **bakgrundslägen**, och kontrollera hello **Remote Notifications** kryssrutan.
   
    ![][IOS3]
3. Gå för**Main.storyboard**, och kontrollera att du har en View-Controller (hänvisade tooas Start View Controller i den här självstudiekursen) från [meddela användaren](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.
4. Lägg till en **navigering Controller** tooyour storyboard och CTRL-dra tooHome View Controller toomake den hello **rot visa** av navigeringen. Se till att hello **är inledande View Controller** i attribut inspector har valts för hello navigering domänkontrollant.
5. Lägg till en **View Controller** toostoryboard och lägga till en **bild visa**. Det här är hello sida som användarna ser när de väljer toolearn mer genom att klicka på hello notifiication. Storyboard bör se ut så här:
   
    ![][IOS4]
6. Klicka på hello **Start View Controller** storyboard och kontrollera att den har **homeViewController** som dess **anpassad klass** och **Storyboard ID**under hello identitet inspector.
7. Hello samma för avbildningen View Controller som **imageViewController**.
8. Skapa sedan en ny View Controller klass som heter **imageViewController** toohandle hello användargränssnitt som du nyss skapade.
9. I **imageViewController.h**, lägga till hello följande toohello domänkontrollant gränssnittet deklarationer. Se till att toocontrol-dra från hello storyboard avbildningen visa toothese egenskaper toolink hello två:
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. I **imageViewController.m**, Lägg till följande hello hello slutet av **viewDidload**:
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. I **AppDelegate.m**, importera hello avbildningen domänkontrollant som du skapade:
    
        #import "imageViewController.h"
12. Lägga till ett gränssnitt avsnitt med hello följande deklaration:
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. I **AppDelegate**, kontrollera att din app registrerar för tyst meddelanden i **program: didFinishLaunchingWithOptions**:
    
        // Software version
        self.iOS8 = [[UIApplication sharedApplication] respondsToSelector:@selector(registerUserNotificationSettings:)] && [[UIApplication sharedApplication] respondsToSelector:@selector(registerForRemoteNotifications)];
    
        // Register for remote notifications for iOS8 and previous versions
        if (self.iOS8) {
            NSLog(@"This device is running with iOS8.");
    
            // Action
            UIMutableUserNotificationAction *richPushAction = [[UIMutableUserNotificationAction alloc] init];
            richPushAction.identifier = @"richPushMore";
            richPushAction.activationMode = UIUserNotificationActivationModeForeground;
            richPushAction.authenticationRequired = NO;
            richPushAction.title = @"More";
    
            // Notification category
            UIMutableUserNotificationCategory* richPushCategory = [[UIMutableUserNotificationCategory alloc] init];
            richPushCategory.identifier = @"richPush";
            [richPushCategory setActions:@[richPushAction] forContext:UIUserNotificationActionContextDefault];
    
            // Notification categories
            NSSet* richPushCategories = [NSSet setWithObjects:richPushCategory, nil];

            UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                    UIUserNotificationTypeAlert |
                                                    UIUserNotificationTypeBadge
                                                                                     categories:richPushCategories];

            [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
            [[UIApplication sharedApplication] registerForRemoteNotifications];

        }
        else {
            // Previous iOS versions
            NSLog(@"This device is running with iOS7 or earlier versions.");

            [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
        }

        return YES;

1. Ersätt i hello efter implementering för **program: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard Användargränssnittet ändras i beräkningen:
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. Lägg sedan till följande metoder för hello**AppDelegate.m** tooretrieve hello-avbildning från din slutpunkt och skicka ett lokala meddelande när hämtningen är klar. Se till att toosubstitute hello platshållare `{backend endpoint}` med backend-slutpunkten:
   
       NSString *const GetNotificationEndpoint = @"{backend endpoint}/api/notifications";
   
       // Helper: retrieve notification content from backend with rich notification id
       - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion {
           UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
           homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
           NSString* authenticationHeader = hvc.registerClient.authenticationHeader;
           // Check if authenticated
           if (!authenticationHeader) return;
   
           NSURLSession* session = [NSURLSession
                                    sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                    delegate:nil
                                    delegateQueue:nil];
   
           NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, richId]];
           NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
           [request setHTTPMethod:@"GET"];
           NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
           [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
   
           NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
   
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
               if (!error && httpResponse.statusCode == 200) {
                   // From NSData tooUIImage
                   self.imagePayload = [UIImage imageWithData:data];
   
                   completion(nil);
               }
               else {
                   NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                   if (error)
                       completion(error);
                   else {
                       completion([NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                   }
               }
           }];
           [dataTask resume];
       }
   
       // Handle silent push notifications when id is sent from backend
       - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler {
           self.userInfo = userInfo;
           int richId = [[self.userInfo objectForKey:@"richId"] intValue];
           NSString* richType = [self.userInfo objectForKey:@"richType"];
   
           // Retrieve image data
           if ([richType isEqualToString:@"img"]) {  
               [self retrieveRichImageWithId:richId completion:^(NSError* error) {
                   if (!error){
                       // Send local notification
                       UILocalNotification* localNotification = [[UILocalNotification alloc] init];
   
                       // "5" is arbitrary here toogive you enough time tooquit out of hello app and receive push notifications
                       localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:5];
                       localNotification.userInfo = self.userInfo;
                       localNotification.alertBody = [self.userInfo objectForKey:@"richMessage"];
                       localNotification.timeZone = [NSTimeZone defaultTimeZone];
   
                       // iOS8 categories
                       if (self.iOS8) {
                           localNotification.category = @"richPush";
                       }
   
                       [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                       handler(UIBackgroundFetchResultNewData);
                   }
                   else{
                       handler(UIBackgroundFetchResultFailed);
                   }
               }];
           }
           // Add "else if" here toohandle more types of rich content such as url, sound files, etc.
       }
3. Hantera lokala meddelanden om hello senare genom att öppna hello avbildningen view controller i **AppDelegate.m** med hello följande metoder:
   
       // Helper: redirect users tooimage view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image tooimage view controller
           imgViewController.imagePayload = img;
   
           // Redirect
           [navigationController pushViewController:imgViewController animated:YES];
       }
   
       // Handle local notification sent above in didReceiveRemoteNotification
       - (void)application:(UIApplication *)application didReceiveLocalNotification:(UILocalNotification *)notification {
           if (application.applicationState == UIApplicationStateActive) {
               // Show in-app alert with an extra "more" button
               UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:notification.alertBody delegate:self cancelButtonTitle:@"OK" otherButtonTitles:@"More", nil];
   
               [alert show];
           }
           // App becomes active from user's tap on notification
           else {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
       }
   
       // Handle buttons in in-app alerts and redirect with data/image
       - (void)alertView:(UIAlertView *)alertView clickedButtonAtIndex:(NSInteger)buttonIndex {
           // Handle "more" button
           if (buttonIndex == 1)
           {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           // Add "else if" here toohandle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-hello-application"></a>Kör hello program
1. Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).
2. Ange ett användarnamn och lösenord för hello samma värde för autentisering och klicka på i hello iOS-app UI, **loggar In**.
3. Klicka på **skicka push** och du bör se en avisering i appen. Om du klickar på **mer**, kommer du att åter toohello bilden som du har valt tooinclude i din Apps serverdel.
4. Du kan också klicka på **skicka push** och omedelbart på hello hem av enheten. Om en stund får du ett push-meddelande. Om du knackar på den eller klicka på mer kommer du innehållet i tooyour appen och hello omfattande operativsystemsavbildningen.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
