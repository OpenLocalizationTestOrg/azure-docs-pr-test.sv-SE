---
title: aaaAzure Mobile Engagement iOS SDK-Integration | Microsoft Docs
description: "Senaste uppdateringarna och procedurer för iOS SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 947ea44b-00c1-450f-9a3b-74437954dc56
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.openlocfilehash: 66ce34efabede7d882caa8a91431a8df71e4fb59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-ios"></a>Hur tooIntegrate Engagement för iOS
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
>
>

Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i ditt iOS-program.

Hej Engagement SDK kräver iOS7 + och Xcode 8 +: hello distributionsmålet för ditt program måste vara minst iOS 7.

> [!NOTE]
> Om du verkligen är beroende av XCode 7 så att du kan använda hello [iOS Engagement SDK v3.2.4](https://aka.ms/r6oouh). Det finns ett känt fel på hello Reach-modulen för den här tidigare version när du kör på iOS 10 enheter Se [hello reach-modulen integration](mobile-engagement-ios-integrate-engagement-reach.md) för mer information. Om du väljer toouse hello SDK v3.2.4 bara hoppa hello `UserNotifications.framework` importera i hello nästa steg.
>
>

hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals. hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i din iOS-app](mobile-engagement-ios-use-engagement-api.md) sedan statistiken är programmet beroende.

