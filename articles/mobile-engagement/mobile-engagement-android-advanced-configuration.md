---
title: "aaaAdvanced konfigurationen för Azure Mobile Engagement Android SDK"
description: Beskriver hello avancerade konfigurationsalternativ inklusive hello Android Manifest med Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 37d2c09a-86fa-473d-8987-c7e35a0eb3e8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 757abf362021fd018f444cae6305524623e77062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-azure-mobile-engagement-android-sdk"></a>Avancerad konfiguration för Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Universell Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
>
>

Den här proceduren beskriver hur tooconfigure olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="permission-requirements"></a>Behörighet som krävs
Vissa alternativ kräver särskilda behörigheter, som anges här referens- och raden i hello specifika funktioner. Lägg till dessa behörigheter toohello AndroidManifest.xml i ditt projekt omedelbart före eller efter hello `<application>` tagg.

hello behörighet kod måste toolook som hello följande, där du fyller i hello behörighet från hello i tabellen nedan.

    <uses-permission android:name="android.permission.[specific permission]"/>


| Behörighet | När den används |
| --- | --- |
| INTERNET |Krävs. För grundläggande rapportering |
| ACCESS_NETWORK_STATE |Krävs. För grundläggande rapportering |
| RECEIVE_BOOT_COMPLETED |Krävs. tooshow in hello meddelanden center efter omstart av enheten |
| WAKE_LOCK |Rekommenderas. Gör det möjligt att samla in data när du använder Wi-Fi eller när skärmen är inaktiverat |
| VIBRERAR |Valfri. Aktiverar vibration när meddelanden tas emot |
| DOWNLOAD_WITHOUT_NOTIFICATION |Valfri. Aktiverar Android helheten meddelanden |
| WRITE_EXTERNAL_STORAGE |Valfri. Aktiverar Android helheten meddelanden |
| ACCESS_COARSE_LOCATION |Valfri. Aktiverar rapportering i realtid plats |
| ACCESS_FINE_LOCATION |Valfri. Aktiverar GPS-baserad plats reporting |

Från och med Android M [vissa behörigheter hanteras vid körning](mobile-engagement-android-location-reporting.md#android-m-permissions).

Om du redan använder ``ACCESS_FINE_LOCATION``, och du inte behöver tooalso använder ``ACCESS_COARSE_LOCATION``.

## <a name="android-manifest-configuration-options"></a>Android Manifest konfigurationsalternativ
### <a name="crash-report"></a>Kraschrapport
toodisable kraschrapporter, Lägg till denna kod mellan hello `<application>` och `</application>` taggar:

    <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst tröskelvärde
Som standard loggas hello Engagement service-rapporter i realtid. Om din rapport programloggarna ofta varierar, är det bättre toobuffer hello loggar och tooreport dem på en gång på vanlig taget base (kallas ”burst läge”). toodo Lägg därför till den här koden mellan hello `<application>` och `</application>` taggar:

    <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Burst läge något ökar hello batterilivslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb varaktighet är avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas). Burst-tröskelvärde ska vara längre än 30000 (30s).

### <a name="session-timeout"></a>Tidsgräns för session
 Du kan avsluta en aktivitet genom att trycka på hello **Start** eller **tillbaka** nyckel genom att ange hello phone inaktiv eller genom att hoppa över i ett annat program. Som standard avslutas session tio sekunder efter den senaste aktiviteten hello slut. Detta förhindrar en session-delning varje gång hello användare avslutas och returnerar toohello program snabbt, vilket kan hända när hello användare hämtar en avbildning, kontrollerar en avisering, osv. Du kanske vill toomodify den här parametern. toodo Lägg därför till den här koden mellan hello `<application>` och `</application>` taggar:

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
* Hej `engagement:agent:settings:mode` nyckeln är används toodefine hello läge hello delade inställningsfilen. Använd hello samma läge som i din `PreferenceActivity`. hello-läge måste skickas som ett tal: Om du använder en kombination av konstant flaggor i koden, kontrollera hello totala värdet.

Engagement alltid använder hello `engagement:key` booleskt nyckel i hello inställningsfilen för att hantera den här inställningen.

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
