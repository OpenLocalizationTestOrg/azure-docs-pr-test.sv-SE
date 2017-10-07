---
title: "aaaRegister hello aktuell användare för push-meddelanden med hjälp av Web API | Microsoft Docs"
description: "Lär dig hur toorequest registrera push-meddelande i en iOS-app med Azure Notification Hubs när registrering utförs av ASP.NET Web API."
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
ms.openlocfilehash: f859feb436093e703d7e1db38354dd356fff8efe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="register-hello-current-user-for-push-notifications-by-using-aspnet"></a><span data-ttu-id="543f3-103">Registrera hello aktuell användare för push-meddelanden med hjälp av ASP.NET</span><span class="sxs-lookup"><span data-stu-id="543f3-103">Register hello current user for push notifications by using ASP.NET</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="543f3-104">iOS</span><span class="sxs-lookup"><span data-stu-id="543f3-104">iOS</span></span>](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="543f3-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="543f3-105">Overview</span></span>
<span data-ttu-id="543f3-106">Det här avsnittet visar hur toorequest registrera push-meddelande med Azure Notification Hubs när registreringen utförs av ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="543f3-106">This topic shows you how toorequest push notification registration with Azure Notification Hubs when registration is performed by ASP.NET Web API.</span></span> <span data-ttu-id="543f3-107">Det här avsnittet utökar hello kursen [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="543f3-107">This topic extends hello tutorial [Notify users with Notification Hubs].</span></span> <span data-ttu-id="543f3-108">Du måste redan ha slutfört hello krävs steg i självstudiekursen toocreate hello autentiserad mobila tjänsten.</span><span class="sxs-lookup"><span data-stu-id="543f3-108">You must have already completed hello required steps in that tutorial toocreate hello authenticated mobile service.</span></span> <span data-ttu-id="543f3-109">För mer information om hello meddela användare scenariot, se [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="543f3-109">For more information on hello notify users scenario, see [Notify users with Notification Hubs].</span></span>

## <a name="update-your-app"></a><span data-ttu-id="543f3-110">Uppdatera ditt program</span><span class="sxs-lookup"><span data-stu-id="543f3-110">Update your app</span></span>
1. <span data-ttu-id="543f3-111">Lägg till följande komponenter från objektbiblioteket för hello hello i din MainStoryboard_iPhone.storyboard:</span><span class="sxs-lookup"><span data-stu-id="543f3-111">In your MainStoryboard_iPhone.storyboard, add hello following components from hello object library:</span></span>
   
   * <span data-ttu-id="543f3-112">**Etiketten**: ”Push tooUser med Notification Hubs”</span><span class="sxs-lookup"><span data-stu-id="543f3-112">**Label**: "Push tooUser with Notification Hubs"</span></span>
   * <span data-ttu-id="543f3-113">**Etiketten**: ”InstallationId”</span><span class="sxs-lookup"><span data-stu-id="543f3-113">**Label**: "InstallationId"</span></span>
   * <span data-ttu-id="543f3-114">**Etiketten**: ”användare”</span><span class="sxs-lookup"><span data-stu-id="543f3-114">**Label**: "User"</span></span>
   * <span data-ttu-id="543f3-115">**Textfält**: ”användare”</span><span class="sxs-lookup"><span data-stu-id="543f3-115">**Text Field**: "User"</span></span>
   * <span data-ttu-id="543f3-116">**Etiketten**: ”Password”</span><span class="sxs-lookup"><span data-stu-id="543f3-116">**Label**: "Password"</span></span>
   * <span data-ttu-id="543f3-117">**Textfält**: ”Password”</span><span class="sxs-lookup"><span data-stu-id="543f3-117">**Text Field**: "Password"</span></span>
   * <span data-ttu-id="543f3-118">**Knappen**: ”inloggning”</span><span class="sxs-lookup"><span data-stu-id="543f3-118">**Button**: "Login"</span></span>
     
     <span data-ttu-id="543f3-119">Nu storyboard ser ut så hello följande:</span><span class="sxs-lookup"><span data-stu-id="543f3-119">At this point, your storyboard looks like hello following:</span></span>
     
      ![][0]
2. <span data-ttu-id="543f3-120">I Redigeraren för hello assistenten skapa uttag för alla hello växlade kontroller och anropa dem, ansluta hello textfält med hello View Controller (ombud) och skapa en **åtgärd** för hello **inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="543f3-120">In hello assistant editor, create outlets for all hello switched controls and call them, connect hello text fields with hello View Controller (delegate), and create an **Action** for hello **login** button.</span></span>
   
       ![][1]
   
       Your BreakingNewsViewController.h file should now contain hello following code:
   
        @property (weak, nonatomic) IBOutlet UILabel *installationId;
        @property (weak, nonatomic) IBOutlet UITextField *User;
        @property (weak, nonatomic) IBOutlet UITextField *Password;
   
        - (IBAction)login:(id)sender;
3. <span data-ttu-id="543f3-121">Skapa en klass som heter **DeviceInfo**, och kopiera hello följande kod i hello gränssnittet avsnitt i hello filen DeviceInfo.h:</span><span class="sxs-lookup"><span data-stu-id="543f3-121">Create a class named **DeviceInfo**, and copy hello following code into hello interface section of hello file DeviceInfo.h:</span></span>
   
        @property (readonly, nonatomic) NSString* installationId;
        @property (nonatomic) NSData* deviceToken;
4. <span data-ttu-id="543f3-122">Kopiera följande kod i hello implementeringsavsnittet hello DeviceInfo.m filen hello:</span><span class="sxs-lookup"><span data-stu-id="543f3-122">Copy hello following code in hello implementation section of hello DeviceInfo.m file:</span></span>
   
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
   
                    //store hello install ID so we don't generate a new one next time
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
5. <span data-ttu-id="543f3-123">Lägg till följande egenskapen singleton hello i PushToUserAppDelegate.h:</span><span class="sxs-lookup"><span data-stu-id="543f3-123">In PushToUserAppDelegate.h, add hello following property singleton:</span></span>
   
        @property (strong, nonatomic) DeviceInfo* deviceInfo;
6. <span data-ttu-id="543f3-124">I hello **didFinishLaunchingWithOptions** metod i PushToUserAppDelegate.m, Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="543f3-124">In hello **didFinishLaunchingWithOptions** method in PushToUserAppDelegate.m, add hello following code:</span></span>
   
        self.deviceInfo = [[DeviceInfo alloc] init];
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
   
    <span data-ttu-id="543f3-125">hello första raden initierar hello **DeviceInfo** singleton.</span><span class="sxs-lookup"><span data-stu-id="543f3-125">hello first line initializes hello **DeviceInfo** singleton.</span></span> <span data-ttu-id="543f3-126">hello andra raden startar hello registrering för push-meddelanden som redan finns är du redan har slutfört hello [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="543f3-126">hello second line starts hello registration for push notifications, which is already present is you have already completed hello [Get Started with Notification Hubs] tutorial.</span></span>
7. <span data-ttu-id="543f3-127">Implementera hello-metoden i PushToUserAppDelegate.m, **didRegisterForRemoteNotificationsWithDeviceToken** i din AppDelegate och Lägg till följande kod hello:</span><span class="sxs-lookup"><span data-stu-id="543f3-127">In PushToUserAppDelegate.m, implement hello method **didRegisterForRemoteNotificationsWithDeviceToken** in your AppDelegate and add hello following code:</span></span>
   
        self.deviceInfo.deviceToken = deviceToken;
   
    <span data-ttu-id="543f3-128">Detta anger hello enhetstoken för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="543f3-128">This sets hello device token for hello request.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="543f3-129">Nu är får det inte vara någon annan kod i den här metoden.</span><span class="sxs-lookup"><span data-stu-id="543f3-129">At this point, there should not be any other code in this method.</span></span> <span data-ttu-id="543f3-130">Om du redan har en anropet toohello **registerNativeWithDeviceToken** metod som har lagts till när du har slutfört hello [Kom igång med Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) självstudier, måste du kommenterar ut eller ta bort anropa.</span><span class="sxs-lookup"><span data-stu-id="543f3-130">If you already have a call toohello **registerNativeWithDeviceToken** method that was added when you completed hello [Get Started with Notification Hubs](/manage/services/notification-hubs/get-started-notification-hubs-ios/) tutorial, you must comment-out or remove that call.</span></span>
   > 
   > 
8. <span data-ttu-id="543f3-131">Lägg till följande hanterarmetoden hello hello PushToUserAppDelegate.m fil:</span><span class="sxs-lookup"><span data-stu-id="543f3-131">In hello PushToUserAppDelegate.m file, add hello following handler method:</span></span>
   
   * <span data-ttu-id="543f3-132">(void) programmet:(UIApplication *) program didReceiveRemoteNotification:(NSDictionary *) användarinformationen {NSLog (@ ”% @”, användarinformationen);   UIAlertView * avisering = [[UIAlertView allokeringsenhets] initWithTitle:@"Notification” meddelande: [användarinformationen objectForKey:@"inAppMessage”] ombud: nil cancelButtonTitle: @ ”OK” otherButtonTitles:nil, nil];   [varning visa]; }</span><span class="sxs-lookup"><span data-stu-id="543f3-132">(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Notification" message:                         [userInfo objectForKey:@"inAppMessage"] delegate:nil cancelButtonTitle:                         @"OK" otherButtonTitles:nil, nil];   [alert show]; }</span></span>
   
   <span data-ttu-id="543f3-133">Den här metoden visas ett varningsmeddelande i hello Användargränssnittet när appen tar emot meddelanden när den körs.</span><span class="sxs-lookup"><span data-stu-id="543f3-133">This method displays an alert in hello UI when your app receives notifications while it is running.</span></span>
9. <span data-ttu-id="543f3-134">Öppna hello PushToUserViewController.m fil- och returnera hello tangentbord i hello efter implementering:</span><span class="sxs-lookup"><span data-stu-id="543f3-134">Open hello PushToUserViewController.m file, and return hello keyboard in hello following implementation:</span></span>
   
        - (BOOL)textFieldShouldReturn:(UITextField *)theTextField {
            if (theTextField == self.User || theTextField == self.Password) {
                [theTextField resignFirstResponder];
            }
            return YES;
        }
10. <span data-ttu-id="543f3-135">I hello **viewDidLoad** metod i hello PushToUserViewController.m filen initiera hello installationId etikett på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="543f3-135">In hello **viewDidLoad** method in hello PushToUserViewController.m file, initialize hello installationId label as follows:</span></span>
    
         DeviceInfo* deviceInfo = [(PushToUserAppDelegate*)[[UIApplication sharedApplication]delegate] deviceInfo];
         Self.installationId.text = deviceInfo.installationId;
11. <span data-ttu-id="543f3-136">Lägg till följande egenskaper i gränssnittet i PushToUserViewController.m hello:</span><span class="sxs-lookup"><span data-stu-id="543f3-136">Add hello following properties in interface in PushToUserViewController.m:</span></span>
    
        @property (readonly) NSOperationQueue* downloadQueue;
        - (NSString*)base64forData:(NSData*)theData;
12. <span data-ttu-id="543f3-137">Lägg sedan till hello efter implementering:</span><span class="sxs-lookup"><span data-stu-id="543f3-137">Then, add hello following implementation:</span></span>
    
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
13. <span data-ttu-id="543f3-138">Kopiera hello följande kod i hello **inloggning** hanterarmetoden som skapats av XCode:</span><span class="sxs-lookup"><span data-stu-id="543f3-138">Copy hello following code into hello **login** handler method created by XCode:</span></span>
    
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
    
    <span data-ttu-id="543f3-139">Den här metoden hämtar ett installations-ID och ett kanal för push-meddelanden och skickar den, tillsammans med hello enhetstyp, toohello autentiserad Web API-metod som skapar en registrering i Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="543f3-139">This method gets both an installation ID and channel for push notifications and sends it, along with hello device type, toohello authenticated Web API method that creates a registration in Notification Hubs.</span></span> <span data-ttu-id="543f3-140">Webb-API har definierats i [meddela användare med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="543f3-140">This Web API was defined in [Notify users with Notification Hubs].</span></span>

<span data-ttu-id="543f3-141">Nu när hello-klientappen har uppdaterats, returnera toohello [meddela användare med Notification Hubs] och uppdatera hello Mobiltjänst toosend meddelanden med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="543f3-141">Now that hello client app has been updated, return toohello [Notify users with Notification Hubs] and update hello mobile service toosend notifications by using Notification Hubs.</span></span>

<!-- Anchors. -->

<!-- Images. -->
[0]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios1.png
[1]: ./media/notification-hubs-ios-aspnet-register-user-push-notifications/notification-hub-user-aspnet-ios2.png

<!-- URLs. -->
[meddela användare med Notification Hubs]: /manage/services/notification-hubs/notify-users-aspnet

[Kom igång med Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-ios