## <a name="embed-hello-engagement-sdk-into-your-ios-project"></a>Bädda in hello Engagement SDK i din iOS-projekt
* Hämta hello iOS SDK från [här](http://aka.ms/qk2rnj).
* Lägg till hello Engagement SDK tooyour iOS-projektet: i Xcode, högerklicka på projektet och välj **”Lägg till filer för...”** och välj hello `EngagementSDK` mapp.
* Engagement kräver ytterligare ramverk toowork: i hello Projektutforskaren, öppna ditt projekt och välj hello rätt mål. Öppna hello **”Build-faser”** fliken och i hello **”länka binär med bibliotek”** menyn Lägg till dessa ramverk:

  * `UserNotifications.framework`-Ange hello länka som`Optional`
  * `AdSupport.framework`-Ange hello länka som`Optional`
  * `SystemConfiguration.framework`
  * `CoreTelephony.framework`
  * `CFNetwork.framework`
  * `CoreLocation.framework`
  * `libxml2.dylib`

> [!NOTE]
> Hej AdSupport framework kan tas bort. Engagement måste den här framework toocollect hello IDFA. Men du kan inaktivera IDFA-insamling \<ios-sdk-engagement-idfa\> toocomply med hello nya Apple policy för detta ID.
>
>

## <a name="initialize-hello-engagement-sdk"></a>Initiera hello Engagement SDK
Du behöver toomodify Programdelegaten:

* Importera hello Engagement agent hello över din implementering fil:

      [...]
      #import "EngagementAgent.h"
* Initiera Engagement inuti hello metoden '**applicationDidFinishLaunching:**'eller'**program: didFinishLaunchingWithOptions:**':

      - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
      {
        [...]
        [EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];
        [...]
      }

## <a name="basic-reporting"></a>Grundläggande reporting
### <a name="recommended-method-overload-your-uiviewcontroller-classes"></a>Rekommenderad metod: överlagra din `UIViewController` klasser
I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `UIViewController` underordnade klasser ärver från hello `EngagementViewController` klasser (samma regel för `UITableViewController`  - \> `EngagementTableViewController`).

**Utan Engagement:**

    #import <UIKit/UIKit.h>

    @interface Tab1ViewController : UIViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

**Med Engagement:**

    #import <UIKit/UIKit.h>
    #import "EngagementViewController.h"

    @interface Tab1ViewController : EngagementViewController<UITextFieldDelegate> {
      UITextField* myTextField1;
      UITextField* myTextField2;
    }

    @property (nonatomic, retain) IBOutlet UITextField* myTextField1;
    @property (nonatomic, retain) IBOutlet UITextField* myTextField2;

### <a name="alternate-method-call-startactivity-manually"></a>Alternativ metod: anropa `startActivity()` manuellt
Om du inte kan eller inte vill att toooverload din `UIViewController` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent`'s metoder direkt.

> [!IMPORTANT]
> hello iOS SDK automatiskt anropar hello `endActivity()` metod när hello programmet stängs. Därför är det *hög* rekommenderas toocall hello `startActivity` metod när hello aktiviteten hos hello användaren ändrar och för*aldrig* anrop hello `endActivity` metoden sedan anropar den här metoden tvingar hello aktuell session toobe avslutades.
>
>

## <a name="location-reporting"></a>Platsrapportering
Apple användarvillkoren tillåter inte program toouse spårning för endast statistik ändamål. Därför rekommenderas tooenable plats rapporter bara om programmet använder också hello spårning av en annan anledning.

Från och med iOS 8, måste du ange en beskrivning av hur din app använder platstjänster genom att ange en sträng för nyckeln hello [NSLocationWhenInUseUsageDescription] eller [NSLocationAlwaysUsageDescription]i filen Info.plist för din app. Om du vill tooreport plats i hello bakgrund med Engagement kan du lägga till hello nyckel NSLocationAlwaysUsageDescription. Lägga till hello nyckel NSLocationWhenInUseUsageDescription i andra fall. Observera att du måste både NSLocationAlwaysAndWhenInUseUsageDescription och NSLocationWhenInUseUsageDescription tooreport bakgrund plats på iOS 11.

### <a name="lazy-area-location-reporting"></a>Områdesrapportering
Områdesrapportering tillåter tooreport hello land, region och plats som är associerade toodevices. Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi). hello enheten området rapporteras högst en gång per session. hello GPS aldrig används och därmed den här typen av plats rapport har bara ett fåtal (inte toosay inga) inverkan på hello batteri.

Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel. De kan också användas som kriterium i Reach-kampanjer. hello senast fungerande område som rapporterats för en enhet kan vara hämtade tack toohello [Device API].

tooenable lazy Områdesplats rapportering, Lägg till följande rad efter initiering hello Engagement agent hello:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
      [...]
      [[EngagementAgent shared] setLazyAreaLocationReport:YES];
      [...]
    }

### <a name="real-time-location-reporting"></a>Rapportering av platsen i realtid
Realtid plats kan tooreport hello latitud och longitud associerade toodevices. Den här typen av plats reporting använder endast nätverksplatser (baserat på cellen ID eller Wi-Fi) som standard och hello reporting är bara aktivt när hello-program körs i förgrunden (dvs. under en session).

Realtid platser är *inte* används toocompute statistik. Syftet endast är tooallow hello användning av realtid geobegränsning \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.

tooenable realtid plats rapportering, Lägg till följande rad efter initiering hello Engagement agent hello:

    [[EngagementAgent shared] setRealtimeLocationReport:YES];

#### <a name="gps-based-reporting"></a>GPS baseras reporting
Som standard används rapportering av platsen i realtid endast baserat nätverksplatser. tooenable hello användning av GPS-baserade platser (vilket är mycket mer exakt), lägga till:

    [[EngagementAgent shared] setFineRealtimeLocationReport:YES];

#### <a name="background-reporting"></a>Rapportering i bakgrunden
Som standard är realtid plats reporting bara aktivt när hello-program körs i förgrunden (dvs. under en session). tooenable hello reporting även i bakgrunden, lägger du till:

    [[EngagementAgent shared] setBackgroundRealtimeLocationReport:YES withLaunchOptions:launchOptions];

> [!NOTE]
> När hello-program körs i bakgrunden, endast baserat nätverksplatser rapporteras, även om du har aktiverat hello GPS.
>
>

Implementering av den här funktionen ska ringa [startMonitoringSignificantLocationChanges] när programmet blir hello bakgrund. Tänk på att den automatiskt ska starta tillämpningsprogrammet till hello bakgrund om en ny plats händelse anländer.

## <a name="advanced-reporting"></a>Avancerad rapportering
Du kan också om du vill tooreport programmet specifika händelser, fel och jobb, måste toouse hello Engagement API via hello metoder för hello `EngagementAgent` klass. Ett objekt av den här klassen kan hämtas genom att anropa hello `[EngagementAgent shared]` statisk metod.

Hej Engagement API kan toouse alla Engagement avancerade funktioner och beskrivs i hello hur tooUse Engagement API på iOS (samt som i hello tekniska dokumentationen för hello `EngagementAgent` klassen).

## <a name="disable-idfa-collection"></a>Inaktivera IDFA-insamling
Som standard använder Engagement hello [IDFA] toouniquely identifiera en användare. Men om du inte använder annonserar någon annanstans i hello app kan du kanske avvisas av hello granskningsprocess App Store. IDFA-insamling kan inaktiveras genom att lägga till hello preprocessor makrot `ENGAGEMENT_DISABLE_IDFA` i filen pch (eller i hello `Build Settings` av programmet). Detta säkerställer att det inte finns några referenser för`ASIdentifierManager`, `advertisingIdentifier` eller `isAdvertisingTrackingEnabled` i hello programmet build.

Integrering i hello **prefix.pch** fil:

    #define ENGAGEMENT_DISABLE_IDFA
    ...

Du kan kontrollera att hello IDFA-insamling korrekt är inaktiverad i ditt program genom att kontrollera hello Engagement test loggar. Se hello Integration testa\<ios sdk-engagement-test-idfa\> dokumentationen för mer information.

## <a name="disable-log-reporting"></a>Inaktivera rapportering av logg
### <a name="using-a-method-call"></a>Med hjälp av ett metodanrop
Om du vill Engagement toostop skicka loggar, kan du anropa:

    [[EngagementAgent shared] setEnabled:NO];

Det här anropet är beständiga: den använder `NSUserDefaults` toostore hello information.

Du kan aktivera loggen reporting igen genom att anropa hello samma fungerar med `YES`.

### <a name="integration-in-your-settings-bundle"></a>Integrering i inställningar-paket
I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `Settings.bundle` fil. Hej sträng `engagement_agent_enabled` måste användas som en identifierare för hello-inställningar och den måste vara associerad tooa växla växel (`PSToggleSwitchSpecifier`).

Hej följande exempel på `Settings.bundle` visar hur tooimplement den:

    <dict>
        <key>PreferenceSpecifiers</key>
        <array>
            <dict>
                <key>DefaultValue</key>
                <true/>
                <key>Key</key>
                <string>engagement_agent_enabled</string>
                <key>Title</key>
                <string>Log reporting enabled</string>
                <key>Type</key>
                <string>PSToggleSwitchSpecifier</string>
            </dict>
        </array>
        <key>StringsTable</key>
        <string>Root</string>
    </dict>

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
[NSLocationWhenInUseUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26
[NSLocationAlwaysUsageDescription]:https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18
[startMonitoringSignificantLocationChanges]:http://developer.apple.com/library/IOs/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html#//apple_ref/occ/instm/CLLocationManager/startMonitoringSignificantLocationChanges
[IDFA]:https://developer.apple.com/library/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/ASIdentifierManager.html#//apple_ref/occ/instp/ASIdentifierManager/advertisingIdentifier
