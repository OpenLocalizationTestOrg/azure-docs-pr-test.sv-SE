---
title: "aaaAzure Meddelandeanvändare Notification Hubs för iOS med .NET-serverdel"
description: "Lär dig hur toosend push-meddelanden toousers i Azure. Kodexempel som skrivits i Objective-C och hello .NET-API för hello serverdel."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 1f7d1410-ef93-4c4b-813b-f075eed20082
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 56aed5b04d2d985b3f0e50c58991607f07b61248
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a><span data-ttu-id="0a64b-104">Meddela användare för iOS med .NET-serverdel i Azure Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="0a64b-104">Azure Notification Hubs Notify Users for iOS with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="0a64b-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="0a64b-105">Overview</span></span>
<span data-ttu-id="0a64b-106">Stöd för push-meddelanden i Azure kan du tooaccess ett enkelt att använda och multiplatform skalats ut push-infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.</span><span class="sxs-lookup"><span data-stu-id="0a64b-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="0a64b-107">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa specifik app användare på en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="0a64b-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="0a64b-108">En ASP.NET-WebAPI-serverdel är används tooauthenticate klienter och toogenerate meddelanden som visas i hello vägledning avsnittet [registrering från din Apps serverdel](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span><span class="sxs-lookup"><span data-stu-id="0a64b-108">An ASP.NET WebAPI backend is used tooauthenticate clients and toogenerate notifications, as shown in hello guidance topic [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).</span></span>

> [!NOTE]
> <span data-ttu-id="0a64b-109">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0a64b-109">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span> <span data-ttu-id="0a64b-110">Den här kursen är också hello nödvändiga toohello [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-110">This tutorial is also hello prerequisite toohello [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) tutorial.</span></span>
> <span data-ttu-id="0a64b-111">Om du vill toouse Mobile Apps som backend-tjänst finns hello [Mobile Apps Kom igång med Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="0a64b-111">If you want toouse Mobile Apps as your backend service, see hello [Mobile Apps Get Started with Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a><span data-ttu-id="0a64b-112">Ändra din iOS-app</span><span class="sxs-lookup"><span data-stu-id="0a64b-112">Modify your iOS app</span></span>
1. <span data-ttu-id="0a64b-113">Öppna hello sida visa app som du skapade i hello [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-113">Open hello Single Page view app you created in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0a64b-114">I det här avsnittet förutsätter att projektet har konfigurerats med ett tomt organisationsnamn.</span><span class="sxs-lookup"><span data-stu-id="0a64b-114">In this section we assume that your project is configured with an empty organization name.</span></span> <span data-ttu-id="0a64b-115">Om du inte behöver du tooprepend din organisation namn tooall klassnamn.</span><span class="sxs-lookup"><span data-stu-id="0a64b-115">If not, you will need tooprepend your organization name tooall class names.</span></span>
   > 
   > 
2. <span data-ttu-id="0a64b-116">Lägg till hello-komponenter som visas i hello skärmbilden nedan från objektbiblioteket för hello i din Main.storyboard.</span><span class="sxs-lookup"><span data-stu-id="0a64b-116">In your Main.storyboard add hello components shown in hello screenshot below from hello object library.</span></span>
   
    ![][1]
   
   * <span data-ttu-id="0a64b-117">**Användarnamnet**: A UITextField med platshållartext, *ange användarnamn*genast skickar resultaten etikett och begränsad toohello kvar under hello och högerklicka marginaler och under hello skicka resultaten etikett.</span><span class="sxs-lookup"><span data-stu-id="0a64b-117">**Username**: A UITextField with placeholder text, *Enter Username*, immediately beneath hello send results label and constrained toohello left and right margins and beneath hello send results label.</span></span>
   * <span data-ttu-id="0a64b-118">**Lösenordet**: A UITextField med platshållartext, *ange lösenord för*, omedelbart innanför hello användarnamn textfältet och begränsad toohello vänster och höger marginaler och under hello användarnamn textfältet.</span><span class="sxs-lookup"><span data-stu-id="0a64b-118">**Password**: A UITextField with placeholder text, *Enter Password*, immediately beneath hello username text field and constrained toohello left and right margins and beneath hello username text field.</span></span> <span data-ttu-id="0a64b-119">Kontrollera hello **Secure textinmatning** alternativet i hello attributet Inspector under *returnera nyckeln*.</span><span class="sxs-lookup"><span data-stu-id="0a64b-119">Check hello **Secure Text Entry** option in hello Attribute Inspector, under *Return Key*.</span></span>
   * <span data-ttu-id="0a64b-120">**Logga in**: A UIButton etikett direkt under hello lösenordsfältet och avmarkerar hello **aktiverad** alternativet i hello attribut Inspector under *kontrollens innehåll*</span><span class="sxs-lookup"><span data-stu-id="0a64b-120">**Log in**: A UIButton labeled immediately beneath hello password text field and uncheck hello **Enabled** option in hello Attributes Inspector, under *Control-Content*</span></span>
   * <span data-ttu-id="0a64b-121">**WNS**: etikett och växel tooenable skicka hello notification Windows Notification Service om den har konfigurerats på hello hubben.</span><span class="sxs-lookup"><span data-stu-id="0a64b-121">**WNS**: Label and switch tooenable sending hello notification Windows Notification Service if it has been setup on hello hub.</span></span> <span data-ttu-id="0a64b-122">Se hello [Windows komma igång](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-122">See hello [Windows Getting Started](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span>
   * <span data-ttu-id="0a64b-123">**GCM**: etikett och växel tooenable skicka hello meddelande tooGoogle Cloud Messaging om den har konfigurerats på hello hubben.</span><span class="sxs-lookup"><span data-stu-id="0a64b-123">**GCM**: Label and switch tooenable sending hello notification tooGoogle Cloud Messaging if it has been setup on hello hub.</span></span> <span data-ttu-id="0a64b-124">Se [Android igång](notification-hubs-android-push-notification-google-gcm-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-124">See [Android Getting Started](notification-hubs-android-push-notification-google-gcm-get-started.md) tutorial.</span></span>
   * <span data-ttu-id="0a64b-125">**APN**: etikett och växel tooenable skicka hello meddelande toohello Apple Platform Notification Service.</span><span class="sxs-lookup"><span data-stu-id="0a64b-125">**APNS**: Label and switch tooenable sending hello notification toohello Apple Platform Notification Service.</span></span>
   * <span data-ttu-id="0a64b-126">**Recipent Username**: A UITextField med platshållartext, *mottagaren användarnamn taggen*omedelbart under hello GCM etikett och begränsad toohello vänster och höger marginaler och under hello GCM etiketten.</span><span class="sxs-lookup"><span data-stu-id="0a64b-126">**Recipent Username**:A UITextField with placeholder text, *Recipient username tag*, immediately beneath hello GCM label and constrained toohello left and right margins and beneath hello GCM label.</span></span>

    <span data-ttu-id="0a64b-127">Vissa komponenter har lagts till i hello [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-127">Some components were added in hello [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) tutorial.</span></span>

1. <span data-ttu-id="0a64b-128">**CTRL** dra från hello komponenter i hello visa tooViewController.h och Lägg till dessa nya uttag.</span><span class="sxs-lookup"><span data-stu-id="0a64b-128">**Ctrl** drag from hello components in hello view tooViewController.h and add these new outlets.</span></span>
   
        @property (weak, nonatomic) IBOutlet UITextField *UsernameField;
        @property (weak, nonatomic) IBOutlet UITextField *PasswordField;
        @property (weak, nonatomic) IBOutlet UITextField *RecipientField;
        @property (weak, nonatomic) IBOutlet UITextField *NotificationField;
   
        // Used tooenable hello buttons on hello UI
        @property (weak, nonatomic) IBOutlet UIButton *LogInButton;
        @property (weak, nonatomic) IBOutlet UIButton *SendNotificationButton;
   
        // Used tooenabled sending notifications across platforms
        @property (weak, nonatomic) IBOutlet UISwitch *WNSSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *GCMSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *APNSSwitch;
   
        - (IBAction)LogInAction:(id)sender;
2. <span data-ttu-id="0a64b-129">Lägg till följande hello i ViewController.h, `#define` under import-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0a64b-129">In ViewController.h, add hello following `#define` just below your import statements.</span></span> <span data-ttu-id="0a64b-130">Ersätt hello *< Endpoint ange Backend\>*  med hello mål-URL som du använde toodeploy din Apps serverdel hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0a64b-130">Substitute hello *<Enter Your Backend Endpoint\>* placeholder with hello Destination URL you used toodeploy your app backend in hello previous section.</span></span> <span data-ttu-id="0a64b-131">Till exempel *http://you_backend.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="0a64b-131">For example, *http://you_backend.azurewebsites.net*.</span></span>
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. <span data-ttu-id="0a64b-132">I projektet, skapa en ny **Cocoa Touch klassen** med namnet **RegisterClient** toointerface med hello ASP.NET-backend du skapat.</span><span class="sxs-lookup"><span data-stu-id="0a64b-132">In your project, create a new **Cocoa Touch class** named **RegisterClient** toointerface with hello ASP.NET back-end you created.</span></span> <span data-ttu-id="0a64b-133">Skapa hello-klass som ärver från `NSObject`.</span><span class="sxs-lookup"><span data-stu-id="0a64b-133">Create hello class inheriting from `NSObject`.</span></span> <span data-ttu-id="0a64b-134">Lägg sedan till följande kod i hello RegisterClient.h hello.</span><span class="sxs-lookup"><span data-stu-id="0a64b-134">Then add hello following code in hello RegisterClient.h.</span></span>
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. <span data-ttu-id="0a64b-135">Uppdatera hello i hello RegisterClient.m `@interface` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0a64b-135">In hello RegisterClient.m update hello `@interface` section:</span></span>
   
        @interface RegisterClient ()
   
        @property (strong, nonatomic) NSURLSession* session;
        @property (strong, nonatomic) NSURLSession* endpoint;
   
        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion;
        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion;
        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSString*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion;
   
        @end
5. <span data-ttu-id="0a64b-136">Ersätt hello `@implementation` avsnitt i hello RegisterClient.m med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="0a64b-136">Replace hello `@implementation` section in hello RegisterClient.m with hello following code.</span></span>

        @implementation RegisterClient

        // Globals used by RegisterClient
        NSString *const RegistrationIdLocalStorageKey = @"RegistrationId";

        -(instancetype) initWithEndpoint:(NSString*)Endpoint
        {
            self = [super init];
            if (self) {
                NSURLSessionConfiguration* config = [NSURLSessionConfiguration defaultSessionConfiguration];
                _session = [NSURLSession sessionWithConfiguration:config delegate:nil delegateQueue:nil];
                _endpoint = Endpoint;
            }
            return self;
        }

        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
                    andCompletion:(void(^)(NSError*))completion
        {
            [self tryToRegisterWithDeviceToken:token tags:tags retry:YES andCompletion:completion];
        }

        -(void) tryToRegisterWithDeviceToken:(NSData*)token tags:(NSSet*)tags retry:(BOOL)retry
                    andCompletion:(void(^)(NSError*))completion
        {
            NSSet* tagsSet = tags?tags:[[NSSet alloc] init];

            NSString *deviceTokenString = [[token description]
                stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
            deviceTokenString = [[deviceTokenString stringByReplacingOccurrencesOfString:@" " withString:@""]
                                    uppercaseString];

            [self retrieveOrRequestRegistrationIdWithDeviceToken: deviceTokenString
                completion:^(NSString* registrationId, NSError *error) {
                NSLog(@"regId: %@", registrationId);
                if (error) {
                    completion(error);
                    return;
                }

                [self upsertRegistrationWithRegistrationId:registrationId deviceToken:deviceTokenString
                    tags:tagsSet andCompletion:^(NSURLResponse * response, NSError *error) {
                    if (error) {
                        completion(error);
                        return;
                    }

                    NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*)response;
                    if (httpResponse.statusCode == 200) {
                        completion(nil);
                    } else if (httpResponse.statusCode == 410 && retry) {
                        [self tryToRegisterWithDeviceToken:token tags:tags retry:NO andCompletion:completion];
                    } else {
                        NSLog(@"Registration error with response status: %ld", (long)httpResponse.statusCode);

                        completion([NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }

                }];
            }];
        }

        -(void) upsertRegistrationWithRegistrationId:(NSString*)registrationId deviceToken:(NSData*)token
                    tags:(NSSet*)tags andCompletion:(void(^)(NSURLResponse*, NSError*))completion
        {
            NSDictionary* deviceRegistration = @{@"Platform" : @"apns", @"Handle": token,
                                                    @"Tags": [tags allObjects]};
            NSData* jsonData = [NSJSONSerialization dataWithJSONObject:deviceRegistration
                                options:NSJSONWritingPrettyPrinted error:nil];

            NSLog(@"JSON registration: %@", [[NSString alloc] initWithData:jsonData
                                                encoding:NSUTF8StringEncoding]);

            NSString* endpoint = [NSString stringWithFormat:@"%@/api/register/%@", _endpoint,
                                    registrationId];
            NSURL* requestURL = [NSURL URLWithString:endpoint];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"PUT"];
            [request setHTTPBody:jsonData];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];
            [request setValue:@"application/json" forHTTPHeaderField:@"Content-Type"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                if (!error)
                {
                    completion(response, error);
                }
                else
                {
                    NSLog(@"Error request: %@", error);
                    completion(nil, error);
                }
            }];
            [dataTask resume];
        }

        -(void) retrieveOrRequestRegistrationIdWithDeviceToken:(NSString*)token
                    completion:(void(^)(NSString*, NSError*))completion
        {
            NSString* registrationId = [[NSUserDefaults standardUserDefaults]
                                        objectForKey:RegistrationIdLocalStorageKey];

            if (registrationId)
            {
                completion(registrationId, nil);
                return;
            }

            // request new one & save
            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/api/register?handle=%@",
                                    _endpoint, token]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"POST"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",
                                                    self.authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [self.session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSString* registrationId = [[NSString alloc] initWithData:data
                        encoding:NSUTF8StringEncoding];

                    // remove quotes
                    registrationId = [registrationId substringWithRange:NSMakeRange(1,
                                        [registrationId length]-2)];

                    [[NSUserDefaults standardUserDefaults] setObject:registrationId
                        forKey:RegistrationIdLocalStorageKey];
                    [[NSUserDefaults standardUserDefaults] synchronize];

                    completion(registrationId, nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"Registration" code:httpResponse.statusCode
                                    userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }

        @end

    <span data-ttu-id="0a64b-137">hello koden ovan implementerar hello logik som beskrivs i artikeln hello [registrering från din Apps serverdel](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession tooperform REST-anropen tooyour appserverdelen och NSUserDefaults toolocally store hello registrationId som returneras av hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="0a64b-137">hello code above implements hello logic explained in hello guidance article [Registering from your app backend](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) using NSURLSession tooperform REST calls tooyour app backend, and NSUserDefaults toolocally store hello registrationId returned by hello notification hub.</span></span>

    <span data-ttu-id="0a64b-138">Observera att den här klassen kräver egenskapen **authorizationHeader** toobe som angetts i order toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="0a64b-138">Note that this class requires its property **authorizationHeader** toobe set in order toowork properly.</span></span> <span data-ttu-id="0a64b-139">Den här egenskapen anges av hello **ViewController** klassen när hello logga in.</span><span class="sxs-lookup"><span data-stu-id="0a64b-139">This property is set by hello **ViewController** class after hello log in.</span></span>

1. <span data-ttu-id="0a64b-140">ViewController.h, lägga till en `#import` -instruktion för RegisterClient.h.</span><span class="sxs-lookup"><span data-stu-id="0a64b-140">In ViewController.h, add a `#import` statement for RegisterClient.h.</span></span> <span data-ttu-id="0a64b-141">Lägger du till en deklaration för hello enhetstoken och referera till tooa `RegisterClient` instans i hello `@interface` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0a64b-141">Then add a declaration for hello device token and reference tooa `RegisterClient` instance in hello `@interface` section:</span></span>
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. <span data-ttu-id="0a64b-142">ViewController.m, lägga till en privat metoddeklaration i hello `@interface` avsnitt:</span><span class="sxs-lookup"><span data-stu-id="0a64b-142">In ViewController.m, add a private method declaration in hello `@interface` section:</span></span>
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> <span data-ttu-id="0a64b-143">hello följande kodavsnitt är inte en säker autentiseringsschema, ska du ersätta hello implementering av hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** med specifika autentiseringsmekanism som genererar en token toobe autentisering som används av hello registrera klientklass, t.ex. OAuth Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0a64b-143">hello following snippet is not a secure authentication scheme, you should substitute hello implementation of hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** with your specific authentication mechanism that generates an authentication token toobe consumed by hello register client class, e.g. OAuth, Active Directory.</span></span>
> 
> 

1. <span data-ttu-id="0a64b-144">I hello `@implementation` avsnitt i ViewController.m Lägg till följande kod som lägger till hello implementering för inställningen hello enhet token och autentisering huvudet hello.</span><span class="sxs-lookup"><span data-stu-id="0a64b-144">Then in hello `@implementation` section of ViewController.m add hello following code which adds hello implementation for setting hello device token and authentication header.</span></span>
   
        -(void) setDeviceToken: (NSData*) deviceToken
        {
            _deviceToken = deviceToken;
            self.LogInButton.enabled = YES;
        }
   
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
        {
            NSString* headerValue = [NSString stringWithFormat:@"%@:%@", username, password];
   
            NSData* encodedData = [[headerValue dataUsingEncoding:NSUTF8StringEncoding] base64EncodedDataWithOptions:NSDataBase64EncodingEndLineWithCarriageReturn];
   
            self.registerClient.authenticationHeader = [[NSString alloc] initWithData:encodedData
                                                        encoding:NSUTF8StringEncoding];
        }
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
   
    <span data-ttu-id="0a64b-145">Observera hur inställningen hello enhetstoken möjliggör hello inloggningen knappen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-145">Note how setting hello device token enables hello log in button.</span></span> <span data-ttu-id="0a64b-146">Det beror på att som en del av hello inloggningen åtgärden registrerar hello view controller för push-meddelanden med hello-appserverdelen.</span><span class="sxs-lookup"><span data-stu-id="0a64b-146">This is becasue as a part of hello login action, hello view controller registers for push notifications with hello app backend.</span></span> <span data-ttu-id="0a64b-147">Därför måste vill vi inte logga In åtgärd toobe tillgänglig tills hello enhetstoken har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="0a64b-147">Hence, we do not want Log In action toobe accessible till hello device token has been properly set up.</span></span> <span data-ttu-id="0a64b-148">Du kan frikopplar hello logga in från registreringen av push-hello så länge hello f.d. sker innan hello senare.</span><span class="sxs-lookup"><span data-stu-id="0a64b-148">You can decouple hello log in from hello push registration as long as hello former happens before hello latter.</span></span>
2. <span data-ttu-id="0a64b-149">I ViewController.m, använder du följande kodavsnitt tooimplement hello åtgärdsmetod för hello din **loggar In** knapp och en metod toosend hello notification meddelande med hello ASP.NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="0a64b-149">In ViewController.m, use hello following snippets tooimplement hello action method for your **Log In** button and a method toosend hello notification message using hello ASP.NET backend.</span></span>
   
       - <span data-ttu-id="0a64b-150">(IBAction) LogInAction: (id) avsändaren {/ / skapa autentiseringshuvud och ange den registrera klienten NSString * användarnamn = self. UsernameField.text;   NSString * lösenord = self. PasswordField.text;</span><span class="sxs-lookup"><span data-stu-id="0a64b-150">(IBAction)LogInAction:(id)sender {   // create authentication header and set it in register client   NSString* username = self.UsernameField.text;   NSString* password = self.PasswordField.text;</span></span>
   
           <span data-ttu-id="0a64b-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span><span class="sxs-lookup"><span data-stu-id="0a64b-151">[self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];</span></span>
   
           <span data-ttu-id="0a64b-152">__weak ViewController * selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken taggar: nil andCompletion:^(NSError* error) {Om (! fel) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = Ja.               [self MessageBox:@"Success” message:@"Registered har”!];});}}];}</span><span class="sxs-lookup"><span data-stu-id="0a64b-152">__weak ViewController* selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken tags:nil       andCompletion:^(NSError* error) {       if (!error) {           dispatch_async(dispatch_get_main_queue(),           ^{               selfie.SendNotificationButton.enabled = YES;               [self MessageBox:@"Success" message:@"Registered successfully!"];           });       }   }]; }</span></span>

        <span data-ttu-id="0a64b-153">-(void) SendNotificationASPNETBackend: (NSString*) pns UsernameTag: (NSString*) usernameTag meddelande: (NSString*) meddelande {NSURLSession* session = [NSURLSession sessionWithConfiguration: [NSURLSessionConfiguration defaultSessionConfiguration] ombud: nil delegateQueue:nil];</span><span class="sxs-lookup"><span data-stu-id="0a64b-153">- (void)SendNotificationASPNETBackend:(NSString*)pns UsernameTag:(NSString*)usernameTag            Message:(NSString*)message {    NSURLSession* session = [NSURLSession        sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration] delegate:nil        delegateQueue:nil];</span></span>

            <span data-ttu-id="0a64b-154">Skicka hello pns och username-tagg som parametrar med hello URL för REST toohello ASP.NET-serverdel NSURL * requestURL = [NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @”, BACKEND_ENDPOINT pns, usernameTag]];</span><span class="sxs-lookup"><span data-stu-id="0a64b-154">// Pass hello pns and username tag as parameters with hello REST URL toohello ASP.NET backend    NSURL* requestURL = [NSURL URLWithString:[NSString        stringWithFormat:@"%@/api/notifications?pns=%@&to_tag=%@", BACKEND_ENDPOINT, pns,        usernameTag]];</span></span>

            <span data-ttu-id="0a64b-155">NSMutableURLRequest * begäran = [NSMutableURLRequest requestWithURL:requestURL];    [begär setHTTPMethod:@"POST”];</span><span class="sxs-lookup"><span data-stu-id="0a64b-155">NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];    [request setHTTPMethod:@"POST"];</span></span>

            <span data-ttu-id="0a64b-156">Hämta hello fingerad authenticationheader från hello registrera klienten NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @”, self.registerClient.authenticationHeader];    [begär setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization”];</span><span class="sxs-lookup"><span data-stu-id="0a64b-156">// Get hello mock authenticationheader from hello register client    NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@",        self.registerClient.authenticationHeader];    [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];</span></span>

            <span data-ttu-id="0a64b-157">Lägg till hello-meddelande meddelandetexten [begära setValue:@"application/json;charset=utf-8” forHTTPHeaderField:@"Content-Type”];    [begära setHTTPBody: [meddelande dataUsingEncoding:NSUTF8StringEncoding]];</span><span class="sxs-lookup"><span data-stu-id="0a64b-157">//Add hello notification message body    [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];    [request setHTTPBody:[message dataUsingEncoding:NSUTF8StringEncoding]];</span></span>

            <span data-ttu-id="0a64b-158">Köra hello skicka meddelande REST API på hello ASP.NET-serverdel NSURLSessionDataTask * dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *fel) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) svar;        Om (fel || httpResponse.statusCode! = 200) {NSString* status = [NSString stringWithFormat:@"Error Status för % @: % d\nError: %@\n”, pns, httpResponse.statusCode, fel];            dispatch_async(dispatch_get_main_queue(), ^ {/ / lägga till text eftersom alla 3 PNS-anrop har också information tooview [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span><span class="sxs-lookup"><span data-stu-id="0a64b-158">// Execute hello send notification REST API on hello ASP.NET Backend    NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request        completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)    {        NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;        if (error || httpResponse.statusCode != 200)        {            NSString* status = [NSString stringWithFormat:@"Error Status for %@: %d\nError: %@\n",                                pns, httpResponse.statusCode, error];            dispatch_async(dispatch_get_main_queue(),            ^{                // Append text because all 3 PNS calls may also have information tooview                [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }</span></span>

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            <span data-ttu-id="0a64b-159">}];    [dataTask återuppta]; }</span><span class="sxs-lookup"><span data-stu-id="0a64b-159">}];    [dataTask resume]; }</span></span>


