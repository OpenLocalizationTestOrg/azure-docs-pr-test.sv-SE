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
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="064c7-104">Azure Notification Hubs omfattande Push</span><span class="sxs-lookup"><span data-stu-id="064c7-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="064c7-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="064c7-105">Overview</span></span>
<span data-ttu-id="064c7-106">Ett program kanske vill toopush utöver oformaterad text i ordning tooengage användare med snabbmeddelanden omfattande innehållet.</span><span class="sxs-lookup"><span data-stu-id="064c7-106">In order tooengage users with instant rich contents, an application might want toopush beyond plain text.</span></span> <span data-ttu-id="064c7-107">Dessa aviseringar främja användarinteraktioner och finns innehåll, till exempel URL: er, ljud, bilder/kuponger och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="064c7-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="064c7-108">Den här kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) avsnittet, och visar hur toosend push-meddelanden med nyttolaster (till exempel bild).</span><span class="sxs-lookup"><span data-stu-id="064c7-108">This tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how toosend push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="064c7-109">Den här kursen är kompatibel med iOS 7 och 8.</span><span class="sxs-lookup"><span data-stu-id="064c7-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="064c7-110">På en hög nivå:</span><span class="sxs-lookup"><span data-stu-id="064c7-110">At a high level:</span></span>

1. <span data-ttu-id="064c7-111">Hej appserverdelen:</span><span class="sxs-lookup"><span data-stu-id="064c7-111">hello app backend:</span></span>
   * <span data-ttu-id="064c7-112">Lagrar hello omfattande nyttolasten (i det här fallet bild) i hello backend-databas/lokal lagring</span><span class="sxs-lookup"><span data-stu-id="064c7-112">Stores hello rich payload (in this case, image) in hello backend database/local storage</span></span>
   * <span data-ttu-id="064c7-113">Skickar ID för den här omfattande meddelande toohello enhet</span><span class="sxs-lookup"><span data-stu-id="064c7-113">Sends ID of this rich notification toohello device</span></span>
2. <span data-ttu-id="064c7-114">Appen på hello enhet:</span><span class="sxs-lookup"><span data-stu-id="064c7-114">App on hello device:</span></span>
   * <span data-ttu-id="064c7-115">Kontakter hello backend begär hello omfattande nyttolast med hello-ID som den tar emot</span><span class="sxs-lookup"><span data-stu-id="064c7-115">Contacts hello backend requesting hello rich payload with hello ID it receives</span></span>
   * <span data-ttu-id="064c7-116">Skickar meddelanden för användarna på hello enhet när datahämtning är klar och visar hello nyttolast omedelbart när användarna trycker på toolearn mer</span><span class="sxs-lookup"><span data-stu-id="064c7-116">Sends users notifications on hello device when data retrieval is complete, and shows hello payload immediately when users tap toolearn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="064c7-117">WebAPI-projekt</span><span class="sxs-lookup"><span data-stu-id="064c7-117">WebAPI Project</span></span>
