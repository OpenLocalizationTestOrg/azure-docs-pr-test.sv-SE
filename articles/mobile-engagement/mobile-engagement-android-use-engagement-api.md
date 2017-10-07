---
title: "aaaHow tooUse hello Engagement API på Android"
description: "Senaste Android SDK - hur tooUse hello Engagement API på Android"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 09b62659-82ae-4a55-8784-fca0b6b22eaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: e0b2d484616c0c7874e77c5283d94c3063949ed2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-engagement-api-on-android"></a>Hur tooUse hello Engagement API på Android
Det här dokumentet är tillägg toohello [Reporting avancerade alternativ för Android Mobile Engagement SDK](mobile-engagement-android-advanced-reporting.md). Det ger i djup information om hur hello toouse Engagement API tooreport programstatistik.

Tänk på att om du bara vill Engagement tooreport programmets sessioner, aktiviteter, krascher och teknisk information sedan hello enklaste sättet är toomake alla dina `Activity` underordnade klasser ärver från hello motsvarande `EngagementActivity` klass.

Om du vill toodo mer, till exempel om du behöver tooreport programmet specifika händelser, fel och jobb, eller om du har tooreport ditt programs aktiviteter i ett annat sätt än hello en implementeras i hello `EngagementActivity` klasser måste toouse hello Engagement API.

Hej Engagement API tillhandahålls av hello `EngagementAgent` klass. En instans av den här klassen kan hämtas genom att anropa hello `EngagementAgent.getInstance(Context)` statisk metod (Observera att hello `EngagementAgent` objektet som returnerades är en singleton).

## <a name="engagement-concepts"></a>Koncept i engagement
hello följande delar förfina hello vanliga [koncept i Mobile Engagement](mobile-engagement-concepts.md), för hello Android-plattformen.

### <a name="session-and-activity"></a>`Session` och `Activity`
Om hello användaren är fler än några få sekunder inaktiv mellan två *aktiviteter*, sedan hans sekvens av *aktiviteter* är uppdelad i två olika *sessioner*. Dessa några sekunder kallas hello ”sessionstidsgränsen”.

En *aktiviteten* är ofta kopplade till en skärm på hello-program som är toosay hello *aktiviteten* när hello-skärmen visas och stoppas när hello-skärmen stängs: Detta är hello fall när Hej Engagement SDK är integrerad med hjälp av hello `EngagementActivity` klasser.

Men *aktiviteter* kan också kontrolleras manuellt med hjälp av hello Engagement API. Detta gör toosplit en viss skärm i flera sub delar tooget mer information om hello användning av den här skärmen (till exempel hur ofta tooknown och hur länge dialogrutor används i den här skärmen).

## <a name="reporting-activities"></a>Rapportering
> [!IMPORTANT]
> Du behöver inte tooreport aktiviteter som beskrivs i det här avsnittet om du använder hello `EngagementActivity` klassen och dess varianter enligt beskrivningen i hello hur tooIntegrate Engagement för Android-dokument.
> 
> 

### <a name="user-starts-a-new-activity"></a>Användaren startar en ny aktivitet
            EngagementAgent.getInstance(this).startActivity(this, "MyUserActivity", null);
            // Passing hello current activity is required for Reach toodisplay in-app notifications, passing null will postpone such announcements and polls.

Du behöver toocall `startActivity()` varje gång hello användaraktivitet ändras. hello första anropet toothis funktionen startar en ny session.

Hej bästa plats toocall den här funktionen finns på varje aktivitet `onResume` återanrop.

### <a name="user-ends-his-current-activity"></a>Användaren avslutar sin aktuella aktiviteten
            EngagementAgent.getInstance(this).endActivity();

Du behöver toocall `endActivity()` minst en gång när hello användaren är klar användarens sista aktivitet. Det informerar hello Engagement SDK att hello användare är för närvarande inaktiv och att hello användarsession måste toobe stängd när hello sessionstidsgränsen upphör att gälla (om du anropar `startActivity()` innan hello sessionstidsgränsen upphör att gälla hello-sessionen återupptas bara).

