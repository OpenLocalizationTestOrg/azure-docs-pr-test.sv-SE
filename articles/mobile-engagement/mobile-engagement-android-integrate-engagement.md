---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 4f79936ea0fa6102023dec2b4682032a4a81fa9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-engagement-on-android"></a>Hur tooIntegrate Engagement på Android
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i din Android-App.

> [!IMPORTANT]
> Den lägsta Android SDK-API-nivån måste vara 10 eller senare (Android 2.3.3 eller högre).
> 
> 

hello följande är tillräckligt med tooactivates hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals. hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement i din Android-märkning API](mobile-engagement-android-use-engagement-api.md) eftersom dessa statistik är programberoende.

## <a name="embed-hello-engagement-sdk-and-service-into-your-android-project"></a>Bädda in hello Engagement SDK och tjänsten i din Android-projekt
Hämta hello Android SDK från [här](https://aka.ms/vq9mfn) hämta `mobile-engagement-VERSION.jar` och placera dem i hello `libs` mappen för din Android-projekt (skapa hello biblioteksmappen om det inte finns ännu).

> [!IMPORTANT]
> Om du skapar ditt programpaket med ProGuard måste tookeep vissa klasser. Du kan använda följande konfiguration fragment hello:
> 
> -Behåll publik klass * utökar android.os.IInterface-hålla klassen com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
> 
> <methods>; }
> 
> 

Ange anslutningssträngen Engagement genom att anropa hello följande metod i hello starta aktiviteten:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

hello anslutningssträngen för ditt program visas på Azure Portal.

* Om saknas, lägger du till följande Android behörigheter hello (innan hello `<application>` tagg):
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* Lägg till följande avsnitt hello (mellan hello `<application>` och `</application>` taggar):
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* Ändra `<Your application name>` av hello namnet på ditt program.

> [!TIP]
> Hej `android:label` attribut kan du toochoose hello namnet på hello Engagement tjänsten som det visas toohello slutanvändare i hello ”körs tjänster” skärmen i sin telefon. Det är rekommenderat tooset attributet för`"<Your application name>Service"` (t.ex. `"AcmeFunGameService"`).
> 
> 

Att ange hello `android:process` attributet säkerställer att hello Engagement tjänsten kommer att köras i en egen process (kör Engagement hello samma process som programmet gör main/UI-tråden potentiellt mindre känslig).

> [!NOTE]
> All kod som du `Application.onCreate()` och andra program återanrop ska köras för ditt programs processer, inklusive hello Engagement service. Det kan ha oönskade sidoeffekter (till exempel onödiga minnesallokering och trådar i hello Engagement process, dubbla broadcast mottagare eller tjänster).
> 
> 

Om du åsidosätter `Application.onCreate()`, är det rekommenderade tooadd hello följande kodavsnitt hello början av din `Application.onCreate()` funktionen:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

Du kan göra samma sak för hello `Application.onTerminate()`, `Application.onLowMemory()` och `Application.onConfigurationChanged(...)`.

Du kan också utöka `EngagementApplication` i stället för att utöka `Application`: hello återanrop `Application.onCreate()` hello processen kontroll och anrop `Application.onApplicationProcessCreate()` endast om hello aktuella processen inte hello en hello Engagement värdtjänsten hello samma regler gäller för Hej andra återanrop.

## <a name="basic-reporting"></a>Grundläggande reporting
### <a name="recommended-method-overload-your-activity-classes"></a>Rekommenderad metod: överlagra din `Activity` klasser
I ordning tooactivate hello rapport alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik du precis har toomake alla dina `*Activity` underordnade klasser ärver från hello motsvarande `Engagement*Activity` klasser (t.ex. Om din bakåtkompatibelt utökar `ListActivity`, se den utökar `EngagementListActivity`).

**Utan Engagement:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Med Engagement:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> När du använder `EngagementListActivity` eller `EngagementExpandableListActivity`, kontrollera anrop för`requestWindowFeature(...);` görs före hello anrop för`super.onCreate(...);`, annars en krasch inträffar.
> 
> 

Du hittar de här klasserna i hello `src` mappen och kopiera dem till ditt projekt. hello klasser finns också i hello **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternativ metod: anropa `startActivity()` och `endActivity()` manuellt
Om du inte kan eller inte vill att toooverload din `Activity` klasser, kan du i stället börja och sluta dina aktiviteter genom att anropa `EngagementAgent`'s metoder direkt.

