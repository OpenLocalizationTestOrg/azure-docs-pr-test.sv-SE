---
title: "aaaAzure Mobile Engagement iOS SDK nå Integration | Microsoft Docs"
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f5f5857-867c-40c5-9d76-675a343a0296
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 12/13/2016
ms.author: piyushjo
ms.openlocfilehash: 40c9bfbdb475ab0b97bdbc9cea798a59cb8a71ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-reach-on-ios"></a>Hur tooIntegrate Engagement nå på iOS
Du måste följa hello integration beskrivs i hello [hur tooIntegrate Engagement för iOS-dokument](mobile-engagement-ios-integrate-engagement.md) innan du följer den här guiden.

Den här dokumentationen kräver XCode 8. Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Det finns ett känt fel på den här tidigare version när du kör på iOS 10-enheter: systemmeddelanden är inte åtgärdade. toofix detta behöver tooimplement hello föråldrad API `application:didReceiveRemoteNotification:` i din app delegera på följande sätt:

    - (void)application:(UIApplication*)application
    didReceiveRemoteNotification:(NSDictionary*)userInfo
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:nil];
    }

> [!IMPORTANT]
> **Vi rekommenderar inte den här lösningen** som detta beteende kan ändras i kommande (även mindre) iOS version uppgraderar eftersom den här iOS API är inaktuell. Du bör växla tooXCode 8 så snart som möjligt.
>
>

### <a name="enable-your-app-tooreceive-silent-push-notifications"></a>Aktivera din app tooreceive tysta Push-meddelanden
[!INCLUDE [mobile-engagement-ios-silent-push](../../includes/mobile-engagement-ios-silent-push.md)]

## <a name="integration-steps"></a>Integreringen
### <a name="embed-hello-engagement-reach-sdk-into-your-ios-project"></a>Bädda in hello Engagement nå SDK i din iOS-projekt
* Lägg till hello Reach sdk i Xcode-projektet. I Xcode gå för**projekt \> lägga till tooproject** och välj hello `EngagementReach` mapp.

### <a name="modify-your-application-delegate"></a>Ändra programdelegaten
* Importera hello Engagement nå modulen hello över din implementering fil:

      [...]
      #import "AEReachModule.h"
* Inuti metoden `applicationDidFinishLaunching:` eller `application:didFinishLaunchingWithOptions:`, skapar du en räckviddsmodul och skickar den tooyour befintliga initieringsrad:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
        [...]

        return YES;
      }
* Ändra **'icon.png'** med hello avbildningens namn som du vill ha som ditt meddelandeikonen.
* Om du vill toouse hello alternativet *Aktivitetsikon uppdateringsvärde* i Reach-kampanjer eller om du vill toouse interna push \<SaaS Reach/API/kampanj format/intern Push\> kampanjer, måste du låta hello Reach-modulen hantera hello Aktivitetsikon ikonen sig själv (den tas bort automatiskt i hello programmet Aktivitetsikon och även återställa hello-värde som lagras av Engagement varje gång hello program är igång eller foregrounded). Detta görs genom att lägga till följande rad efter Reach-modulen initiering hello:

      [reach setAutoBadgeEnabled:YES];
* Om du vill toohandle Reach data-push, måste du låta programdelegaten överensstämmer toohello `AEReachDataPushDelegate` protokoll. Lägg till följande rad efter Reach-modulen initiering hello:

      [reach setDataPushDelegate:self];
* Sedan kan du implementera hello metoder `onDataPushStringReceived:` och `onDataPushBase64ReceivedWithDecodedBody:andEncodedBody:` i programdelegaten:

      -(BOOL)didReceiveStringDataPushWithCategory:(NSString*)category body:(NSString*)body
      {
         NSLog(@"String data push message with category <%@> received: %@", category, body);
         return YES;
      }

      -(BOOL)didReceiveBase64DataPushWithCategory:(NSString*)category decodedBody:(NSData *)decodedBody encodedBody:(NSString *)encodedBody
      {
         NSLog(@"Base64 data push message with category <%@> received: %@", category, encodedBody);
         // Do something useful with decodedBody like updating an image view
         return YES;
      }

