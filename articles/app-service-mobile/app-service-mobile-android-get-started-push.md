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
# <a name="add-push-notifications-tooyour-android-app"></a><span data-ttu-id="a201b-103">Lägg till push-meddelanden tooyour Android-app</span><span class="sxs-lookup"><span data-stu-id="a201b-103">Add push notifications tooyour Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="a201b-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a201b-104">Overview</span></span>
<span data-ttu-id="a201b-105">I kursen får du lägga till push-meddelanden toohello [Android Snabbstart] projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="a201b-105">In this tutorial, you add push notifications toohello [Android quick start] project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="a201b-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a201b-106">If you do not use hello downloaded quick start server project, you need hello push notification extension package.</span></span> <span data-ttu-id="a201b-107">Mer information finns i [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="a201b-107">For more information, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a201b-108">Krav</span><span class="sxs-lookup"><span data-stu-id="a201b-108">Prerequisites</span></span>
<span data-ttu-id="a201b-109">Du behöver hello följande:</span><span class="sxs-lookup"><span data-stu-id="a201b-109">You need hello following:</span></span>

* <span data-ttu-id="a201b-110">IDE-miljö, beroende på ditt projekt serverdel:</span><span class="sxs-lookup"><span data-stu-id="a201b-110">An IDE, depending on your project's back end:</span></span>

  * <span data-ttu-id="a201b-111">[Android Studio](https://developer.android.com/sdk/index.html) om det här programmet har en Node.js-serverdel.</span><span class="sxs-lookup"><span data-stu-id="a201b-111">[Android Studio](https://developer.android.com/sdk/index.html) if this app has a Node.js back end.</span></span>
  * <span data-ttu-id="a201b-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) eller senare, om det här programmet har en Microsoft .NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="a201b-112">[Visual Studio Community 2013](https://go.microsoft.com/fwLink/p/?LinkID=391934) or later if this app has a Microsoft .NET back end.</span></span>
* <span data-ttu-id="a201b-113">Android 2.3 eller senare, Google databasen revision 27 eller senare och Google Play-tjänster 9.0.2 för Firebase Cloud Messaging eller senare.</span><span class="sxs-lookup"><span data-stu-id="a201b-113">Android 2.3 or later, Google Repository revision 27 or later, and Google Play Services 9.0.2 or later for Firebase Cloud Messaging.</span></span>
* <span data-ttu-id="a201b-114">Fullständig hello [Android Snabbstart].</span><span class="sxs-lookup"><span data-stu-id="a201b-114">Complete hello [Android quick start].</span></span>

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a><span data-ttu-id="a201b-115">Skapa ett projekt som har stöd för Firebase Cloud Messaging</span><span class="sxs-lookup"><span data-stu-id="a201b-115">Create a project that supports Firebase Cloud Messaging</span></span>
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a><span data-ttu-id="a201b-116">Konfigurera en meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="a201b-116">Configure a notification hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="configure-azure-toosend-push-notifications"></a><span data-ttu-id="a201b-117">Konfigurera Azure toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a201b-117">Configure Azure toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push-for-firebase.md)]

## <a name="enable-push-notifications-for-hello-server-project"></a><span data-ttu-id="a201b-118">Aktivera push-meddelanden för hello serverprojekt</span><span class="sxs-lookup"><span data-stu-id="a201b-118">Enable push notifications for hello server project</span></span>
[!INCLUDE [app-service-mobile-dotnet-backend-configure-push-google](../../includes/app-service-mobile-dotnet-backend-configure-push-google.md)]

## <a name="add-push-notifications-tooyour-app"></a><span data-ttu-id="a201b-119">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="a201b-119">Add push notifications tooyour app</span></span>
<span data-ttu-id="a201b-120">I det här avsnittet kan du uppdatera din klient Android-app toohandle push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a201b-120">In this section, you update your client Android app toohandle push notifications.</span></span>

### <a name="verify-android-sdk-version"></a><span data-ttu-id="a201b-121">Verifiera version av Android SDK</span><span class="sxs-lookup"><span data-stu-id="a201b-121">Verify Android SDK version</span></span>
[!INCLUDE [app-service-mobile-verify-android-sdk-version](../../includes/app-service-mobile-verify-android-sdk-version.md)]

<span data-ttu-id="a201b-122">Nästa steg är tooinstall Google Play-tjänster.</span><span class="sxs-lookup"><span data-stu-id="a201b-122">Your next step is tooinstall Google Play services.</span></span> <span data-ttu-id="a201b-123">Google Cloud Messaging har nivån vissa minimikrav API för utveckling och testning, vilka hello **minSdkVersion** egenskap i hello manifestet måste motsvara.</span><span class="sxs-lookup"><span data-stu-id="a201b-123">Google Cloud Messaging has some minimum API level requirements for development and testing, which hello **minSdkVersion** property in hello manifest must conform to.</span></span>

<span data-ttu-id="a201b-124">Om du vill testa med en äldre enhet konsultera [ställa in Google Play Services SDK] toodetermine låg hur du kan ange ett värde och anges korrekt.</span><span class="sxs-lookup"><span data-stu-id="a201b-124">If you are testing with an older device, consult [Set Up Google Play Services SDK] toodetermine how low you can set this value, and set it appropriately.</span></span>

### <a name="add-google-play-services-toohello-project"></a><span data-ttu-id="a201b-125">Lägga till Google Play services toohello projekt</span><span class="sxs-lookup"><span data-stu-id="a201b-125">Add Google Play services toohello project</span></span>
[!INCLUDE [Add Play Services](../../includes/app-service-mobile-add-google-play-services.md)]

### <a name="add-code"></a><span data-ttu-id="a201b-126">Lägg till kod</span><span class="sxs-lookup"><span data-stu-id="a201b-126">Add code</span></span>
[!INCLUDE [app-service-mobile-android-getting-started-with-push](../../includes/app-service-mobile-android-getting-started-with-push.md)]

## <a name="test-hello-app-against-hello-published-mobile-service"></a><span data-ttu-id="a201b-127">Testa hello appen mot hello publicerade Mobiltjänst</span><span class="sxs-lookup"><span data-stu-id="a201b-127">Test hello app against hello published mobile service</span></span>
<span data-ttu-id="a201b-128">Du kan testa hello appen genom att koppla en Android-telefon med en USB-kabel direkt eller genom att använda en virtuell enhet i hello-emulatorn.</span><span class="sxs-lookup"><span data-stu-id="a201b-128">You can test hello app by directly attaching an Android phone with a USB cable, or by using a virtual device in hello emulator.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a201b-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a201b-129">Next steps</span></span>
<span data-ttu-id="a201b-130">Nu när du har slutfört den här självstudiekursen Tänk fortsätter tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="a201b-130">Now that you completed this tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* <span data-ttu-id="a201b-131">[Lägg till autentisering tooyour Android app](app-service-mobile-android-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="a201b-131">[Add authentication tooyour Android app](app-service-mobile-android-get-started-users.md).</span></span>
  <span data-ttu-id="a201b-132">Lär dig hur tooadd autentisering toohello todolist quickstart projektet på Android använder en stöds identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="a201b-132">Learn how tooadd authentication toohello todolist quickstart project on Android using a supported identity provider.</span></span>
* <span data-ttu-id="a201b-133">[Aktivera offlinesynkronisering av din Android-app](app-service-mobile-android-get-started-offline-data.md).</span><span class="sxs-lookup"><span data-stu-id="a201b-133">[Enable offline sync for your Android app](app-service-mobile-android-get-started-offline-data.md).</span></span>
  <span data-ttu-id="a201b-134">Lär dig hur tooadd offline stöder tooyour appen med hjälp av en Mobile Apps-serverdel.</span><span class="sxs-lookup"><span data-stu-id="a201b-134">Learn how tooadd offline support tooyour app by using a Mobile Apps back end.</span></span> <span data-ttu-id="a201b-135">Med offlinesynkronisering, användare kan interagera med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="a201b-135">With offline sync, users can interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs -->
[Android Snabbstart]: app-service-mobile-android-get-started.md

[ställa in Google Play Services SDK]:https://developers.google.com/android/guides/setup