1. <span data-ttu-id="0a64b-160">Uppdatera hello åtgärden för hello **skicka meddelande** knappen toouse hello ASP.NET-serverdel och skicka tooany PNS aktiveras med en växel.</span><span class="sxs-lookup"><span data-stu-id="0a64b-160">Update hello action for hello **Send Notification** button toouse hello ASP.NET backend and send tooany PNS enabled by a switch.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            //[self SendNotificationRESTAPI];
            [self SendToEnabledPlatforms];
        }


        -(void)SendToEnabledPlatforms
        {
            NSString* json = [NSString stringWithFormat:@"\"%@\"",self.notificationMessage.text];

            [self.sendResults setText:@""];

            if ([self.WNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"wns" UsernameTag:self.RecipientField.text Message:json];

            if ([self.GCMSwitch isOn])
                [self SendNotificationASPNETBackend:@"gcm" UsernameTag:self.RecipientField.text Message:json];

            if ([self.APNSSwitch isOn])
                [self SendNotificationASPNETBackend:@"apns" UsernameTag:self.RecipientField.text Message:json];
        }



1. <span data-ttu-id="0a64b-161">I funktionen **ViewDidLoad**, lägga till hello följande tooinstantiate hello RegisterClient instans och ange hello ombud för textfält.</span><span class="sxs-lookup"><span data-stu-id="0a64b-161">In function **ViewDidLoad**, add hello following tooinstantiate hello RegisterClient instance and set hello delegate for your text fields.</span></span>
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. <span data-ttu-id="0a64b-162">Nu i **AppDelegate.m**, ta bort alla hello innehåll för hello metoden **program: didRegisterForPushNotificationWithDeviceToken:** och Ersätt den med följande toomake att hello som hello vyn domänkontrollanten innehåller hello senaste enhetstoken som hämtats från APN:</span><span class="sxs-lookup"><span data-stu-id="0a64b-162">Now in **AppDelegate.m**, remove all hello content of hello method **application:didRegisterForPushNotificationWithDeviceToken:** and replace it with hello following toomake sure that hello view controller contains hello latest device token retrieved from APNs:</span></span>
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. <span data-ttu-id="0a64b-163">Slutligen i **AppDelegate.m**, kontrollera att du har följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="0a64b-163">Finally in **AppDelegate.m**, make sure you have hello following method:</span></span>
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a><span data-ttu-id="0a64b-164">Testa hello program</span><span class="sxs-lookup"><span data-stu-id="0a64b-164">Test hello Application</span></span>
1. <span data-ttu-id="0a64b-165">Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).</span><span class="sxs-lookup"><span data-stu-id="0a64b-165">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="0a64b-166">Ange ett användarnamn och lösenord i hello iOS app Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="0a64b-166">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="0a64b-167">Dessa kan vara valfri sträng, men de måste båda vara hello samma strängvärde.</span><span class="sxs-lookup"><span data-stu-id="0a64b-167">These can be any string, but they must both be hello same string value.</span></span> <span data-ttu-id="0a64b-168">Klicka på **loggar In**.</span><span class="sxs-lookup"><span data-stu-id="0a64b-168">Then click **Log In**.</span></span>
   
    ![][2]
