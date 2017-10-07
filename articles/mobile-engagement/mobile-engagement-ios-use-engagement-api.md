---
title: "aaaHow tooUse hello Engagement API på iOS"
description: "Senaste iOS SDK - hur tooUse hello Engagement API på iOS"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1fb4509e-3804-46c1-949f-1cf727f91f9f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 7fb9b95ad319cf3b1e2de81b5d6aee5b30266069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-ios"></a>Hur tooUse hello Engagement API på iOS
Det här dokumentet är ett tillägg toohello hur tooIntegrate Engagement för iOS: ger i djup information om hur hello toouse Engagement API tooreport programstatistik.

Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information, sedan hello enklaste vägen är toomake dina anpassade `UIViewController` objekt ärver från hello motsvarande `EngagementViewController` klass .

Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementViewController` klasser måste toouse hello Engagement API.

Hej Engagement API tillhandahålls av hello `EngagementAgent` klass. En instans av den här klassen kan hämtas genom att anropa hello `[EngagementAgent shared]` statisk metod (Observera att hello `EngagementAgent` objektet som returnerades är en singleton).

Innan alla API-anrop, hello `EngagementAgent` objektet måste initieras genom att anropa metoden hello`[EngagementAgent init:@"Endpoint={YOUR_APP_COLLECTION.DOMAIN};SdkKey={YOUR_SDK_KEY};AppId={YOUR_APPID}"];`

## <a name="engagement-concepts"></a>Koncept i engagement
hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md) för hello iOS-plattformen.

### <a name="session-and-activity"></a>`Session` och `Activity`
En *aktiviteten* är ofta kopplade till en skärm på hello-program som är toosay hello *aktiviteten* när hello-skärmen visas och stoppas när hello-skärmen stängs: Detta är hello fall när Hej Engagement SDK är integrerad med hjälp av hello `EngagementViewController` klasser.

Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API. Detta gör toosplit en viss skärm i flera sub delar tooget mer information om hello användning av den här skärmen (till exempel hur ofta tooknown och hur länge dialogrutor används i den här skärmen).

## <a name="reporting-activities"></a>Rapportering
### <a name="user-starts-a-new-activity"></a>Användaren startar en ny aktivitet
            [[EngagementAgent shared] startActivity:@"MyUserActivity" extras:nil];

Du behöver toocall `startActivity()` varje gång hello användaraktivitet ändras. hello första anropet toothis funktionen startar en ny session.

### <a name="user-ends-his-current-activity"></a>Användaren avslutar sin aktuella aktiviteten
            [[EngagementAgent shared] endActivity];

> [!WARNING]
> Du bör **aldrig** anropa den här funktionen själv, utom om du vill toosplit den användning av programmet till flera sessioner: ett anrop toothis funktionen skulle avslutas hello aktuell session omedelbart, så ett efterföljande anrop för`startActivity()`kan starta en ny session. Den här funktionen anropas automatiskt av hello SDK när programmet stängs.
> 
> 

## <a name="reporting-events"></a>Rapporteringshändelser
### <a name="session-events"></a>Sessionshändelser
Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.

**Exempel utan extra data:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:nil];
            ...
       }
       [...]
    }

**Exempel med extra data:**

    @implementation MyViewController {
       [...]
       - (void)willRotateToInterfaceOrientation:(UIInterfaceOrientation)toInterfaceOrientation duration:(NSTimeInterval)duration
       {
        [super willRotateToInterfaceOrientation:toInterfaceOrientation duration:duration];
            ...
        NSMutableDictionary* extras = [NSMutableDictionary dictionary];
        [extras setObject:[NSNumber numberWithInt:toInterfaceOrientation] forKey:@"to_orientation_id"];
        [extras setObject:[NSNumber numberWithDouble:duration] forKey:@"duration"];
        [[EngagementAgent shared] sendSessionEvent:@"will_rotate" extras:extras];
            ...
       }
       [...]
    }

### <a name="standalone-events"></a>Fristående händelser
Motstridiga toosession händelser, fristående händelser kan användas utanför hello kontexten för en session.

**Exempel:**

    [[EngagementAgent shared] sendEvent:@"received_notification" extras:nil];

## <a name="reporting-errors"></a>Rapporterat fel
### <a name="session-errors"></a>Sessionen fel
Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.

**Exempel:**

    /** hello user has entered invalid data in a form */
    @implementation MyViewController {
      [...]
      -(void)onMyFormSubmitted:(MyForm*)form {
        [...]
        /* hello user has entered an invalid email address */
        [[EngagementAgent shared] sendSessionError:@"sign_up_email" extras:nil]
        [...]
      }
      [...]
    }

### <a name="standalone-errors"></a>Fristående fel
Motstridiga toosession fel, fristående fel kan användas utanför hello kontexten för en session.

**Exempel:**

    [[EngagementAgent shared] sendError:@"something_failed" extras:nil];

## <a name="reporting-jobs"></a>Rapportering av jobb
**Exempel:**

Anta att du vill tooreport hello varaktighet logga in:

    [...]
    -(void)signIn
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      [... sign in ...]

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    }
    [...]

### <a name="report-errors-during-a-job"></a>Rapportera fel under ett jobb
Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.

**Exempel:**

Anta att du vill ha tooreport ett fel under din inloggningen:

    [...]
    -(void)signin
    {
      /* Start job */
      [[EngagementAgent shared] startJob:@"sign_in" extras:nil];

      BOOL success = NO;
      while (!success) {
        /* Try toosign in */
        NSError* error = nil;
        [self trySigin:&error];
        success = error == nil;

        /* If an error occured report it */
        if(!success)
        {
          [[EngagementAgent shared] sendJobError:@"sign_in_error"
                         jobName:@"sign_in"
                          extras:[NSDictionary dictionaryWithObject:[error localizedDescription] forKey:@"error"]];

          /* Retry after a moment */
          [NSThread sleepForTimeInterval:20];
        }
      }

      /* End job */
      [[EngagementAgent shared] endJob:@"sign_in"];
    };
    [...]

### <a name="events-during-a-job"></a>Händelser under ett jobb
Händelser kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.

**Exempel:**

Anta att vi har ett sociala nätverk och vi använder en jobbet tooreport hello total tid under vilken hello användare är anslutna toohello server. hello användare kan ta emot meddelanden från sina vänner, detta är en jobbhändelse.

    [...]
    - (void) signin
    {
      [...Sign in code...]
      [[EngagementAgent shared] startJob:@"connection" extras:nil];
    }
    [...]
    - (void) signout
    {
      [...Sign out code...]
      [[EngagementAgent shared] endJob:@"connection"];
    }
    [...]
    - (void) onMessageReceived
    {
      [...Notify user...]
      [[EngagementAgent shared] sendJobEvent:@"connection" jobName:@"message_received" extras:nil];
    }
    [...]

## <a name="extra-parameters"></a>Extra parametrar
Godtycklig data kan vara anslutna tooevents, fel, aktiviteter och jobb.

Dessa data kan vara strukturerad, används Ios's NSDictionary klass.

Observera att tillägg kan innehålla `arrays(NSArray, NSMutableArray)`, `numbers(NSNumber class)`, `strings(NSString, NSMutableString)`, `urls(NSURL)`, `data(NSData, NSMutableData)` eller andra `NSDictionary` instanser.

> [!NOTE]
> Hej serialiseras extraparametern i JSON. Om du vill toopass olika objekt än hello som beskrivs ovan, måste du implementera hello följande metod i klassen:
> 
> -(NSString*) JSONRepresentation;
> 
> hello-metoden ska returnera en JSON-representation av objektet.
> 
> 

### <a name="example"></a>Exempel
    NSMutableDictionary* extras = [NSMutableDictionary dictionaryWithCapacity:2];
    [extras setObject:[NSNumber numberWithInt:123] forKey:@"video_id"];
    [extras setObject:@"http://foobar.com/blog" forKey:@"ref_click"];
    [[EngagementAgent shared] sendEvent:@"video_clicked" extras:extras];

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello `NSDictionary` måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Tillägg är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement agent).

I hello är 58 tecken i föregående exempel hello JSON skickas toohello server:

    {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Rapportering programinformation
Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av hello `sendAppInfo:` funktion.

Observera att denna information kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.

Som extra händelse, hello `NSDictionary` klass är används tooabstract programinformationen, Observera att matriser eller underordnade ordböcker behandlas som flat strängar (med JSON-serialisering).

**Exempel:**

    NSMutableDictionary* appInfo = [NSMutableDictionary dictionaryWithCapacity:2];
    [appInfo setObject:@"female" forKey:@"gender"];
    [appInfo setObject:@"1983-12-07" forKey:@"birthdate"]; // December 7th 1983
    [[EngagementAgent shared] sendAppInfo:appInfo];

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello `NSDictionary` måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Information om programmet är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement agent).

I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:

    {"birthdate":"1983-12-07","gender":"female"}
