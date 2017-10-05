---
title: Azure Notification Hubs omfattande Push
description: "Lär dig hur du skickar omfattande push-meddelanden till en iOS-app från Azure. Kodexempel som skrivits i Objective-C och C#."
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
ms.openlocfilehash: 394efdc2dfaff0666bc23d8a448b0a00d414da99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-rich-push"></a><span data-ttu-id="0ab86-104">Azure Notification Hubs omfattande Push</span><span class="sxs-lookup"><span data-stu-id="0ab86-104">Azure Notification Hubs Rich Push</span></span>
## <a name="overview"></a><span data-ttu-id="0ab86-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0ab86-105">Overview</span></span>
<span data-ttu-id="0ab86-106">För att kunna interagera med snabbmeddelanden omfattande innehållet användarna kanske ett program push utöver oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="0ab86-106">In order to engage users with instant rich contents, an application might want to push beyond plain text.</span></span> <span data-ttu-id="0ab86-107">Dessa aviseringar främja användarinteraktioner och finns innehåll, till exempel URL: er, ljud, bilder/kuponger och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="0ab86-107">These notifications promote user interactions and  present content such as urls, sounds, images/coupons, and more.</span></span> <span data-ttu-id="0ab86-108">Den här kursen bygger på den [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) avsnittet, och visar hur du skickar push-meddelanden med nyttolaster (till exempel bild).</span><span class="sxs-lookup"><span data-stu-id="0ab86-108">This tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) topic, and shows how to send push notifications that incorporate payloads (for example, image).</span></span>

<span data-ttu-id="0ab86-109">Den här kursen är kompatibel med iOS 7 och 8.</span><span class="sxs-lookup"><span data-stu-id="0ab86-109">This tutorial is compatible with iOS 7 & 8.</span></span>

  ![][IOS1]

<span data-ttu-id="0ab86-110">På en hög nivå:</span><span class="sxs-lookup"><span data-stu-id="0ab86-110">At a high level:</span></span>

1. <span data-ttu-id="0ab86-111">Appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="0ab86-111">The app backend:</span></span>
   * <span data-ttu-id="0ab86-112">Lagrar omfattande nyttolasten (i det här fallet bild) i databasen/lokal serverdelslagring</span><span class="sxs-lookup"><span data-stu-id="0ab86-112">Stores the rich payload (in this case, image) in the backend database/local storage</span></span>
   * <span data-ttu-id="0ab86-113">Skickar ID för omfattande meddelandet till enheten</span><span class="sxs-lookup"><span data-stu-id="0ab86-113">Sends ID of this rich notification to the device</span></span>
2. <span data-ttu-id="0ab86-114">Appen på enheten:</span><span class="sxs-lookup"><span data-stu-id="0ab86-114">App on the device:</span></span>
   * <span data-ttu-id="0ab86-115">Kontaktar serverdelen som begär den omfattande nyttolasten med det ID som den tar emot</span><span class="sxs-lookup"><span data-stu-id="0ab86-115">Contacts the backend requesting the rich payload with the ID it receives</span></span>
   * <span data-ttu-id="0ab86-116">Skickar meddelanden för användare på enheten när datahämtning är klar och visar nyttolasten omedelbart när användarna trycker på Läs mer</span><span class="sxs-lookup"><span data-stu-id="0ab86-116">Sends users notifications on the device when data retrieval is complete, and shows the payload immediately when users tap to learn more</span></span>

