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
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="e2890-104">Azure Notification Hubs säker Push</span><span class="sxs-lookup"><span data-stu-id="e2890-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e2890-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="e2890-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="e2890-106">iOS</span><span class="sxs-lookup"><span data-stu-id="e2890-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="e2890-107">Android</span><span class="sxs-lookup"><span data-stu-id="e2890-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="e2890-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="e2890-108">Overview</span></span>
<span data-ttu-id="e2890-109">Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push-infrastruktur som förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.</span><span class="sxs-lookup"><span data-stu-id="e2890-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="e2890-110">På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="e2890-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="e2890-111">Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klientenheter och hello-appserverdelen.</span><span class="sxs-lookup"><span data-stu-id="e2890-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="e2890-112">På en hög nivå är hello flödet följande:</span><span class="sxs-lookup"><span data-stu-id="e2890-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="e2890-113">hello appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="e2890-113">hello app back-end:</span></span>
   * <span data-ttu-id="e2890-114">Lagrar säker nyttolast i backend-databas.</span><span class="sxs-lookup"><span data-stu-id="e2890-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="e2890-115">Skickar hello-ID för meddelande toohello enheten (ingen säker information skickas).</span><span class="sxs-lookup"><span data-stu-id="e2890-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="e2890-116">hello appen på hello enhet, när du tar emot hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="e2890-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="e2890-117">hello enheten kontaktar hello backend-begärande hello säker nyttolast.</span><span class="sxs-lookup"><span data-stu-id="e2890-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="e2890-118">hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="e2890-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="e2890-119">Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="e2890-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="e2890-120">Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token.</span><span class="sxs-lookup"><span data-stu-id="e2890-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="e2890-121">Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello-meddelande visa ett allmänt meddelande fråga hello användaren toolaunch hello app.</span><span class="sxs-lookup"><span data-stu-id="e2890-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="e2890-122">hello app sedan autentiserar hello användare och visar hello notification nyttolast.</span><span class="sxs-lookup"><span data-stu-id="e2890-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="e2890-123">Den här säkra Push-kursen visar hur toosend ett push-meddelande på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="e2890-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="e2890-124">hello kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först.</span><span class="sxs-lookup"><span data-stu-id="e2890-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-ios-apple-apns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="e2890-125">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e2890-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (iOS)](notification-hubs-ios-apple-push-notification-apns-get-started.md).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-ios-project"></a><span data-ttu-id="e2890-126">Ändra hello iOS-projekt</span><span class="sxs-lookup"><span data-stu-id="e2890-126">Modify hello iOS project</span></span>
<span data-ttu-id="e2890-127">Nu när du har ändrat din app backend-toosend bara hello *id* av ett meddelande du har toochange din iOS-app toohandle att meddelanden och motringning backend-tooretrieve-hello säkra meddelandet toobe visas.</span><span class="sxs-lookup"><span data-stu-id="e2890-127">Now that you modified your app back-end toosend just hello *id* of a notification, you have toochange your iOS app toohandle that notification and call back your back-end tooretrieve hello secure message toobe displayed.</span></span>

<span data-ttu-id="e2890-128">tooachieve det här målet, har vi toowrite hello logik tooretrieve hello skyddat innehåll från hello appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="e2890-128">tooachieve this goal, we have toowrite hello logic tooretrieve hello secure content from hello app back-end.</span></span>

1. <span data-ttu-id="e2890-129">I **AppDelegate.m**, gör att hello app register för tyst meddelanden så att den bearbetar hello meddelande-id som skickades från hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="e2890-129">In **AppDelegate.m**, make sure hello app registers for silent notifications so it processes hello notification id sent from hello backend.</span></span> <span data-ttu-id="e2890-130">Lägg till hello **UIRemoteNotificationTypeNewsstandContentAvailability** alternativ i didFinishLaunchingWithOptions:</span><span class="sxs-lookup"><span data-stu-id="e2890-130">Add hello **UIRemoteNotificationTypeNewsstandContentAvailability** option in didFinishLaunchingWithOptions:</span></span>
   
        [[UIApplication sharedApplication] registerForRemoteNotificationTypes: UIRemoteNotificationTypeAlert | UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeNewsstandContentAvailability];
2. <span data-ttu-id="e2890-131">I din **AppDelegate.m** lägga till en implementeringsavsnittet överst hello med hello följande deklaration:</span><span class="sxs-lookup"><span data-stu-id="e2890-131">In your **AppDelegate.m** add an implementation section at hello top with hello following declaration:</span></span>
   
        @interface AppDelegate ()
        - (void) retrieveSecurePayloadWithId:(int)payloadId completion: (void(^)(NSString*, NSError*)) completion;
        @end