3. <span data-ttu-id="0a64b-169">Du bör se ett popup-fönster som informerar dig om registreringen lyckades.</span><span class="sxs-lookup"><span data-stu-id="0a64b-169">You should see a pop-up informing you of registration success.</span></span> <span data-ttu-id="0a64b-170">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0a64b-170">Click **OK**.</span></span>
   
    ![][3]
4. <span data-ttu-id="0a64b-171">I hello **mottagaren användarnamn taggen* text, ange hello namnet användartaggen används med hello registrering från en annan enhet.</span><span class="sxs-lookup"><span data-stu-id="0a64b-171">In hello **Recipient username tag* text field, enter hello user name tag used with hello registration from another device.</span></span>
5. <span data-ttu-id="0a64b-172">Ange ett meddelande och klicka på **skicka meddelande**.</span><span class="sxs-lookup"><span data-stu-id="0a64b-172">Enter a notification message and click **Send Notification**.</span></span>  <span data-ttu-id="0a64b-173">Endast hello-enheter som har en registrering med hello mottagarens namn användartaggen får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="0a64b-173">Only hello devices that have a registration with hello recipient user name tag receive hello notification message.</span></span>  <span data-ttu-id="0a64b-174">Den skickas endast toothose användare.</span><span class="sxs-lookup"><span data-stu-id="0a64b-174">It is only sent toothose users.</span></span>
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
