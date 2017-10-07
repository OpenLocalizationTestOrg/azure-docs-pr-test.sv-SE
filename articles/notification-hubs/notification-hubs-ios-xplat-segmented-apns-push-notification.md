---
title: "aaaNotification bryter nyheter kurs om Händelsehubbar - iOS"
description: "Lär dig hur toouse Azure Service Bus Notification Hubs toosend bryter nyheter meddelanden tooiOS enheter."
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
ms.openlocfilehash: 763b80b5ffed238b351d95bd3d6a96cb914f53cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="69b26-103">Använda Notification Hubs toosend senaste nytt</span><span class="sxs-lookup"><span data-stu-id="69b26-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="69b26-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="69b26-104">Overview</span></span>
<span data-ttu-id="69b26-105">Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooan iOS-app.</span><span class="sxs-lookup"><span data-stu-id="69b26-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooan iOS app.</span></span> <span data-ttu-id="69b26-106">När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="69b26-106">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="69b26-107">Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="69b26-107">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="69b26-108">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69b26-108">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="69b26-109">När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="69b26-109">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="69b26-110">Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="69b26-110">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="69b26-111">Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="69b26-111">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69b26-112">Krav</span><span class="sxs-lookup"><span data-stu-id="69b26-112">Prerequisites</span></span>
<span data-ttu-id="69b26-113">Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="69b26-113">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="69b26-114">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="69b26-114">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="69b26-115">Lägg till kategori markeringen toohello app</span><span class="sxs-lookup"><span data-stu-id="69b26-115">Add category selection toohello app</span></span>
<span data-ttu-id="69b26-116">hello första steget är tooadd hello UI-element tooyour befintliga storyboard som möjliggör hello användaren tooselect kategorier tooregister.</span><span class="sxs-lookup"><span data-stu-id="69b26-116">hello first step is tooadd hello UI elements tooyour existing storyboard that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="69b26-117">hello kategorier som är markerad som en användare som lagras på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="69b26-117">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="69b26-118">När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.</span><span class="sxs-lookup"><span data-stu-id="69b26-118">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="69b26-119">Lägg till följande komponenter från objektbiblioteket för hello hello i din MainStoryboard_iPhone.storyboard:</span><span class="sxs-lookup"><span data-stu-id="69b26-119">In your MainStoryboard_iPhone.storyboard add hello following components from hello object library:</span></span>
   
   * <span data-ttu-id="69b26-120">En etikett med ”bryter nyheter” text</span><span class="sxs-lookup"><span data-stu-id="69b26-120">A label with "Breaking News" text,</span></span>
   * <span data-ttu-id="69b26-121">Etiketter med kategori text ”World”, ”politik”, ”företag”, ”teknik”, ”vetenskap”, ”sport”</span><span class="sxs-lookup"><span data-stu-id="69b26-121">Labels with category texts "World", "Politics", "Business", "Technology", "Science", "Sports",</span></span>
   * <span data-ttu-id="69b26-122">Sex växlar en per kategori och ställa in varje switch **tillstånd** toobe **av** som standard.</span><span class="sxs-lookup"><span data-stu-id="69b26-122">Six switches, one per category, set each switch **State** toobe **Off** by default.</span></span>
   * <span data-ttu-id="69b26-123">En knapp med etiketten ”prenumerera”</span><span class="sxs-lookup"><span data-stu-id="69b26-123">One button labeled "Subscribe"</span></span>
     
     <span data-ttu-id="69b26-124">Storyboard bör se ut så här:</span><span class="sxs-lookup"><span data-stu-id="69b26-124">Your storyboard should look as follows:</span></span>
     
     ![][3]
