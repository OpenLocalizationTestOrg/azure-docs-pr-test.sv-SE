---
title: Notification Hubs senaste nyheterna kursen - iOS
description: "Lär dig hur du använder Azure Service Bus Notification Hubs för att skicka senaste nyheterna meddelanden till iOS-enheter."
services: notification-hubs
documentationcenter: ios
author: ysxu
manager: erikre
editor: 
ms.assetid: 6ead4169-deff-4947-858c-8c6cf03cc3b2
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: dc47250db6fb3a2853dae24e02bda236154d93fb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="69f43-103">Använda Notification Hubs för att skicka de senaste nyheterna</span><span class="sxs-lookup"><span data-stu-id="69f43-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="69f43-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="69f43-104">Overview</span></span>
<span data-ttu-id="69f43-105">Det här avsnittet visar hur du använder Azure Notification Hubs för att sända senaste nyheterna meddelanden till en iOS-app.</span><span class="sxs-lookup"><span data-stu-id="69f43-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to an iOS app.</span></span> <span data-ttu-id="69f43-106">När du är klar kommer du att kunna registrera för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="69f43-106">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="69f43-107">Det här scenariot är ett vanligt mönster för många appar där meddelanden har skickas till grupper av användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="69f43-107">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="69f43-108">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69f43-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="69f43-109">När meddelanden skickas till en tagg för tar alla enheter som har registrerats för taggen emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="69f43-109">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="69f43-110">Eftersom taggar är bara strängar, behöver de inte etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="69f43-110">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="69f43-111">Mer information om taggar finns i [Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="69f43-111">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69f43-112">Krav</span><span class="sxs-lookup"><span data-stu-id="69f43-112">Prerequisites</span></span>
<span data-ttu-id="69f43-113">Det här avsnittet bygger på appen som du skapade i [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="69f43-113">This topic builds on the app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="69f43-114">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="69f43-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="69f43-115">Lägg till kategori markeringen i appen</span><span class="sxs-lookup"><span data-stu-id="69f43-115">Add category selection to the app</span></span>
<span data-ttu-id="69f43-116">Det första steget är att lägga till de UI-element i din befintliga storyboard som gör att användaren kan välja kategorier för att registrera.</span><span class="sxs-lookup"><span data-stu-id="69f43-116">The first step is to add the UI elements to your existing storyboard that enable the user to select categories to register.</span></span> <span data-ttu-id="69f43-117">Vilka kategorier av en användare som lagras på enheten.</span><span class="sxs-lookup"><span data-stu-id="69f43-117">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="69f43-118">När appen startar skapas en enhetsregistrering i din meddelandehubb med de valda kategorierna som taggar.</span><span class="sxs-lookup"><span data-stu-id="69f43-118">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="69f43-119">Lägg till följande komponenter från objektbiblioteket i din MainStoryboard_iPhone.storyboard:</span><span class="sxs-lookup"><span data-stu-id="69f43-119">In your MainStoryboard_iPhone.storyboard add the following components from the object library:</span></span>
   
   * <span data-ttu-id="69f43-120">En etikett med ”bryter nyheter” text</span><span class="sxs-lookup"><span data-stu-id="69f43-120">A label with "Breaking News" text,</span></span>
   * <span data-ttu-id="69f43-121">Etiketter med kategori text ”World”, ”politik”, ”företag”, ”teknik”, ”vetenskap”, ”sport”</span><span class="sxs-lookup"><span data-stu-id="69f43-121">Labels with category texts "World", "Politics", "Business", "Technology", "Science", "Sports",</span></span>
   * <span data-ttu-id="69f43-122">Sex växlar en per kategori och ställa in varje switch **tillstånd** ska **av** som standard.</span><span class="sxs-lookup"><span data-stu-id="69f43-122">Six switches, one per category, set each switch **State** to be **Off** by default.</span></span>
   * <span data-ttu-id="69f43-123">En knapp med etiketten ”prenumerera”</span><span class="sxs-lookup"><span data-stu-id="69f43-123">One button labeled "Subscribe"</span></span>
     
     <span data-ttu-id="69f43-124">Storyboard bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="69f43-124">Your storyboard should look as follows:</span></span>
     
     ![][3]
2. <span data-ttu-id="69f43-125">I Redigeraren för assistenten skapa uttag för alla växlar och anropa dem ”WorldSwitch”, ”PoliticsSwitch”, ”BusinessSwitch”, ”TechnologySwitch”, ”ScienceSwitch”, ”SportsSwitch”</span><span class="sxs-lookup"><span data-stu-id="69f43-125">In the assistant editor, create outlets for all the switches and call them "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span></span>
3. <span data-ttu-id="69f43-126">Skapa en åtgärd för en knapp som kallas ”prenumerera”.</span><span class="sxs-lookup"><span data-stu-id="69f43-126">Create an Action for your button called "subscribe".</span></span> <span data-ttu-id="69f43-127">Din ViewController.h bör innehålla följande:</span><span class="sxs-lookup"><span data-stu-id="69f43-127">Your ViewController.h should contain the following:</span></span>
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. <span data-ttu-id="69f43-128">Skapa en ny **Cocoa Touch klassen** kallas `Notifications`.</span><span class="sxs-lookup"><span data-stu-id="69f43-128">Create a new **Cocoa Touch Class** called `Notifications`.</span></span> <span data-ttu-id="69f43-129">Kopiera följande kod i avsnittet gränssnitt i filen Notifications.h:</span><span class="sxs-lookup"><span data-stu-id="69f43-129">Copy the following code in the interface section of the file Notifications.h:</span></span>
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. <span data-ttu-id="69f43-130">Lägg till följande importdirektiv Notifications.m:</span><span class="sxs-lookup"><span data-stu-id="69f43-130">Add the following import directive to Notifications.m:</span></span>
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. <span data-ttu-id="69f43-131">Kopiera följande kod i implementeringsavsnittet av filen Notifications.m.</span><span class="sxs-lookup"><span data-stu-id="69f43-131">Copy the following code in the implementation section of the file Notifications.m.</span></span>
   
        SBNotificationHub* hub;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName{
   
            hub = [[SBNotificationHub alloc] initWithConnectionString:listenConnectionString
                                        notificationHubPath:hubName];
   
            return self;
        }
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];
            [defaults setValue:[categories allObjects] forKey:@"BreakingNewsCategories"];
   
            [self subscribeWithCategories:categories completion:completion];
        }

        - (NSSet*)retrieveCategories {
            NSUserDefaults* defaults = [NSUserDefaults standardUserDefaults];

            NSArray* categories = [defaults stringArrayForKey:@"BreakingNewsCategories"];

            if (!categories) return [[NSSet alloc] init];
            return [[NSSet alloc] initWithArray:categories];
        }


        - (void)subscribeWithCategories:(NSSet *)categories completion:(void (^)(NSError *))completion
        {
           //[hub registerNativeWithDeviceToken:self.deviceToken tags:categories completion: completion];

            NSString* templateBodyAPNS = @"{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            [hub registerTemplateWithDeviceToken:self.deviceToken name:@"simpleAPNSTemplate" 
                jsonBodyTemplate:templateBodyAPNS expiryTemplate:@"0" tags:categories completion:completion];
        }



    <span data-ttu-id="69f43-132">Den här klassen använder lokal lagring för att lagra och hämta kategorier av nyheter som den här enheten ska ta emot.</span><span class="sxs-lookup"><span data-stu-id="69f43-132">This class uses local storage to store and retrieve the categories of news that this device will receive.</span></span> <span data-ttu-id="69f43-133">Den innehåller också en metod för att registrera dig för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registrering.</span><span class="sxs-lookup"><span data-stu-id="69f43-133">Also, it contains a method to register for these categories using a [Template](notification-hubs-templates-cross-platform-push-messages.md) registration.</span></span>

