---
title: "aaaGet igång med Azure Mobile Engagement för Unity iOS-distribution"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och Push-meddelanden för Unity-appar som distribueras tooiOS enheter."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4247b0a9240cbe2bf1a6618388919d3554c07fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a><span data-ttu-id="575f8-103">Kom igång med Azure Mobile Engagement för Unity iOS-distribution</span><span class="sxs-lookup"><span data-stu-id="575f8-103">Get Started with Azure Mobile Engagement for Unity iOS deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="575f8-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand appanvändningen och hur toosend skicka meddelanden toosegmented användare i ett Unity-program när du distribuerar tooan iOS-enhet.</span><span class="sxs-lookup"><span data-stu-id="575f8-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Unity application when deploying tooan iOS device.</span></span>
<span data-ttu-id="575f8-105">Den här självstudiekursen använder hello klassiska kursen Unity Roll kulan självstudiekursen som hello som startpunkt.</span><span class="sxs-lookup"><span data-stu-id="575f8-105">This tutorial uses hello classic Unity Roll a Ball tutorial as hello starting point.</span></span> <span data-ttu-id="575f8-106">Du bör följa hello stegen i den här [kursen](mobile-engagement-unity-roll-a-ball.md) innan du fortsätter med hello Mobile Engagement-integreringen som visas i hello kursen nedan.</span><span class="sxs-lookup"><span data-stu-id="575f8-106">You should follow hello steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with hello Mobile Engagement integration we showcase in hello tutorial below.</span></span> 

<span data-ttu-id="575f8-107">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="575f8-107">This tutorial requires hello following:</span></span>

* [<span data-ttu-id="575f8-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="575f8-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="575f8-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="575f8-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="575f8-110">XCode Editor</span><span class="sxs-lookup"><span data-stu-id="575f8-110">XCode Editor</span></span>

> [!NOTE]
> <span data-ttu-id="575f8-111">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="575f8-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="575f8-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="575f8-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="575f8-113">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span><span class="sxs-lookup"><span data-stu-id="575f8-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).</span></span>
> 
> 

