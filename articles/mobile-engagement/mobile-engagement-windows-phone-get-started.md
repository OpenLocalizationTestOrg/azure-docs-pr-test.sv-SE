---
title: "Kom igång med Azure Mobile Engagement för Windows Phone Silverlight-appar"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för Windows Phone Silverlight-appar."
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
ms.openlocfilehash: d2334a59d83c90bdd02c4fa29261d36aad292892
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a><span data-ttu-id="64f11-103">Kom igång med Azure Mobile Engagement för Windows Phone Silverlight-appar</span><span class="sxs-lookup"><span data-stu-id="64f11-103">Get started with Azure Mobile Engagement for Windows Phone Silverlight apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="64f11-104">I den här artikeln beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett Windows Phone Silverlight-program.</span><span class="sxs-lookup"><span data-stu-id="64f11-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users of a Windows Phone Silverlight application.</span></span>
<span data-ttu-id="64f11-105">I den här kursen visas ett enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="64f11-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="64f11-106">Du får skapa en tom Windows Phone Silverlight-app som samlar in grundläggande data och tar emot push-meddelanden med Microsoft Push Notification Service (MPNS).</span><span class="sxs-lookup"><span data-stu-id="64f11-106">In it, you create a blank Windows Phone Silverlight app that collects basic data and receives push notifications using Microsoft Push Notification Service (MPNS).</span></span>

> [!NOTE]
> <span data-ttu-id="64f11-107">Tjänsten Azure Mobile Engagement kommer att dras tillbaka i mars 2018 och är för närvarande endast tillgänglig för befintliga kunder.</span><span class="sxs-lookup"><span data-stu-id="64f11-107">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="64f11-108">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="64f11-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

