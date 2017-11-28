---
title: "aaaGet igång med Azure Mobile Engagement för Unity Android-distribution"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Unity-appar som distribueras tooiOS enheter."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a><span data-ttu-id="4d63c-103">Kom igång med Azure Mobile Engagement för Unity Android-distribution</span><span class="sxs-lookup"><span data-stu-id="4d63c-103">Get Started with Azure Mobile Engagement for Unity Android deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="4d63c-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend skicka meddelanden toosegmented användare i ett Unity-program när du distribuerar tooan Android-enhet.</span><span class="sxs-lookup"><span data-stu-id="4d63c-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan Android device.</span></span>
<span data-ttu-id="4d63c-105">Den här självstudiekursen använder hello klassiska kursen Unity Roll kulan självstudiekursen som hello som startpunkt.</span><span class="sxs-lookup"><span data-stu-id="4d63c-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="4d63c-106">Du bör följa hello stegen i den här [kursen](mobile-engagement-unity-roll-a-ball.md) innan du fortsätter med hello Mobile Engagement-integreringen som visas i hello kursen nedan.</span><span class="sxs-lookup"><span data-stu-id="4d63c-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="4d63c-107">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d63c-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="4d63c-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="4d63c-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="4d63c-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="4d63c-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="4d63c-110">Google Android SDK</span><span class="sxs-lookup"><span data-stu-id="4d63c-110">Google Android SDK</span></span>

> [!NOTE]
> <span data-ttu-id="4d63c-111">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="4d63c-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="4d63c-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4d63c-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4d63c-113">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="4d63c-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="4d63c-114"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Android-app</span><span class="sxs-lookup"><span data-stu-id="4d63c-114"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="4d63c-115"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="4d63c-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="4d63c-116">Importera hello Unity-paketet</span><span class="sxs-lookup"><span data-stu-id="4d63c-116">Import hello Unity package</span></span>
1. <span data-ttu-id="4d63c-117">Hämta hello [Mobile Engagement Unity-paketet](https://aka.ms/azmeunitysdk) och spara den tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="4d63c-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="4d63c-118">Gå för**tillgångar -> Importera paket -> anpassat paket** och välj hello-paket som du hämtade i hello senare steg.</span><span class="sxs-lookup"><span data-stu-id="4d63c-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="4d63c-119">Kontrollera att alla filer har markerats och klicka på knappen **Import** (Importera).</span><span class="sxs-lookup"><span data-stu-id="4d63c-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="4d63c-120">När importen är klar visas hello importerade SDK-filerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="4d63c-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="4d63c-121">Uppdatera hello EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="4d63c-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="4d63c-122">Öppna hello **EngagementConfiguration** skriptfilen hello SDK-mappen och uppdatera hello **ANDROID\_anslutning\_sträng** med hello anslutningssträng som du hämtade tidigare från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d63c-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="4d63c-123">Spara filen med hello</span><span class="sxs-lookup"><span data-stu-id="4d63c-123">Save hello file</span></span> 
3. <span data-ttu-id="4d63c-124">Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest).</span><span class="sxs-lookup"><span data-stu-id="4d63c-124">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="4d63c-125">Detta är hello plugin-program läggs till av hello Mobile Engagement SDK och klicka på det uppdateras automatiskt dina Projektinställningar.</span><span class="sxs-lookup"><span data-stu-id="4d63c-125">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

> [!IMPORTANT]
> <span data-ttu-id="4d63c-126">Se till att tooexecute detta varje gång du uppdaterar hello **EngagementConfiguration** filen annars inte visas ändringarna i hello app.</span><span class="sxs-lookup"><span data-stu-id="4d63c-126">Make sure tooexecute this every time you update hello **EngagementConfiguration** file otherwise your changes will not be reflected in hello app.</span></span> 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="4d63c-127">Konfigurera hello appen för grundläggande spårning</span><span class="sxs-lookup"><span data-stu-id="4d63c-127">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="4d63c-128">Öppna hello **PlayerController** skript kopplat toohello Player-objektet för redigering.</span><span class="sxs-lookup"><span data-stu-id="4d63c-128">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="4d63c-129">Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="4d63c-129">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="4d63c-130">Lägg till följande toohello hello `Start()` metod</span><span class="sxs-lookup"><span data-stu-id="4d63c-130">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="4d63c-131">Distribuera och köra hello app</span><span class="sxs-lookup"><span data-stu-id="4d63c-131">Deploy and run hello app</span></span>
<span data-ttu-id="4d63c-132">Kontrollera att du har Android SDK är installerat på datorn innan du försöker utföra toodeploy Unity app tooyour enheten.</span><span class="sxs-lookup"><span data-stu-id="4d63c-132">Make sure that you have Android SDK installed on your machine before attempting toodeploy this Unity app tooyour device.</span></span> 