### <a name="category"></a>Kategori
hello kategori parametern är valfri när du skapar en Data-Push-kampanj och gör att du toofilter data push-meddelanden. Detta är användbart om du vill toopush olika typer av `Base64` data och vill tooidentify typ innan parsningen dem.

**Tillämpningsprogrammet är nu redo tooreceive och visa nå innehållet!**

## <a name="how-tooreceive-announcements-and-polls-at-any-time"></a>Hur tooreceive meddelanden och avsökningar när som helst
Engagement kan skicka Reach meddelanden tooyour slutanvändare när som helst genom att använda hello Apple Push Notification Service.

tooenable den här funktionen kan du ha tooprepare ditt program för Apple push-meddelanden och ändra programdelegaten.

### <a name="prepare-your-application-for-apple-push-notifications"></a>Förbered ditt program för Apple push-meddelanden
Följ guiden hello: [hur tooPrepare ditt program för Apple Push-meddelanden](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html#//apple_ref/doc/uid/TP40012582-CH26-SW6)

### <a name="add-hello-necessary-client-code"></a>Lägg till hello nödvändiga klientkod
*Ditt program bör nu ha en registrerad Apple push-certifikat hello Engagement klientdel.*

Om det inte är redan gjort måste tooregister programmet tooreceive push-meddelanden.

* Importera hello `User Notification` framework:

        #import <UserNotifications/UserNotifications.h>
* Lägg till följande rad när programmet startas hello (vanligtvis i `application:didFinishLaunchingWithOptions:`):

        if (NSFoundationVersionNumber >= NSFoundationVersionNumber_iOS_8_0)
        {
            if (NSFoundationVersionNumber > NSFoundationVersionNumber_iOS_9_x_Max)
            {
                [UNUserNotificationCenter.currentNotificationCenter requestAuthorizationWithOptions:(UNAuthorizationOptionBadge | UNAuthorizationOptionSound | UNAuthorizationOptionAlert) completionHandler:^(BOOL granted, NSError * _Nullable error) {}];
            }else
            {
                [application registerUserNotificationSettings:[UIUserNotificationSettings settingsForTypes:(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | UIUserNotificationTypeAlert)   categories:nil]];
            }
            [application registerForRemoteNotifications];
        }
        else
        {
            [application registerForRemoteNotificationTypes:(UIRemoteNotificationTypeBadge | UIRemoteNotificationTypeSound | UIRemoteNotificationTypeAlert)];
        }

Du måste sedan tooprovide tooEngagement hello enhetstoken som returnerades av Apple-servrar. Detta görs i hello metod med namnet `application:didRegisterForRemoteNotificationsWithDeviceToken:` i programdelegaten:

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
        [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

Slutligen har du tooinform hello Engagement SDK när programmet tar emot ett meddelande om fjärråtkomst. toodo som anropar hello metoden `applicationDidReceiveRemoteNotification:fetchCompletionHandler:` i programdelegaten:

    - (void)application:(UIApplication*)application didReceiveRemoteNotification:(NSDictionary*)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

> [!IMPORTANT]
> Som standard kontrollerar Engagement nå hello completionHandler. Om du vill toomanually svara toohello `handler` blockera i koden, du kan skicka nil för hello `handler` argumentet och kontroll hello slutförande blockera själv. Se hello `UIBackgroundFetchResult` typ för en lista över möjliga värden.
>
>

### <a name="full-example"></a>Fullständigt exempel
Här är ett fullständigt exempel på integrering:

    #pragma mark -
    #pragma mark Application lifecycle

    - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
    {
      /* Reach module */
      AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
      [reach setAutoBadgeEnabled:YES];

      /* Engagement initialization */
      [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}" modules:reach, nil];
      [[EngagementAgent shared] setPushDelegate:self];

      /* Views */
      [window addSubview:[tabBarController view]];
      [window makeKeyAndVisible];

      [application registerForRemoteNotificationTypes:UIRemoteNotificationTypeAlert|UIRemoteNotificationTypeBadge|UIRemoteNotificationTypeSound];
      return YES;
    }

    - (void)application:(UIApplication*)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData*)deviceToken
    {
      [[EngagementAgent shared] registerDeviceToken:deviceToken];
    }

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))handler
    {
        [[EngagementAgent shared] applicationDidReceiveRemoteNotification:userInfo fetchCompletionHandler:handler];
    }