1. <span data-ttu-id="69f43-134">Lägg till en import-sats för Notifications.h i filen AppDelegate.h och lägga till en egenskap för en instans av klassen meddelanden:</span><span class="sxs-lookup"><span data-stu-id="69f43-134">In the AppDelegate.h file, add an import statement for Notifications.h and add a property for an instance of the Notifications class:</span></span>
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. <span data-ttu-id="69f43-135">I den **didFinishLaunchingWithOptions** metod i AppDelegate.m, Lägg till koden för att initiera meddelanden instans i början av metoden.</span><span class="sxs-lookup"><span data-stu-id="69f43-135">In the **didFinishLaunchingWithOptions** method in AppDelegate.m, add the code to initialize the notifications instance at the beginning of the method.</span></span>  
   
    <span data-ttu-id="69f43-136">`HUBNAME`och `HUBLISTENACCESS` (definieras i hubinfo.h) har redan den `<hub name>` och `<connection string with listen access>` platshållare ersättas med namnet på din meddelandehubb och anslutningssträngen för *DefaultListenSharedAccessSignature* som du tidigare hämtade</span><span class="sxs-lookup"><span data-stu-id="69f43-136">`HUBNAME` and `HUBLISTENACCESS` (defined in hubinfo.h) should already have the `<hub name>` and `<connection string with listen access>` placeholders replaced with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier</span></span>
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > <span data-ttu-id="69f43-137">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="69f43-137">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="69f43-138">Lyssna åtkomst aktiverar inte går att ändra din app att registrera för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69f43-138">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="69f43-139">Fullständig åtkomst-nyckeln används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="69f43-139">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
3. <span data-ttu-id="69f43-140">I den **didRegisterForRemoteNotificationsWithDeviceToken** metod i AppDelegate.m, Ersätt Koden i metoden med följande kod för att skicka enhetstoken till klassen meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69f43-140">In the **didRegisterForRemoteNotificationsWithDeviceToken** method in AppDelegate.m, replace the code in the method with the following code to pass the device token to the notifications class.</span></span> <span data-ttu-id="69f43-141">Klassen meddelanden utför registrering för meddelanden med kategorier.</span><span class="sxs-lookup"><span data-stu-id="69f43-141">The notifications class will perform the registering for notifications with the categories.</span></span> <span data-ttu-id="69f43-142">Om användaren ändrar kategori, som vi kallar det `subscribeWithCategories` metod som svar på de **prenumerera** om du vill uppdatera dem.</span><span class="sxs-lookup"><span data-stu-id="69f43-142">If the user changes category selections, we call the `subscribeWithCategories` method in response to the **subscribe** button to update them.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="69f43-143">Eftersom enhetstoken som tilldelats av Apple Push Notification Service (APNS) kan risken när som helst, bör du registrera dig för meddelanden ofta att undvika fel i meddelande.</span><span class="sxs-lookup"><span data-stu-id="69f43-143">Because the device token assigned by the Apple Push Notification Service (APNS) can chance at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="69f43-144">Det här exemplet registrerar för meddelande varje gång appen startar.</span><span class="sxs-lookup"><span data-stu-id="69f43-144">This example registers for notification every time that the app starts.</span></span> <span data-ttu-id="69f43-145">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering för att bevara bandbredd om mindre än en dag har gått sedan den tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="69f43-145">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves the categories from local storage and requests a registration for these categories
        // each time the app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    <span data-ttu-id="69f43-146">Observera att då det bör finnas någon kod i den **didRegisterForRemoteNotificationsWithDeviceToken** metod.</span><span class="sxs-lookup"><span data-stu-id="69f43-146">Note that at this point there should be no other code in the **didRegisterForRemoteNotificationsWithDeviceToken** method.</span></span>