> [!IMPORTANT]
> hello Android SDK anropar aldrig hello `endActivity()` metod, även om programmet hello avslutas (på Android program faktiskt aldrig stängs). Därför är det *hög* rekommenderas toocall hello `startActivity()` metod i hello `onResume` återanrop av *alla* dina aktiviteter och hello `endActivity()` metod i hello `onPause()` återanrop av *alla* dina aktiviteter. Detta är hello endast sätt toobe till att sessioner inte spridas. Om en session är känd koppla hello Engagement service aldrig från hello Engagement-serverdelen (eftersom hello tjänsten fortfarande ansluten så länge en session väntar).
> 
> 

Här är ett exempel:

            public class MyActivity extends Some3rdPartyActivity
            {
              @Override
              protected void onResume()
              {
                super.onResume();
                String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
                EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
              }

              @Override
              protected void onPause()
              {
                super.onPause();
                EngagementAgent.getInstance(this).endActivity();
              }
            }

Det här exemplet mycket lik toohello `EngagementActivity` klassen och dess varianter vars källkoden har angetts i hello `src` mapp.

## <a name="test"></a>Testa
Nu verifiera din integrering genom att köra din mobila app i en emulator eller en enhet och verifiera att den registreras en session på fliken för övervakning av hello.

hello nästa avsnitt är valfria.

## <a name="location-reporting"></a>Platsrapportering
Om du vill att platser toobe rapporterade måste tooadd configuration några rader (mellan hello `<application>` och `</application>` taggar).

### <a name="lazy-area-location-reporting"></a>Områdesrapportering
Områdesrapportering tillåter tooreport hello land, region och plats som är associerade toodevices. Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi). hello enheten området rapporteras högst en gång per session. hello GPS aldrig används och därmed den här typen av plats rapport har bara ett fåtal (inte toosay inga) inverkan på hello batteri.

Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel. De kan också användas som kriterium i Reach-kampanjer.

tooenable lazy Områdesplats rapportering, du kan göra det med hjälp av hello configuration ovan i den här proceduren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Du måste också tooadd hello efter behörighet om saknas:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Eller du kan fortsätta att använda ``ACCESS_FINE_LOCATION`` om du redan använder den i ditt program.

### <a name="real-time-location-reporting"></a>Rapportering av platsen i realtid
Realtid plats kan tooreport hello latitud och longitud associerade toodevices. Den här typen av plats reporting använder endast nätverksplatser (baserat på cellen ID eller Wi-Fi) som standard och hello reporting är bara aktivt när hello-program körs i förgrunden (dvs. under en session).

Realtid platser är *inte* används toocompute statistik. Syftet endast är tooallow hello användning av realtid geobegränsning \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.

tooenable realtid plats rapportering, du kan göra det med hjälp av hello configuration ovan i den här proceduren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Du måste också tooadd hello efter behörighet om saknas:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Eller du kan fortsätta att använda ``ACCESS_FINE_LOCATION`` om du redan använder den i ditt program.

#### <a name="gps-based-reporting"></a>GPS baseras reporting
Som standard används rapportering av platsen i realtid endast baserat nätverksplatser. tooenable hello användning av GPS-baserade platser (vilket är mycket mer exakt), använder hello konfigurationsobjekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Du måste också tooadd hello efter behörighet om saknas:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Rapportering i bakgrunden
Som standard är realtid plats reporting bara aktivt när hello-program körs i förgrunden (dvs. under en session). tooenable hello reporting även i bakgrunden, använda hello konfigurationsobjekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> När hello-program körs i bakgrunden, endast baserat nätverksplatser rapporteras, även om du har aktiverat hello GPS.
> 
> 

hello bakgrund plats rapporten kommer att stoppas om hello användaren startar om sin enhet, kan du lägga till den här toomake den startas om när datorn startas:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

Du måste också tooadd hello efter behörighet om saknas:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M-behörigheter
Från och med Android M kan vissa behörigheter hanteras under körning och måste användare att godkänna.

hello runtime behörigheter inaktiveras som standard för nya installationer av appar om du riktar Android API-nivå 23. Annars aktiveras som standard.

hello användare kan aktivera och inaktivera dessa behörigheter hello enheten inställningsmenyn. Om du inaktiverar behörigheter från system-menyn stängs bakgrundsprocesser av programmet hello, det här är ett systembeteende och har ingen inverkan på möjlighet tooreceive push i bakgrunden.