2. <span data-ttu-id="69b26-125">I Redigeraren för hello assistenten skapa uttag för alla hello-växlar och anropa dem ”WorldSwitch”, ”PoliticsSwitch”, ”BusinessSwitch”, ”TechnologySwitch”, ”ScienceSwitch”, ”SportsSwitch”</span><span class="sxs-lookup"><span data-stu-id="69b26-125">In hello assistant editor, create outlets for all hello switches and call them "WorldSwitch", "PoliticsSwitch", "BusinessSwitch", "TechnologySwitch", "ScienceSwitch", "SportsSwitch"</span></span>
3. <span data-ttu-id="69b26-126">Skapa en åtgärd för en knapp som kallas ”prenumerera”.</span><span class="sxs-lookup"><span data-stu-id="69b26-126">Create an Action for your button called "subscribe".</span></span> <span data-ttu-id="69b26-127">Din ViewController.h bör innehålla hello följande:</span><span class="sxs-lookup"><span data-stu-id="69b26-127">Your ViewController.h should contain hello following:</span></span>
   
        @property (weak, nonatomic) IBOutlet UISwitch *WorldSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *PoliticsSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *BusinessSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *TechnologySwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *ScienceSwitch;
        @property (weak, nonatomic) IBOutlet UISwitch *SportsSwitch;
   
        - (IBAction)subscribe:(id)sender;
4. <span data-ttu-id="69b26-128">Skapa en ny **Cocoa Touch klassen** kallas `Notifications`.</span><span class="sxs-lookup"><span data-stu-id="69b26-128">Create a new **Cocoa Touch Class** called `Notifications`.</span></span> <span data-ttu-id="69b26-129">Kopiera följande kod i hello-gränssnittet i hello filen Notifications.h för hello:</span><span class="sxs-lookup"><span data-stu-id="69b26-129">Copy hello following code in hello interface section of hello file Notifications.h:</span></span>
   
        @property NSData* deviceToken;
   
        - (id)initWithConnectionString:(NSString*)listenConnectionString HubName:(NSString*)hubName;
   
        - (void)storeCategoriesAndSubscribeWithCategories:(NSArray*)categories
                    completion:(void (^)(NSError* error))completion;
   
        - (NSSet*)retrieveCategories;
   
        - (void)subscribeWithCategories:(NSSet*)categories completion:(void (^)(NSError *))completion;
5. <span data-ttu-id="69b26-130">Lägg till hello efter import direktiv tooNotifications.m:</span><span class="sxs-lookup"><span data-stu-id="69b26-130">Add hello following import directive tooNotifications.m:</span></span>
   
        #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
6. <span data-ttu-id="69b26-131">Kopiera följande kod i hello implementering i hello filen Notifications.m för hello.</span><span class="sxs-lookup"><span data-stu-id="69b26-131">Copy hello following code in hello implementation section of hello file Notifications.m.</span></span>
   
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



    <span data-ttu-id="69b26-132">Den här klassen använder lokal lagring toostore och hämta hello kategorier av nyheter som den här enheten ska ta emot.</span><span class="sxs-lookup"><span data-stu-id="69b26-132">This class uses local storage toostore and retrieve hello categories of news that this device will receive.</span></span> <span data-ttu-id="69b26-133">Den innehåller också en metod tooregister för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registrering.</span><span class="sxs-lookup"><span data-stu-id="69b26-133">Also, it contains a method tooregister for these categories using a [Template](notification-hubs-templates-cross-platform-push-messages.md) registration.</span></span>

1. <span data-ttu-id="69b26-134">Lägga till en import-sats för Notifications.h i hello AppDelegate.h-filen och Lägg till en egenskap för en instans av hello meddelanden klass:</span><span class="sxs-lookup"><span data-stu-id="69b26-134">In hello AppDelegate.h file, add an import statement for Notifications.h and add a property for an instance of hello Notifications class:</span></span>
   
        #import "Notifications.h"
   
        @property (nonatomic) Notifications* notifications;
2. <span data-ttu-id="69b26-135">I hello **didFinishLaunchingWithOptions** metod i AppDelegate.m, lägga till hello kod tooinitialize hello meddelanden instans hello början av hello-metoden.</span><span class="sxs-lookup"><span data-stu-id="69b26-135">In hello **didFinishLaunchingWithOptions** method in AppDelegate.m, add hello code tooinitialize hello notifications instance at hello beginning of hello method.</span></span>  
   
    <span data-ttu-id="69b26-136">`HUBNAME`och `HUBLISTENACCESS` (definieras i hubinfo.h) har redan hello `<hub name>` och `<connection string with listen access>` platshållare ersätts med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature*som du fick tidigare</span><span class="sxs-lookup"><span data-stu-id="69b26-136">`HUBNAME` and `HUBLISTENACCESS` (defined in hubinfo.h) should already have hello `<hub name>` and `<connection string with listen access>` placeholders replaced with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier</span></span>
   
        self.notifications = [[Notifications alloc] initWithConnectionString:HUBLISTENACCESS HubName:HUBNAME];
   
   > [!NOTE]
   > <span data-ttu-id="69b26-137">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="69b26-137">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="69b26-138">Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69b26-138">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="69b26-139">hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="69b26-139">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