> [!NOTE]
> <span data-ttu-id="64f11-109">Windows Phone 8.1 och projekt i tidigare versioner stöds inte i Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="64f11-109">Windows Phone 8.1 and prior version projects are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="64f11-110">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="64f11-110">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="64f11-111">Om du utvecklar för Windows Phone 8.1 (icke-Silverlight), finns det mer info i den [universella Windows-kursen](mobile-engagement-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="64f11-111">If you are targeting Windows Phone 8.1 (non-Silverlight), refer to the [Windows Universal tutorial](mobile-engagement-windows-store-dotnet-get-started.md).</span></span>
> 
> 

<span data-ttu-id="64f11-112">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="64f11-112">This tutorial requires the following:</span></span>

* <span data-ttu-id="64f11-113">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="64f11-113">Visual Studio 2013</span></span>
* <span data-ttu-id="64f11-114">[MicrosoftAzure.MobileEngagement] NuGet-paket</span><span class="sxs-lookup"><span data-stu-id="64f11-114">[MicrosoftAzure.MobileEngagement] Nuget package</span></span>

> [!NOTE]
> <span data-ttu-id="64f11-115">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="64f11-115">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="64f11-116">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="64f11-116">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="64f11-117">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [Kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span><span class="sxs-lookup"><span data-stu-id="64f11-117">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).</span></span>
> 
> 

## <span data-ttu-id="64f11-118"><a id="setup-azme"></a>Konfigurera Mobile Engagement för din Windows Phone-app</span><span class="sxs-lookup"><span data-stu-id="64f11-118"><a id="setup-azme"></a>Setup Mobile Engagement for your Windows Phone app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="64f11-119"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="64f11-119"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="64f11-120">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="64f11-120">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="64f11-121">Den fullständiga integrationsdokumentationen finns i [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span><span class="sxs-lookup"><span data-stu-id="64f11-121">The complete integration documentation can be found in the [Mobile Engagement Windows Phone SDK integration](mobile-engagement-windows-phone-sdk-overview.md)</span></span>

<span data-ttu-id="64f11-122">Vi skapar en grundläggande app i Visual Studio för att demonstrera integrationen.</span><span class="sxs-lookup"><span data-stu-id="64f11-122">We will create a basic app with Visual Studio to demonstrate the integration.</span></span>

### <a name="create-a-new-windows-phone-silverlight-project"></a><span data-ttu-id="64f11-123">Skapa ett nytt Windows Phone Silverlight-projekt</span><span class="sxs-lookup"><span data-stu-id="64f11-123">Create a new Windows Phone Silverlight project</span></span>
<span data-ttu-id="64f11-124">I följande steg används Visual Studio 2015, men stegen är ganska lika i tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="64f11-124">The following steps assume the use of Visual Studio 2015 though the steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="64f11-125">Starta Visual Studio och välj **New Project** (Nytt projekt) på **startskärmen**.</span><span class="sxs-lookup"><span data-stu-id="64f11-125">Start Visual Studio, and in the **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="64f11-126">Välj **Windows 8** -> **Windows Phone** -> **Blank App (Windows Phone Silverlight)** (Tom app (Windows Phone Silverlight)) i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="64f11-126">In the pop-up, select **Windows 8** -> **Windows Phone** -> **Blank App (Windows Phone Silverlight)**.</span></span> <span data-ttu-id="64f11-127">Ange appens **namn** och **lösningens namn**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="64f11-127">Fill in the app **Name** and **Solution name**, and then click **OK**.</span></span>
   
    ![][1]
3. <span data-ttu-id="64f11-128">Du kan välja antingen **Windows Phone 8.0** eller **Windows Phone 8.1**.</span><span class="sxs-lookup"><span data-stu-id="64f11-128">You can choose to target either **Windows Phone 8.0** or **Windows Phone 8.1**.</span></span>

<span data-ttu-id="64f11-129">Nu har du skapat en ny Windows Phone Silverlight-app där Azure Mobile Engagement SDK kan integreras.</span><span class="sxs-lookup"><span data-stu-id="64f11-129">You have now created a new Windows Phone Silverlight app into which we will integrate the Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="64f11-130">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="64f11-130">Connect your app to the Mobile Engagement backend</span></span>
1. <span data-ttu-id="64f11-131">Installera NuGet-paketet [MicrosoftAzure.MobileEngagement] i projektet.</span><span class="sxs-lookup"><span data-stu-id="64f11-131">Install the [MicrosoftAzure.MobileEngagement] nuget package in your project.</span></span>
2. <span data-ttu-id="64f11-132">Öppna `WMAppManifest.xml` (under egenskapsmappen) och kontrollera att följande har angetts (lägg till dem om de saknas) i taggen `<Capabilities />`:</span><span class="sxs-lookup"><span data-stu-id="64f11-132">Open `WMAppManifest.xml` (under the Properties folder) and make sure the following are declared (add them if they are not) in the `<Capabilities />` tag:</span></span>
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. <span data-ttu-id="64f11-133">Klistra sedan in anslutningssträngen som du kopierade tidigare för Mobile Engagement-appen och klistra in den i filen `Resources\EngagementConfiguration.xml`, mellan taggarna `<connectionString>` och `</connectionString>`:</span><span class="sxs-lookup"><span data-stu-id="64f11-133">Now paste the connection string that you copied earlier for your Mobile Engagement app and paste it in the `Resources\EngagementConfiguration.xml` file, between the `<connectionString>` and `</connectionString>` tags:</span></span>
   
    ![][3]
4. <span data-ttu-id="64f11-134">I filen `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="64f11-134">In the `App.xaml.cs` file:</span></span>
   
    <span data-ttu-id="64f11-135">a.</span><span class="sxs-lookup"><span data-stu-id="64f11-135">a.</span></span> <span data-ttu-id="64f11-136">Lägg till instruktionen `using`:</span><span class="sxs-lookup"><span data-stu-id="64f11-136">Add the `using` statement:</span></span>
   
            using Microsoft.Azure.Engagement;
   
    <span data-ttu-id="64f11-137">b.</span><span class="sxs-lookup"><span data-stu-id="64f11-137">b.</span></span> <span data-ttu-id="64f11-138">Initiera SDK i metoden `Application_Launching`:</span><span class="sxs-lookup"><span data-stu-id="64f11-138">Initialize the SDK in the `Application_Launching` method:</span></span>
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    <span data-ttu-id="64f11-139">c.</span><span class="sxs-lookup"><span data-stu-id="64f11-139">c.</span></span> <span data-ttu-id="64f11-140">Infoga följande i `Application_Activated`:</span><span class="sxs-lookup"><span data-stu-id="64f11-140">Insert the following in the `Application_Activated`:</span></span>
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <span data-ttu-id="64f11-141"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="64f11-141"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="64f11-142">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm (aktivitet) till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="64f11-142">In order to start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="64f11-143">Lägg till instruktionen `using` i MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="64f11-143">In the MainPage.xaml.cs, add the `using` statement:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="64f11-144">Ersätt grundklassen i **MainPage**, som kommer före **PhoneApplicationPage**, med **EngagementPage**.</span><span class="sxs-lookup"><span data-stu-id="64f11-144">Replace the base class of **MainPage**, which is before **PhoneApplicationPage**, with **EngagementPage**.</span></span>
   
        class MainPage : EngagementPage 
3. <span data-ttu-id="64f11-145">I filen `MainPage.xml`:</span><span class="sxs-lookup"><span data-stu-id="64f11-145">In your `MainPage.xml` file:</span></span>
   
    <span data-ttu-id="64f11-146">a.</span><span class="sxs-lookup"><span data-stu-id="64f11-146">a.</span></span> <span data-ttu-id="64f11-147">Lägg till följande i namnområdesdeklarationerna:</span><span class="sxs-lookup"><span data-stu-id="64f11-147">Add to your namespaces declarations:</span></span>
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    <span data-ttu-id="64f11-148">b.</span><span class="sxs-lookup"><span data-stu-id="64f11-148">b.</span></span> <span data-ttu-id="64f11-149">Ersätt `phone:PhoneApplicationPage` i XML-taggnamnet med `engagement:EngagementPage`.</span><span class="sxs-lookup"><span data-stu-id="64f11-149">Replace `phone:PhoneApplicationPage` in the XML tag name with `engagement:EngagementPage`.</span></span>

## <span data-ttu-id="64f11-150"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="64f11-150"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="64f11-151"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="64f11-151"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="64f11-152">Med Mobile Engagement kan du samverka med användarna, och köra kampanjer med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="64f11-152">Mobile Engagement allows you to interact and reach your users with Push Notifications and in-app Messaging in the context of campaigns.</span></span> <span data-ttu-id="64f11-153">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="64f11-153">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="64f11-154">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="64f11-154">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-mpns-push-notifications"></a><span data-ttu-id="64f11-155">Konfigurera appen för att ta emot push-meddelanden med MPNS</span><span class="sxs-lookup"><span data-stu-id="64f11-155">Enable your app to receive MPNS Push Notifications</span></span>
<span data-ttu-id="64f11-156">Lägg till nya funktioner i filen `WMAppManifest.xml`:</span><span class="sxs-lookup"><span data-stu-id="64f11-156">Add new Capabilities to your `WMAppManifest.xml` file:</span></span>

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-the-reach-sdk"></a><span data-ttu-id="64f11-157">Initiera REACH SDK</span><span class="sxs-lookup"><span data-stu-id="64f11-157">Initialize the REACH SDK</span></span>
1. <span data-ttu-id="64f11-158">I `App.xaml.cs` anropar du `EngagementReach.Instance.Init();` i funktionen **Application_Launching**, direkt efter agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="64f11-158">In `App.xaml.cs`, call `EngagementReach.Instance.Init();` in the **Application_Launching** function, right after the agent initialization:</span></span>
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. <span data-ttu-id="64f11-159">I `App.xaml.cs` anropar du `EngagementReach.Instance.OnActivated(e);` i funktionen **Application_Activated**, direkt efter agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="64f11-159">In `App.xaml.cs`, call `EngagementReach.Instance.OnActivated(e);` in the **Application_Activated** function, right after the agent initialization:</span></span>
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

<span data-ttu-id="64f11-160">Då var allt klart.</span><span class="sxs-lookup"><span data-stu-id="64f11-160">You're all set.</span></span> <span data-ttu-id="64f11-161">Nu är det dags att kontrollera att den grundläggande integrationen har genomförts.</span><span class="sxs-lookup"><span data-stu-id="64f11-161">Now we will verify that you have correctly cried out this basic integration.</span></span>

## <span data-ttu-id="64f11-162"><a id="send"></a>Skicka ett meddelande till din app</span><span class="sxs-lookup"><span data-stu-id="64f11-162"><a id="send"></a>Send a notification to your app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="64f11-163">Du bör nu se ett meddelande på enheten som visas som en avisering via app om appen är öppen, annars visas det i form av ett popup-meddelande som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="64f11-163">You should now see a notification on your device which will show up as an in-app notification if the app is open otherwise as a toast notification like the following:</span></span> 

![][6]

<!-- URLs. -->
<span data-ttu-id="64f11-164">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664</span><span class="sxs-lookup"><span data-stu-id="64f11-164">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664</span></span>
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