### <a name="resolve-unusernotificationcenter-delegate-conflicts"></a>UNUserNotificationCenter ombud konfliktlösning

*Om varken ditt program eller en av tredjeparts-biblioteken implementerar en `UNUserNotificationCenterDelegate` och sedan kan du hoppa över den här delen.*

En `UNUserNotificationCenter` delegat som används av hello SDK toomonitor hello livscykel Engagement meddelanden på enheter som kör IOS 10 eller senare. hello SDK har sin egen implementering av hello `UNUserNotificationCenterDelegate` protokoll, men det kan bara finnas ett `UNUserNotificationCenter` delegera per program. Andra ombud läggs toohello `UNUserNotificationCenter` objekt står i konflikt med hello Engagement en. Om hello SDK upptäcker din eller någon annan tredje parts ombud kommer det inte att använda sin egen implementering toogive hello du en chans tooresolve konflikter. Du måste tooadd hello Engagement logik tooyour äger en delegat i ordning tooresolve hello konflikter.

Det finns två sätt tooachieve detta.

Förslag 1, genom att helt enkelt vidarebefordra ombudet anropar toohello SDK:

    #import <UIKit/UIKit.h>
    #import "EngagementAgent.h"
    #import <UserNotifications/UserNotifications.h>


    @interface MyAppDelegate : NSObject <UIApplicationDelegate, UNUserNotificationCenterDelegate>
    @end

    @implementation MyAppDelegate

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterWillPresentNotification:notification withCompletionHandler:completionHandler]
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [[EngagementAgent shared] userNotificationCenterDidReceiveNotificationResponse:response withCompletionHandler:completionHandler]
    }
    @end

Eller förslag 2, med arv från hello `AEUserNotificationHandler` klass

    #import "AEUserNotificationHandler.h"
    #import "EngagementAgent.h"

    @interface CustomUserNotificationHandler :AEUserNotificationHandler
    @end

    @implementation CustomUserNotificationHandler

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center willPresentNotification:notification withCompletionHandler:completionHandler];
    }

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse: UNNotificationResponse *)response withCompletionHandler:(void(^)())completionHandler
    {
      // Your own logic.

      [super userNotificationCenter:center didReceiveNotificationResponse:response withCompletionHandler:completionHandler];
    }

    @end

> [!NOTE]
> Du kan fastställa om ett meddelande kommer från Engagement eller inte genom att ange dess `userInfo` ordlista toohello Agent `isEngagementPushPayload:` klassen metoden.

Kontrollera att hello `UNUserNotificationCenter` objektets ombud anges tooyour ombud inom antingen hello `application:willFinishLaunchingWithOptions:` eller hello `application:didFinishLaunchingWithOptions:` metod för programdelegaten.
Till exempel om du har implementerat hello ovan förslag 1:

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        // Any other code

        [UNUserNotificationCenter currentNotificationCenter].delegate = self;
        return YES;
      }

## <a name="how-toocustomize-campaigns"></a>Hur toocustomize kampanjer
### <a name="notifications"></a>Meddelanden
Det finns två typer av aviseringar: systemet och i app-meddelanden.

Systemmeddelanden hanteras av iOS och kan inte anpassas.

I appen meddelanden består av en vy som läggs till dynamiskt toohello aktuella-fönstret. Detta kallas en meddelandet överlägget. Meddelande överlägg är bra för en snabb integrering eftersom de inte kräver du toomodify vyn i ditt program.

#### <a name="layout"></a>Layout
toomodify hello utseendet på meddelanden i appen, du kan bara ändra hello filen `AENotificationView.xib` tooyour måste, förutsatt att du behåller hello taggvärden och typer av hello befintliga undervyer.