3. <span data-ttu-id="69b26-140">I hello **didRegisterForRemoteNotificationsWithDeviceToken** metod i AppDelegate.m, Ersätt hello koden i hello-metoden med följande kod toopass hello token toohello meddelanden enhetsklass hello.</span><span class="sxs-lookup"><span data-stu-id="69b26-140">In hello **didRegisterForRemoteNotificationsWithDeviceToken** method in AppDelegate.m, replace hello code in hello method with hello following code toopass hello device token toohello notifications class.</span></span> <span data-ttu-id="69b26-141">hello meddelanden klassen utför hello registrering för meddelanden med hello kategorier.</span><span class="sxs-lookup"><span data-stu-id="69b26-141">hello notifications class will perform hello registering for notifications with hello categories.</span></span> <span data-ttu-id="69b26-142">Om hello användaren ändrar kategori, vi kallar hello `subscribeWithCategories` metod i svaret toohello **prenumerera** knappen tooupdate dem.</span><span class="sxs-lookup"><span data-stu-id="69b26-142">If hello user changes category selections, we call hello `subscribeWithCategories` method in response toohello **subscribe** button tooupdate them.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="69b26-143">Eftersom hello enhetstoken som tilldelats av hello Apple Push-aviseringstjänsten (APNS) kan risken när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel.</span><span class="sxs-lookup"><span data-stu-id="69b26-143">Because hello device token assigned by hello Apple Push Notification Service (APNS) can chance at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="69b26-144">Det här exemplet registrerar för meddelande varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="69b26-144">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="69b26-145">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="69b26-145">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
   > 
   > 
   
        self.notifications.deviceToken = deviceToken;
   
        // Retrieves hello categories from local storage and requests a registration for these categories
        // each time hello app starts and performs a registration.
   
        NSSet* categories = [self.notifications retrieveCategories];
        [self.notifications subscribeWithCategories:categories completion:^(NSError* error) {
            if (error != nil) {
                NSLog(@"Error registering for notifications: %@", error);
            }
        }];

    <span data-ttu-id="69b26-146">Observera att då det ska vara någon annan kod i hello **didRegisterForRemoteNotificationsWithDeviceToken** metod.</span><span class="sxs-lookup"><span data-stu-id="69b26-146">Note that at this point there should be no other code in hello **didRegisterForRemoteNotificationsWithDeviceToken** method.</span></span>

