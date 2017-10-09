---
title: aaaSending push-meddelanden tooiOS med Azure Notification Hubs | Microsoft Docs
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program."
services: notification-hubs
documentationcenter: ios
keywords: push-meddelande, push-meddelanden, push-meddelanden i ios
author: ysxu
manager: erikre
editor: 
ms.assetid: b7fcd916-8db8-41a6-ae88-fc02d57cb914
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: d8bb47fee4c229b3ed2a7a4dbff25a56a7a7d009
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-tooios-with-azure-notification-hubs"></a>Skicka push-meddelanden tooiOS med Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-ios-get-started).
> 
> 

Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooan iOS-program. Du skapar en tom iOS-app som tar emot push-meddelanden med hjälp av hello [Apple Push Notification service (APNs)](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html). 

När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.

## <a name="before-you-begin"></a>Innan du börjar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

hello slutförts koden för den här självstudiekursen hittar [på GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/iOS/GetStartedNH/GetStarted). 

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* [Mobile Services iOS SDK version 1.2.4]
* Den senaste versionen av [Xcode]
* En enhet som är kompatibel med iOS 8 (eller senare versioner)
* Medlemskap i [Apple Developer Program](https://developer.apple.com/programs/).
  
  > [!NOTE]
  > På grund av konfigurationskrav för push-meddelanden, måste du distribuera och testa push-meddelanden på en fysisk iOS-enhet (iPhone eller iPad) i stället för hello iOS-simulatorn.
  > 
  > 

Du måste slutföra den här självstudiekursen innan du börjar någon annan kurs om Notification Hubs för iOS-appar.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="configure-your-notification-hub-for-ios-push-notifications"></a>Konfigurera din Notification Hub för att skicka push-meddelanden till iOS
Det här avsnittet vägleder dig genom att skapa en ny meddelandehubb och konfigurerar autentisering med APNS med hello **.p12** push-certifikat som du skapade. Om du vill toouse en meddelandehubb som du redan har skapat kan du hoppa över toostep 5.

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li>

<p>Klicka på hello <b>Notification Services</b> knapp i hello <b>inställningar</b> bladet välj <b>Apple (APNS)</b>. Klicka på <b>överför certifikat</b> och välj hello <b>.p12</b> -fil som du exporterade tidigare. Kontrollera att du även ange hello rätt lösenord.</p>

<p>Se till att tooselect <b>Sandbox</b> läge eftersom det är för utveckling. Använd bara hello <b>produktion</b> om du vill toosend push-meddelanden toousers som köpt din app hello butiken.</p>
</li>
</ol>
&emsp;&emsp;&emsp;&emsp;![Konfigurera APN i Azure-portalen](./media/notification-hubs-ios-get-started/notification-hubs-apple-config.png)

&emsp;&emsp;&emsp;&emsp;![Konfigurera APNS-certifikat i Azure-portalen](./media/notification-hubs-ios-get-started/notification-hubs-apple-config-cert.png)

Din meddelandehubb är nu konfigurerad toowork med APNS och du har hello anslutning strängar tooregister din app och skicka push-meddelanden.

## <a name="connect-your-ios-app-toonotification-hubs"></a>Ansluta tooNotification din iOS-app hub
1. Skapa ett nytt iOS-projekt i Xcode och välj hello **program enkel vy** mall.
   
    ![Xcode – Program enkel vy][8]
    
2. När du anger hello alternativ för ditt nya projekt, kontrollera toouse hello samma **produktnamn** och **organisations-ID** som du använde när du har definierat hello paket-ID för hello Apple-utvecklare portalen.
   
    ![Xcode – projektalternativ][11]
    
3. Under **mål**, klicka på ditt projektnamn, hello **Versionsinställningar** fliken och expandera **identitet för kodsignering**, och sedan under **felsöka**, ange din identitet för kodsignering. Växla **nivåer** från **grundläggande** för**alla**, och ange **Etableringsprofil** toohello etableringsprofil som du skapade tidigare .
   
    Om du inte ser hello nya etableringsprofil som du skapade i Xcode, försök att uppdatera hello profiler för din signeringsidentitet. Klicka på **Xcode** hello menyraden klickar du på **inställningar**, klickar du på hello **konto** klickar du på hello **visa information om** , klicka på din signering identitet och klicka sedan på hello uppdateringsknappen i hello nedre högra hörnet.
   
    ![Xcode – etableringsprofil][9]
4. Hämta hello [Mobile Services iOS SDK version 1.2.4] och packa hello-filen. Högerklicka på ditt projekt i Xcode och klicka på hello **Lägg till filer i** alternativet tooadd hello **WindowsAzureMessaging.framework** mappen tooyour Xcode-projekt. Välj **Kopiera objekt vid behov** och klicka sedan på **Lägg till**.
   
   > [!NOTE]
   > hello notification hubs SDK inte stöder för närvarande bitcode på Xcode 7.  Du måste ange **aktivera Bitcode** för**nr** i hello **Versionsalternativ** för projektet.
   > 
   > 
   
    ![Packa upp Azure SDK][10]
5. Lägg till ett nytt huvud filen tooyour projekt med namnet `HubInfo.h`. Den här filen innehåller hello konstanterna för din meddelandehubb.  Lägg till hello följande definitioner och Ersätt hello-platshållarnas textsträngar med ditt *hubbnamn* och hello *DefaultListenSharedAccessSignature* du antecknade tidigare.
   
        #ifndef HubInfo_h
        #define HubInfo_h
   
            #define HUBNAME @"<Enter hello name of your hub>"
            #define HUBLISTENACCESS @"<Enter your DefaultListenSharedAccess connection string"
   
        #endif /* HubInfo_h */
6. Öppna din `AppDelegate.h` filen Lägg till följande importeringsdirektiv hello:
   
         #import <WindowsAzureMessaging/WindowsAzureMessaging.h> 
         #import "HubInfo.h"
7. I din `AppDelegate.m file`, Lägg till följande kod i hello hello `didFinishLaunchingWithOptions` metod baserat på din iOS-version. Den här koden registrerar din enhetshantering med APNS:
   
    För iOS 8:
   
         UIUserNotificationSettings *settings = [UIUserNotificationSettings settingsForTypes:UIUserNotificationTypeSound |
                                                UIUserNotificationTypeAlert | UIUserNotificationTypeBadge categories:nil];
   
        [[UIApplication sharedApplication] registerUserNotificationSettings:settings];
        [[UIApplication sharedApplication] registerForRemoteNotifications];
   
    För iOS-versioner tidigare too8:
   
         [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound];
8. I hello samma fil, Lägg till hello följande metoder. Den här koden ansluter toohello meddelandehubben genom att använda hello anslutningsinformation som du angav i HubInfo.h. Den lämnar sedan hello enhet token toohello meddelandehubben så att hello ska kunna skicka meddelanden:
   
        - (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken {
            SBNotificationHub* hub = [[SBNotificationHub alloc] initWithConnectionString:HUBLISTENACCESS
                                        notificationHubPath:HUBNAME];
   
            [hub registerNativeWithDeviceToken:deviceToken tags:nil completion:^(NSError* error) {
                if (error != nil) {
                    NSLog(@"Error registering for notifications: %@", error);
                }
                else {
                    [self MessageBox:@"Registration Status" message:@"Registered"];
                }
            }];
        }
   
        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }
9. Hej samma fil, lägger du till följande metod toodisplay hello i en **UIAlert** om hello meddelandet tas emot medan hello appen är aktiv:

        - (void)application:(UIApplication *)application didReceiveRemoteNotification: (NSDictionary *)userInfo {
            NSLog(@"%@", userInfo);
            [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]];
        }

