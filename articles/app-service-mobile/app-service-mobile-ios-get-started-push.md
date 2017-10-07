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
# <a name="add-push-notifications-tooyour-ios-app"></a><span data-ttu-id="c7064-103">Lägg till Push-meddelanden tooyour iOS-App</span><span class="sxs-lookup"><span data-stu-id="c7064-103">Add Push Notifications tooyour iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="c7064-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="c7064-104">Overview</span></span>
<span data-ttu-id="c7064-105">I kursen får du lägga till push-meddelanden toohello [iOS snabb start] projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="c7064-105">In this tutorial, you add push notifications toohello [iOS quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="c7064-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c7064-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="c7064-107">Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="c7064-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

<span data-ttu-id="c7064-108">Hej [stöder inte push-meddelanden i iOS-simulatorn](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="c7064-108">hello [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span> <span data-ttu-id="c7064-109">Behöver du en fysisk iOS-enhet och en [Apple Developer Program medlemskap](https://developer.apple.com/programs/ios/).</span><span class="sxs-lookup"><span data-stu-id="c7064-109">You need a physical iOS device and an [Apple Developer Program membership](https://developer.apple.com/programs/ios/).</span></span>

## <span data-ttu-id="c7064-110"><a name="configure-hub"></a>Konfigurera Meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="c7064-110"><a name="configure-hub"></a>Configure Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="c7064-111"><a id="register"></a>Registrera app för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c7064-111"><a id="register"></a>Register app for push notifications</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="c7064-112">Konfigurera Azure toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c7064-112">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <span data-ttu-id="c7064-113"><a id="update-server"></a>Uppdatera backend toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c7064-113"><a id="update-server"></a>Update backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <span data-ttu-id="c7064-114"><a id="add-push"></a>Lägg till push-meddelanden tooapp</span><span class="sxs-lookup"><span data-stu-id="c7064-114"><a id="add-push"></a>Add push notifications tooapp</span></span>
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <span data-ttu-id="c7064-115"><a id="test"></a>Testa push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="c7064-115"><a id="test"></a>Test push notifications</span></span>
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <span data-ttu-id="c7064-116"><a id="more"></a>Mer</span><span class="sxs-lookup"><span data-stu-id="c7064-116"><a id="more"></a>More</span></span>
* <span data-ttu-id="c7064-117">Mallar ger dig flexibilitet toosend plattformsoberoende push-meddelanden och lokaliserade push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c7064-117">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="c7064-118">[Hur tooUse iOS-klientbiblioteket för Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) visas hur tooregister mallar.</span><span class="sxs-lookup"><span data-stu-id="c7064-118">[How tooUse iOS Client Library for Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) shows you how tooregister templates.</span></span>

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
[iOS snabb start]: app-service-mobile-ios-get-started.md