Hej bästa plats toocall den här funktionen finns på varje aktivitet `onPause` återanrop.

## <a name="reporting-events"></a>Rapporteringshändelser
### <a name="session-events"></a>Sessionshändelser
Sessionshändelser är oftast används tooreport hello åtgärder som utförs av en användare under sin session.

**Exempel utan extra data:**

            public MyActivity extends EngagementActivity {
               [...]
               @Override
               public boolean onPrepareOptionsMenu(Menu menu) {
                  getEngagementAgent().sendSessionEvent("menu_shown", null);
               }
               [...]
            }

**Exempel med extra data:**

            public MyActivity extends EngagementActivity {
              [...]
              @Override
              public boolean onMenuItemSelected(int featureId, MenuItem item) {
                Bundle extras = new Bundle();
                extras.putInt("id", item.getItemId());
                getEngagementAgent().sendSessionEvent("menu_selected", extras);
              }
              [...]
            }

### <a name="standalone-events"></a>Fristående händelser
Motstridiga toosession händelser, fristående händelser kan inträffa utanför hello kontexten för en session.

**Exempel:**

Anta att du vill tooreport händelser som inträffar när en broadcast mottagare utlöses:

            /** Triggered by Intent.ACTION_BATTERY_LOW */
            public BatteryLowReceiver extends BroadcastReceiver {
              [...]
              @Override
              public void onReceive(Context context, Intent intent) {
                EngagementAgent.getInstance(context).sendEvent("battery_low", null);
              }
              [...]
            }

## <a name="reporting-errors"></a>Rapporterat fel
### <a name="session-errors"></a>Sessionen fel
Sessionen felen är oftast används tooreport hello fel påverkar hello användaren under sin session.

**Exempel:**

            /** hello user has entered invalid data in a form */
            public MyActivity extends EngagementActivity {
              [...]
              public void onMyFormSubmitted(MyForm form) {
                [...]
                /* hello user has entered an invalid email address */
                getEngagementAgent().sendSessionError("sign_up_email", null);
                [...]
              }
              [...]
            }

### <a name="standalone-errors"></a>Fristående fel
Motstridiga toosession fel, fristående kan det uppstå fel utanför hello kontexten för en session.

**Exempel:**

hello följande exempel visas hur tooreport ett fel när hello minne blir låg på hello telefon när programmet processen körs.

            public MyApplication extends EngagementApplication {

              @Override
              protected void onApplicationProcessLowMemory() {
                EngagementAgent.getInstance(this).sendError("low_memory", null);
              }
            }

