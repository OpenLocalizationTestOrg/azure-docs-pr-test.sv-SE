---
title: "aaaGet igång med Azure Mobile Engagement för Windows Phone Silverlight-appar"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för Windows Phone Silverlight-appar."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a><span data-ttu-id="4c5bd-103">Kom igång med Azure Mobile Engagement för Windows Phone Silverlight-appar</span><span class="sxs-lookup"><span data-stu-id="4c5bd-103">Get started with Azure Mobile Engagement for Windows Phone Silverlight apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="4c5bd-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett Windows Phone Silverlight-program.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Phone Silverlight application.</span></span>
<span data-ttu-id="4c5bd-105">Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="4c5bd-106">Du får skapa en tom Windows Phone Silverlight-app som samlar in grundläggande data och tar emot push-meddelanden med Microsoft Push Notification Service (MPNS).</span><span class="sxs-lookup"><span data-stu-id="4c5bd-106">In it, you create a blank Windows Phone Silverlight app that collects basic data and receives push notifications using Microsoft Push Notification Service (MPNS).</span></span>

> [!NOTE]
> <span data-ttu-id="4c5bd-107">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="4c5bd-108">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="4c5bd-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

> [!NOTE]
> <span data-ttu-id="4c5bd-109">Windows Phone 8.1 och projekt i tidigare versioner stöds inte i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-109">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="4c5bd-110">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="4c5bd-110">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="4c5bd-111">Om du utvecklar för Windows Phone 8.1 (utan Silverlight), se toohello [universella Windows-kursen](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="4c5bd-111">If you are targeting Windows Phone 8.1 (non-Silverlight), refer toohello [Windows Universal tutorial](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>
> 
> 

<span data-ttu-id="4c5bd-112">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-112">This tutorial requires hello following:</span></span>

* <span data-ttu-id="4c5bd-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4c5bd-113">Visual Studio 2013</span></span>
* <span data-ttu-id="4c5bd-114">[MicrosoftAzure.MobileEngagement] NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="4c5bd-114">[MicrosoftAzure.MobileEngagement] Nuget package</span></span>

> [!NOTE]
> <span data-ttu-id="4c5bd-115">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-115">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="4c5bd-116">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-116">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="4c5bd-117">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span><span class="sxs-lookup"><span data-stu-id="4c5bd-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span></span>
> 
> 

## <span data-ttu-id="4c5bd-118"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Windows Phone-app</span><span class="sxs-lookup"><span data-stu-id="4c5bd-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Windows Phone app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="4c5bd-119"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="4c5bd-119"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="4c5bd-120">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-120">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="4c5bd-121">hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="4c5bd-121">hello complete integration documentation can be found in hello [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span></span>

<span data-ttu-id="4c5bd-122">Vi skapar en grundläggande app i Visual Studio toodemonstrate hello-integration.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-122">We will create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-windows-phone-silverlight-project"></a><span data-ttu-id="4c5bd-123">Skapa ett nytt Windows Phone Silverlight-projekt</span><span class="sxs-lookup"><span data-stu-id="4c5bd-123">Create a new Windows Phone Silverlight project</span></span>
<span data-ttu-id="4c5bd-124">hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-124">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="4c5bd-125">Starta Visual Studio och i hello **Start** väljer **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-125">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="4c5bd-126">Välj i popup-fönstret hello **Windows 8** -> **Windows Phone** -> **tom App (Windows Phone Silverlight)**.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-126">In hello pop-up, select **Windows 8** -> **Windows Phone** -> **Blank App (Windows Phone Silverlight)**.</span></span> <span data-ttu-id="4c5bd-127">Fyll i hello app **namn** och **lösningsnamn**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-127">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>
   
    ![][1]
3. <span data-ttu-id="4c5bd-128">Du kan antingen välja tootarget **Windows Phone 8.0** eller **Windows Phone 8.1**.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-128">You can choose tootarget either **Windows Phone 8.0** or **Windows Phone 8.1**.</span></span>

<span data-ttu-id="4c5bd-129">Du har nu skapat en ny Windows Phone Silverlight-app som vi integrerar hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-129">You have now created a new Windows Phone Silverlight app into which we will integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="4c5bd-130">Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="4c5bd-130">Connect your app toohello Mobile Engagement backend</span></span>
1. <span data-ttu-id="4c5bd-131">Installera hello [MicrosoftAzure.MobileEngagement] nuget-paket i projektet.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-131">Install hello [MicrosoftAzure.MobileEngagement] nuget package in your project.</span></span>
2. <span data-ttu-id="4c5bd-132">Öppna `WMAppManifest.xml` (under egenskapsmappen hello) och kontrollera att följande hello har angetts (Lägg till dem om de inte) i hello `<Capabilities />` tagg:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-132">Open `WMAppManifest.xml` (under hello Properties folder) and make sure hello following are declared (add them if they are not) in hello `<Capabilities />` tag:</span></span>
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. <span data-ttu-id="4c5bd-133">Nu klistra in hello anslutningssträng som du kopierade tidigare för Mobile Engagement-appen och klistra in den i hello `Resources\EngagementConfiguration.xml` fil mellan hello `<connectionString>` och `</connectionString>` taggar:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-133">Now paste hello connection string that you copied earlier for your Mobile Engagement app and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>
   
    ![][3]