1. <span data-ttu-id="69f43-147">Följande metoder ska redan finnas i AppDelegate.m från att utföra den [Kom igång med Notification Hubs] [ get-started] kursen.</span><span class="sxs-lookup"><span data-stu-id="69f43-147">The following methods should already be present in AppDelegate.m from completing the [Get started with Notification Hubs][get-started] tutorial.</span></span>  <span data-ttu-id="69f43-148">Om du inte lägga till dem.</span><span class="sxs-lookup"><span data-stu-id="69f43-148">If not, add them.</span></span>
   
    <span data-ttu-id="69f43-149">-(void) MessageBox:(NSString *) rubrik meddelandet:(NSString *) messageText {</span><span class="sxs-lookup"><span data-stu-id="69f43-149">-(void)MessageBox:(NSString *)title message:(NSString *)messageText  {</span></span>
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    <span data-ttu-id="69f43-150">}</span><span class="sxs-lookup"><span data-stu-id="69f43-150">}</span></span>
   
   * <span data-ttu-id="69f43-151">(void) programmet:(UIApplication *) program didReceiveRemoteNotification: (NSDictionary *) användarinformationen {NSLog (@ ”% @”, användarinformationen);   [self MessageBox:@"Notification” meddelande: [valueForKey:@"alert [användarinformationen objectForKey:@"aps] ””]]; }</span><span class="sxs-lookup"><span data-stu-id="69f43-151">(void)application:(UIApplication *)application didReceiveRemoteNotification:   (NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]]; }</span></span>
   
   <span data-ttu-id="69f43-152">Den här metoden hanterar meddelanden när appen körs genom att visa en enkel **UIAlert**.</span><span class="sxs-lookup"><span data-stu-id="69f43-152">This method handles notifications received when the app is running by displaying a simple **UIAlert**.</span></span>
2. <span data-ttu-id="69f43-153">Lägg till en import-sats för AppDelegate.h i ViewController.m, och kopiera följande kod i XCode-genererade **prenumerera** metod.</span><span class="sxs-lookup"><span data-stu-id="69f43-153">In ViewController.m, add a import statement for AppDelegate.h and copy the following code into the XCode-generated **subscribe** method.</span></span> <span data-ttu-id="69f43-154">Den här koden kommer att uppdatera registreringen för meddelanden om du vill använda de nya kategori etiketter som användaren har valt i användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="69f43-154">This code will update the notification registration to use the new category tags the user has chosen in the user interface.</span></span>
   
       ```
       #import "Notifications.h"
       ```
   
       NSMutableArray* categories = [[NSMutableArray alloc] init];
   
       if (self.WorldSwitch.isOn) [categories addObject:@"World"];
       if (self.PoliticsSwitch.isOn) [categories addObject:@"Politics"];
       if (self.BusinessSwitch.isOn) [categories addObject:@"Business"];
       if (self.TechnologySwitch.isOn) [categories addObject:@"Technology"];
       if (self.ScienceSwitch.isOn) [categories addObject:@"Science"];
       if (self.SportsSwitch.isOn) [categories addObject:@"Sports"];
   
       Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];
   
       [notifications storeCategoriesAndSubscribeWithCategories:categories completion: ^(NSError* error) {
           if (!error) {
               [(AppDelegate*)[[UIApplication sharedApplication]delegate] MessageBox:@"Notification" message:@"Subscribed!"];
           } else {
               NSLog(@"Error subscribing: %@", error);
           }
       }];
   
   <span data-ttu-id="69f43-155">Den här metoden skapar en **NSMutableArray** kategorier och använder den **meddelanden** klass som lagrar listan i lokal lagring och registrerar motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69f43-155">This method creates an **NSMutableArray** of categories and uses the **Notifications** class to store the list in the local storage and registers the corresponding tags with your notification hub.</span></span> <span data-ttu-id="69f43-156">När kategorier ändras, återskapas registreringen med de nya kategorierna.</span><span class="sxs-lookup"><span data-stu-id="69f43-156">When categories are changed, the registration is recreated with the new categories.</span></span>
3. <span data-ttu-id="69f43-157">I ViewController.m, lägger du till följande kod i den **viewDidLoad** metod för att ange användargränssnittet baserat på kategorierna som sparats tidigare.</span><span class="sxs-lookup"><span data-stu-id="69f43-157">In ViewController.m, add the following code in the **viewDidLoad** method to set the user interface based on the previously saved categories.</span></span>

        // This updates the UI on startup based on the status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



<span data-ttu-id="69f43-158">Appen kan nu användas för att lagra en uppsättning kategorier i enhetens lokala lagring som används för att registrera med notification hub när appen startar.</span><span class="sxs-lookup"><span data-stu-id="69f43-158">The app can now store a set of categories in the device local storage used to register with the notification hub whenever the app starts.</span></span>  <span data-ttu-id="69f43-159">Användaren kan ändra valet av kategorier vid körning och klicka på den **prenumerera** metod för att uppdatera registreringen för enheten.</span><span class="sxs-lookup"><span data-stu-id="69f43-159">The user can change the selection of categories at runtime and click the **subscribe** method to update the registration for the device.</span></span> <span data-ttu-id="69f43-160">Därefter uppdaterar du appen för att skicka meddelanden för senaste nyheterna direkt i själva appen.</span><span class="sxs-lookup"><span data-stu-id="69f43-160">Next, you will update the app to send the breaking news notifications directly in the app itself.</span></span>

## <a name="optional-sending-tagged-notifications"></a><span data-ttu-id="69f43-161">(valfritt) Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="69f43-161">(optional) Sending tagged notifications</span></span>
<span data-ttu-id="69f43-162">Om du inte har åtkomst till Visual Studio, kan du gå vidare till nästa avsnitt och skicka meddelanden från själva appen.</span><span class="sxs-lookup"><span data-stu-id="69f43-162">If you don't have access to Visual Studio, you can skip to the next section and send notifications from the app itself.</span></span> <span data-ttu-id="69f43-163">Du kan också skicka rätt mall-meddelande från den [klassiska Azure-portalen] med felsökningsfliken för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69f43-163">You can also send the proper template notification from the [Azure Classic Portal] using the debug tab for your notification hub.</span></span> 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-the-device"></a><span data-ttu-id="69f43-164">(valfritt) Skicka meddelanden från enheten</span><span class="sxs-lookup"><span data-stu-id="69f43-164">(optional) Send notifications from the device</span></span>
<span data-ttu-id="69f43-165">Meddelanden skickas normalt med en serverdelstjänst, men du kan skicka senaste nyheterna meddelanden direkt från appen.</span><span class="sxs-lookup"><span data-stu-id="69f43-165">Normally notifications would be sent by a backend service but, you can send breaking news notifications directly from the app.</span></span> <span data-ttu-id="69f43-166">Om du vill vi kommer att uppdatera den `SendNotificationRESTAPI` metod som vi har definierat i den [Kom igång med Notification Hubs] [ get-started] kursen.</span><span class="sxs-lookup"><span data-stu-id="69f43-166">To do this we will update the `SendNotificationRESTAPI` method that we defined in the [Get started with Notification Hubs][get-started] tutorial.</span></span>

1. <span data-ttu-id="69f43-167">I ViewController.m uppdatering av `SendNotificationRESTAPI` metod som följer så att den accepterar en parameter för taggen kategori och skickar rätt [mallen](notification-hubs-templates-cross-platform-push-messages.md) meddelande.</span><span class="sxs-lookup"><span data-stu-id="69f43-167">In ViewController.m update the `SendNotificationRESTAPI` method as follows so that it accepts a parameter for the category tag and sends the proper [template](notification-hubs-templates-cross-platform-push-messages.md) notification.</span></span>
   
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];
   
            NSString *json;
   
            // Construct the messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];
   
            // Generated the token to be used in the authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create the request to add the template notification message to the hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
   
            // Add the category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];
   
            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            //Authenticate the notification message POST request with the SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add the notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send the REST request
            NSURLSessionDataTask* dataTask = [session dataTaskWithRequest:request
                       completionHandler:^(NSData *data, NSURLResponse *response, NSError *error)
               {
               NSHTTPURLResponse* httpResponse = (NSHTTPURLResponse*) response;
                   if (error || httpResponse.statusCode != 200)
                   {
                       NSLog(@"\nError status: %d\nError: %@", httpResponse.statusCode, error);
                   }
                   if (data != NULL)
                   {
                       //xmlParser = [[NSXMLParser alloc] initWithData:data];
                       //[xmlParser setDelegate:self];
                       //[xmlParser parse];
                   }
               }];
   
            [dataTask resume];
        }