## <a name="reporting-jobs"></a>Rapportering av jobb
### <a name="example"></a>Exempel
Anta att du vill tooreport hello varaktighet logga in:

            [...]
            public void signIn(Context context, ...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has started */
              engagementAgent.startJob("sign_in", null);

              [... sign in ...]

              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="report-errors-during-a-job"></a>Rapportera fel under ett jobb
Fel kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.

**Exempel:**

Anta att du vill tooreport ett fel under du logga in processen:

[...] offentliga void inloggning (kontexten kontext,...) {

              /* We need an Android context toocall hello Engagement API, if you are extending Activity, Service, you can pass "this" */
              EngagementAgent engagementAgent = EngagementAgent.getInstance(context);

              /* Report sign in job has been started */
              engagementAgent.startJob("sign_in", null);

              /* Try toosign in */
              while(true)
                try {
                  trySignin();
                  break;
                }
                catch(Exception e) {
                  /* Report hello error tooEngagement */
                  engagementAgent.sendJobError("sign_in_error", "sign_in", null);

                  /* Retry after a moment */
                  sleep(2000);
                }
              [...]
              /* Report sign in job is now ended */
              engagementAgent.endJob("sign_in");
            }
            [...]

### <a name="reporting-events-during-a-job"></a>Rapporteringshändelser under ett jobb
Händelser kan vara relaterade tooa köra jobb i stället för som relaterade toohello aktuella användarsessionen.

**Exempel:**

Anta att vi har ett sociala nätverk och vi använder en jobbet tooreport hello total tid under vilken hello användare är anslutna toohello server. hello användaren kan vara ansluten i bakgrunden även när han använder ett annat program eller hello telefon är i viloläge, så det finns ingen session.

hello användare kan ta emot meddelanden från sina vänner, detta är en jobbhändelse.

            [...]
            public void signin(Context context, ...) {
              [...Sign in code...]
              EngagementAgent.getInstance(context).startJob("connection", null);
            }
            [...]
            public void signout(Context context) {
              [...Sign out code...]
              EngagementAgent.getInstance(context).endJob("connection");
            }
            [...]
            public void onMessageReceived(Context context) {
              [...Notify in status bar...]
              EngagementAgent.getInstance(context).sendJobEvent("message_received", "connection", null);
            }
            [...]

## <a name="extra-parameters"></a>Extra parametrar
Godtycklig data kan vara anslutna tooevents, fel, aktiviteter och jobb.

Dessa data kan vara strukturerad, används Androids paket klass (faktiskt, den fungerar som extra parametrar i Android avsikter). Observera att ett paket kan innehålla matriser eller ett annat paket instanser.

> [!IMPORTANT]
> Om du lägger till i parcelable eller serializable parametrar, kontrollerar du att deras `toString()` metoden är implementerad tooreturn läsbart sträng. Serialiserbara klasser som innehåller icke tillfälligt fält som inte kan serialiseras gör Android kraschar när du anropar`bundle.putSerializable("key",value);`
> 
> [!WARNING]
> Glesa matriser i extra parametrar stöds inte, det vill säga den kommer inte att serialisera som en matris. Du måste konvertera dem till standard matriser innan den används i extra parametrar.
> 
> 

### <a name="example"></a>Exempel
            Bundle extras = new Bundle();
            extras.putString("video_id", 123);
            extras.putString("ref_click", "http://foobar.com/blog");
            EngagementAgent.getInstance(context).sendEvent("video_clicked", extras);

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello `Bundle` måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Tillägg är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement service).

I hello är 58 tecken i föregående exempel hello JSON skickas toohello server:

            {"ref_click":"http:\/\/foobar.com\/blog","video_id":"123"}

## <a name="reporting-application-information"></a>Rapportering programinformation
Du kan manuellt rapportera spåra information (eller andra program specifik information) med hjälp av hello `sendAppInfo()` funktion.

Observera att denna information kan skickas inkrementellt: endast hello senaste värdet för en viss nyckel sparas för en viss enhet.

Hello paket klass är används tooabstract programinformation som händelsen tillägg, Observera att matriser eller underordnad paketerar behandlas som flat strängar (med JSON-serialisering).

### <a name="example"></a>Exempel
Här är en kod exempel toosend användaren kön och födelsedatum:

            Bundle appInfo = new Bundle();
            appInfo.putString("status", "premium");
            appInfo.putString("expiration", "2016-12-07"); // December 7th 2016
            EngagementAgent.getInstance(context).sendAppInfo(appInfo);

### <a name="limits"></a>Begränsningar
#### <a name="keys"></a>Nycklar
Varje nyckel i hello `Bundle` måste matcha hello följande reguljära uttryck:

`^[a-zA-Z][a-zA-Z_0-9]*`

Det innebär att nycklar måste börja med minst en bokstav, följt av bokstäver, siffror eller understreck (\_).

#### <a name="size"></a>Storlek
Information om programmet är begränsad för**1024** tecken per anrop (efter kodning i JSON av hello Engagement service).

I hello är 44 tecken i föregående exempel hello JSON skickas toohello server:

            {"expiration":"2016-12-07","status":"premium"}