1. Skapa och köra hello-app på din enhet tooverify att det inte finns några fel.

## <a name="send-test-push-notifications"></a>Skicka test-push-meddelanden
Du kan testa att ta emot meddelanden i appen genom att skicka push-meddelanden i hello [Azure Portal] via hello **felsökning** avsnitt i hello hubbladet (Använd hello *prova att skicka* alternativet).

![Azure Portal – Prova att skicka][30]

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-from-hello-app"></a>(Valfritt) Skicka push-meddelanden från hello app
> [!IMPORTANT]
> Det här exemplet för att skicka meddelanden från hello-klientappen tillhandahålls endast för. Eftersom detta kräver hello `DefaultFullSharedAccessSignature` toobe finns på hello-klientappen, visar notification hub toohello risken att en användare kan få åtkomst till toosend obehörig meddelanden tooyour klienter.
> 
> 

Om du vill toosend push-meddelanden från en app kan det här avsnittet innehåller ett exempel på hur toodo detta med hjälp av hello REST-gränssnittet.

1. I Xcode öppnar `Main.storyboard` och Lägg till följande UI-komponenter från hello objektet biblioteket tooallow hello användaren toosend push-meddelanden i appen hello hello:
   
   * En etikett utan etikettext. Den kommer att använda tooreport fel i skicka meddelanden. Hej **rader** egenskapen ska anges för**0** så att den automatiskt kommer att storleksändras begränsad toohello högra och vänstra marginalerna samt hello överkant hello vyn.
   * Ett textfält med **platshållare** ange text för**ange meddelande**. Begränsa hello fältet precis under hello etiketten som visas nedan. Ange hello View Controller som uttagsdelegat hello.
   * En knapp med titeln **skicka meddelande** begränsad precis under textfältet hello och hello vågräta center.
     
     hello vyn bör se ut så här:
     
     ![Xcode-designern][32]