4. <span data-ttu-id="4c5bd-134">I hello `App.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-134">In hello `App.xaml.cs` file:</span></span>
   
    <span data-ttu-id="4c5bd-135">a.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-135">a.</span></span> <span data-ttu-id="4c5bd-136">Lägg till hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-136">Add hello `using` statement:</span></span>
   
            using Microsoft.Azure.Engagement;
   
    <span data-ttu-id="4c5bd-137">b.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-137">b.</span></span> <span data-ttu-id="4c5bd-138">Initiera hello SDK i hello `Application_Launching` metoden:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-138">Initialize hello SDK in hello `Application_Launching` method:</span></span>
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    <span data-ttu-id="4c5bd-139">c.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-139">c.</span></span> <span data-ttu-id="4c5bd-140">Infoga följande hello i hello `Application_Activated`:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-140">Insert hello following in hello `Application_Activated`:</span></span>
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <span data-ttu-id="4c5bd-141"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="4c5bd-141"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="4c5bd-142">Du måste skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-142">In order toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="4c5bd-143">Lägga till hello hello MainPage.xaml.cs `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-143">In hello MainPage.xaml.cs, add hello `using` statement:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="4c5bd-144">Ersätt hello basklassen **MainPage**, som kommer före **PhoneApplicationPage**, med **EngagementPage**.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-144">Replace hello base class of **MainPage**, which is before **PhoneApplicationPage**, with **EngagementPage**.</span></span>
   
        class MainPage : EngagementPage 
3. <span data-ttu-id="4c5bd-145">I filen `MainPage.xml`:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-145">In your `MainPage.xml` file:</span></span>
   
    <span data-ttu-id="4c5bd-146">a.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-146">a.</span></span> <span data-ttu-id="4c5bd-147">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-147">Add tooyour namespaces declarations:</span></span>
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    <span data-ttu-id="4c5bd-148">b.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-148">b.</span></span> <span data-ttu-id="4c5bd-149">Ersätt `phone:PhoneApplicationPage` i hello XML-taggnamnet med `engagement:EngagementPage`.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-149">Replace `phone:PhoneApplicationPage` in hello XML tag name with `engagement:EngagementPage`.</span></span>

## <span data-ttu-id="4c5bd-150"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="4c5bd-150"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="4c5bd-151"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="4c5bd-151"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="4c5bd-152">Mobile Engagement kan du toointeract och användarna med Push-meddelanden och meddelanden i appen hello kontexten för kampanjer.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-152">Mobile Engagement allows you toointeract and reach your users with Push Notifications and in-app Messaging in hello context of campaigns.</span></span> <span data-ttu-id="4c5bd-153">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-153">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="4c5bd-154">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-154">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a><span data-ttu-id="4c5bd-155">Aktivera din app tooreceive Push-meddelanden med MPNS</span><span class="sxs-lookup"><span data-stu-id="4c5bd-155">Enable your app tooreceive MPNS Push Notifications</span></span>
<span data-ttu-id="4c5bd-156">Lägga till nya funktioner tooyour `WMAppManifest.xml` fil:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-156">Add new Capabilities tooyour `WMAppManifest.xml` file:</span></span>

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="4c5bd-157">Initiera hello REACH SDK</span><span class="sxs-lookup"><span data-stu-id="4c5bd-157">Initialize hello REACH SDK</span></span>
1. <span data-ttu-id="4c5bd-158">I `App.xaml.cs`, anropa `EngagementReach.Instance.Init();` i hello **Application_Launching** funktion, direkt efter agentinitieringen hello:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-158">In `App.xaml.cs`, call `EngagementReach.Instance.Init();` in hello **Application_Launching** function, right after hello agent initialization:</span></span>
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. <span data-ttu-id="4c5bd-159">I `App.xaml.cs`, anropa `EngagementReach.Instance.OnActivated(e);` i hello **Application_Activated** funktion, direkt efter agentinitieringen hello:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-159">In `App.xaml.cs`, call `EngagementReach.Instance.OnActivated(e);` in hello **Application_Activated** function, right after hello agent initialization:</span></span>
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

<span data-ttu-id="4c5bd-160">Då var allt klart.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-160">You're all set.</span></span> <span data-ttu-id="4c5bd-161">Nu är det dags att kontrollera att den grundläggande integrationen har genomförts.</span><span class="sxs-lookup"><span data-stu-id="4c5bd-161">Now we will verify that you have correctly cried out this basic integration.</span></span>

## <span data-ttu-id="4c5bd-162"><a id="send"></a>Skicka en avisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="4c5bd-162"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="4c5bd-163">Du bör nu se ett meddelande på enheten som visas som en avisering via app om hello appen är öppen, annars som ett popup-meddelande som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="4c5bd-163">You should now see a notification on your device which will show up as an in-app notification if hello app is open otherwise as a toast notification like hello following:</span></span> 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
