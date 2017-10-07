---
title: aaaAzure Notification Hubs Secure Push
description: "Lär dig hur säker toosend push-meddelanden tooan iOS-app från Azure. Kodexempel som skrivits i Objective-C och C#."
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 17d42b0a-2c80-4e35-a1ed-ed510d19f4b4
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 86dd8d7042e5b9e55d2d7ff41cb42f23831fc575
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs säker Push
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Översikt
Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push-infrastruktur som förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.

På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur. Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klientenheter och hello-appserverdelen.

På en hög nivå är hello flödet följande:

1. hello appens serverdel:
   * Lagrar säker nyttolast i backend-databas.
   * Skickar hello-ID för meddelande toohello enheten (ingen säker information skickas).
2. hello appen på hello enhet, när du tar emot hello-meddelande:
   * hello enheten kontaktar hello backend-begärande hello säker nyttolast.
   * hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.

Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in. Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token. Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello-meddelande visa ett allmänt meddelande fråga hello användaren toolaunch hello app. hello app sedan autentiserar hello användare och visar hello notification nyttolast.

Den här säkra Push-kursen visar hur toosend ett push-meddelande på ett säkert sätt. hello kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först.

> [!NOTE]
> Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a>Ändra hello iOS-projekt
Nu när du har ändrat din app backend-toosend bara hello *id* av ett meddelande du har toochange din iOS-app toohandle att meddelanden och motringning backend-tooretrieve-hello säkra meddelandet toobe visas.

tooachieve det här målet, har vi toowrite hello logik tooretrieve hello skyddat innehåll från hello appens serverdel.

1. I **AppDelegate.m**, gör att hello app register för tyst meddelanden så att den bearbetar hello meddelande-id som skickades från hello serverdel. Lägg till hello **UIRemoteNotificationTypeNewsstandContentAvailability** alternativ i didFinishLaunchingWithOptions:
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. I din **AppDelegate.m** lägga till en implementeringsavsnittet överst hello med hello följande deklaration:
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. Lägg sedan till i hello implementering avsnittet hello följande kod, ersätter hello platshållare `{back-end endpoint}` med hello-slutpunkten för din serverdel hämtas tidigare:

```
        NSString *const GetNotificationEndpoint = @"{back-end endpoint}/api/notifications";

        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        {
            // check if authenticated
            ANHViewController* rvc = (ANHViewController*) self.window.rootViewController;
            NSString* authenticationHeader = rvc.registerClient.authenticationHeader;
            if (!authenticationHeader) return;


            NSURLSession* session = [NSURLSession
                                     sessionWithConfiguration:[NSURLSessionConfiguration defaultSessionConfiguration]
                                     delegate:nil
                                     delegateQueue:nil];


            NSURL* requestURL = [NSURL URLWithString:[NSString stringWithFormat:@"%@/%d", GetNotificationEndpoint, payloadId]];
            NSMutableURLRequest* request = [NSMutableURLRequest requestWithURL:requestURL];
            [request setHTTPMethod:@"GET"];
            NSString* authorizationHeaderValue = [NSString stringWithFormat:@"Basic %@", authenticationHeader];
            [request setValue:authorizationHeaderValue forHTTPHeaderField:@"Authorization"];

            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
                NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                if (!error && httpResponse.statusCode == 200)
                {
                    NSLog(@"Received secure payload: %@", [[NSString alloc] initWithData:data encoding:NSUTF8StringEncoding]);

                    NSMutableDictionary *json = [NSJSONSerialization JSONObjectWithData:data options: NSJSONReadingMutableContainers error: &error];

                    completion([json objectForKey:@"Payload"], nil);
                }
                else
                {
                    NSLog(@"Error status: %ld, request: %@", (long)httpResponse.statusCode, error);
                    if (error)
                        completion(nil, error);
                    else {
                        completion(nil, [NSError errorWithDomain:@"APICall" code:httpResponse.statusCode userInfo:nil]);
                    }
                }
            }];
            [dataTask resume];
        }
```

    This method calls your app back-end tooretrieve hello notification content using hello credentials stored in hello shared preferences.

1. Nu vi har toohandle hello inkommande meddelande och använder metoden hello ovan tooretrieve hello innehåll toodisplay. Först har vi tooenable toorun din iOS-app i hello bakgrunden när du får ett push-meddelande. I **XCode**, Välj app-projekt på hello vänstra panelen och klicka på mål-huvudsakliga app i hello **mål** avsnittet från central hello-fönstret.
2. Klicka på din **funktioner** hello överst i din centrala rutan och kontrollera hello **Remote Notifications** kryssrutan.
   
    ![][IOS1]
3. I **AppDelegate.m** lägga till hello följa metoden toohandle push-meddelanden:
   
        -(void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler
        {
            NSLog(@"%@", userInfo);
   
            [self retrieveSecurePayloadWithId:[[userInfo objectForKey:@"secureId"] intValue] completion:^(NSString * payload, NSError *error) {
                if (!error) {
                    // show local notification
                    UILocalNotification* localNotification = [[UILocalNotification alloc] init];
                    localNotification.fireDate = [NSDate dateWithTimeIntervalSinceNow:0];
                    localNotification.alertBody = payload;
                    localNotification.timeZone = [NSTimeZone defaultTimeZone];
                    [[UIApplication sharedApplication] scheduleLocalNotification:localNotification];
   
                    completionHandler(UIBackgroundFetchResultNewData);
                } else {
                    completionHandler(UIBackgroundFetchResultFailed);
                }
            }];
   
        }
   
    Observera att det är bättre toohandle hello fall egenskap som saknas autentisering sidhuvud eller avvisning hello serverdel. hello Särskild hantering av dessa fall beror huvudsakligen på användarupplevelsen mål. Ett alternativ är toodisplay ett meddelande med en allmän fråga om hello tooauthenticate tooretrieve hello faktiska användarmeddelande.

## <a name="run-hello-application"></a>Kör hello program
toorun Hej program, hello följande:

1. Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).
2. Ange ett användarnamn och lösenord i hello iOS app Användargränssnittet. Dessa kan vara valfri sträng, men de måste vara hello samma värde.
3. I hello iOS-app UI, klickar du på **logga in**. Klicka på **skicka push**. Du bör se hello säkert meddelande som visas i din meddelandecentret.

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