Som standard visas i appen meddelanden längst hello hello-skärmen. Om du föredrar toodisplay dem hello överst på skärmen, redigera hello tillhandahålls `AENotificationView.xib` och ändra hello `AutoSizing` egenskapen hello huvudsakliga vyn så att den kan hållas hello överst i dess superview.

#### <a name="categories"></a>Kategorier
När du ändrar hello tillhandahålls layout, kan du ändra hello utseende på alla meddelanden. Kategorier kan toodefine olika riktas söks meddelanden (möjligen beteenden). En kategori kan anges när du skapar en Reach-kampanj. Tänk på att kategorier kan du anpassa meddelanden och avsökningar, som beskrivs senare i det här dokumentet.

tooregister en kategori hanterare för dina meddelanden behöver du tooadd ett anrop när hello nå modulen har initierats.

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:@"my_category"];
    ...

`myNotifier`måste vara en instans av ett objekt som uppfyller toohello protokollet `AENotifier`.

Du kan implementera hello protokollet metoder själv eller välja tooreimplement hello befintlig klass `AEDefaultNotifier` som redan utför de flesta av hello arbete.

Om du vill tooredefine hello Aviseringsvy för en viss kategori, kan du följa det här exemplet:

    #import "AEDefaultNotifier.h"
    #import "AENotificationView.h"
    @interface MyNotifier : AEDefaultNotifier
    @end

    @implementation MyNotifier

    -(NSString*)nibNameForCategory:(NSString*)category
    {
      return "MyNotificationView";
    }

    @end

Det här enkla exemplet på kategorin förutsätter att du har en fil med namnet `MyNotificationView.xib` i programmets-paket. Om hello-metoden inte är kan toofind en motsvarande `.xib`, visas inte hello-meddelande och Engagement kommer att bli ett meddelande i hello-konsolen.

hello tillhandahålls nib filen bör beakta hello följande regler:

* Det får endast innehålla en vy.
* Undervyer ska vara av samma typer som hello viktiga inuti hello tillhandahålls nib fil med namnet hello`AENotificationView.xib`
* Undervyer ska ha hello samma taggar som hello viktiga inuti hello tillhandahålls nib fil med namnet`AENotificationView.xib`

> [!TIP]
> Bara kopiera hello angivna nib-filen som heter `AENotificationView.xib`, och börjar arbeta därifrån. Men tänk på att, hello visa innanför nib filen hör toohello klassen `AENotificationView`. Den här klassen omdefinieras hello metoden `layoutSubViews` toomove och ändra storlek på dess undervyer enligt toocontext. Du kanske vill tooreplace den med en `UIView` eller en anpassad vy klass.
>
>

Om du behöver djupare anpassning av meddelanden (om du vill att till exempel tooload vyn direkt från hello kod), bör tootake en titt på hello tillhandahålls källa kod och klassen i dokumentationen `Protocol ReferencesDefaultNotifier` och `AENotifier`.

Observera att du kan använda hello samma meddelaren för flera kategorier.

Du kan också omdefinierade hello standard meddelaren så här:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerNotifier:myNotifier forCategory:kAEReachDefaultCategory];

##### <a name="notification-handling"></a>Meddelande-hantering
När du använder hello standardkategori, vissa livscykel metoder anropas på hello `AEReachContent` objekt tooreport statistik och uppdatera hello kampanj tillstånd:

* När hello-meddelande visas i programmet hello `displayNotification` metoden anropas (som rapporterar statistik) av `AEReachModule` om `handleNotification:` returnerar `YES`.
* Om hello-meddelande avvisas hello `exitNotification` metoden anropas statistik rapporteras och nästa kampanjer kan nu bearbetas.
* Om du hello-meddelande `actionNotification` är anropas statistik rapporteras och hello associerad åtgärd utförs.

Om din implementering av `AENotifier` kringgår hello standardbeteendet har du toocall metoderna livscykel själv. hello som följande exempel visar tillfällen där hello standardbeteendet kringgås:

* Du inte utökar `AEDefaultNotifier`, t.ex. du har implementerat kategori hantering från grunden.
* Du overrode `prepareNotificationView:forContent:`, vara säker på att toomap minst `onNotificationActioned` eller `onNotificationExited` tooone U.I-kontroller.