## <span data-ttu-id="575f8-114"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din iOS-app</span><span class="sxs-lookup"><span data-stu-id="575f8-114"><a id="setup-azme"></a>Setup Mobile Engagement for your iOS app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="575f8-115"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="575f8-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
### <a name="import-hello-unity-package"></a><span data-ttu-id="575f8-116">Importera hello Unity-paketet</span><span class="sxs-lookup"><span data-stu-id="575f8-116">Import hello Unity package</span></span>
1. <span data-ttu-id="575f8-117">Hämta hello [Mobile Engagement Unity-paketet](https://aka.ms/azmeunitysdk) och spara den tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="575f8-117">Download hello [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it tooyour local machine.</span></span> 
2. <span data-ttu-id="575f8-118">Gå för**tillgångar -> Importera paket -> anpassat paket** och välj hello-paket som du hämtade i hello senare steg.</span><span class="sxs-lookup"><span data-stu-id="575f8-118">Go too**Assets -> Import Package -> Custom Package** and select hello package you downloaded in hello above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="575f8-119">Kontrollera att alla filer har markerats och klicka på knappen **Import** (Importera).</span><span class="sxs-lookup"><span data-stu-id="575f8-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="575f8-120">När importen är klar visas hello importerade SDK-filerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="575f8-120">Once Import is successful, you will see hello imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a><span data-ttu-id="575f8-121">Uppdatera hello EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="575f8-121">Update hello EngagementConfiguration</span></span>
1. <span data-ttu-id="575f8-122">Öppna hello **EngagementConfiguration** skriptfilen hello SDK-mappen och uppdatera hello **IOS\_anslutning\_sträng** med hello anslutningssträng som du tidigare hämtade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="575f8-122">Open up hello **EngagementConfiguration** script file from hello SDK folder and update hello **IOS\_CONNECTION\_STRING** with hello connection string you obtained earlier from hello Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="575f8-123">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="575f8-123">Save hello file.</span></span> 

### <a name="configure-hello-app-for-basic-tracking"></a><span data-ttu-id="575f8-124">Konfigurera hello appen för grundläggande spårning</span><span class="sxs-lookup"><span data-stu-id="575f8-124">Configure hello app for basic tracking</span></span>
1. <span data-ttu-id="575f8-125">Öppna hello **PlayerController** skript kopplat toohello Player-objektet för redigering.</span><span class="sxs-lookup"><span data-stu-id="575f8-125">Open up hello **PlayerController** script attached toohello Player object for editing.</span></span> 
2. <span data-ttu-id="575f8-126">Lägg till hello följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="575f8-126">Add hello following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="575f8-127">Lägg till följande toohello hello `Start()` metod</span><span class="sxs-lookup"><span data-stu-id="575f8-127">Add hello following toohello `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a><span data-ttu-id="575f8-128">Distribuera och köra hello app</span><span class="sxs-lookup"><span data-stu-id="575f8-128">Deploy and run hello app</span></span>
1. <span data-ttu-id="575f8-129">Ansluta en iOS-enhet tooyour dator.</span><span class="sxs-lookup"><span data-stu-id="575f8-129">Connect an iOS device tooyour machine.</span></span> 
2. <span data-ttu-id="575f8-130">Öppna **File -> Build Settings** (Arkiv -> Inställningar för byggande).</span><span class="sxs-lookup"><span data-stu-id="575f8-130">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="575f8-131">Välj **iOS** och klicka sedan på **Switch Platform** (Växla plattform).</span><span class="sxs-lookup"><span data-stu-id="575f8-131">Select **iOS** and then click on **Switch Platform**</span></span>
   
    ![][41]
   
    ![][42]
4. <span data-ttu-id="575f8-132">Klicka på **Player settings** (Player-inställningar) och ange ett giltigt paket-ID.</span><span class="sxs-lookup"><span data-stu-id="575f8-132">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="575f8-133">Klicka slutligen på **Build And Run** (Bygg och kör).</span><span class="sxs-lookup"><span data-stu-id="575f8-133">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="575f8-134">Du tillfrågas tooprovide en mapp namn toostore hello iOS-paketet.</span><span class="sxs-lookup"><span data-stu-id="575f8-134">You may be asked tooprovide a folder name toostore hello iOS package.</span></span> 
   
    ![][43]
7. <span data-ttu-id="575f8-135">Om allt går bra kompileras hello projektet och du öppna det i XCode-programmet.</span><span class="sxs-lookup"><span data-stu-id="575f8-135">If everything goes fine, then hello project will be compiled and you should open it up on your XCode application.</span></span> 
8. <span data-ttu-id="575f8-136">Kontrollera att hello **paketidentifierare** är korrekt i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="575f8-136">Make sure that hello **Bundle identifier** is correct in hello project.</span></span>  
   
    ![][75]
9. <span data-ttu-id="575f8-137">Kör nu hello appen i XCode så att hello paketet är distribuerade tooyour ansluten enhet och du bör se Unity-spelet på din telefon!</span><span class="sxs-lookup"><span data-stu-id="575f8-137">Now run hello app in XCode so that hello package is deployed tooyour connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="575f8-138"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="575f8-138"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="575f8-139"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="575f8-139"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="575f8-140">Mobile Engagement kan du toointeract med användarna och räckvidd med push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="575f8-140">Mobile Engagement allows you toointeract with your users and REACH with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="575f8-141">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="575f8-141">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="575f8-142">Du saknar toodo någon ytterligare konfiguration i din app tooreceive meddelanden och den är redan inställd för den.</span><span class="sxs-lookup"><span data-stu-id="575f8-142">You don't have toodo any additional configuration in your app tooreceive notifications and it is already setup for it.</span></span>

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
