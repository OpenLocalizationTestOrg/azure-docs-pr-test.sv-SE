---
title: "Kom igång med Azure Mobile Engagement för Unity Android-distribution"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för Unity-appar som distribueras till iOS-enheter."
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
ms.openlocfilehash: bf0b758159d475b4ed7eadb84227e4824e11ba86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a><span data-ttu-id="4376b-103">Kom igång med Azure Mobile Engagement för Unity Android-distribution</span><span class="sxs-lookup"><span data-stu-id="4376b-103">Get Started with Azure Mobile Engagement for Unity Android deployment</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="4376b-104">I den här artikeln beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett Unity-program när du distribuerar till en Android-enhet.</span><span class="sxs-lookup"><span data-stu-id="4376b-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and how to send push notifications to segmented users of a Unity application when deploying to an Android device.</span></span>
<span data-ttu-id="4376b-105">Den här kursen använder den klassiska kursen Unity Roll a Ball som startpunkt.</span><span class="sxs-lookup"><span data-stu-id="4376b-105">This tutorial uses the classic Unity Roll a Ball tutorial as the starting point.</span></span> <span data-ttu-id="4376b-106">Följ stegen i den här [kursen](mobile-engagement-unity-roll-a-ball.md) innan du fortsätter med Mobile Engagement-integreringen som visas i kursen nedan.</span><span class="sxs-lookup"><span data-stu-id="4376b-106">You should follow the steps in this [tutorial](mobile-engagement-unity-roll-a-ball.md) before proceeding with the Mobile Engagement integration we showcase in the tutorial below.</span></span> 

<span data-ttu-id="4376b-107">Följande krävs för den här självstudiekursen:</span><span class="sxs-lookup"><span data-stu-id="4376b-107">This tutorial requires the following:</span></span>