> [!WARNING]
> Om `handleNotification:` returnerar ett undantag, hello innehåll tas bort och `drop` är anropas detta rapporteras i statistik och nästa kampanjer kan nu bearbetas.
>
>

#### <a name="include-notification-as-part-of-an-existing-view"></a>Inkludera meddelande som en del av en befintlig vy
Överlägg är bra för en snabb integrering men kan ibland inte vara praktiskt eller kan orsaka oönskade sidoeffekter.

Om du inte är nöjd med hello överlägget systemet i några av dina vyer kan anpassa du den för dessa vyer.

Du kan bestämma tooinclude layout för våra notification i befintliga vyer. toodo, så det finns två implementering format:

1. Lägg till hello notisvy med hjälp av gränssnittet builder

   * Öppna *gränssnitt Builder*
   * Placera 320 x 60 (eller 768 x 60 om du är på iPad) `UIView` där du vill att tooappear för hello-meddelande
   * Ange hello taggvärde för den här vyn för: **36822491**
2. Lägg till hello notisvy programmässigt. Lägg bara till hello efter koden när vyn har initierats:

       UIView* notificationView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 320, 60)]; //Replace x and y coordinate values tooyour needs.
       notificationView.tag = NOTIFICATION_AREA_VIEW_TAG;
       [self.view addSubview:notificationView];

`NOTIFICATION_AREA_VIEW_TAG`makrot finns i `AEDefaultNotifier.h`.

> [!NOTE]
> hello standard meddelaren identifierar automatiskt hello notification layouten ingår i den här vyn och kommer inte att lägga till ett överlägg för den.
>
>

### <a name="announcements-and-polls"></a>Meddelanden och avsökningar
#### <a name="layouts"></a>Layouter
Du kan ändra hello filer `AEDefaultAnnouncementView.xib` och `AEDefaultPollView.xib` så länge som du behåller hello taggvärden och typer av hello befintliga undervyer.

#### <a name="categories"></a>Kategorier
##### <a name="alternate-layouts"></a>Alternativa layouter
Hello kampanj kategori kan vara används toohave alternativa layouter för meddelanden och avsökningar som meddelanden.

toocreate en kategori för ett meddelande måste du utöka **AEAnnouncementViewController** och registrera den när hello reach-modulen har initierats:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:@"my_category"];

> [!NOTE]
> Varje gång en användare kommer klickar du på ett meddelande för ett meddelande med hello kategori ”min\_kategori”, registrerade view-controller (i så fall `MyCustomAnnouncementViewController`) initieras genom att anropa metoden hello `initWithAnnouncement:` och hello vyn kommer att vara tillagda toohello aktuella-fönstret.
>
>

I implementeringen av hello `AEAnnouncementViewController` klass har tooread hello egenskapen `announcement` tooinitialize din undervyer. Överväg att hello exemplet nedan, där två etiketter initierats med `title` och `body` egenskaper för hello `AEReachAnnouncement` klass:

    -(void)loadView
    {
        [super loadView];

        UILabel* titleLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        titleLabel.font = [UIFont systemFontOfSize:32.0];
        titleLabel.text = self.announcement.title;

        UILabel* bodyLabel = [[UILabel alloc] initWithFrame:CGRectMake(10, 20, 300, 60)];
        bodyLabel.font = [UIFont systemFontOfSize:24.0];
        bodyLabel.text = self.announcement.body;

        [self.view addSubview:titleLabel];
        [self.view addSubview:bodyLabel];
    }

Om du inte vill tooload dina vyer själv, men du bara vill tooreuse hello standard meddelande Visa layout, kan du bara se styrenheten för anpassad vy utökar hello tillhandahålls klassen `AEDefaultAnnouncementViewController`. I så fall duplicera hello nib filen `AEDefaultAnnouncementView.xib` och Byt namn på den så den kan läsas av anpassade view-controller (för en domänkontrollant med namnet `CustomAnnouncementViewController`, ska du anropa nib filen `CustomAnnouncementView.xib`).

