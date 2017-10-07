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
# <a name="how-toointegrate-gcm-with-mobile-engagement"></a><span data-ttu-id="c5c8d-103">Hur tooIntegrate GCM med Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="c5c8d-103">How tooIntegrate GCM with Mobile Engagement</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c5c8d-104">Du måste följa hello integration beskrivs i hello hur tooIntegrate Engagement på Android dokumentet innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-104">You must follow hello integration procedure described in hello How tooIntegrate Engagement on Android document before following this guide.</span></span>
> 
> <span data-ttu-id="c5c8d-105">Det här dokumentet är användbar bara om du redan integrerade hello nå modulen och planera toopush Google Play enheter.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-105">This document is useful only if you already integrated hello Reach module and plan toopush Google Play devices.</span></span> <span data-ttu-id="c5c8d-106">toointegrate Reach-kampanjer i ditt program, Läs först hur tooIntegrate Engagement nå på Android.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-106">toointegrate Reach campaigns in your application, please read first How tooIntegrate Engagement Reach on Android.</span></span>
> 
> 

## <a name="introduction"></a><span data-ttu-id="c5c8d-107">Introduktion</span><span class="sxs-lookup"><span data-stu-id="c5c8d-107">Introduction</span></span>
<span data-ttu-id="c5c8d-108">Integrera GCM kan ditt program toobe pushas.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-108">Integrating GCM allows your application toobe pushed.</span></span>

<span data-ttu-id="c5c8d-109">GCM-nyttolaster pushas toohello SDK alltid innehålla hello `azme` nyckel i hello dataobjekt.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-109">GCM payloads pushed toohello SDK always contain hello `azme` key in hello data object.</span></span> <span data-ttu-id="c5c8d-110">Därför om du använder GCM för andra ändamål i ditt program, kan du filtrera push-meddelanden baserat på nyckeln.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-110">Thus if you use GCM for another purpose in your application, you can filter pushes based on that key.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c5c8d-111">Endast enheter som kör Android 2.2 eller senare med Google Play installerad och som har Google bakgrund anslutning aktiverad flyttas med GCM; Du kan dock integrera koden på ett säkert sätt på stöds inte enheter (bara avsikter används).</span><span class="sxs-lookup"><span data-stu-id="c5c8d-111">Only devices running Android 2.2 or above, having Google Play installed and having Google background connection enabled can be pushed using GCM; however, you can integrate this code safely on unsupported devices (it just uses intents).</span></span>
> 
> 

## <a name="create-a-google-cloud-messaging-project-with-api-key"></a><span data-ttu-id="c5c8d-112">Skapa ett Google Cloud Messaging-projekt med API-nyckel</span><span class="sxs-lookup"><span data-stu-id="c5c8d-112">Create a Google Cloud Messaging project with API key</span></span>
[!INCLUDE [mobile-engagement-enable-Google-cloud-messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

## <a name="sdk-integration"></a><span data-ttu-id="c5c8d-113">SDK-integration</span><span class="sxs-lookup"><span data-stu-id="c5c8d-113">SDK integration</span></span>
### <a name="managing-device-registrations"></a><span data-ttu-id="c5c8d-114">Hantera registreringar för enhet</span><span class="sxs-lookup"><span data-stu-id="c5c8d-114">Managing device registrations</span></span>
<span data-ttu-id="c5c8d-115">Varje enhet måste skicka en registrering kommandot toohello Google-servrarna, annars de inte kan nås.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-115">Each device must send a registration command toohello Google servers, otherwise they can't be reached.</span></span>

<span data-ttu-id="c5c8d-116">En enhet kan också avregistrera från GCM-meddelanden (hello enheten är automatiskt avregistrera hello programmet har avinstallerats).</span><span class="sxs-lookup"><span data-stu-id="c5c8d-116">A device can also unregister from GCM notifications (hello device is automatically unregistered if hello application is uninstalled).</span></span>

<span data-ttu-id="c5c8d-117">Om du inte använder [Google Play-SDK] eller du inte redan skickar hello registrering avsikt själv, kan du se Engagement registrerar hello enheten automatiskt åt dig.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-117">If you don't use [Google Play SDK] or you don't already send hello registration intent yourself, you can make Engagement register hello device automatically for you.</span></span>

<span data-ttu-id="c5c8d-118">tooenable detta, Lägg till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagg:</span><span class="sxs-lookup"><span data-stu-id="c5c8d-118">tooenable this, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag:</span></span>

            <!-- If only 1 sender, don't forget hello \n, otherwise it will be parsed as a negative number... -->
            <meta-data android:name="engagement:gcm:sender" android:value="<Your Google Project Number>\n" />

### <a name="communicate-registration-id-toohello-engagement-push-service-and-receive-notifications"></a><span data-ttu-id="c5c8d-119">Kommunicera id toohello Engagement Push registreringstjänsten och ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="c5c8d-119">Communicate registration id toohello Engagement Push service and receive notifications</span></span>
<span data-ttu-id="c5c8d-120">I ordning toocommunicate hello registrerings-id för hello enheten toohello Engagement Push-tjänst och ta emot dess meddelanden, lägga till följande tooyour hello `AndroidManifest.xml` fil i hello `<application/>` tagga (även om du hanterar enheten registreringar själv):</span><span class="sxs-lookup"><span data-stu-id="c5c8d-120">In order toocommunicate hello registration id of hello device toohello Engagement Push service and receive its notifications, add hello following tooyour `AndroidManifest.xml` file, inside hello `<application/>` tag (even if you manage device registrations yourself):</span></span>

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

<span data-ttu-id="c5c8d-121">Kontrollera att du har följande behörigheter i hello din `AndroidManifest.xml` (efter hello `</application>` tagg).</span><span class="sxs-lookup"><span data-stu-id="c5c8d-121">Ensure you have hello following permissions in your `AndroidManifest.xml` (after hello `</application>` tag).</span></span>

            <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
            <uses-permission android:name="<your_package_name>.permission.C2D_MESSAGE" />
            <permission android:name="<your_package_name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />

## <a name="grant-mobile-engagement-access-tooyour-gcm-api-key"></a><span data-ttu-id="c5c8d-122">Bevilja Mobile Engagement åtkomst tooyour GCM API-nyckel</span><span class="sxs-lookup"><span data-stu-id="c5c8d-122">Grant Mobile Engagement access tooyour GCM API Key</span></span>
<span data-ttu-id="c5c8d-123">Följ [handboken](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement åtkomst tooyour GCM API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="c5c8d-123">Follow [this guide](mobile-engagement-android-get-started.md#grant-mobile-engagement-access-to-your-gcm-api-key) toogrant Mobile Engagement access tooyour GCM API Key.</span></span>

[Google Play-SDK]:https://developers.google.com/cloud-messaging/android/start
