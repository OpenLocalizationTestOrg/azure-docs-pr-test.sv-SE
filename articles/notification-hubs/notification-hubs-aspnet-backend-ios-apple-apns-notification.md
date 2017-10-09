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
# <a name="azure-notification-hubs-notify-users-for-ios-with-net-backend"></a>Meddela användare för iOS med .NET-serverdel i Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Översikt
Stöd för push-meddelanden i Azure kan du tooaccess ett enkelt att använda och multiplatform skalats ut push-infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar. Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa specifik app användare på en specifik enhet. En ASP.NET-WebAPI-serverdel är används tooauthenticate klienter och toogenerate meddelanden som visas i hello vägledning avsnittet [registrering från din Apps serverdel](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend).

> [!NOTE]
> Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md). Den här kursen är också hello nödvändiga toohello [Secure Push (iOS)](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md) kursen.
> Om du vill toouse Mobile Apps som backend-tjänst finns hello [Mobile Apps Kom igång med Push](../app-service-mobile/app-service-mobile-ios-get-started-push.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="modify-your-ios-app"></a>Ändra din iOS-app
1. Öppna hello sida visa app som du skapade i hello [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kursen.
   
   > [!NOTE]
   > I det här avsnittet förutsätter att projektet har konfigurerats med ett tomt organisationsnamn. Om du inte behöver du tooprepend din organisation namn tooall klassnamn.
   > 
   > 
2. Lägg till hello-komponenter som visas i hello skärmbilden nedan från objektbiblioteket för hello i din Main.storyboard.
   
    ![][1]
   
   * **Användarnamnet**: A UITextField med platshållartext, *ange användarnamn*genast skickar resultaten etikett och begränsad toohello kvar under hello och högerklicka marginaler och under hello skicka resultaten etikett.
   * **Lösenordet**: A UITextField med platshållartext, *ange lösenord för*, omedelbart innanför hello användarnamn textfältet och begränsad toohello vänster och höger marginaler och under hello användarnamn textfältet. Kontrollera hello **Secure textinmatning** alternativet i hello attributet Inspector under *returnera nyckeln*.
   * **Logga in**: A UIButton etikett direkt under hello lösenordsfältet och avmarkerar hello **aktiverad** alternativet i hello attribut Inspector under *kontrollens innehåll*
   * **WNS**: etikett och växel tooenable skicka hello notification Windows Notification Service om den har konfigurerats på hello hubben. Se hello [Windows komma igång](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kursen.
   * **GCM**: etikett och växel tooenable skicka hello meddelande tooGoogle Cloud Messaging om den har konfigurerats på hello hubben. Se [Android igång](notification-hubs-android-push-notification-google-gcm-get-started.md) kursen.
   * **APN**: etikett och växel tooenable skicka hello meddelande toohello Apple Platform Notification Service.
   * **Recipent Username**: A UITextField med platshållartext, *mottagaren användarnamn taggen*omedelbart under hello GCM etikett och begränsad toohello vänster och höger marginaler och under hello GCM etiketten.

    Vissa komponenter har lagts till i hello [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md) kursen.

1. **CTRL** dra från hello komponenter i hello visa tooViewController.h och Lägg till dessa nya uttag.
   
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
2. Lägg till följande hello i ViewController.h, `#define` under import-instruktioner. Ersätt hello *< Endpoint ange Backend\>*  med hello mål-URL som du använde toodeploy din Apps serverdel hello föregående avsnitt. Till exempel *http://you_backend.azurewebsites.net*.
   
        #define BACKEND_ENDPOINT @"<Enter Your Backend Endpoint>"
3. I projektet, skapa en ny **Cocoa Touch klassen** med namnet **RegisterClient** toointerface med hello ASP.NET-backend du skapat. Skapa hello-klass som ärver från `NSObject`. Lägg sedan till följande kod i hello RegisterClient.h hello.
   
        @interface RegisterClient : NSObject
   
        @property (strong, nonatomic) NSString* authenticationHeader;
   
        -(void) registerWithDeviceToken:(NSData*)token tags:(NSSet*)tags
            andCompletion:(void(^)(NSError*))completion;
   
        -(instancetype) initWithEndpoint:(NSString*)Endpoint;
   
        @end
4. Uppdatera hello i hello RegisterClient.m `@interface` avsnitt:
   
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
5. Ersätt hello `@implementation` avsnitt i hello RegisterClient.m med hello följande kod.

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

    hello koden ovan implementerar hello logik som beskrivs i artikeln hello [registrering från din Apps serverdel](notification-hubs-push-notification-registration-management.md#registration-management-from-a-backend) NSURLSession tooperform REST-anropen tooyour appserverdelen och NSUserDefaults toolocally store hello registrationId som returneras av hello meddelandehubben.

    Observera att den här klassen kräver egenskapen **authorizationHeader** toobe som angetts i order toowork korrekt. Den här egenskapen anges av hello **ViewController** klassen när hello logga in.

1. ViewController.h, lägga till en `#import` -instruktion för RegisterClient.h. Lägger du till en deklaration för hello enhetstoken och referera till tooa `RegisterClient` instans i hello `@interface` avsnitt:
   
        #import "RegisterClient.h"
   
        @property (strong, nonatomic) NSData* deviceToken;
        @property (strong, nonatomic) RegisterClient* registerClient;
2. ViewController.m, lägga till en privat metoddeklaration i hello `@interface` avsnitt:
   
        @interface ViewController () <UITextFieldDelegate, NSURLConnectionDataDelegate, NSXMLParserDelegate>
   
        // create hello Authorization header tooperform Basic authentication with your app back-end
        -(void) createAndSetAuthenticationHeaderWithUsername:(NSString*)username
                        AndPassword:(NSString*)password;
   
        @end

> [!NOTE]
> hello följande kodavsnitt är inte en säker autentiseringsschema, ska du ersätta hello implementering av hello **createAndSetAuthenticationHeaderWithUsername:AndPassword:** med specifika autentiseringsmekanism som genererar en token toobe autentisering som används av hello registrera klientklass, t.ex. OAuth Active Directory.
> 
> 

1. I hello `@implementation` avsnitt i ViewController.m Lägg till följande kod som lägger till hello implementering för inställningen hello enhet token och autentisering huvudet hello.
   
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
   
    Observera hur inställningen hello enhetstoken möjliggör hello inloggningen knappen. Det beror på att som en del av hello inloggningen åtgärden registrerar hello view controller för push-meddelanden med hello-appserverdelen. Därför måste vill vi inte logga In åtgärd toobe tillgänglig tills hello enhetstoken har konfigurerats korrekt. Du kan frikopplar hello logga in från registreringen av push-hello så länge hello f.d. sker innan hello senare.
2. I ViewController.m, använder du följande kodavsnitt tooimplement hello åtgärdsmetod för hello din **loggar In** knapp och en metod toosend hello notification meddelande med hello ASP.NET-serverdel.
   
       - (IBAction) LogInAction: (id) avsändaren {/ / skapa autentiseringshuvud och ange den registrera klienten NSString * användarnamn = self. UsernameField.text;   NSString * lösenord = self. PasswordField.text;
   
           [self createAndSetAuthenticationHeaderWithUsername:username AndPassword:password];
   
           __weak ViewController * selfie = self;   [self.registerClient registerWithDeviceToken:self.deviceToken taggar: nil andCompletion:^(NSError* error) {Om (! fel) {dispatch_async(dispatch_get_main_queue(), ^ {selfie. SendNotificationButton.enabled = Ja.               [self MessageBox:@"Success” message:@"Registered har”!];});}}];}

        -(void) SendNotificationASPNETBackend: (NSString*) pns UsernameTag: (NSString*) usernameTag meddelande: (NSString*) meddelande {NSURLSession* session = [NSURLSession sessionWithConfiguration: [NSURLSessionConfiguration defaultSessionConfiguration] ombud: nil delegateQueue:nil];

            Skicka hello pns och username-tagg som parametrar med hello URL för REST toohello ASP.NET-serverdel NSURL * requestURL = [NSURL URLWithString: [NSString stringWithFormat:@"%@/api/notifications? pns = % @& to_tag = % @”, BACKEND_ENDPOINT pns, usernameTag]];

            NSMutableURLRequest * begäran = [NSMutableURLRequest requestWithURL:requestURL];    [begär setHTTPMethod:@"POST”];

            Hämta hello fingerad authenticationheader från hello registrera klienten NSString * authorizationHeaderValue = [NSString stringWithFormat:@"Basic % @”, self.registerClient.authenticationHeader];    [begär setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization”];

            Lägg till hello-meddelande meddelandetexten [begära setValue:@"application/json;charset=utf-8” forHTTPHeaderField:@"Content-Type”];    [begära setHTTPBody: [meddelande dataUsingEncoding:NSUTF8StringEncoding]];

            Köra hello skicka meddelande REST API på hello ASP.NET-serverdel NSURLSessionDataTask * dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError  *fel) {NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) svar;        Om (fel || httpResponse.statusCode! = 200) {NSString* status = [NSString stringWithFormat:@"Error Status för % @: % d\nError: %@\n”, pns, httpResponse.statusCode, fel];            dispatch_async(dispatch_get_main_queue(), ^ {/ / lägga till text eftersom alla 3 PNS-anrop har också information tooview [self.sendResults setText:[self.sendResults.text stringByAppendingString:status]];            });            NSLog(status);        }

                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                    [xmlParser parse];
                }
            }];    [dataTask återuppta]; }


1. Uppdatera hello åtgärden för hello **skicka meddelande** knappen toouse hello ASP.NET-serverdel och skicka tooany PNS aktiveras med en växel.

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



1. I funktionen **ViewDidLoad**, lägga till hello följande tooinstantiate hello RegisterClient instans och ange hello ombud för textfält.
   
       self.UsernameField.delegate = self;
       self.PasswordField.delegate = self;
       self.RecipientField.delegate = self;
       self.registerClient = [[RegisterClient alloc] initWithEndpoint:BACKEND_ENDPOINT];
2. Nu i **AppDelegate.m**, ta bort alla hello innehåll för hello metoden **program: didRegisterForPushNotificationWithDeviceToken:** och Ersätt den med följande toomake att hello som hello vyn domänkontrollanten innehåller hello senaste enhetstoken som hämtats från APN:
   
       // Add import toohello top of hello file
       #import "ViewController.h"
   
       - (void)application:(UIApplication *)application
                   didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken
       {
           ViewController* rvc = (ViewController*) self.window.rootViewController;
           rvc.deviceToken = deviceToken;
       }
3. Slutligen i **AppDelegate.m**, kontrollera att du har följande metod hello:
   
       - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
           NSLog(@"%@", userInfo);
           [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
       }

## <a name="test-hello-application"></a>Testa hello program
1. Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).
2. Ange ett användarnamn och lösenord i hello iOS app Användargränssnittet. Dessa kan vara valfri sträng, men de måste båda vara hello samma strängvärde. Klicka på **loggar In**.
   
    ![][2]
3. Du bör se ett popup-fönster som informerar dig om registreringen lyckades. Klicka på **OK**.
   
    ![][3]
4. I hello **mottagaren användarnamn taggen* text, ange hello namnet användartaggen används med hello registrering från en annan enhet.
5. Ange ett meddelande och klicka på **skicka meddelande**.  Endast hello-enheter som har en registrering med hello mottagarens namn användartaggen får hello-meddelande.  Den skickas endast toothose användare.
   
    ![][4]

[1]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-interface.png
[2]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-user-pwd.png
[3]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-registered.png
[4]: ./media/notification-hubs-aspnet-backend-ios-notify-users/notification-hubs-ios-notify-users-enter-msg.png