1. <span data-ttu-id="69b26-147">hello följande metoder ska redan finnas i AppDelegate.m från att slutföras hello [Kom igång med Notification Hubs] [ get-started] kursen.</span><span class="sxs-lookup"><span data-stu-id="69b26-147">hello following methods should already be present in AppDelegate.m from completing hello [Get started with Notification Hubs][get-started] tutorial.</span></span>  <span data-ttu-id="69b26-148">Om du inte lägga till dem.</span><span class="sxs-lookup"><span data-stu-id="69b26-148">If not, add them.</span></span>
   
    <span data-ttu-id="69b26-149">-(void) MessageBox:(NSString *) rubrik meddelandet:(NSString *) messageText {</span><span class="sxs-lookup"><span data-stu-id="69b26-149">-(void)MessageBox:(NSString *)title message:(NSString *)messageText  {</span></span>
   
        UIAlertView *alert = [[UIAlertView alloc] initWithTitle:title message:messageText delegate:self
            cancelButtonTitle:@"OK" otherButtonTitles: nil];
        [alert show];
    <span data-ttu-id="69b26-150">}</span><span class="sxs-lookup"><span data-stu-id="69b26-150">}</span></span>
   
   * <span data-ttu-id="69b26-151">(void) programmet:(UIApplication *) program didReceiveRemoteNotification: (NSDictionary *) användarinformationen {NSLog (@ ”% @”, användarinformationen);   [self MessageBox:@"Notification” meddelande: [valueForKey:@"alert [användarinformationen objectForKey:@"aps] ””]]; }</span><span class="sxs-lookup"><span data-stu-id="69b26-151">(void)application:(UIApplication *)application didReceiveRemoteNotification:   (NSDictionary *)userInfo {   NSLog(@"%@", userInfo);   [self MessageBox:@"Notification" message:[[userInfo objectForKey:@"aps"] valueForKey:@"alert"]]; }</span></span>
   
   <span data-ttu-id="69b26-152">Den här metoden hanterar meddelanden när hello app körs genom att visa en enkel **UIAlert**.</span><span class="sxs-lookup"><span data-stu-id="69b26-152">This method handles notifications received when hello app is running by displaying a simple **UIAlert**.</span></span>
2. <span data-ttu-id="69b26-153">Lägga till en import-sats för AppDelegate.h och kopiera hello följande kod i hello ViewController.m, XCode-genererade **prenumerera** metod.</span><span class="sxs-lookup"><span data-stu-id="69b26-153">In ViewController.m, add a import statement for AppDelegate.h and copy hello following code into hello XCode-generated **subscribe** method.</span></span> <span data-ttu-id="69b26-154">Den här koden kommer att uppdatera hello notification registrering toouse hello nya kategori etiketter hello användaren har valt i hello-användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="69b26-154">This code will update hello notification registration toouse hello new category tags hello user has chosen in hello user interface.</span></span>
   
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
   
   <span data-ttu-id="69b26-155">Den här metoden skapar en **NSMutableArray** av kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrerar hello motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69b26-155">This method creates an **NSMutableArray** of categories and uses hello **Notifications** class toostore hello list in hello local storage and registers hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="69b26-156">När kategorier ändras, återskapas hello registrering med hello nya kategorier.</span><span class="sxs-lookup"><span data-stu-id="69b26-156">When categories are changed, hello registration is recreated with hello new categories.</span></span>
3. <span data-ttu-id="69b26-157">Lägg till följande kod i hello hello i ViewController.m, **viewDidLoad** användargränssnittet för metoden tooset hello baserat på hello sparat tidigare kategorier.</span><span class="sxs-lookup"><span data-stu-id="69b26-157">In ViewController.m, add hello following code in hello **viewDidLoad** method tooset hello user interface based on hello previously saved categories.</span></span>

        // This updates hello UI on startup based on hello status of previously saved categories.

        Notifications* notifications = [(AppDelegate*)[[UIApplication sharedApplication]delegate] notifications];

        NSSet* categories = [notifications retrieveCategories];

        if ([categories containsObject:@"World"]) self.WorldSwitch.on = true;
        if ([categories containsObject:@"Politics"]) self.PoliticsSwitch.on = true;
        if ([categories containsObject:@"Business"]) self.BusinessSwitch.on = true;
        if ([categories containsObject:@"Technology"]) self.TechnologySwitch.on = true;
        if ([categories containsObject:@"Science"]) self.ScienceSwitch.on = true;
        if ([categories containsObject:@"Sports"]) self.SportsSwitch.on = true;



<span data-ttu-id="69b26-158">hello app kan nu lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister med hello notification hub när hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="69b26-158">hello app can now store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello app starts.</span></span>  <span data-ttu-id="69b26-159">hello-användare kan ändra hello valet av kategorier under körning och klicka på hello **prenumerera** metoden tooupdate hello registrering för hello enhet.</span><span class="sxs-lookup"><span data-stu-id="69b26-159">hello user can change hello selection of categories at runtime and click hello **subscribe** method tooupdate hello registration for hello device.</span></span> <span data-ttu-id="69b26-160">Därefter uppdaterar du hello app toosend hello bryta nyheter meddelanden direkt i själva hello appen.</span><span class="sxs-lookup"><span data-stu-id="69b26-160">Next, you will update hello app toosend hello breaking news notifications directly in hello app itself.</span></span>

## <a name="optional-sending-tagged-notifications"></a><span data-ttu-id="69b26-161">(valfritt) Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="69b26-161">(optional) Sending tagged notifications</span></span>
<span data-ttu-id="69b26-162">Om du inte har åtkomst tooVisual Studio, kan du hoppa över toohello nästa avsnitt och skicka meddelanden från själva hello appen.</span><span class="sxs-lookup"><span data-stu-id="69b26-162">If you don't have access tooVisual Studio, you can skip toohello next section and send notifications from hello app itself.</span></span> <span data-ttu-id="69b26-163">Du kan också skicka hello rätt mall meddelande från hello [klassiska Azure-portalen] med hello felsökningsfliken för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69b26-163">You can also send hello proper template notification from hello [Azure Classic Portal] using hello debug tab for your notification hub.</span></span> 

[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="optional-send-notifications-from-hello-device"></a><span data-ttu-id="69b26-164">(valfritt) Skicka meddelanden från hello-enhet</span><span class="sxs-lookup"><span data-stu-id="69b26-164">(optional) Send notifications from hello device</span></span>
<span data-ttu-id="69b26-165">Meddelanden skickas normalt med en serverdelstjänst men du kan skicka senaste nyheterna meddelanden direkt från hello app.</span><span class="sxs-lookup"><span data-stu-id="69b26-165">Normally notifications would be sent by a backend service but, you can send breaking news notifications directly from hello app.</span></span> <span data-ttu-id="69b26-166">toodo detta vi kommer att uppdatera hello `SendNotificationRESTAPI` metod som vi har definierat i hello [Kom igång med Notification Hubs] [ get-started] kursen.</span><span class="sxs-lookup"><span data-stu-id="69b26-166">toodo this we will update hello `SendNotificationRESTAPI` method that we defined in hello [Get started with Notification Hubs][get-started] tutorial.</span></span>

1. <span data-ttu-id="69b26-167">I ViewController.m uppdatering hello `SendNotificationRESTAPI` metod som följer så att den accepterar en parameter för hello category med och skickar hello rätt [mallen](notification-hubs-templates-cross-platform-push-messages.md) meddelande.</span><span class="sxs-lookup"><span data-stu-id="69b26-167">In ViewController.m update hello `SendNotificationRESTAPI` method as follows so that it accepts a parameter for hello category tag and sends hello proper [template](notification-hubs-templates-cross-platform-push-messages.md) notification.</span></span>
   
        - (void)SendNotificationRESTAPI:(NSString*)categoryTag
        {
            NSURLSession* session = [NSURLSession sessionWithConfiguration:[NSURLSessionConfiguration
                                     defaultSessionConfiguration] delegate:nil delegateQueue:nil];
   
            NSString *json;
   
            // Construct hello messages REST endpoint
            NSURL* url = [NSURL URLWithString:[NSString stringWithFormat:@"%@%@/messages/%@", HubEndpoint,
                                               HUBNAME, API_VERSION]];
   
            // Generated hello token toobe used in hello authorization header.
            NSString* authorizationToken = [self generateSasToken:[url absoluteString]];
   
            //Create hello request tooadd hello template notification message toohello hub
            NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url];
            [request setHTTPMethod:@"POST"];
   
            // Add hello category as a tag
            [request setValue:categoryTag forHTTPHeaderField:@"ServiceBusNotification-Tags"];
   
            // Template notification
            json = [NSString stringWithFormat:@"{\"messageParam\":\"Breaking %@ News : %@\"}",
                    categoryTag, self.notificationMessage.text];
   
            // Signify template notification format
            [request setValue:@"template" forHTTPHeaderField:@"ServiceBusNotification-Format"];
   
            // JSON Content-Type
            [request setValue:@"application/json;charset=utf-8" forHTTPHeaderField:@"Content-Type"];
   
            //Authenticate hello notification message POST request with hello SaS token
            [request setValue:authorizationToken forHTTPHeaderField:@"Authorization"];
   
            //Add hello notification message body
            [request setHTTPBody:[json dataUsingEncoding:NSUTF8StringEncoding]];
   
            // Send hello REST request
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
2. <span data-ttu-id="69b26-168">I ViewController.m uppdatering hello **skicka meddelande** åtgärd som visas i hello-kod som följer.</span><span class="sxs-lookup"><span data-stu-id="69b26-168">In ViewController.m update hello **Send Notification** action as shown in hello code that follows.</span></span> <span data-ttu-id="69b26-169">Så att den ska skicka hello-meddelanden med varje tagg individuellt och skicka toomultiple plattformar.</span><span class="sxs-lookup"><span data-stu-id="69b26-169">So that it will send hello notifications using each tag individually and send toomultiple platforms.</span></span>

        - (IBAction)SendNotificationMessage:(id)sender
        {
            self.sendResults.text = @"";

            NSArray* categories = [NSArray arrayWithObjects: @"World", @"Politics", @"Business",
                                    @"Technology", @"Science", @"Sports", nil];

            // Lets send hello message as breaking news for each category tooWNS, GCM, and APNS
            // using a template.
            for(NSString* category in categories)
            {
                [self SendNotificationRESTAPI:category];
            }
        }



1. <span data-ttu-id="69b26-170">Återskapa projektet och kontrollera att du har inga build-fel.</span><span class="sxs-lookup"><span data-stu-id="69b26-170">Rebuild your project and make sure you have no build errors.</span></span>

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="69b26-171">Kör hello app och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="69b26-171">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="69b26-172">Tryck på hello kör knappen toobuild hello projektet och starta hello app.</span><span class="sxs-lookup"><span data-stu-id="69b26-172">Press hello Run button toobuild hello project and start hello app.</span></span> <span data-ttu-id="69b26-173">Välj vissa senaste nyheterna alternativ toosubscribe tooand och tryck sedan på hello **prenumerera** knappen.</span><span class="sxs-lookup"><span data-stu-id="69b26-173">Select some breaking news options toosubscribe tooand then press hello **Subscribe** button.</span></span> <span data-ttu-id="69b26-174">Du bör se en dialogruta som visar hello meddelanden prenumererar på.</span><span class="sxs-lookup"><span data-stu-id="69b26-174">You should see a dialog indicating hello notifications have been subscribed to.</span></span>
   
    ![][1]
   
    <span data-ttu-id="69b26-175">När du väljer **prenumerera**hello appkategorierna konverterar hello som valts i taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="69b26-175">When you choose **Subscribe**, hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span>
2. <span data-ttu-id="69b26-176">Ange ett meddelande toobe skickas som de senaste nyheterna och tryck sedan på hello **skicka meddelande** knappen.</span><span class="sxs-lookup"><span data-stu-id="69b26-176">Enter a message toobe sent as breaking news then press hello **Send Notification** button.</span></span> <span data-ttu-id="69b26-177">Du kan också köra hello .NET konsolen app toogenerate meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69b26-177">Alternatively, run hello .NET console app toogenerate notifications.</span></span>
   
    ![][2]
3. <span data-ttu-id="69b26-178">Varje enhet som prenumererar på toobreaking nyheter visas hello senaste nyheterna meddelanden har skickat.</span><span class="sxs-lookup"><span data-stu-id="69b26-178">Each device subscribed toobreaking news will receive hello breaking news notifications you just sent.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69b26-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69b26-179">Next steps</span></span>
<span data-ttu-id="69b26-180">I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori.</span><span class="sxs-lookup"><span data-stu-id="69b26-180">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="69b26-181">Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:</span><span class="sxs-lookup"><span data-stu-id="69b26-181">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="69b26-182">**[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]**</span><span class="sxs-lookup"><span data-stu-id="69b26-182">**[Use Notification Hubs toobroadcast localized breaking news]**</span></span>
  
    <span data-ttu-id="69b26-183">Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="69b26-183">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-subscribed.png
[2]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios1.png
[3]: ./media/notification-hubs-ios-send-breaking-news/notification-hub-breakingnews-ios2.png








<!-- URLs. -->
[How To: Service Bus Notification Hubs (iOS Apps)]: http://msdn.microsoft.com/library/jj927168.aspx
[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]: notification-hubs-ios-xplat-localized-apns-push-notification.md
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs]: notification-hubs-aspnet-backend-ios-notify-users.md
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/dn530749.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[get-started]: /manage/services/notification-hubs/get-started-notification-hubs-ios/
[klassiska Azure-portalen]: https://manage.windowsazure.com