2. [Lägg till uttag](https://developer.apple.com/library/ios/recipes/xcode_help-IB_connections/chapters/CreatingOutlet.html) för hello etikett och texten är anslutna till din vy och uppdatera din `interface` definition toosupport `UITextFieldDelegate` och `NSXMLParserDelegate`. Lägg till hello tre egenskapsdeklarationer nedan toohelp stöd anropar hello REST-API och parsning hello-svar.
   
    Filen ViewController.h bör se ut så här:
   
        #import <UIKit/UIKit.h>
   
        @interface ViewController : UIViewController <UITextFieldDelegate, NSXMLParserDelegate>
        {
            NSXMLParser *xmlParser;
        }
   
        // Make sure these outlets are connected tooyour UI by ctrl+dragging
        @property (weak, nonatomic) IBOutlet UITextField *notificationMessage;
        @property (weak, nonatomic) IBOutlet UILabel *sendResults;
   
        @property (copy, nonatomic) NSString *statusResult;
        @property (copy, nonatomic) NSString *currentElement;
   
        @end
3. Öppna `HubInfo.h` och Lägg till följande konstanter som ska användas för att skicka meddelanden tooyour hubb hello. Ersätt hello platshållarens teckensträng med den faktiska *DefaultFullSharedAccessSignature* anslutningssträngen.
   
        #define API_VERSION @"?api-version=2015-01"
        #define HUBFULLACCESS @"<Enter Your DefaultFullSharedAccess Connection string>"
4. Lägg till följande hello `#import` instruktioner tooyour `ViewController.h` fil.
   
        #import <CommonCrypto/CommonHMAC.h>
        #import "HubInfo.h"
5. I `ViewController.m` Lägg till följande kod toohello implementeringsgränssnittet hello. Den här koden kommer att parsa anslutningssträngen *DefaultFullSharedAccessSignature*. Som anges i hello [REST API-referensen](http://msdn.microsoft.com/library/azure/dn495627.aspx), denna parsade information kommer att använda toogenerate en SaS-token för hello **auktorisering** huvudet i begäran.
   
        NSString *HubEndpoint;
        NSString *HubSasKeyName;
        NSString *HubSasKeyValue;
   
        -(void)ParseConnectionString
        {
            NSArray *parts = [HUBFULLACCESS componentsSeparatedByString:@";"];
            NSString *part;
   
            if ([parts count] != 3)
            {
                NSException* parseException = [NSException exceptionWithName:@"ConnectionStringParseException"
                    reason:@"Invalid full shared access connection string" userInfo:nil];
   
                @throw parseException;
            }
   
            for (part in parts)
            {
                if ([part hasPrefix:@"Endpoint"])
                {
                    HubEndpoint = [NSString stringWithFormat:@"https%@",[part substringFromIndex:11]];
                }
                else if ([part hasPrefix:@"SharedAccessKeyName"])
                {
                    HubSasKeyName = [part substringFromIndex:20];
                }
                else if ([part hasPrefix:@"SharedAccessKey"])
                {
                    HubSasKeyValue = [part substringFromIndex:16];
                }
            }
        }
6. I `ViewController.m`, uppdatera hello `viewDidLoad` metoden tooparse hello anslutningssträngen när hello vyn läses in. Lägg även till hello verktygsmetoderna, som visas nedan, toohello implementeringsgränssnittet.  

        - (void)viewDidLoad
        {
            [super viewDidLoad];
            [self ParseConnectionString];
            [_notificationMessage setDelegate:self];
        }

        -(NSString *)CF_URLEncodedString:(NSString *)inputString
        {
           return (__bridge NSString *)CFURLCreateStringByAddingPercentEscapes(NULL, (CFStringRef)inputString,
                NULL, (CFStringRef)@"!*'();:@&=+$,/?%#[]", kCFStringEncodingUTF8);
        }

        -(void)MessageBox:(NSString *)title message:(NSString *)messageText
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
                cancelButtonTitle:@"OK" otherButtonTitles: nil];
            [alert show];
        }





