---
title: aaaAdd push-meddelanden tooyour Android-app med Mobile Apps | Microsoft Docs
description: "Lär dig hur toouse Mobile Apps toosend push-meddelanden tooyour Android-app."
services: app-service\mobile
documentationcenter: android
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: 9058ed6d-e871-4179-86af-0092d0ca09d3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 10/12/2016
ms.author: glenga
ms.openlocfilehash: dcfb8463b395904b4bd0bf9bde2f71f259894066
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-android-app"></a>Lägg till push-meddelanden tooyour Android-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [Android Snabbstart] projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Krav
Du behöver hello följande:

* IDE-miljö, beroende på ditt projekt serverdel:

  * [Android Studio](https://developer.android.com/sdk/index.html) om det här programmet har en Node.js-serverdel.
  * [Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) eller senare, om det här programmet har en Microsoft .NET-serverdel.
* Android 2.3 eller senare, Google databasen revision 27 eller senare och Google Play-tjänster 9.0.2 för Firebase Cloud Messaging eller senare.
* Fullständig hello [Android Snabbstart].

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Skapa ett projekt som har stöd för Firebase Cloud Messaging
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Konfigurera en meddelandehubb
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Konfigurera Azure toosend push-meddelanden
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a>Aktivera push-meddelanden för hello serverprojekt
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a>Lägg till push-meddelanden tooyour app
I det här avsnittet kan du uppdatera din klient Android-app toohandle push-meddelanden.

### <a name="verify-android-sdk-version"></a>Verifiera version av Android SDK
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

Nästa steg är tooinstall Google Play-tjänster. Google Cloud Messaging har nivån vissa minimikrav API för utveckling och testning, vilka hello **minSdkVersion** egenskap i hello manifestet måste motsvara.

Om du vill testa med en äldre enhet konsultera [ställa in Google Play Services SDK] toodetermine låg hur du kan ange ett värde och anges korrekt.

### <a name="add-google-play-services-toohello-project"></a>Lägga till Google Play services toohello projekt
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a>Lägg till kod
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a>Testa hello appen mot hello publicerade Mobiltjänst
Du kan testa hello appen genom att koppla en Android-telefon med en USB-kabel direkt eller genom att använda en virtuell enhet i hello-emulatorn.

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här självstudiekursen Tänk fortsätter tooone av hello följande kurser:

* [Lägg till autentisering tooyour Android app](app-service-mobile-android-get-started-users.md).
  Lär dig hur tooadd autentisering toohello todolist quickstart projektet på Android använder en stöds identitetsleverantör.
* [Aktivera offlinesynkronisering av din Android-app](app-service-mobile-android-get-started-offline-data.md).
  Lär dig hur tooadd offline stöder tooyour appen med hjälp av en Mobile Apps-serverdel. Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- URLs -->
[Android Snabbstart]: app-service-mobile-android-get-started.md

[ställa in Google Play Services SDK]:https://developers.google.com/android/guides/setup
