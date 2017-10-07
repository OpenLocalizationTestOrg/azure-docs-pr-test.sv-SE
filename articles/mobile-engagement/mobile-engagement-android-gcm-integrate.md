---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d72b5014-a22b-4a7f-a470-d2b8145b5b86
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 10/10/2016
ms.author: piyushjo
ms.openlocfilehash: e81230cbc99a209f2909cc163c4e566df67dc828
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a>Hur tooIntegrate GCM med Mobile Engagement
> [!IMPORTANT]
> Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.
> 
> Det här dokumentet är användbar bara om du redan integrerade hello nå modulen och planera toopush Google Play enheter. toointegrate Reach-kampanjer i ditt program, Läs först hur tooIntegrate Engagement nå på Android.
> 
> 

## <a name="introduction"></a>Introduktion
Integrera GCM kan ditt program toobe pushas.

GCM-nyttolaster pushas toohello SDK alltid innehålla hello `azme` nyckel i hello dataobjekt. Därför om du använder GCM för andra ändamål i ditt program, kan du filtrera push-meddelanden baserat på nyckeln.

> [!IMPORTANT]
> Endast enheter som kör Android 2.2 eller senare med Google Play installerad och som har Google bakgrund anslutning aktiverad flyttas med GCM; Du kan dock integrera koden på ett säkert sätt på stöds inte enheter (bara avsikter används).
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a>Skapa ett Google Cloud Messaging-projekt med API-nyckel
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a>SDK-integration
### <a name="managing-device-registrations"></a>Hantera registreringar för enhet
Varje enhet måste skicka en registrering kommandot toohello Google-servrarna, annars de inte kan nås.

En enhet kan också avregistrera från GCM-meddelanden (hello enheten är automatiskt avregistrera hello programmet har avinstallerats).

Om du inte använder [Google Play-SDK] eller du inte redan skickar hello registrering avsikt själv, kan du se Engagement registrerar hello enheten automatiskt åt dig.

tooenable detta, Lägg till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagg:

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Kommunicera id toohello Engagement Push registreringstjänsten och ta emot meddelanden
I ordning toocommunicate hello registrerings-id för hello enheten toohello Engagement Push-tjänst och ta emot dess meddelanden, lägga till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagga (även om du hanterar enheten registreringar själv):

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
              android:exported="false">
              <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
              </intent-filter>
            </receiver>

            <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
              <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your_package_name>" />
              </intent-filter>
            </receiver>

Kontrollera att du har följande behörigheter i hello din `AndroidManifest.xml` (efter hello `</application>` tagg).

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a>Bevilja Mobile Engagement åtkomst tooyour GCM API-nyckel
Följ [handboken](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement åtkomst tooyour GCM API-nyckel.

[Google Play-SDK]:https://developers.google.com/cloud-messaging/android/start
