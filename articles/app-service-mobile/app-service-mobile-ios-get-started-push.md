---
title: aaaAdd Push-meddelanden tooiOS App med Azure Mobile Apps
description: "Lär dig hur toouse Azure Mobile Apps toosend push-meddelanden tooyour iOS-app."
services: app-service\mobile
documentationcenter: ios
manager: syntaxc4
editor: 
author: ggailey777
ms.assetid: fa503833-d23e-4925-8d93-341bb3fbab7d
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 10/10/2016
ms.author: glenga
ms.openlocfilehash: 11588c56bc8987a71257dbad4fcdebf1b3209b1b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-ios-app"></a>Lägg till Push-meddelanden tooyour iOS-App
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [iOS snabb start] projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.

Hej [stöder inte push-meddelanden i iOS-simulatorn](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html). Behöver du en fysisk iOS-enhet och en [Apple Developer Program medlemskap](https://developer.apple.com/programs/ios/).

## <a name="configure-hub"></a>Konfigurera Meddelandehubben
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a id="register"></a>Registrera app för push-meddelanden
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a>Konfigurera Azure toosend push-meddelanden
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <a id="update-server"></a>Uppdatera backend toosend push-meddelanden
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <a id="add-push"></a>Lägg till push-meddelanden tooapp
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <a id="test"></a>Testa push-meddelanden
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <a id="more"></a>Mer
* Mallar ger dig flexibilitet toosend plattformsoberoende push-meddelanden och lokaliserade push-meddelanden. [Hur tooUse iOS-klientbiblioteket för Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) visas hur tooregister mallar.

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS snabb start]: app-service-mobile-ios-get-started.md
