---
title: "Lägg till push-meddelanden till din Android-app med Mobile Apps | Microsoft Docs"
description: "Lär dig hur du använder Mobile Apps för att skicka push-meddelanden till din Android-app."
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
ms.openlocfilehash: b89e9af55342d5d7473d848956996f846250b4b5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-android-app"></a><span data-ttu-id="b0fc7-103">Lägg till push-meddelanden i appen för Android</span><span class="sxs-lookup"><span data-stu-id="b0fc7-103">Add push notifications to your Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="b0fc7-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="b0fc7-104">Overview</span></span>
<span data-ttu-id="b0fc7-105">I kursen får du lägga till push-meddelanden till den [Android Snabbstart] projekt så att ett push-meddelande skickas till enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-105">In this tutorial, you add push notifications to the [Android quick start] project so that a push notification is sent to the device every time a record is inserted.</span></span>

<span data-ttu-id="b0fc7-106">Om du inte använder hämtade Snabbstart serverprojekt måste push notification extension-paketet.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-106">If you do not use the downloaded quick start server project, you need the push notification extension package.</span></span> <span data-ttu-id="b0fc7-107">Mer information finns i [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="b0fc7-107">For more information, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0fc7-108">Krav</span><span class="sxs-lookup"><span data-stu-id="b0fc7-108">Prerequisites</span></span>
<span data-ttu-id="b0fc7-109">Du behöver följande:</span><span class="sxs-lookup"><span data-stu-id="b0fc7-109">You need the following:</span></span>

* <span data-ttu-id="b0fc7-110">IDE-miljö, beroende på ditt projekt serverdel:</span><span class="sxs-lookup"><span data-stu-id="b0fc7-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="b0fc7-111">[Android Studio](https://developer.android.com/sdk/index.html) om det här programmet har en Node.js-serverdel.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="b0fc7-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) eller senare, om det här programmet har en Microsoft .NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="b0fc7-113">Android 2.3 eller senare, Google databasen revision 27 eller senare och Google Play-tjänster 9.0.2 för Firebase Cloud Messaging eller senare.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="b0fc7-114">Slutför den [Android Snabbstart].</span><span class="sxs-lookup"><span data-stu-id="b0fc7-114">Complete the [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="b0fc7-115">Skapa ett projekt som har stöd för Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="b0fc7-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="b0fc7-116">Konfigurera en meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="b0fc7-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-to-send-push-notifications"></a><span data-ttu-id="b0fc7-117">Konfigurera Azure för att skicka push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="b0fc7-117">Configure Azure to send push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-the-server-project"></a><span data-ttu-id="b0fc7-118">Aktivera push-meddelanden för serverprojekt</span><span class="sxs-lookup"><span data-stu-id="b0fc7-118">Enable push notifications for the server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-to-your-app"></a><span data-ttu-id="b0fc7-119">Lägg till push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="b0fc7-119">Add push notifications to your app</span></span>
<span data-ttu-id="b0fc7-120">I det här avsnittet kan du uppdatera din Android-app i klienten för att hantera push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-120">In this section, you update your client Android app to handle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="b0fc7-121">Verifiera version av Android SDK</span><span class="sxs-lookup"><span data-stu-id="b0fc7-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="b0fc7-122">Nästa steg är att installera Google Play-tjänster.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-122">Your next step is to install Google Play services.</span></span> <span data-ttu-id="b0fc7-123">Google Cloud Messaging har nivån vissa minimikrav API för utveckling och testning, som den **minSdkVersion** egenskap i manifestet måste motsvara.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which the **minSdkVersion** property in the manifest must conform to.</span></span>

<span data-ttu-id="b0fc7-124">Om du vill testa med en äldre enhet konsultera [ställa in Google Play Services SDK] att avgöra hur låg du kan ange ett värde och anges korrekt.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] to determine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-to-the-project"></a><span data-ttu-id="b0fc7-125">Lägga till Google Play-tjänster till projektet</span><span class="sxs-lookup"><span data-stu-id="b0fc7-125">Add Google Play services to the project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="b0fc7-126">Lägg till kod</span><span class="sxs-lookup"><span data-stu-id="b0fc7-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-the-app-against-the-published-mobile-service"></a><span data-ttu-id="b0fc7-127">Testa appen mot den publicerade mobiltjänsten</span><span class="sxs-lookup"><span data-stu-id="b0fc7-127">Test the app against the published mobile service</span></span>
<span data-ttu-id="b0fc7-128">Du kan testa appen genom att koppla en Android-telefon med en USB-kabel direkt eller genom att använda en virtuell enhet i emulatorn.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-128">You can test the app by directly attaching an Android phone with a USB cable, or by using a virtual device in the emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0fc7-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0fc7-129">Next steps</span></span>
<span data-ttu-id="b0fc7-130">Nu när du har slutfört den här självstudiekursen Tänk fortsätter in på något av följande kurser:</span><span class="sxs-lookup"><span data-stu-id="b0fc7-130">Now that you completed this tutorial, consider continuing on to one of the following tutorials:</span></span>

* <span data-ttu-id="b0fc7-131">[Lägg till autentisering i din Android-app](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="b0fc7-131">[Add authentication to your Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="b0fc7-132">Lär dig mer om att lägga till autentisering i todolist snabbstartsprojekt på Android använder en stöds identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-132">Learn how to add authentication to the todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="b0fc7-133">[Aktivera offlinesynkronisering av din Android-app](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="b0fc7-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="b0fc7-134">Lär dig hur du lägger till offlinestöd i appen med hjälp av en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-134">Learn how to add offline support to your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="b0fc7-135">Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="b0fc7-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
<span data-ttu-id="b0fc7-136">[Android Snabbstart]: app-service-mobile-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="b0fc7-136">[Android quick start]: app-service-mobile-android-get-started.md</span></span>

<span data-ttu-id="b0fc7-137">[ställa in Google Play Services SDK]:https://developers.google.com/android/guides/setup</span><span class="sxs-lookup"><span data-stu-id="b0fc7-137">[Set Up Google Play Services SDK]:https://developers.google.com/android/guides/setup</span></span>