1. <span data-ttu-id="4d63c-133">Anslut en Android-enhet tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="4d63c-133">Connect an Android device tooyour machine.</span></span> 
2. <span data-ttu-id="4d63c-134">Öppna **File -> Build Settings** (Arkiv -> Inställningar för byggande).</span><span class="sxs-lookup"><span data-stu-id="4d63c-134">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="4d63c-135">Välj **Android** och klicka sedan på **Switch Platform** (Växla plattform).</span><span class="sxs-lookup"><span data-stu-id="4d63c-135">Select **Android** and then click on **Switch Platform**</span></span>
   
    ![][51]
   
    ![][52]
4. <span data-ttu-id="4d63c-136">Klicka på **Player settings** (Player-inställningar) och ange ett giltigt paket-ID.</span><span class="sxs-lookup"><span data-stu-id="4d63c-136">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="4d63c-137">Klicka slutligen på **Build And Run** (Bygg och kör).</span><span class="sxs-lookup"><span data-stu-id="4d63c-137">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="4d63c-138">Du tillfrågas tooprovide en mapp namn toostore hello Android-paketet.</span><span class="sxs-lookup"><span data-stu-id="4d63c-138">You may be asked tooprovide a folder name toostore hello Android package.</span></span> 
7. <span data-ttu-id="4d63c-139">Om allt går bra, hello paketet ska vara distribuerade tooyour anslutna bör enheter och du se Unity-spelet på din telefon!</span><span class="sxs-lookup"><span data-stu-id="4d63c-139">If everything goes fine, then hello package will be deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="4d63c-140"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="4d63c-140"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="4d63c-141"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="4d63c-141"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="4d63c-142">Uppdatera hello EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="4d63c-142">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="4d63c-143">Öppna hello **EngagementConfiguration** skriptfilen hello SDK-mappen och uppdatera hello **ANDROID\_GOOGLE\_nummer** med hello **Google-projekt Antal** du tidigare hämtade från hello Google Cloud Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d63c-143">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **ANDROID\_GOOGLE\_NUMBER** with hello **Google Project Number** you obtained earlier from hello Google Cloud Developer portal.</span></span> <span data-ttu-id="4d63c-144">Detta är en sträng som värde så se till att tooenclose den med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="4d63c-144">This is a string value so make sure tooenclose it in double quotes.</span></span> 
   
    ![][75]
2. <span data-ttu-id="4d63c-145">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="4d63c-145">Save hello file.</span></span> 
3. <span data-ttu-id="4d63c-146">Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest).</span><span class="sxs-lookup"><span data-stu-id="4d63c-146">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="4d63c-147">Detta är hello plugin-program läggs till av hello Mobile Engagement SDK och klicka på det uppdateras automatiskt dina Projektinställningar.</span><span class="sxs-lookup"><span data-stu-id="4d63c-147">This is hello plugin added by hello Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a><span data-ttu-id="4d63c-148">Konfigurera hello app tooreceive meddelanden</span><span class="sxs-lookup"><span data-stu-id="4d63c-148">Configure hello app tooreceive notifications</span></span>
1. <span data-ttu-id="4d63c-149">Öppna hello **PlayerController** skript kopplat toohello Player-objektet för redigering.</span><span class="sxs-lookup"><span data-stu-id="4d63c-149">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="4d63c-150">Lägg till följande toohello hello `Start()` metod</span><span class="sxs-lookup"><span data-stu-id="4d63c-150">Add hello following toohello `Start()` method</span></span>
   
        EngagementReachAgent.Initialize();
3. <span data-ttu-id="4d63c-151">Nu när hello appen har uppdaterats, distribuera och köra hello app på en enhet enligt hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="4d63c-151">Now that hello app is updated, deploy and run hello app on a device per hello instructions provided below.</span></span> 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