1. I `ViewController.m`, Lägg till följande kod toohello gränssnittet implementering toogenerate hello SaS-autentiseringstoken som ska anges i hello hello **auktorisering** rubriken, som anges i hello [REST API Referens för](http://msdn.microsoft.com/library/azure/dn495627.aspx).
   
        -(NSString*) generateSasToken:(NSString*)uri
        {
            NSString *targetUri;
            NSString* utf8LowercasedUri = NULL;
            NSString *signature = NULL;
            NSString *token = NULL;
   
            @try
            {
                // Add expiration
                uri = [uri lowercaseString];
                utf8LowercasedUri = [self CF_URLEncodedString:uri];
                targetUri = [utf8LowercasedUri lowercaseString];
                NSTimeInterval expiresOnDate = [[NSDate date] timeIntervalSince1970];
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60;
                UInt64 expires = trunc(expiresOnDate);
                NSString* toSign = [NSString stringWithFormat:@"%@\n%qu", targetUri, expires];
   
                // Get an hmac_sha1 Mac instance and initialize with hello signing key
                const char *cKey  = [HubSasKeyValue cStringUsingEncoding:NSUTF8StringEncoding];
                const char *cData = [toSign cStringUsingEncoding:NSUTF8StringEncoding];
                unsigned char cHMAC[CC_SHA256_DIGEST_LENGTH];
                CCHmac(kCCHmacAlgSHA256, cKey, strlen(cKey), cData, strlen(cData), cHMAC);
                NSData *rawHmac = [[NSData alloc] initWithBytes:cHMAC length:sizeof(cHMAC)];
                signature = [self CF_URLEncodedString:[rawHmac base64EncodedStringWithOptions:0]];
   
                // Construct authorization token string
                token = [NSString stringWithFormat:@"SharedAccessSignature sig=%@&se=%qu&skn=%@&sr=%@",
                    signature, expires, HubSasKeyName, targetUri];
            }
            @catch (NSException *exception)
            {
                [self MessageBox:@"Exception Generating SaS Token" message:[exception reason]];
            }
            @finally
            {
                if (utf8LowercasedUri != NULL)
                    CFRelease((CFStringRef)utf8LowercasedUri);
                if (signature != NULL)
                CFRelease((CFStringRef)signature);
            }
   
            return token;
        }
2. CTRL -dra från hello **skicka meddelande** knappen för`ViewController.m` tooadd en åtgärd med namnet **SendNotificationMessage** för hello **Touch Down** händelse. Uppdatera metoden med hello följande kod toosend hello meddelanden med hello REST API.
   
        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";
            [self SendNotificationRESTAPI];
        }
   
        - (void)SendNotificationRESTAPI
        {
            NSURLSession* session = [NSURLSession
                             sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                             delegate:nil delegateQueue:nil];
   
            // Apple Notification format of hello notification message
            NSString *json = [NSString stringWithFormat:@"{\"aps\":{\"alert\":\"%@\"}}",
                                self.notificationMessage.text];
   
            // Construct hello message's REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                                HUBNAME, API_VERSION]];
   
            // Generate hello token toobe used in hello authorization header
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello APNs notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            // Signify Apple notification format
            [request setValue:@"apple" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
            {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (error || (httpResponse.statusCode != 200 && httpResponse.statusCode != 201))
                {
                    NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                }
                if (data != NULL)
                {
                    xmlParser = [[NSXMLParser alloc] initWithData:data];
                    [xmlParser setDelegate:self];
                       [xmlParser parse];
                }
            }];
            [dataTask resume];
        }
3. I `ViewController.m`, lägga till hello följande ombud metoden toosupport stänger hello tangentbordet för textfältet hello. CTRL -dra från hello text fältet toohello View Controller ikonen i hello gränssnittet designer tooset hello visa controller som uttagsdelegat hello.
   
        //===[ Implement UITextFieldDelegate methods ]===
   
        -(BOOL)textFieldShouldReturn:(UITextField *)textField
        {
            [textField resignFirstResponder];
            return YES;
        }
