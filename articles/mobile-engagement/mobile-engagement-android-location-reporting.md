---
title: "aaaLocation rapportering för Azure Mobile Engagement Android SDK"
description: "Beskriver hur tooconfigure plats rapportering för Azure Mobile Engagement Android SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: c2cb097df2a77bee2d56ffe9509dc116548db408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Plats för Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Det här avsnittet beskrivs hur toodo plats rapportering för din Android-App.

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Platsrapportering
Om du vill att platser toobe rapporterade måste tooadd configuration några rader (mellan hello `<application>` och `</application>` taggar).

### <a name="lazy-area-location-reporting"></a>Områdesrapportering
Områdesrapportering kan reporting hello land, region och plats som är associerade med enheter. Den här typen av plats reporting används endast nätverksplatser (baserat på cellen ID eller Wi-Fi). hello enheten området rapporteras högst en gång per session. hello GPS används aldrig och därmed den här typen av plats rapporten har låg påverkan på hello batteri.

Rapporterat områden är används toocompute geografiska statistik om användare, sessioner, händelser och fel. De kan också användas som kriterium i Reach-kampanjer.

Du aktiverar lazy Områdesplats rapportering med hello-konfiguration som tidigare nämnts i den här proceduren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Du måste också toospecify en plats-behörighet. Den här koden använder ``COARSE`` behörighet:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Om din app kräver det, kan du använda ``ACCESS_FINE_LOCATION`` i stället.

### <a name="real-time-location-reporting"></a>Rapportering i realtid plats
Rapportering i realtid plats kan reporting hello-latitud och longitud som är associerade med enheter. Som standard används den här typen av plats reporting endast platser i nätverket, baserat på cellen ID eller Wi-Fi. hello reporting är endast aktiv när hello-program körs i förgrunden (till exempel under en session).

Realtid platser är *inte* används toocompute statistik. Syftet endast är tooallow hello användning av geobegränsning i realtid \<Reach-målgruppen-geofencing\> kriterium i Reach-kampanjer.

tooenable realtid plats rapportering, lägga till en rad med kod toowhere som hello Engagement anslutningssträngen i hello starta aktiviteten. hello resultatet ser ut så hello följande:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need toospecify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS baseras reporting
Som standard använder rapportering i realtid plats endast nätverksbaserade platser. tooenable hello GPS-baserade platser som är mycket mer exakt, använda hello konfigurationsobjekt:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Du måste också tooadd hello efter behörighet om saknas:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Rapportering i bakgrunden
Som standard är rapportering i realtid plats bara aktivt när hello-program körs i förgrunden (till exempel under en session). tooenable hello reporting även i bakgrunden, Använd det här konfigurationsobjektet:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> När hello-program körs i bakgrunden, nätverksbaserade platser rapporteras, även om du har aktiverat hello GPS.
> 
> 

Om hello användaren startar om sin enhet, har hello bakgrund plats rapporten stoppats. toomake den startas om när datorn startas, lägga till den här koden.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Du måste också tooadd hello efter behörighet om saknas:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M-behörigheter
Från och med Android M kan vissa behörigheter hanteras vid körning och kräver användargodkännande.

Om du riktar Android API-nivå 23 är hello runtime behörigheter inaktiverade som standard för nya installationer av appar. Annars är aktiverade som standard.

Du kan aktivera och inaktivera dessa behörigheter hello enheten inställningsmenyn. Om du inaktiverar behörigheter hello system menyn stängs hello bakgrundsprocesser av hello programmet, vilket är en systembeteende och har ingen inverkan på möjlighet tooreceive push i bakgrunden.

Hello gäller Mobile Engagement plats reporting är är hello behörigheten som kräver godkännande vid körning:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

Begär behörighet från hello-användare som använder en standard systemdialogruta. Berätta om hello användaren godkänner ``EngagementAgent`` tootake ändra hänsyn i realtid. Annars är hello ändra bearbetade hello nästa gång hello startar hello användarprogram.

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
         * Request location permission, but this doesn't explain why it is needed toohello user.
         * hello standard Android documentation explains with more details how toodisplay a rationale activity tooexplain hello user why hello permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of hello same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence hello request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