2. <span data-ttu-id="69f43-168">I ViewController.m uppdatering av **skicka meddelande** åtgärd som visas i den kod som följer.</span><span class="sxs-lookup"><span data-stu-id="69f43-168">In ViewController.m update the **Send Notification** action as shown in the code that follows.</span></span> <span data-ttu-id="69f43-169">Så att den ska skicka meddelanden med hjälp av varje tagg individuellt och skicka till flera plattformar.</span><span class="sxs-lookup"><span data-stu-id="69f43-169">So that it will send the notifications using each tag individually and send to multiple platforms.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send the message as breaking news for each category to WNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. <span data-ttu-id="69f43-170">Återskapa projektet och kontrollera att du har inga build-fel.</span><span class="sxs-lookup"><span data-stu-id="69f43-170">Rebuild your project and make sure you have no build errors.</span></span>

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="69f43-171">Kör appen och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="69f43-171">Run the app and generate notifications</span></span>
1. <span data-ttu-id="69f43-172">Klicka på Kör för att bygga projektet och starta appen.</span><span class="sxs-lookup"><span data-stu-id="69f43-172">Press the Run button to build the project and start the app.</span></span> <span data-ttu-id="69f43-173">Välj senaste nyheterna alternativ prenumererar på och trycker sedan på den **prenumerera** knappen.</span><span class="sxs-lookup"><span data-stu-id="69f43-173">Select some breaking news options to subscribe to and then press the **Subscribe** button.</span></span> <span data-ttu-id="69f43-174">Du bör se en dialogruta som visar meddelanden prenumererar på.</span><span class="sxs-lookup"><span data-stu-id="69f43-174">You should see a dialog indicating the notifications have been subscribed to.</span></span>
   
    ![][1]
   
    <span data-ttu-id="69f43-175">När du väljer **prenumerera**, appen konverterar valda kategorier till taggar och begär en ny enhetsregistrering för de valda taggarna från meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69f43-175">When you choose **Subscribe**, the app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span>