## <a name="webapi-project"></a><span data-ttu-id="0ab86-117">WebAPI-projekt</span><span class="sxs-lookup"><span data-stu-id="0ab86-117">WebAPI Project</span></span>
1. <span data-ttu-id="0ab86-118">Öppna i Visual Studio den **AppBackend** projekt som du skapade i den [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-118">In Visual Studio, open the **AppBackend** project that you created in the [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
2. <span data-ttu-id="0ab86-119">Hämta en avbildning som du vill meddela användare med och placera den i en **img** mappen i projektkatalogen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-119">Obtain an image you would like to notify users with, and put it in an **img** folder in your project directory.</span></span>
3. <span data-ttu-id="0ab86-120">Klicka på **visa alla filer** i Solution Explorer och högerklicka på mappen till **inkluderar i projektet**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-120">Click **Show All Files** in the Solution Explorer, and right-click the folder to **Include In Project**.</span></span>
4. <span data-ttu-id="0ab86-121">Ändra bilden är markerad i fönstret Egenskaper för att skapa åtgärden **inbäddad resurs**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-121">With the image selected, change its Build Action in Properties window to **Embedded Resource**.</span></span>
   
    ![][IOS2]
5. <span data-ttu-id="0ab86-122">I **Notifications.cs**, Lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="0ab86-122">In **Notifications.cs**, add the following using statement:</span></span>
   
        using System.Reflection;
6. <span data-ttu-id="0ab86-123">Uppdatera hela **meddelanden** klassen med följande kod.</span><span class="sxs-lookup"><span data-stu-id="0ab86-123">Update the whole **Notifications** class with the following code.</span></span> <span data-ttu-id="0ab86-124">Se till att ersätta platshållarna med dina autentiseringsuppgifter för notification hub och bildfilens namn.</span><span class="sxs-lookup"><span data-stu-id="0ab86-124">Be sure to replace the placeholders with your notification hub credentials and image file name.</span></span>
   
        public class Notification {
            public int Id { get; set; }
            // Initial notification message to display to users
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
                // Placeholders: replace with the connection string (with full access) for your notification hub and the hub name from the Azure Classics Portal
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
   > <span data-ttu-id="0ab86-125">(valfritt) Referera till [så att bädda in och komma åt resurser med hjälp av Visual C#](http://support.microsoft.com/kb/319292) för mer information om hur du lägger till och hämta projektresurser.</span><span class="sxs-lookup"><span data-stu-id="0ab86-125">(optional) Refer to [How to embed and access resources by using Visual C#](http://support.microsoft.com/kb/319292) for more information on how to add and obtain project resources.</span></span>
   > 
   > 
7. <span data-ttu-id="0ab86-126">I **NotificationsController.cs**, omdefiniera **NotificationsController** med följande kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="0ab86-126">In **NotificationsController.cs**, redefine **NotificationsController**  with the following snippets.</span></span> <span data-ttu-id="0ab86-127">Detta skickar ett inledande tyst omfattande meddelande-id till enheten och kan klientsidan hämtning av avbildningen:</span><span class="sxs-lookup"><span data-stu-id="0ab86-127">This sends an initial silent rich notification id to device and allows client-side retrieval of image:</span></span>
   
        // Return http response with image binary
        public HttpResponseMessage Get(int id) {
            var stream = Notifications.Instance.ReadImage(id);
   
            var result = new HttpResponseMessage(HttpStatusCode.OK);
            result.Content = new StreamContent(stream);
            // Switch in your image extension for "png"
            result.Content.Headers.ContentType = new System.Net.Http.Headers.MediaTypeHeaderValue("image/{png}");
   
            return result;
        }
   
        // Create rich notification and send initial silent notification (containing id) to client
        public async Task<HttpResponseMessage> Post() {
            // Replace the placeholder with image file name
            var richNotificationInTheBackend = Notifications.Instance.CreateNotification("Check this image out!", "img",  "{logo.png}");
   
            var usernameTag = "username:" + HttpContext.Current.User.Identity.Name;
   
            // Silent notification with content available
            var aboutUser = "{\"aps\": {\"content-available\": 1, \"sound\":\"\"}, \"richId\": \"" + richNotificationInTheBackend.Id.ToString() + "\",  \"richMessage\": \"" + richNotificationInTheBackend.Message + "\", \"richType\": \"" + richNotificationInTheBackend.RichType + "\"}";
   
            // Send notification to apns
            await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(aboutUser, usernameTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
8. <span data-ttu-id="0ab86-128">Vi ska nu omdistribuera den här appen till en Azure-webbplats för att göra den tillgänglig från alla enheter.</span><span class="sxs-lookup"><span data-stu-id="0ab86-128">Now we will re-deploy this app to an Azure Website in order to make it accessible from all devices.</span></span> <span data-ttu-id="0ab86-129">Högerklicka på **AppBackend**-projektet och välj **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-129">Right-click on the **AppBackend** project and select **Publish**.</span></span>
9. <span data-ttu-id="0ab86-130">Välj Azure-webbplatsen som publicera-mål.</span><span class="sxs-lookup"><span data-stu-id="0ab86-130">Select Azure Website as your publish target.</span></span> <span data-ttu-id="0ab86-131">Logga in med ditt Azure-konto och markera en befintlig eller ny webbplats och anteckna den **Måladress** egenskap i den **anslutning** fliken.</span><span class="sxs-lookup"><span data-stu-id="0ab86-131">Log in with your Azure account and select an existing or new Website, and make a note of the **destination URL** property in the **Connection** tab.</span></span> <span data-ttu-id="0ab86-132">Vi ska referera till den här URL:en som *serverdelens slutpunkt* senare i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-132">We will refer to this URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="0ab86-133">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-133">Click **Publish**.</span></span>

## <a name="modify-the-ios-project"></a><span data-ttu-id="0ab86-134">Ändra iOS-projektet</span><span class="sxs-lookup"><span data-stu-id="0ab86-134">Modify the iOS project</span></span>
<span data-ttu-id="0ab86-135">Nu när du har ändrat din Apps serverdel för att skicka bara den *id* av ett meddelande ska du ändra din iOS-app för att hantera detta id och hämta omfattande meddelandet från din serverdel.</span><span class="sxs-lookup"><span data-stu-id="0ab86-135">Now that you have modified your app backend to send just the *id* of a notification, you will change your iOS app to handle that id and retrieve the rich message from your backend.</span></span>

1. <span data-ttu-id="0ab86-136">Öppna projektet iOS och aktivera fjärråtkomst-meddelanden genom att gå till dina viktigaste app mål i den **mål** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0ab86-136">Open your iOS project, and enable remote notifications by going to your main app target in the **Targets** section.</span></span>
2. <span data-ttu-id="0ab86-137">Klicka på **funktioner**, aktivera **bakgrundslägen**, och kontrollera den **Remote Notifications** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="0ab86-137">Click on **Capabilities**, turn on **Background Modes**, and check the **Remote Notifications** checkbox.</span></span>
   
    ![][IOS3]
3. <span data-ttu-id="0ab86-138">Gå till **Main.storyboard**, och kontrollera att du har en View-Controller (kallade Start View Controller i den här självstudiekursen) från [meddela användaren](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-138">Go to **Main.storyboard**, and make sure you have a View Controller (refered to as Home View Controller in this tutorial) from [Notify User](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial.</span></span>
4. <span data-ttu-id="0ab86-139">Lägg till en **navigering Controller** till storyboard och CTRL-dra till Start View Controller så att den den **rot visa** av navigeringen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-139">Add a **Navigation Controller** to your storyboard, and control-drag to Home View Controller to make it the **root view** of navigation.</span></span> <span data-ttu-id="0ab86-140">Kontrollera att den **är inledande View Controller** i attribut inspector väljs för navigering-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="0ab86-140">Make sure the **Is Initial View Controller** in Attributes inspector is selected for the Navigation Controller only.</span></span>
5. <span data-ttu-id="0ab86-141">Lägg till en **View Controller** storyboard och lägga till en **bild visa**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-141">Add a **View Controller** to storyboard and add an **Image View**.</span></span> <span data-ttu-id="0ab86-142">Detta är den sida som användarna ser när de vill veta mer genom att klicka på notifiication.</span><span class="sxs-lookup"><span data-stu-id="0ab86-142">This is the page users will see once they choose to learn more by clicking on the notifiication.</span></span> <span data-ttu-id="0ab86-143">Storyboard bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="0ab86-143">Your storyboard should look as follows:</span></span>
   
    ![][IOS4]
6. <span data-ttu-id="0ab86-144">Klicka på den **Start View Controller** storyboard och kontrollera att den har **homeViewController** som dess **anpassad klass** och **Storyboard ID**under Identity-inspector.</span><span class="sxs-lookup"><span data-stu-id="0ab86-144">Click on the **Home View Controller** in storyboard, and make sure it has **homeViewController** as its **Custom Class** and **Storyboard ID** under the Identity inspector.</span></span>
7. <span data-ttu-id="0ab86-145">Gör likadant för avbildningen View Controller som **imageViewController**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-145">Do the same for Image View Controller as **imageViewController**.</span></span>
8. <span data-ttu-id="0ab86-146">Skapa sedan en ny View Controller klass som heter **imageViewController** att hantera Användargränssnittet som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="0ab86-146">Then, create a new View Controller class titled **imageViewController** to handle the UI you just created.</span></span>
9. <span data-ttu-id="0ab86-147">I **imageViewController.h**, lägger du till följande den styrenheten gränssnittet deklarationer.</span><span class="sxs-lookup"><span data-stu-id="0ab86-147">In **imageViewController.h**, add the following to the controller's interface declarations.</span></span> <span data-ttu-id="0ab86-148">Se till att CTRL-dra från storyboard avbildningen dessa egenskaper för att länka samman:</span><span class="sxs-lookup"><span data-stu-id="0ab86-148">Make sure to control-drag from the storyboard image view to these properties to link the two:</span></span>
   
        @property (weak, nonatomic) IBOutlet UIImageView *myImage;
        @property (strong) UIImage* imagePayload;
10. <span data-ttu-id="0ab86-149">I **imageViewController.m**, Lägg till följande i slutet av **viewDidload**:</span><span class="sxs-lookup"><span data-stu-id="0ab86-149">In **imageViewController.m**, add the following at the end of **viewDidload**:</span></span>
    
        // Display the UI Image in UI Image View
        [self.myImage setImage:self.imagePayload];
11. <span data-ttu-id="0ab86-150">I **AppDelegate.m**, importera bild-domänkontrollant som du skapade:</span><span class="sxs-lookup"><span data-stu-id="0ab86-150">In **AppDelegate.m**, import the image controller you created:</span></span>
    
        #import "imageViewController.h"
12. <span data-ttu-id="0ab86-151">Lägg till ett gränssnitt avsnitt med följande deklaration:</span><span class="sxs-lookup"><span data-stu-id="0ab86-151">Add an interface section with the following declaration:</span></span>
    
        @interface AppDelegate ()
    
        @property UIImage* imagePayload;
        @property NSDictionary* userInfo;
        @property BOOL iOS8;
    
        // Obtain content from backend with notification id
        - (void)retrieveRichImageWithId:(int)richId completion: (void(^)(NSError*)) completion;
    
        // Redirect to Image View Controller after notification interaction
        - (void)redirectToImageViewWithImage: (UIImage *)img;
    
        @end
13. <span data-ttu-id="0ab86-152">I **AppDelegate**, kontrollera att din app registrerar för tyst meddelanden i **program: didFinishLaunchingWithOptions**:</span><span class="sxs-lookup"><span data-stu-id="0ab86-152">In **AppDelegate**, make sure your app registers for silent notifications in **application: didFinishLaunchingWithOptions**:</span></span>
    
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

1. <span data-ttu-id="0ab86-153">Ersätt i följande implementeringen för **program: didRegisterForRemoteNotificationsWithDeviceToken** ta storyboard Användargränssnittet ändras i beräkningen:</span><span class="sxs-lookup"><span data-stu-id="0ab86-153">Subsitute in the following implementation for **application:didRegisterForRemoteNotificationsWithDeviceToken** to take the storyboard UI changes into account:</span></span>
   
       // Access navigation controller which is at the root of window
       UINavigationController *nc = (UINavigationController *)self.window.rootViewController;
       // Get home view controller from stack on navigation controller
       homeViewController *hvc = (homeViewController *)[nc.viewControllers objectAtIndex:0];
       hvc.deviceToken = deviceToken;
2. <span data-ttu-id="0ab86-154">Lägg sedan till följande metoder för att **AppDelegate.m** att hämta bilden från din slutpunkt och skicka ett meddelande om lokala när hämtningen är klar.</span><span class="sxs-lookup"><span data-stu-id="0ab86-154">Then, add the following methods to **AppDelegate.m** to retrieve the image from your endpoint and send a local notification when retrieval is complete.</span></span> <span data-ttu-id="0ab86-155">Ersätt platshållaren `{backend endpoint}` med backend-slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="0ab86-155">Make sure to substitute the placeholder `{backend endpoint}` with your backend endpoint:</span></span>
   
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
                   // From NSData to UIImage
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
   
                       // "5" is arbitrary here to give you enough time to quit out of the app and receive push notifications
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
           // Add "else if" here to handle more types of rich content such as url, sound files, etc.
       }
3. <span data-ttu-id="0ab86-156">Hantera lokala meddelandet ovan genom att öppna bilden view controller i **AppDelegate.m** med följande metoder:</span><span class="sxs-lookup"><span data-stu-id="0ab86-156">Handle the local notification above by opening up the image view controller in **AppDelegate.m** with the following methods:</span></span>
   
       // Helper: redirect users to image view controller
       - (void)redirectToImageViewWithImage: (UIImage *)img {
           UINavigationController *navigationController = (UINavigationController*) self.window.rootViewController;
           UIStoryboard *mainStoryboard = [UIStoryboard storyboardWithName:@"Main"
                                                                    bundle: nil];
           imageViewController *imgViewController = [mainStoryboard instantiateViewControllerWithIdentifier: @"imageViewController"];
           // Pass data/image to image view controller
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
           // Add "else if" here to handle more buttons
       }
   
       // Handle notification setting actions in iOS8
       - (void)application:(UIApplication *)application handleActionWithIdentifier:(NSString *)identifier forLocalNotification:(UILocalNotification *)notification completionHandler:(void (^)())completionHandler {
           // Handle richPush related buttons
           if ([identifier isEqualToString:@"richPushMore"]) {
               [self redirectToImageViewWithImage:self.imagePayload];
           }
           completionHandler();
       }

## <a name="run-the-application"></a><span data-ttu-id="0ab86-157">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="0ab86-157">Run the Application</span></span>
1. <span data-ttu-id="0ab86-158">Kör appen i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i simulatorn).</span><span class="sxs-lookup"><span data-stu-id="0ab86-158">In XCode, run the app on a physical iOS device (push notifications will not work in the simulator).</span></span>
2. <span data-ttu-id="0ab86-159">I appens användargränssnitt, anger du ett användarnamn och lösenord med samma värde för autentisering och klickar på **loggar In**.</span><span class="sxs-lookup"><span data-stu-id="0ab86-159">In the iOS app UI, enter a username and password of the same value for authentication and click **Log In**.</span></span>
3. <span data-ttu-id="0ab86-160">Klicka på **skicka push** och du bör se en avisering i appen.</span><span class="sxs-lookup"><span data-stu-id="0ab86-160">Click **Send push** and you should see an in-app alert.</span></span> <span data-ttu-id="0ab86-161">Om du klickar på **mer**, kommer du till den bild som du vill inkludera i din Apps serverdel.</span><span class="sxs-lookup"><span data-stu-id="0ab86-161">If you click on **More**, you will be brought to the image you chose to include in your app backend.</span></span>
4. <span data-ttu-id="0ab86-162">Du kan också klicka på **skicka push** och tryck på knappen Startsida för enheten omedelbart.</span><span class="sxs-lookup"><span data-stu-id="0ab86-162">You can also click **Send push** and immediately press the home button of your device.</span></span> <span data-ttu-id="0ab86-163">Om en stund får du ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0ab86-163">In a few moments, you will receive a push notification.</span></span> <span data-ttu-id="0ab86-164">Om du knackar på den eller klicka på mer kommer din app och omfattande avbildningen innehåll.</span><span class="sxs-lookup"><span data-stu-id="0ab86-164">If you tap on it or click More, you will be brought to your app and the rich image content.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-1.png
[IOS2]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-2.png
[IOS3]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-3.png
[IOS4]: ./media/notification-hubs-aspnet-backend-ios-rich-push/rich-push-ios-4.png