* [<span data-ttu-id="4376b-108">Unity Editor</span><span class="sxs-lookup"><span data-stu-id="4376b-108">Unity Editor</span></span>](http://unity3d.com/get-unity)
* [<span data-ttu-id="4376b-109">Mobile Engagement Unity SDK</span><span class="sxs-lookup"><span data-stu-id="4376b-109">Mobile Engagement Unity SDK</span></span>](https://aka.ms/azmeunitysdk)
* <span data-ttu-id="4376b-110">Google Android SDK</span><span class="sxs-lookup"><span data-stu-id="4376b-110">Google Android SDK</span></span>

> [!NOTE]
> <span data-ttu-id="4376b-111">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="4376b-111">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="4376b-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4376b-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4376b-113">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="4376b-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="4376b-114"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Android-app</span><span class="sxs-lookup"><span data-stu-id="4376b-114"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="4376b-115"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="4376b-115"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
### <a name="import-the-unity-package"></a><span data-ttu-id="4376b-116">Importera Unity-paketet</span><span class="sxs-lookup"><span data-stu-id="4376b-116">Import the Unity package</span></span>
1. <span data-ttu-id="4376b-117">Hämta [Mobile Engagement Unity-paketet](https://aka.ms/azmeunitysdk) och spara det på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="4376b-117">Download the [Mobile Engagement Unity package](https://aka.ms/azmeunitysdk) and save it to your local machine.</span></span> 
2. <span data-ttu-id="4376b-118">Gå till **Assets -> Import Package -> Custom Package** (Tillgångar -> Importera paket -> Anpassat paket) och välj paketet som du hämtade i steget ovan.</span><span class="sxs-lookup"><span data-stu-id="4376b-118">Go to **Assets -> Import Package -> Custom Package** and select the package you downloaded in the above step.</span></span> 
   
    ![][70] 
3. <span data-ttu-id="4376b-119">Kontrollera att alla filer har markerats och klicka på knappen **Import** (Importera).</span><span class="sxs-lookup"><span data-stu-id="4376b-119">Make sure all files are selected and click **Import** button.</span></span> 
   
    ![][71] 
4. <span data-ttu-id="4376b-120">När importen är klar visas de importerade SDK-filerna i projektet.</span><span class="sxs-lookup"><span data-stu-id="4376b-120">Once Import is successful, you will see the imported SDK files in your project.</span></span>  
   
    ![][72] 

### <a name="update-the-engagementconfiguration"></a><span data-ttu-id="4376b-121">Uppdatera EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="4376b-121">Update the EngagementConfiguration</span></span>
1. <span data-ttu-id="4376b-122">Öppna skriptfilen **EngagementConfiguration** i SDK-mappen och uppdatera **ANDROID\_CONNECTION\_STRING** med den anslutningssträng som du hämtade tidigare i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="4376b-122">Open up the **EngagementConfiguration** script file from the SDK folder and update the **ANDROID\_CONNECTION\_STRING** with the connection string you obtained earlier from the Azure portal.</span></span>  
   
    ![][73]
2. <span data-ttu-id="4376b-123">Spara filen</span><span class="sxs-lookup"><span data-stu-id="4376b-123">Save the file</span></span> 
3. <span data-ttu-id="4376b-124">Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest).</span><span class="sxs-lookup"><span data-stu-id="4376b-124">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="4376b-125">Det här är plugin-programmet som läggs till av Mobile Engagement SDK. När du klickar på det uppdateras automatiskt dina projektinställningar.</span><span class="sxs-lookup"><span data-stu-id="4376b-125">This is the plugin added by the Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

> [!IMPORTANT]
> <span data-ttu-id="4376b-126">Var noga med att köra programmet varje gång du uppdaterar filen **EngagementConfiguration**, annars återspeglas inte dina ändringar i appen.</span><span class="sxs-lookup"><span data-stu-id="4376b-126">Make sure to execute this every time you update the **EngagementConfiguration** file otherwise your changes will not be reflected in the app.</span></span> 
> 
> 

### <a name="configure-the-app-for-basic-tracking"></a><span data-ttu-id="4376b-127">Konfigurera appen för grundläggande spårning</span><span class="sxs-lookup"><span data-stu-id="4376b-127">Configure the app for basic tracking</span></span>
1. <span data-ttu-id="4376b-128">Öppna **PlayerController**-skriptet som är kopplat till Player-objektet för redigering.</span><span class="sxs-lookup"><span data-stu-id="4376b-128">Open up the **PlayerController** script attached to the Player object for editing.</span></span> 
2. <span data-ttu-id="4376b-129">Lägg till följande med instruktionen:</span><span class="sxs-lookup"><span data-stu-id="4376b-129">Add the following using statement:</span></span>
   
        using Microsoft.Azure.Engagement.Unity;
3. <span data-ttu-id="4376b-130">Lägg till följande i metoden `Start()`</span><span class="sxs-lookup"><span data-stu-id="4376b-130">Add the following to the `Start()` method</span></span>
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-the-app"></a><span data-ttu-id="4376b-131">Distribuera och kör appen</span><span class="sxs-lookup"><span data-stu-id="4376b-131">Deploy and run the app</span></span>
<span data-ttu-id="4376b-132">Kontrollera att Android SDK är installerat på datorn innan du försöker distribuera den här Unity-appen till din enhet.</span><span class="sxs-lookup"><span data-stu-id="4376b-132">Make sure that you have Android SDK installed on your machine before attempting to deploy this Unity app to your device.</span></span> 

1. <span data-ttu-id="4376b-133">Anslut en Android-enhet till datorn.</span><span class="sxs-lookup"><span data-stu-id="4376b-133">Connect an Android device to your machine.</span></span> 
2. <span data-ttu-id="4376b-134">Öppna **File -> Build Settings** (Arkiv -> Inställningar för byggande).</span><span class="sxs-lookup"><span data-stu-id="4376b-134">Open up **File -> Build Settings**</span></span> 
   
    ![][40]
3. <span data-ttu-id="4376b-135">Välj **Android** och klicka sedan på **Switch Platform** (Växla plattform).</span><span class="sxs-lookup"><span data-stu-id="4376b-135">Select **Android** and then click on **Switch Platform**</span></span>
   
    ![][51]
   
    ![][52]
4. <span data-ttu-id="4376b-136">Klicka på **Player settings** (Player-inställningar) och ange ett giltigt paket-ID.</span><span class="sxs-lookup"><span data-stu-id="4376b-136">Click on **Player settings** and provide a valid Bundle Identifier.</span></span> 
   
    ![][53]
5. <span data-ttu-id="4376b-137">Klicka slutligen på **Build And Run** (Bygg och kör).</span><span class="sxs-lookup"><span data-stu-id="4376b-137">Finally click on **Build And Run**</span></span>
   
    ![][54]
6. <span data-ttu-id="4376b-138">Du kan bli ombedd att ange ett mappnamn där Android-paketet ska lagras.</span><span class="sxs-lookup"><span data-stu-id="4376b-138">You may be asked to provide a folder name to store the Android package.</span></span> 
7. <span data-ttu-id="4376b-139">Om allt går bra distribueras paketet till den anslutna enheten. Sedan kan du se Unity-spelet på din mobil.</span><span class="sxs-lookup"><span data-stu-id="4376b-139">If everything goes fine, then the package will be deployed to your connected device and you should see your Unity game on your phone!</span></span> 

## <span data-ttu-id="4376b-140"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="4376b-140"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="4376b-141"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="4376b-141"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-the-engagementconfiguration"></a><span data-ttu-id="4376b-142">Uppdatera EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="4376b-142">Update the EngagementConfiguration</span></span>
1. <span data-ttu-id="4376b-143">Öppna skriptfilen **EngagementConfiguration** i SDK-mappen och uppdatera **ANDROID\_GOOGLE\_NUMBER** med det **Google-projektnummer** som du tidigare hämtade från Google Cloud Developer-portalen.</span><span class="sxs-lookup"><span data-stu-id="4376b-143">Open up the **EngagementConfiguration** script file from the SDK folder and update the **ANDROID\_GOOGLE\_NUMBER** with the **Google Project Number** you obtained earlier from the Google Cloud Developer portal.</span></span> <span data-ttu-id="4376b-144">Det här är ett strängvärde så var noga med att omge det med dubbla citattecken.</span><span class="sxs-lookup"><span data-stu-id="4376b-144">This is a string value so make sure to enclose it in double quotes.</span></span> 
   
    ![][75]
2. <span data-ttu-id="4376b-145">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="4376b-145">Save the file.</span></span> 
3. <span data-ttu-id="4376b-146">Kör **File -> Engagement -> Generate Android Manifest** (Arkiv -> Engagement -> Generera Android-manifest).</span><span class="sxs-lookup"><span data-stu-id="4376b-146">Execute **File -> Engagement -> Generate Android Manifest**.</span></span> <span data-ttu-id="4376b-147">Det här är plugin-programmet som läggs till av Mobile Engagement SDK. När du klickar på det uppdateras automatiskt dina projektinställningar.</span><span class="sxs-lookup"><span data-stu-id="4376b-147">This is the plugin added by the Mobile Engagement SDK and clicking on it will automatically update your project settings.</span></span> 
   
    ![][74]

### <a name="configure-the-app-to-receive-notifications"></a><span data-ttu-id="4376b-148">Konfigurera appen för att ta emot meddelanden</span><span class="sxs-lookup"><span data-stu-id="4376b-148">Configure the app to receive notifications</span></span>
1. <span data-ttu-id="4376b-149">Öppna **PlayerController**-skriptet som är kopplat till Player-objektet för redigering.</span><span class="sxs-lookup"><span data-stu-id="4376b-149">Open up the **PlayerController** script attached to the Player object for editing.</span></span> 
2. <span data-ttu-id="4376b-150">Lägg till följande i metoden `Start()`</span><span class="sxs-lookup"><span data-stu-id="4376b-150">Add the following to the `Start()` method</span></span>
   
        EngagementReachAgent.Initialize();
3. <span data-ttu-id="4376b-151">Nu när appen har uppdaterats kan du distribuera och köra appen på en enhet enligt anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="4376b-151">Now that the app is updated, deploy and run the app on a device per the instructions provided below.</span></span> 

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