2. <span data-ttu-id="69f43-176">Ange ett meddelande som ska skickas som senaste nyheterna tryck sedan på den **skicka meddelande** knappen.</span><span class="sxs-lookup"><span data-stu-id="69f43-176">Enter a message to be sent as breaking news then press the **Send Notification** button.</span></span> <span data-ttu-id="69f43-177">Du kan också köra .NET-konsolapp för att generera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69f43-177">Alternatively, run the .NET console app to generate notifications.</span></span>
   
    ![][2]
3. <span data-ttu-id="69f43-178">Varje enhet som prenumererar på senaste nytt får senaste nyheterna meddelanden som du har skickat.</span><span class="sxs-lookup"><span data-stu-id="69f43-178">Each device subscribed to breaking news will receive the breaking news notifications you just sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69f43-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69f43-179">Next steps</span></span>
<span data-ttu-id="69f43-180">Vi lärt dig hur du broadcast senaste nyheterna efter kategori i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="69f43-180">In this tutorial we learned how to broadcast breaking news by category.</span></span> <span data-ttu-id="69f43-181">Överväg att fylla i en av följande kurser som markerar andra avancerade Meddelandehubbar scenarier:</span><span class="sxs-lookup"><span data-stu-id="69f43-181">Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="69f43-182">**[Använda Notification Hubs för att sända lokaliserade senaste nyheterna]**</span><span class="sxs-lookup"><span data-stu-id="69f43-182">**[Use Notification Hubs to broadcast localized breaking news]**</span></span>
  
    <span data-ttu-id="69f43-183">Lär dig mer om att utöka senaste nyheterna appen om du vill aktivera skicka lokaliserade meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69f43-183">Learn how to expand the breaking news app to enable sending localized notifications.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
<span data-ttu-id="69f43-184">[Använda Notification Hubs för att sända lokaliserade senaste nyheterna]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="69f43-184">[Use Notification Hubs to broadcast localized breaking news]: notification-hubs-ios-xplat-localized-apns-push-notification.md</span></span>
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
<span data-ttu-id="69f43-185">[klassiska Azure-portalen]: https://manage.windowsazure.com</span><span class="sxs-lookup"><span data-stu-id="69f43-185">[Azure Classic Portal]: https://manage.windowsazure.com</span></span>
