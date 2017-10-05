---
title: "Lägg till Push-meddelanden i iOS-App med Azure Mobile Apps"
description: "Lär dig hur du använder Azure Mobile Apps för att skicka push-meddelanden till iOS-app."
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
ms.openlocfilehash: 08a8c35b89386bd0dbe7bba406a6985a5a0d7eb8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-ios-app"></a><span data-ttu-id="e4795-103">Lägg till Push-meddelanden i din iOS-App</span><span class="sxs-lookup"><span data-stu-id="e4795-103">Add Push Notifications to your iOS App</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="e4795-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e4795-104">Overview</span></span>
<span data-ttu-id="e4795-105">I kursen får du lägga till push-meddelanden till den [iOS snabb start] projekt så att ett push-meddelande skickas till enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="e4795-105">In this tutorial, you add push notifications to the [iOS quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="e4795-106">Om du inte använder hämtade Snabbstart serverprojekt behöver push notification extension-paketet.</span><span class="sxs-lookup"><span data-stu-id="e4795-106">If you do not use the downloaded quick start server project, you will need the push notification extension package.</span></span> <span data-ttu-id="e4795-107">Se [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="e4795-107">See [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

<span data-ttu-id="e4795-108">Den [stöder inte push-meddelanden i iOS-simulatorn](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span><span class="sxs-lookup"><span data-stu-id="e4795-108">The [iOS simulator does not support push notifications](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).</span></span> <span data-ttu-id="e4795-109">Behöver du en fysisk iOS-enhet och en [Apple Developer Program medlemskap](https://developer.apple.com/programs/ios/).</span><span class="sxs-lookup"><span data-stu-id="e4795-109">You need a physical iOS device and an [Apple Developer Program membership](https://developer.apple.com/programs/ios/).</span></span>

## <span data-ttu-id="e4795-110"><a name="configure-hub"></a>Konfigurera Meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="e4795-110"><a name="configure-hub"></a>Configure Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <span data-ttu-id="e4795-111"><a id="register"></a>Registrera app för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="e4795-111"><a id="register"></a>Register app for push notifications</span></span>
[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

## <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="e4795-112">Konfigurera Azure för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="e4795-112">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

## <span data-ttu-id="e4795-113"><a id="update-server"></a>Uppdatera serverdel och skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="e4795-113"><a id="update-server"></a>Update backend to send push notifications</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-apns](../../includes/app-service-mobile-dotnet-backend-configure-push-apns.md)]

## <span data-ttu-id="e4795-114"><a id="add-push"></a>Lägg till push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="e4795-114"><a id="add-push"></a>Add push notifications to app</span></span>
[!INCLUDE [app-service-mobile-add-push-notifications-to-ios-app.md](../../includes/app-service-mobile-add-push-notifications-to-ios-app.md)]

## <span data-ttu-id="e4795-115"><a id="test"></a>Testa push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="e4795-115"><a id="test"></a>Test push notifications</span></span>
[!INCLUDE [Test Push Notifications in App](../../includes/test-push-notifications-in-app.md)]

## <span data-ttu-id="e4795-116"><a id="more"></a>Mer</span><span class="sxs-lookup"><span data-stu-id="e4795-116"><a id="more"></a>More</span></span>
* <span data-ttu-id="e4795-117">Mallar ger dig möjlighet att skicka push-meddelanden för flera plattformar och lokaliserade push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e4795-117">Templates give you flexibility to send cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="e4795-118">[Så här används iOS-klientbiblioteket för Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) visar hur du registrerar mallar.</span><span class="sxs-lookup"><span data-stu-id="e4795-118">[How to Use iOS Client Library for Azure Mobile Apps](app-service-mobile-ios-how-to-use-client-library.md#templates) shows you how to register templates.</span></span>

<!-- Anchors.  -->

<!-- Images. -->

<!-- URLs. -->
<span data-ttu-id="e4795-119">[iOS snabb start]: app-service-mobile-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="e4795-119">[iOS quick start]: app-service-mobile-ios-get-started.md</span></span>