1. <span data-ttu-id="064c7-118">Öppna i Visual Studio hello **AppBackend** projekt som du skapade i hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="064c7-118">In Visual Studio, open hello **AppBackend** project that you created in hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="064c7-119">Hämta en avbildning som du vill toonotify användare med och placera den i en **img** mappen i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="064c7-119">Obtain an image you would like toonotify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="064c7-120">Klicka på **visa alla filer** hello i Solution Explorer och högerklicka på hello mappen för**inkluderar i projektet**.</span><span class="sxs-lookup"><span data-stu-id="064c7-120">Click **Show All Files** in hello Solution Explorer, and right-click hello folder too**Include In Project**.</span></span>
4. <span data-ttu-id="064c7-121">Med hello bilden är markerad ändra dess Skapa-åtgärd i fönstret Egenskaper för**inbäddad resurs**.</span><span class="sxs-lookup"><span data-stu-id="064c7-121">With hello image selected, change its Build Action in Properties window too**Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="064c7-122">I **Notifications.cs**, lägga till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="064c7-122">In **Notifications.cs**, add hello following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="064c7-123">Uppdatera hello hela **meddelanden** klassen med följande kod hello.</span><span class="sxs-lookup"><span data-stu-id="064c7-123">Update hello whole **Notifications** class with hello following code.</span></span> <span data-ttu-id="064c7-124">Vara säker på att tooreplace hello-platshållare med dina autentiseringsuppgifter för notification hub och bildfilens namn.</span><span class="sxs-lookup"><span data-stu-id="064c7-124">Be sure tooreplace hello placeholders with your notification hub credentials and image file name.</span></span>
   
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
   > <span data-ttu-id="064c7-125">(valfritt) Se för[hur tooembed och komma åt resurser med hjälp av Visual C#](http://support.microsoft.com/kb/319292) för mer information om hur tooadd och hämta projektresurser.</span><span class="sxs-lookup"><span data-stu-id="064c7-125">(optional) Refer too[How tooembed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how tooadd and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="064c7-126">I **NotificationsController.cs**, omdefiniera **NotificationsController** med hello följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="064c7-126">In **NotificationsController.cs**, redefine **NotificationsController**  with hello following snippets.</span></span> <span data-ttu-id="064c7-127">Detta skickar ett inledande tyst omfattande meddelande-id toodevice och tillåter klientsidan hämtning av avbildningen:</span><span class="sxs-lookup"><span data-stu-id="064c7-127">This sends an initial silent rich notification id toodevice and allows client-side retrieval of image:</span></span>
   
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
8. <span data-ttu-id="064c7-128">Nu kommer vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter.</span><span class="sxs-lookup"><span data-stu-id="064c7-128">Now we will re-deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="064c7-129">Högerklicka på hello **AppBackend** projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="064c7-129">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="064c7-130">Välj Azure-webbplatsen som publicera-mål.</span><span class="sxs-lookup"><span data-stu-id="064c7-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="064c7-131">Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna hello **Måladress** egenskap i hello **anslutning** fliken. Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="064c7-131">Log in with your Azure account and select an existing or new Website, and make a note of hello **destination URL** property in hello **Connection** tab. We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="064c7-132">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="064c7-132">Click **Publish**.</span></span>

## <a name="modify-hello-ios-project"></a><span data-ttu-id="064c7-133">Ändra hello iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="064c7-133">Modify hello iOS project</span></span>
<span data-ttu-id="064c7-134">Nu när du har ändrat din app backend toosend bara hello *id* av ett meddelande du och ändrar din iOS-app toohandle detta id och hämta omfattande hello-meddelande från din serverdel.</span><span class="sxs-lookup"><span data-stu-id="064c7-134">Now that you have modified your app backend toosend just hello *id* of a notification, you will change your iOS app toohandle that id and retrieve hello rich message from your backend.</span></span>

1. <span data-ttu-id="064c7-135">Öppna projektet iOS och aktivera fjärråtkomst-meddelanden genom att gå tooyour huvudsakliga app mål i hello **mål** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="064c7-135">Open your iOS project, and enable remote notifications by going tooyour main app target in hello **Targets** section.</span></span>
2. <span data-ttu-id="064c7-136">Klicka på **funktioner**, aktivera **bakgrundslägen**, och kontrollera hello **Remote Notifications** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="064c7-136">Click on **Capabilities**, turn on **Background Modes**, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="064c7-137">Gå för**Main.storyboard**, och kontrollera att du har en View-Controller (hänvisade tooas Start View Controller i den här självstudiekursen) från [meddela användaren](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="064c7-137">Go too**Main.storyboard**, and make sure you have a View Controller (refered tooas Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="064c7-138">Lägg till en **navigering Controller** tooyour storyboard och CTRL-dra tooHome View Controller toomake den hello **rot visa** av navigeringen.</span><span class="sxs-lookup"><span data-stu-id="064c7-138">Add a **Navigation Controller** tooyour storyboard, and control-drag tooHome View Controller toomake it hello **root view** of navigation.</span></span> <span data-ttu-id="064c7-139">Se till att hello **är inledande View Controller** i attribut inspector har valts för hello navigering domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="064c7-139">Make sure hello **Is Initial View Controller** in Attributes inspector is selected for hello Navigation Controller only.</span></span>
5. <span data-ttu-id="064c7-140">Lägg till en **View Controller** toostoryboard och lägga till en **bild visa**.</span><span class="sxs-lookup"><span data-stu-id="064c7-140">Add a **View Controller** toostoryboard and add an **Image View**.</span></span> <span data-ttu-id="064c7-141">Det här är hello sida som användarna ser när de väljer toolearn mer genom att klicka på hello notifiication.</span><span class="sxs-lookup"><span data-stu-id="064c7-141">This is hello page users will see once they choose toolearn more by clicking on hello notifiication.</span></span> <span data-ttu-id="064c7-142">Storyboard bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="064c7-142">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="064c7-143">Klicka på hello **Start View Controller** storyboard och kontrollera att den har **homeViewController** som dess **anpassad klass** och **Storyboard ID**under hello identitet inspector.</span><span class="sxs-lookup"><span data-stu-id="064c7-143">Click on hello **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under hello Identity inspector.</span></span>
7. <span data-ttu-id="064c7-144">Hello samma för avbildningen View Controller som **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="064c7-144">Do hello same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="064c7-145">Skapa sedan en ny View Controller klass som heter **imageViewController** toohandle hello användargränssnitt som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="064c7-145">Then, create a new View Controller class titled **imageViewController** toohandle hello UI you just created.</span></span>
9. <span data-ttu-id="064c7-146">I **imageViewController.h**, lägga till hello följande toohello domänkontrollant gränssnittet deklarationer.</span><span class="sxs-lookup"><span data-stu-id="064c7-146">In **imageViewController.h**, add hello following toohello controller's interface declarations.</span></span> <span data-ttu-id="064c7-147">Se till att toocontrol-dra från hello storyboard avbildningen visa toothese egenskaper toolink hello två:</span><span class="sxs-lookup"><span data-stu-id="064c7-147">Make sure toocontrol-drag from hello storyboard image view toothese properties toolink hello two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="064c7-148">I **imageViewController.m**, Lägg till följande hello hello slutet av **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="064c7-148">In **imageViewController.m**, add hello following at hello end of **viewDidload**:</span></span>
    
        // Display hello UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="064c7-149">I **AppDelegate.m**, importera hello avbildningen domänkontrollant som du skapade:</span><span class="sxs-lookup"><span data-stu-id="064c7-149">In **AppDelegate.m**, import hello image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="064c7-150">Lägga till ett gränssnitt avsnitt med hello följande deklaration:</span><span class="sxs-lookup"><span data-stu-id="064c7-150">Add an interface section with hello following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect tooImage View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="064c7-151">I **AppDelegate**, kontrollera att din app registrerar för tyst meddelanden i **program: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="064c7-151">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
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

1. <span data-ttu-id="064c7-152">Ersätt i hello efter implementering för **program: didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard Användargränssnittet ändras i beräkningen:</span><span class="sxs-lookup"><span data-stu-id="064c7-152">Subsitute in hello following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** tootake hello storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at hello root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="064c7-153">Lägg sedan till följande metoder för hello**AppDelegate.m** tooretrieve hello-avbildning från din slutpunkt och skicka ett lokala meddelande när hämtningen är klar.</span><span class="sxs-lookup"><span data-stu-id="064c7-153">Then, add hello following methods too**AppDelegate.m** tooretrieve hello image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="064c7-154">Se till att toosubstitute hello platshållare `{backend endpoint}` med backend-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="064c7-154">Make sure toosubstitute hello placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
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
3. <span data-ttu-id="064c7-155">Hantera lokala meddelanden om hello senare genom att öppna hello avbildningen view controller i **AppDelegate.m** med hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="064c7-155">Handle hello local notification above by opening up hello image view controller in **AppDelegate.m** with hello following methods:</span></span>
   
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

## <a name="run-hello-application"></a><span data-ttu-id="064c7-156">Kör hello program</span><span class="sxs-lookup"><span data-stu-id="064c7-156">Run hello Application</span></span>
1. <span data-ttu-id="064c7-157">Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).</span><span class="sxs-lookup"><span data-stu-id="064c7-157">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="064c7-158">Ange ett användarnamn och lösenord för hello samma värde för autentisering och klicka på i hello iOS-app UI, **loggar In**.</span><span class="sxs-lookup"><span data-stu-id="064c7-158">In hello iOS app UI, enter a username and password of hello same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="064c7-159">Klicka på **skicka push** och du bör se en avisering i appen.</span><span class="sxs-lookup"><span data-stu-id="064c7-159">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="064c7-160">Om du klickar på **mer**, kommer du att åter toohello bilden som du har valt tooinclude i din Apps serverdel.</span><span class="sxs-lookup"><span data-stu-id="064c7-160">If you click on **More**, you will be brought toohello image you chose tooinclude in your app backend.</span></span>
4. <span data-ttu-id="064c7-161">Du kan också klicka på **skicka push** och omedelbart på hello hem av enheten.</span><span class="sxs-lookup"><span data-stu-id="064c7-161">You can also click **Send push** and immediately press hello home button of your device.</span></span> <span data-ttu-id="064c7-162">Om en stund får du ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="064c7-162">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="064c7-163">Om du knackar på den eller klicka på mer kommer du innehållet i tooyour appen och hello omfattande operativsystemsavbildningen.</span><span class="sxs-lookup"><span data-stu-id="064c7-163">If you tap on it or click More, you will be brought tooyour app and hello rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