Hello gäller Mobile Engagement är är hello behörigheten som kräver godkännande vid körning:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* `WRITE_EXTERNAL_STORAGE`(endast när måldatorn Android API-nivå 23 för den här)

hello extern lagring används endast för Reach helheten funktionen. Om du hittar genom att be användarna den här behörigheten toobe störande, du kan ta bort den om används endast för Mobile Engagement men på hello kostnaden för att inaktivera funktionen för stor bild.

För hello plats funktioner, bör du begära behörigheter toouser med en standard systemdialogruta. Om hello användaren godkänner du behöver tootell ``EngagementAgent`` tootake ändra hänsyn verkliga tidpunkt (annars hello ändringen kommer att bearbetade hello nästa gång hello startar hello användarprogram).

Här är en kod exempel toouse i en aktivitet i dina program toorequest behörigheter och vidarebefordra hello resultat om positivt för``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this won't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want tookeep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a>Avancerad rapportering
Du kan också om du vill tooreport programmet specifika händelser, fel och jobb, måste toouse hello Engagement API via hello metoder för hello `EngagementAgent` klass. Ett objekt av den här klassen kan vara retreived genom att anropa hello `EngagementAgent.getInstance()` statisk metod.

Hej Engagement API kan toouse alla Engagement avancerade funktioner och beskrivs i hello hur tooUse Engagement API på Android (samt som i hello tekniska dokumentationen för hello `EngagementAgent` klassen).

## <a name="advanced-configuration-in-androidmanifestxml"></a>Avancerad konfiguration (i AndroidManifest.xml)
### <a name="wake-locks"></a>Aktivera Lås
Om du vill att statistik skickas i realtid vid användning av Wi-Fi eller när hello-skärmen av toobe, lägger du till följande valfria behörighet hello:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Kraschrapport
Om du vill toodisable kraschrapporter, Lägg till denna (mellan hello `<application>` och `</application>` taggar):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst tröskelvärde
Som standard loggas hello Engagement service-rapporter i realtid. Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”). toodo så, Lägg till denna (mellan hello `<application>` och `</application>` taggar):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas). Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s).

### <a name="session-timeout"></a>Tidsgräns för session
Som standard är en session avslutades 10-tal efter hello slutet av den senaste aktiviteten (som vanligtvis genom att trycka på hello start eller säkerhetskopiera nyckeln genom att ange hello phone inaktiv eller genom att hoppa över i ett annat program). Det här är tooavoid en session-delning varje gång hello användare Avsluta och återgå toohello program snabbt (som kan hända när han hämta en avbildning, kontrollera en avisering, osv.). Du kanske vill toomodify den här parametern. toodo så, Lägg till denna (mellan hello `<application>` och `</application>` taggar):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Inaktivera rapportering av logg
### <a name="using-a-method-call"></a>Med hjälp av ett metodanrop
Om du vill Engagement toostop skicka loggar, kan du anropa:

            EngagementAgent.getInstance(context).setEnabled(false);

Det här anropet är beständiga: filen delade inställningar används.

Det kan ta en minut för hello service toostop om Engagement är aktiv när du anropar den här funktionen. Men det kommer inte att starta hello-tjänsten på alla hello nästa gång du startar programmet hello.

Du kan aktivera loggen reporting igen genom att anropa hello samma fungerar med `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integrering i din egen`PreferenceActivity`
I stället för att anropa den här funktionen kan du också integrera inställningen direkt i din befintliga `PreferenceActivity`.

Du kan konfigurera Engagement toouse inställningarna filen (med hello läge) i hello `AndroidManifest.xml` filen med `application meta-data`:

* Hej `engagement:agent:settings:name` nyckeln är används toodefine hello namnet på hello delade inställningsfilen.
* Hej `engagement:agent:settings:mode` nyckeln är används toodefine hello läge hello delade inställningsfilen bör du använda hello samma läge som i din `PreferenceActivity`. hello-läge måste skickas som ett tal: Om du använder en kombination av konstant flaggor i koden, kontrollera hello totala värdet.

Engagement alltid använda hello `engagement:key` booleskt nyckel i hello inställningsfilen för att hantera den här inställningen.

Hej följande exempel på `AndroidManifest.xml` visar hello standardvärden:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Du kan lägga till en `CheckBoxPreference` i layouten inställningar som hello efter:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
