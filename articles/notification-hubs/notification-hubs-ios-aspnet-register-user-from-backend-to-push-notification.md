---
title: "Registrera den aktuella användaren för push-meddelanden med hjälp av Web API | Microsoft Docs"
description: "Lär dig mer om att begära registreringen av push-meddelanden i en iOS-app med Azure Notification Hubs vid registrering utförs av ASP.NET Web API."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: fd56bb2dd627b31f00363851a4e76484aa382988
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="register-the-current-user-for-push-notifications-by-using-aspnet"></a><span data-ttu-id="252ba-103">Registrera den aktuella användaren för push-meddelanden med hjälp av ASP.NET</span><span class="sxs-lookup"><span data-stu-id="252ba-103">Register the current user for push notifications by using ASP.NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="252ba-104">iOS</span><span class="sxs-lookup"><span data-stu-id="252ba-104">iOS</span></span>](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="252ba-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="252ba-105">Overview</span></span>
<span data-ttu-id="252ba-106">Det här avsnittet visar hur du begär registreringen av push-meddelanden med Azure Notification Hubs när registreringen utförs av ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="252ba-106">This topic shows you how to request push notification registration with Azure Notification Hubs when registration is performed by ASP.NET Web API.</span></span> <span data-ttu-id="252ba-107">Det här avsnittet utökar kursen [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="252ba-107">This topic extends the tutorial [Notify users with Notification Hubs].</span></span> <span data-ttu-id="252ba-108">Du måste redan har slutfört nödvändiga steg i att kursen hjälper dig att skapa autentiserade mobiltjänsten.</span><span class="sxs-lookup"><span data-stu-id="252ba-108">You must have already completed the required steps in that tutorial to create the authenticated mobile service.</span></span> <span data-ttu-id="252ba-109">Läs mer på meddela användare scenario [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="252ba-109">For more information on the notify users scenario, see [Notify users with Notification Hubs].</span></span>

## <a name="update-your-app"></a><span data-ttu-id="252ba-110">Uppdatera ditt program</span><span class="sxs-lookup"><span data-stu-id="252ba-110">Update your app</span></span>
1. <span data-ttu-id="252ba-111">Lägg till följande komponenter från objektbiblioteket i din MainStoryboard_iPhone.storyboard:</span><span class="sxs-lookup"><span data-stu-id="252ba-111">In your MainStoryboard_iPhone.storyboard, add the following components from the object library:</span></span>
   
   * <span data-ttu-id="252ba-112">**Etiketten**: ”Push till användare med Notification Hubs”</span><span class="sxs-lookup"><span data-stu-id="252ba-112">**Label**: "Push to User with Notification Hubs"</span></span>
   * <span data-ttu-id="252ba-113">**Etiketten**: ”InstallationId”</span><span class="sxs-lookup"><span data-stu-id="252ba-113">**Label**: "InstallationId"</span></span>
   * <span data-ttu-id="252ba-114">**Etiketten**: ”användare”</span><span class="sxs-lookup"><span data-stu-id="252ba-114">**Label**: "User"</span></span>
   * <span data-ttu-id="252ba-115">**Textfält**: ”användare”</span><span class="sxs-lookup"><span data-stu-id="252ba-115">**Text Field**: "User"</span></span>
   * <span data-ttu-id="252ba-116">**Etiketten**: ”Password”</span><span class="sxs-lookup"><span data-stu-id="252ba-116">**Label**: "Password"</span></span>
   * <span data-ttu-id="252ba-117">**Textfält**: ”Password”</span><span class="sxs-lookup"><span data-stu-id="252ba-117">**Text Field**: "Password"</span></span>
   * <span data-ttu-id="252ba-118">**Knappen**: ”inloggning”</span><span class="sxs-lookup"><span data-stu-id="252ba-118">**Button**: "Login"</span></span>
     
     <span data-ttu-id="252ba-119">Nu storyboard ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="252ba-119">At this point, your storyboard looks like the following:</span></span>
     
      ![][0]
2. <span data-ttu-id="252ba-120">I Redigeraren för Installationsassistenten skapa uttag av avstängda kontrollerna och anropa dem ansluta textfält med View-Controller (ombud) och skapa en **åtgärd** för den **inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="252ba-120">In the assistant editor, create outlets for all the switched controls and call them, connect the text fields with the View Controller (delegate), and create an **Action** for the **login** button.</span></span>
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain the following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. <span data-ttu-id="252ba-121">Skapa en klass som heter **DeviceInfo**, och kopiera följande kod i avsnittet gränssnitt i filen DeviceInfo.h:</span><span class="sxs-lookup"><span data-stu-id="252ba-121">Create a class named **DeviceInfo**, and copy the following code into the interface section of the file DeviceInfo.h:</span></span>
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. <span data-ttu-id="252ba-122">Kopiera följande kod i implementeringsavsnittet av filen DeviceInfo.m:</span><span class="sxs-lookup"><span data-stu-id="252ba-122">Copy the following code in the implementation section of the DeviceInfo.m file:</span></span>
   
            @synthesize installationId = _installationId;
   
            - (id)init {
                if (!(self = [super init]))
                    return nil;
   
                NSUserDefaults *defaults = [NSUserDefaults standardUserDefaults];
                _installationId = [defaults stringForKey:@"PushToUserInstallationId"];
                if(!_installationId) {
                    CFUUIDRef newUUID = CFUUIDCreate(kCFAllocatorDefault);
                    _installationId = (__bridge_transfer NSString *)CFUUIDCreateString(kCFAllocatorDefault, newUUID);
                    CFRelease(newUUID);
   
                    //store the install ID so we don't generate a new one next time
                    [defaults setObject:_installationId forKey:@"PushToUserInstallationId"];
                    [defaults synchronize];
                }
   
                return self;
            }
   
            - (NSString*)getDeviceTokenInHex {
                const unsigned *tokenBytes = [[self deviceToken] bytes];
                NSString *hexToken = [NSString stringWithFormat:@"%08X%08X%08X%08X%08X%08X%08X%08X",
                                      ntohl(tokenBytes[0]), ntohl(tokenBytes[1]), ntohl(tokenBytes[2]),
                                      ntohl(tokenBytes[3]), ntohl(tokenBytes[4]), ntohl(tokenBytes[5]),
                                      ntohl(tokenBytes[6]), ntohl(tokenBytes[7])];
                return hexToken;
            }
5. <span data-ttu-id="252ba-123">Lägg till följande egenskapen singleton PushToUserAppDelegate.h:</span><span class="sxs-lookup"><span data-stu-id="252ba-123">In PushToUserAppDelegate.h, add the following property singleton:</span></span>
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. <span data-ttu-id="252ba-124">I den **didFinishLaunchingWithOptions** metod i PushToUserAppDelegate.m, Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="252ba-124">In the **didFinishLaunchingWithOptions** method in PushToUserAppDelegate.m, add the following code:</span></span>
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    <span data-ttu-id="252ba-125">Den första raden initierar den **DeviceInfo** singleton.</span><span class="sxs-lookup"><span data-stu-id="252ba-125">The first line initializes the **DeviceInfo** singleton.</span></span> <span data-ttu-id="252ba-126">De andra raden startar registreringen av push-meddelanden som redan finns är du redan har slutfört den [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="252ba-126">The second line starts the registration for push notifications, which is already present is you have already completed the [Get Started with Notification Hubs] tutorial.</span></span>
7. <span data-ttu-id="252ba-127">I PushToUserAppDelegate.m, implementerar du metoden **didRegisterForRemoteNotificationsWithDeviceToken** i din AppDelegate och Lägg till följande kod:</span><span class="sxs-lookup"><span data-stu-id="252ba-127">In PushToUserAppDelegate.m, implement the method **didRegisterForRemoteNotificationsWithDeviceToken** in your AppDelegate and add the following code:</span></span>
   
        self.deviceInfo.deviceToken = deviceToken;
   
    <span data-ttu-id="252ba-128">Detta anger enhetens token för begäran.</span><span class="sxs-lookup"><span data-stu-id="252ba-128">This sets the device token for the request.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="252ba-129">Nu är får det inte vara någon annan kod i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="252ba-129">At this point, there should not be any other code in this method.</span></span> <span data-ttu-id="252ba-130">Om du redan har ett anrop till den **registerNativeWithDeviceToken** metod som har lagts till när du slutfört den [Kom igång med Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) självstudier, måste du kommenterar ut eller ta bort detta anrop.</span><span class="sxs-lookup"><span data-stu-id="252ba-130">If you already have a call to the **registerNativeWithDeviceToken** method that was added when you completed the [Get Started with Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) tutorial, you must comment-out or remove that call.</span></span>
   > 
   > 
8. <span data-ttu-id="252ba-131">Lägg till följande hanterarmetoden i filen PushToUserAppDelegate.m:</span><span class="sxs-lookup"><span data-stu-id="252ba-131">In the PushToUserAppDelegate.m file, add the following handler method:</span></span>
   
   * <span data-ttu-id="252ba-132">(void) programmet:(UIApplication *) program didReceiveRemoteNotification:(NSDictionary *) användarinformationen {NSLog (@ ”% @”, användarinformationen);   UIAlertView * avisering = [[UIAlertView allokeringsenhets] initWithTitle:@"Notification” meddelande: [användarinformationen objectForKey:@"inAppMessage”] ombud: nil cancelButtonTitle: @ ”OK” otherButtonTitles:nil, nil];   [varning visa]; }</span><span class="sxs-lookup"><span data-stu-id="252ba-132">(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:                         [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:                         @"OK" otherButtonTitles:nil, nil];   [alert show]; }</span></span>
   
   <span data-ttu-id="252ba-133">Den här metoden visas ett varningsmeddelande i Användargränssnittet när appen tar emot meddelanden när den körs.</span><span class="sxs-lookup"><span data-stu-id="252ba-133">This method displays an alert in the UI when your app receives notifications while it is running.</span></span>
9. <span data-ttu-id="252ba-134">Öppna filen PushToUserViewController.m och returnera tangentbordet i följande implementering:</span><span class="sxs-lookup"><span data-stu-id="252ba-134">Open the PushToUserViewController.m file, and return the keyboard in the following implementation:</span></span>
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. <span data-ttu-id="252ba-135">I den **viewDidLoad** metod i filen PushToUserViewController.m initiera installationId etiketten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="252ba-135">In the **viewDidLoad** method in the PushToUserViewController.m file, initialize the installationId label as follows:</span></span>
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. <span data-ttu-id="252ba-136">Lägg till följande egenskaper i gränssnittet i PushToUserViewController.m:</span><span class="sxs-lookup"><span data-stu-id="252ba-136">Add the following properties in interface in PushToUserViewController.m:</span></span>
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. <span data-ttu-id="252ba-137">Lägg sedan till följande implementering:</span><span class="sxs-lookup"><span data-stu-id="252ba-137">Then, add the following implementation:</span></span>
    
            - (NSOperationQueue *)downloadQueue {
                if (!_downloadQueue) {
                    _downloadQueue = [[NSOperationQueue alloc] init];
                    _downloadQueue.name = @"Download Queue";
                    _downloadQueue.maxConcurrentOperationCount = 1;
                }
                return _downloadQueue;
            }
    
            // base64 encoding
            - (NSString*)base64forData:(NSData*)theData
            {
                const uint8_t* input = (const uint8_t*)[theData bytes];
                NSInteger length = [theData length];
    
                static char table[] = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/=";
    
                NSMutableData* data = [NSMutableData dataWithLength:((length + 2) / 3) * 4];
                uint8_t* output = (uint8_t*)data.mutableBytes;
    
                NSInteger i;
                for (i=0; i < length; i += 3) {
                    NSInteger value = 0;
                    NSInteger j;
                    for (j = i; j < (i + 3); j++) {
                        value <<= 8;
    
                        if (j < length) {
                            value |= (0xFF & input[j]);
                        }
                    }
    
                    NSInteger theIndex = (i / 3) * 4;
                    output[theIndex + 0] =                    table[(value >> 18) & 0x3F];
                    output[theIndex + 1] =                    table[(value >> 12) & 0x3F];
                    output[theIndex + 2] = (i + 1) < length ? table[(value >> 6)  & 0x3F] : '=';
                    output[theIndex + 3] = (i + 2) < length ? table[(value >> 0)  & 0x3F] : '=';
                }
    
                return [[NSString alloc] initWithData:data encoding:NSASCIIStringEncoding];
            }
13. <span data-ttu-id="252ba-138">Kopiera följande kod i den **inloggning** hanterarmetoden som skapats av XCode:</span><span class="sxs-lookup"><span data-stu-id="252ba-138">Copy the following code into the **login** handler method created by XCode:</span></span>
    
            DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
    
            // build JSON
            NSString* json = [NSString stringWithFormat:@"{\"platform\":\"ios\", \"instId\":\"%@\", \"deviceToken\":\"%@\"}", deviceInfo.installationId, [deviceInfo getDeviceTokenInHex]];
    
            // build auth string
            NSString* authString = [NSString stringWithFormat:@"%@:%@", self.User.text, self.Password.text];
    
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:@"http://nhnotifyuser.azurewebsites.net/api/register"]];
            [request setHTTPMethod:@"POST"];
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
            [request addValue:[@([json lengthOfBytesUsingEncoding:NSUTF8StringEncoding]) description] forHTTPHeaderField:@"Content-Length"];
            [request addValue:@"application/json" forHTTPHeaderField:@"Content-Type"];
            [request addValue:[NSString stringWithFormat:@"Basic %@",[self base64forData:[authString dataUsingEncoding:NSUTF8StringEncoding]]] forHTTPHeaderField:@"Authorization"];
    
            // connect with POST
            [NSURLConnection sendAsynchronousRequest:request queue:[self downloadQueue] completionHandler:^(NSURLResponse* response, NSData* data, NSError* error) {
                // add UIAlert depending on response.
                if (error != nil) {
                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if ([httpResponse statusCode] == 200) {
                        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Back-end registration" message:@"Registration successful" delegate:nil cancelButtonTitle: @"OK" otherButtonTitles:nil, nil];
                        [alert show];
                    } else {
                        NSLog(@"status: %ld", (long)[httpResponse statusCode]);
                    }
                } else {
                    NSLog(@"error: %@", error);
                }
            }];
    
    <span data-ttu-id="252ba-139">Den här metoden hämtar ett installations-ID och ett kanal för push-meddelanden och skickar den, tillsammans med typ av enhet på den autentiserade Web API-metod som skapar en registrering i Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="252ba-139">This method gets both an installation ID and channel for push notifications and sends it, along with the device type, to the authenticated Web API method that creates a registration in Notification Hubs.</span></span> <span data-ttu-id="252ba-140">Webb-API har definierats i [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="252ba-140">This Web API was defined in [Notify users with Notification Hubs].</span></span>

<span data-ttu-id="252ba-141">Nu när klientappen har uppdaterats, gå tillbaka till den [meddela användare med Notification Hubs] och uppdatera mobiltjänsten för att skicka meddelanden med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="252ba-141">Now that the client app has been updated, return to the [Notify users with Notification Hubs] and update the mobile service to send notifications by using Notification Hubs.</span></span>

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
<span data-ttu-id="252ba-142">[meddela användare med Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet</span><span class="sxs-lookup"><span data-stu-id="252ba-142">[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet</span></span>

<span data-ttu-id="252ba-143">[Kom igång med Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios</span><span class="sxs-lookup"><span data-stu-id="252ba-143">[Get Started with Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios</span></span>