tooreplace hello Standardkategori för meddelanden, helt enkelt registrera styrenheten anpassad vy för hello kategori som definierats i `kAEReachDefaultCategory`:

    [reach registerAnnouncementController:[MyCustomAnnouncementViewController class] forCategory:kAEReachDefaultCategory];

Omröstningar kan vara anpassade hello samma sätt:

    AEReachModule* reach = [AEReachModule moduleWithNotificationIcon:[UIImage imageNamed:@"icon.png"]];
    [reach registerPollController:[MyCustomPollViewController class] forCategory:@"my_category"];

Den här gången kan hello tillhandahålls `MyCustomPollViewController` måste utöka `AEPollViewController`. Du kan också välja tooextend från hello standard domänkontrollant: `AEDefaultPollViewController`.

> [!IMPORTANT]
> Glöm inte toocall antingen `action` (`submitAnswers:` för anpassade avsökningsdatum visa domänkontrollanter) eller `exit` metoden innan hello view controller avvisas. Annars statistik skickas inte (dvs. inga analyser på hello kampanj) och mer allt nästa kampanjer meddelas inte förrän hello programprocessen har startats om.
>
>

##### <a name="implementation-example"></a>Exempel på implementering
I den här implementeringen hello anpassade meddelande vyn har lästs in från en extern xib-fil.

För avancerade notification anpassning bör som toolook på hello källkoden för hello standardimplementeringen.

`CustomAnnouncementViewController.h`

    //Interface
    @interface CustomAnnouncementViewController : AEAnnouncementViewController {
      UILabel* titleLabel;
      UITextView* descTextView;
      UIWebView* htmlWebView;
      UIButton* okButton;
      UIButton* cancelButton;
    }

    @property (nonatomic, retain) IBOutlet UILabel* titleLabel;
    @property (nonatomic, retain) IBOutlet UITextView* descTextView;
    @property (nonatomic, retain) IBOutlet UIWebView* htmlWebView;
    @property (nonatomic, retain) IBOutlet UIButton* okButton;
    @property (nonatomic, retain) IBOutlet UIButton* cancelButton;

    -(IBAction)okButtonClicked:(id)sender;
    -(IBAction)cancelButtonClicked:(id)sender;

`CustomAnnouncementViewController.m`

    //Implementation
    @implementation CustomAnnouncementViewController
    @synthesize titleLabel;
    @synthesize descTextView;
    @synthesize htmlWebView;
    @synthesize okButton;
    @synthesize cancelButton;

    -(id)initWithAnnouncement:(AEReachAnnouncement*)anAnnouncement
    {
      self = [super initWithNibName:@"CustomAnnouncementViewController" bundle:nil];
      if (self != nil) {
        self.announcement = anAnnouncement;
      }
      return self;
    }

    - (void) dealloc
    {
      [titleLabel release];
      [descTextView release];
      [htmlWebView release];
      [okButton release];
      [cancelButton release];
      [super dealloc];
    }

    - (void)viewDidLoad {
      [super viewDidLoad];

      /* Init announcement title */
      titleLabel.text = self.announcement.title;

      /* Init announcement body */
      if(self.announcement.type == AEAnnouncementTypeHtml)
      {
        titleLabel.hidden = YES;
        htmlWebView.hidden = NO;
        [htmlWebView loadHTMLString:self.announcement.body baseURL:[NSURL URLWithString:@"http://localhost/"]];
      }
      else
      {
        titleLabel.hidden = NO;
        htmlWebView.hidden = YES;
        descTextView.text = self.announcement.body;
      }

      /* Set action button label */
      if([self.announcement.actionLabel length] > 0)
        [okButton setTitle:self.announcement.actionLabel forState:UIControlStateNormal];

      /* Set exit button label */
      if([self.announcement.exitLabel length] > 0)
        [cancelButton setTitle:self.announcement.exitLabel forState:UIControlStateNormal];
    }

    #pragma mark Actions

    -(IBAction)okButtonClicked:(id)sender
    {
        [self action];
    }

    -(IBAction)cancelButtonClicked:(id)sender
    {
        [self exit];
    }

    @end