4. I `ViewController.m`, Lägg till följande hello delegera metoder toosupport tolkning hello svaret med hjälp av `NSXMLParser`.
   
       //===[ Implement NSXMLParserDelegate methods ]===
   
       -(void)parserDidStartDocument:(NSXMLParser *)parser
       {
           self.statusResult = @"";
       }
   
       -(void)parser:(NSXMLParser *)parser didStartElement:(NSString *)elementName
           namespaceURI:(NSString *)namespaceURI qualifiedName:(NSString *)qName
           attributes:(NSDictionary *)attributeDict
       {
           NSString * element = [elementName lowercaseString];
           NSLog(@"*** New element parsed : %@ ***",element);
   
           if ([element isEqualToString:@"code"] | [element isEqualToString:@"detail"])
           {
               self.currentElement = element;
           }
       }
   
       -(void) parser:(NSXMLParser *)parser foundCharacters:(NSString *)parsedString
       {
           self.statusResult = [self.statusResult stringByAppendingString:
               [NSString stringWithFormat:@"%@ : %@\n", self.currentElement, parsedString]];
       }
   
       -(void)parserDidEndDocument:(NSXMLParser *)parser
       {
           // Set hello status label text on hello UI thread
           dispatch_async(dispatch_get_main_queue(),
           ^{
               [self.sendResults setText:self.statusResult];
           });
       }
5. Skapa hello projektet och kontrollera att det inte finns några fel.

> [!NOTE]
> Om du får ett build-fel i Xcode7 om bitcode-support, bör du ändra hello **Versionsinställningar** > **aktivera Bitcode (ENABLE_BITCODE)** för**nr** i Xcode. hello Notification Hubs SDK stöder för närvarande inte bitcode. 
> 
> 

Du hittar alla hello möjliga nyttolaster för meddelanden i hello Apple [Push Notification Programmeringsguide för lokala och].

## <a name="checking-if-your-app-can-receive-push-notifications"></a>Kontrollera om din app kan ta emot push-meddelanden
tootest push-meddelanden på iOS, måste du distribuera hello app tooa fysisk iOS-enhet. Du kan inte skicka Apple push-meddelanden med hjälp av hello iOS-simulatorn.

1. Kör hello appen och verifiera att registreringen lyckas och tryck sedan på **OK**.
   
    ![Registreringstest för push-meddelanden i iOS-appar][33]
2. Du kan skicka ett test-push-meddelande från hello [Azure Portal], enligt beskrivningen ovan. Om du har lagt till kod för att skicka push-meddelanden i appen hello touch inuti hello text fältet tooenter ett meddelande. Tryck på hello **skicka** knappen på tangentbordet hello eller hello **skicka meddelande** knapp i hello visa toosend hello-meddelande.
   
    ![Test av att skicka push-meddelanden i iOS-appar][34]
3. hello push-meddelanden skickas tooall-enheter som är registrerade tooreceive hello meddelanden från hello viss Notification Hub.
   
    ![Test av att ta emot push-meddelanden i iOS-appar][35]

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickade du push-meddelanden tooall dina registrerade iOS-enheter. Vi rekommenderar att som ett nästa steg i din utbildning du går vidare toohello [Azure Notification Hubs Meddelandeanvändare för iOS med .NET-serverdel] kursen vägleder dig genom att skapa en serverdel toosend push-meddelanden med taggar. 

Om du vill toosegment in användarna efter intressegrupper, kan du även gå på toohello [använda Notification Hubs toosend senaste nytt] kursen. 

Allmän information om Notification Hubs finns i [Riktlinjer om Notification Hubs].

<!-- Images. -->

[6]: ./media/notification-hubs-ios-get-started/notification-hubs-configure-ios.png
[8]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app.png
[9]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app2.png
[10]: ./media/notification-hubs-ios-get-started/notification-hubs-create-ios-app3.png
[11]: ./media/notification-hubs-ios-get-started/notification-hubs-xcode-product-name.png

[30]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png

[31]: ./media/notification-hubs-ios-get-started/notification-hubs-ios-ui.png
[32]: ./media/notification-hubs-ios-get-started/notification-hubs-storyboard-view.png
[33]: ./media/notification-hubs-ios-get-started/notification-hubs-test1.png
[34]: ./media/notification-hubs-ios-get-started/notification-hubs-test2.png
[35]: ./media/notification-hubs-ios-get-started/notification-hubs-test3.png



<!-- URLs. -->
[Mobile Services iOS SDK version 1.2.4]: http://aka.ms/kymw2g
[Mobile Services iOS SDK]: http://go.microsoft.com/fwLink/?LinkID=266533
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-ios
[Azure Classic Portal]: https://manage.windowsazure.com/
[Riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[iOS Provisioning Portal]: http://go.microsoft.com/fwlink/p/?LinkId=272456

[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-ios-get-started-push.md
[Azure Notification Hubs Meddelandeanvändare för iOS med .NET-serverdel]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-ios-xplat-segmented-apns-push-notification.md

[Push Notification Programmeringsguide för lokala och]: http://developer.apple.com/library/mac/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW1
[Azure Portal]: https://portal.azure.com
