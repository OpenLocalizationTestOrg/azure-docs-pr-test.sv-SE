---
title: aaaAzure Mobile Engagement Android SDK-Integration
description: "Senaste uppdateringarna och procedurer för Android SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a7d719ec-67b3-4be3-9d7f-0b61a57fe978
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c57132ff49cf8c335627a72b37f9b78529e84f48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toointegrate-adm-with-engagement"></a>Hur tooIntegrate ADM med Engagement
> [!IMPORTANT]
> Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.
> 
> Det här dokumentet är användbar bara om du redan har integrerat hello Reach-modulen och planera toopush Amazon-enheter. toointegrate Reach-kampanjer i ditt program, Läs först hur tooIntegrate Engagement nå på Android.
> 
> 

## <a name="introduction"></a>Introduktion
Integrera ADM kan ditt program toobe pushas när måldatorn Amazon Android-enheter.

ADM-nyttolaster pushas toohello SDK alltid innehålla hello `azme` nyckel i hello dataobjekt. Därför om du använder ADM för andra ändamål i ditt program, kan du filtrera push-meddelanden baserat på nyckeln.

> [!IMPORTANT]
> Endast Amazon Kindle enheter som kör Android 4.0.3 eller senare som stöds av Amazon Device Messaging; Du kan dock integrera koden på ett säkert sätt på andra enheter.
> 
> 

## <a name="sign-up-tooadm"></a>Registrera dig tooADM
Om du inte redan har gjort, måste du aktivera ADM på Amazon-konto.

hello proceduren beskrivs på: [ <https://developer.amazon.com/sdk/adm/credentials.html>].

När du slutför hello proceduren får du:

* OAuth autentiseringsuppgifter (klient-ID och en Klienthemlighet) för Engagement toobe kan toopush dina enheter.
* En API-nyckel som integreras i ditt program.

## <a name="sdk-integration"></a>SDK-integration
### <a name="managing-device-registrations"></a>Hantera registreringar för enhet
Varje enhet måste skicka en registrering kommandot toohello ADM-servrar, annars de inte kan nås.

Om du redan använder hello [ADM-klientbiblioteket], och redan har [integrerad ADM] du kan gå direkt tooandroid och sdk-adm-ta emot.

Om du inte har integrerat ADM ännu Engagement har ett enklare sätt tooenable den i ditt program:

Redigera din `AndroidManifest.xml` fil:

* Lägg till Hej namnområdet för Amazon, hello filen börja så här:
  
      <?xml version="1.0" encoding="utf-8"?>
      <manifest xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:amazon="http://schemas.amazon.com/apk/res/android"
* I hello `<application/>` tagg, lägga till det här avsnittet:
  
      <amazon:enable-feature
         android:name="com.amazon.device.messaging"
         android:required="false"/>
  
      <meta-data android:name="engagement:adm:register" android:value="true" />
* Du kan ha ett build-fel om ditt mål för projektgenerering är lägre än Android 2.1 när du lägger till hello amazon-tagg. Du har toouse en **Android 2.1 +** skapa mål (oroa dig inte, kan du fortfarande ha en `minSdkVersion` ange too4).
* Integrera hello ADM API-nyckel som en tillgång genom att följa [proceduren].

Följ sedan instruktionerna för hello hello nästa avsnitt.

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a>Kommunicera id toohello Engagement Push registreringstjänsten och ta emot meddelanden
I ordning toocommunicate hello registrerings-id för hello enheten toohello Engagement Push-tjänst och ta emot dess meddelanden, lägga till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagga (även om du använder ADM utan behov av):

        <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMEnabler"
          android:exported="false">
          <intent-filter>
            <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT"/>
          </intent-filter>
        </receiver>

         <receiver android:name="com.microsoft.azure.engagement.adm.EngagementADMReceiver"
           android:permission="com.amazon.device.messaging.permission.SEND">
          <intent-filter>
            <action android:name="com.amazon.device.messaging.intent.REGISTRATION"/>
            <action android:name="com.amazon.device.messaging.intent.RECEIVE"/>
            <category android:name="<your_package_name>"/>
          </intent-filter>
        </receiver>   

Kontrollera att du har följande behörigheter i hello din `AndroidManifest.xml` (innan hello `</application>` tagg).

        <uses-permission android:name="android.permission.WAKE_LOCK"/>
        <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE"/>
        <uses-permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE"/>
        <permission android:name="<your_package_name>.permission.RECEIVE_ADM_MESSAGE" android:protectionLevel="signature"/>

## <a name="grant-engagement-oauth-credentials"></a>Bevilja Engagement OAuth-autentiseringsuppgifter
Skicka dina inloggningsuppgifter för OAuth (klient-ID och Klienthemlighet) i Engagement-portalen.

[< https://developer.amazon.com/sdk/adm/credentials.html>]:https://developer.amazon.com/sdk/adm/credentials.html
[ADM-klientbiblioteket]:https://developer.amazon.com/sdk/adm/setup.html
[integrerad ADM]:https://developer.amazon.com/sdk/adm/integrating-app.html
[proceduren]:https://developer.amazon.com/sdk/adm/integrating-app.html#Asset