3. <span data-ttu-id="e2890-132">Lägg sedan till i hello implementering avsnittet hello följande kod, ersätter hello platshållare `{back-end endpoint}` med hello-slutpunkten för din serverdel hämtas tidigare:</span><span class="sxs-lookup"><span data-stu-id="e2890-132">Then add in hello implementation section hello following code, substituting hello placeholder `{back-end endpoint}` with hello endpoint for your back-end obtained previously:</span></span>

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

1. <span data-ttu-id="e2890-133">Nu vi har toohandle hello inkommande meddelande och använder metoden hello ovan tooretrieve hello innehåll toodisplay.</span><span class="sxs-lookup"><span data-stu-id="e2890-133">Now we have toohandle hello incoming notification and use hello method above tooretrieve hello content toodisplay.</span></span> <span data-ttu-id="e2890-134">Först har vi tooenable toorun din iOS-app i hello bakgrunden när du får ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="e2890-134">First, we have tooenable your iOS app toorun in hello background when receiving a push notification.</span></span> <span data-ttu-id="e2890-135">I **XCode**, Välj app-projekt på hello vänstra panelen och klicka på mål-huvudsakliga app i hello **mål** avsnittet från central hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="e2890-135">In **XCode**, select your app project on hello left panel, then click your main app target in hello **Targets** section from hello central pane.</span></span>
2. <span data-ttu-id="e2890-136">Klicka på din **funktioner** hello överst i din centrala rutan och kontrollera hello **Remote Notifications** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e2890-136">Then click your **Capabilities** tab at hello top of your central pane, and check hello **Remote Notifications** checkbox.</span></span>
   
    ![][IOS1]
3. <span data-ttu-id="e2890-137">I **AppDelegate.m** lägga till hello följa metoden toohandle push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="e2890-137">In **AppDelegate.m** add hello following method toohandle push notifications:</span></span>
   
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
   
    <span data-ttu-id="e2890-138">Observera att det är bättre toohandle hello fall egenskap som saknas autentisering sidhuvud eller avvisning hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="e2890-138">Note that it is preferable toohandle hello cases of missing authentication header property or rejection by hello back-end.</span></span> <span data-ttu-id="e2890-139">hello Särskild hantering av dessa fall beror huvudsakligen på användarupplevelsen mål.</span><span class="sxs-lookup"><span data-stu-id="e2890-139">hello specific handling of these cases depend mostly on your target user experience.</span></span> <span data-ttu-id="e2890-140">Ett alternativ är toodisplay ett meddelande med en allmän fråga om hello tooauthenticate tooretrieve hello faktiska användarmeddelande.</span><span class="sxs-lookup"><span data-stu-id="e2890-140">One option is toodisplay a notification with a generic prompt for hello user tooauthenticate tooretrieve hello actual notification.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="e2890-141">Kör hello program</span><span class="sxs-lookup"><span data-stu-id="e2890-141">Run hello Application</span></span>
<span data-ttu-id="e2890-142">toorun Hej program, hello följande:</span><span class="sxs-lookup"><span data-stu-id="e2890-142">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="e2890-143">Kör hello app i XCode på en fysisk iOS-enhet (push-meddelanden inte fungerar i hello simulator).</span><span class="sxs-lookup"><span data-stu-id="e2890-143">In XCode, run hello app on a physical iOS device (push notifications will not work in hello simulator).</span></span>
2. <span data-ttu-id="e2890-144">Ange ett användarnamn och lösenord i hello iOS app Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="e2890-144">In hello iOS app UI, enter a username and password.</span></span> <span data-ttu-id="e2890-145">Dessa kan vara valfri sträng, men de måste vara hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="e2890-145">These can be any string, but they must be hello same value.</span></span>
3. <span data-ttu-id="e2890-146">I hello iOS-app UI, klickar du på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="e2890-146">In hello iOS app UI, click **Log in**.</span></span> <span data-ttu-id="e2890-147">Klicka på **skicka push**.</span><span class="sxs-lookup"><span data-stu-id="e2890-147">Then click **Send push**.</span></span> <span data-ttu-id="e2890-148">Du bör se hello säkert meddelande som visas i din meddelandecentret.</span><span class="sxs-lookup"><span data-stu-id="e2890-148">You should see hello secure notification being displayed in your notification center.</span></span>

[IOS1]: ./media/notification-hubs-aspnet-backend-ios-secure-push/secure-push-ios-1.png
