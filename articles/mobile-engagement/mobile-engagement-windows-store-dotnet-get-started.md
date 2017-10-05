---
title: "Komma igång med Azure Mobile Engagement för universella Windows-appar"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för universella Windows-appar."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 40db7e4dd151ec391c754dc6d4145aeeb8058eca
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a><span data-ttu-id="da8b7-103">Kom igång med Azure Mobile Engagement för universella Windows-appar</span><span class="sxs-lookup"><span data-stu-id="da8b7-103">Get started with Azure Mobile Engagement for Windows Universal Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="da8b7-104">I den här artikeln beskrivs hur du använder Azure Mobile Engagement för att förstå appanvändningen, och hur du skickar push-meddelanden till segmenterade användare i ett universellt Windows-program.</span><span class="sxs-lookup"><span data-stu-id="da8b7-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and send push notifications to segmented users of a Windows Universal application.</span></span>
<span data-ttu-id="da8b7-105">I den här kursen går vi igenom ett enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="da8b7-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="da8b7-106">Du skapar en tom universell Windows-app som samlar in grundläggande appanvändningsdata och tar emot push-meddelanden med Windows Notification Service (WNS).</span><span class="sxs-lookup"><span data-stu-id="da8b7-106">You create a blank Windows Universal App that collects basic app usage data and receives push notifications using Windows Notification Service (WNS).</span></span>

> [!NOTE]
> <span data-ttu-id="da8b7-107">Tjänsten Azure Mobile Engagement kommer att dras tillbaka i mars 2018 och är för närvarande endast tillgänglig för befintliga kunder.</span><span class="sxs-lookup"><span data-stu-id="da8b7-107">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="da8b7-108">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="da8b7-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="da8b7-109">Krav</span><span class="sxs-lookup"><span data-stu-id="da8b7-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a><span data-ttu-id="da8b7-110">Konfigurera Mobile Engagement för din universella Windows-app</span><span class="sxs-lookup"><span data-stu-id="da8b7-110">Set up Mobile Engagement for your Windows Universal app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="da8b7-111"><a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="da8b7-111"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="da8b7-112">I den här kursen behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="da8b7-112">This tutorial presents a "basic integration," which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="da8b7-113">Den fullständiga integrationsdokumentationen finns i [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="da8b7-113">The complete integration documentation can be found in the [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span></span>

<span data-ttu-id="da8b7-114">Du skapar en grundläggande app i Visual Studio för att demonstrera integrationen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-114">You create a basic app with Visual Studio to demonstrate the integration.</span></span>

### <a name="create-a-windows-universal-app-project"></a><span data-ttu-id="da8b7-115">Skapa ett universellt Windows-approjekt</span><span class="sxs-lookup"><span data-stu-id="da8b7-115">Create a Windows Universal App project</span></span>
<span data-ttu-id="da8b7-116">I följande steg används Visual Studio 2015, men stegen är ganska lika i tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da8b7-116">The following steps assume the use of Visual Studio 2015 though the steps are similar in earlier versions of Visual Studio.</span></span>

1. <span data-ttu-id="da8b7-117">Starta Visual Studio och välj **New Project** (Nytt projekt) på **startskärmen**.</span><span class="sxs-lookup"><span data-stu-id="da8b7-117">Start Visual Studio, and in the **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="da8b7-118">Välj **Windows** -> **Universal** -> **Blank App (Universal Windows)** (Tom app [Universellt Windows]) i popup-fönstret.</span><span class="sxs-lookup"><span data-stu-id="da8b7-118">In the pop-up, select **Windows** -> **Universal** -> **Blank App (Universal Windows)**.</span></span> <span data-ttu-id="da8b7-119">Ange appens **namn** och **lösningens namn**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="da8b7-119">Fill in the app **Name** and **Solution name**, and then click **OK**.</span></span>

    ![][1]

<span data-ttu-id="da8b7-120">Du har nu skapat ett universellt Windows-app-projekt där du sedan integrerar Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="da8b7-120">You have now created a Windows Universal App project into which you next integrate the Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="da8b7-121">Ansluta appen till Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="da8b7-121">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="da8b7-122">Installera Nuget-paketet [MicrosoftAzure.MobileEngagement] i projektet.</span><span class="sxs-lookup"><span data-stu-id="da8b7-122">Install the [MicrosoftAzure.MobileEngagement] Nuget package in your project.</span></span> <span data-ttu-id="da8b7-123">Om du utvecklar för både Windows- och Windows Phone-plattformen måste du göra det här för båda projekten.</span><span class="sxs-lookup"><span data-stu-id="da8b7-123">If you are targeting both Windows and Windows Phone platforms, you need to do this for both projects.</span></span> <span data-ttu-id="da8b7-124">Samma Nuget-paket placerar rätt plattformsspecifika binärfiler i varje projekt för Windows 8.x och Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="da8b7-124">For Windows 8.x and Windows Phone 8.1, the same Nuget package places the correct platform-specific binaries in each project.</span></span>
2. <span data-ttu-id="da8b7-125">Öppna **Package.appxmanifest** och kontrollera att följande funktion har lagts till:</span><span class="sxs-lookup"><span data-stu-id="da8b7-125">Open **Package.appxmanifest** and make sure that the following capability is added there:</span></span>

        Internet (Client)

    ![][2]
3. <span data-ttu-id="da8b7-126">Ta sedan anslutningssträngen som du kopierade tidigare för Mobile Engagement-appen och klistra in den i filen `Resources\EngagementConfiguration.xml`, mellan taggarna `<connectionString>` och `</connectionString>`:</span><span class="sxs-lookup"><span data-stu-id="da8b7-126">Now copy the connection string that you copied earlier for your Mobile Engagement App and paste it in the `Resources\EngagementConfiguration.xml` file, between the `<connectionString>` and `</connectionString>` tags:</span></span>

    ![][3]

    > [!TIP]
    > <span data-ttu-id="da8b7-127">Om appen används på både Windows- och Windows Phone-plattformen ska du fortfarande skapa två Mobile Engagement-program – ett för varje plattform som stöds.</span><span class="sxs-lookup"><span data-stu-id="da8b7-127">If your App targets both Windows and Windows Phone platforms, you should still create two Mobile Engagement Applications - one for each supported platform.</span></span> <span data-ttu-id="da8b7-128">Genom att ha två appar säkerställer du att du kan skapa rätt segmentering av målgruppen och skicka lämpliga målanpassade meddelanden för varje plattform.</span><span class="sxs-lookup"><span data-stu-id="da8b7-128">Having two apps ensures that you can create correct segmentation of the audience and can send appropriately targeted notifications for each platform.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="da8b7-129">NuGet kopierar inte SDK-resurserna automatiskt i ditt program för Windows 10 UWP.</span><span class="sxs-lookup"><span data-stu-id="da8b7-129">NuGet does not automatically copy the SDK resources in your Windows 10 UWP application.</span></span> <span data-ttu-id="da8b7-130">Du måste göra det manuellt via de steg som visas (readme.txt) när NuGet-paketet installeras.</span><span class="sxs-lookup"><span data-stu-id="da8b7-130">You have to do it manually following the steps which show up (readme.txt) when the Nuget package is installed.</span></span>  

1. <span data-ttu-id="da8b7-131">I filen `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="da8b7-131">In the `App.xaml.cs` file:</span></span>

    <span data-ttu-id="da8b7-132">a.</span><span class="sxs-lookup"><span data-stu-id="da8b7-132">a.</span></span> <span data-ttu-id="da8b7-133">Lägg till instruktionen `using`:</span><span class="sxs-lookup"><span data-stu-id="da8b7-133">Add the `using` statement:</span></span>

            using Microsoft.Azure.Engagement;

    <span data-ttu-id="da8b7-134">b.</span><span class="sxs-lookup"><span data-stu-id="da8b7-134">b.</span></span> <span data-ttu-id="da8b7-135">Lägg till en metod som initierar Engagement:</span><span class="sxs-lookup"><span data-stu-id="da8b7-135">Add a method that initializes the Engagement:</span></span>

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of the code
           }

    <span data-ttu-id="da8b7-136">c.</span><span class="sxs-lookup"><span data-stu-id="da8b7-136">c.</span></span> <span data-ttu-id="da8b7-137">Initiera SDK:n i metoden **OnLaunched**:</span><span class="sxs-lookup"><span data-stu-id="da8b7-137">Initialize the SDK in the **OnLaunched** method:</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

    <span data-ttu-id="da8b7-138">c.</span><span class="sxs-lookup"><span data-stu-id="da8b7-138">c.</span></span> <span data-ttu-id="da8b7-139">Infoga följande i metoden **OnActivated** och lägg till metoden om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="da8b7-139">Insert the following in the **OnActivated** method and add the method if it is not already present:</span></span>

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of the code
            }

## <span data-ttu-id="da8b7-140"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="da8b7-140"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="da8b7-141">För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en skärm (aktivitet) till Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-141">To start sending data and ensuring that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

1. <span data-ttu-id="da8b7-142">Lägg till följande `using`-instruktion i **MainPage.xaml.cs**:</span><span class="sxs-lookup"><span data-stu-id="da8b7-142">In the **MainPage.xaml.cs**, add the following `using` statement:</span></span>

    <span data-ttu-id="da8b7-143">using Microsoft.Azure.Engagement.Overlay;</span><span class="sxs-lookup"><span data-stu-id="da8b7-143">using Microsoft.Azure.Engagement.Overlay;</span></span>
2. <span data-ttu-id="da8b7-144">Byt ut grundklassen för **MainPage** från **Page** mot **EngagementPageOverlay**:</span><span class="sxs-lookup"><span data-stu-id="da8b7-144">Change the base class of **MainPage** from **Page** to **EngagementPageOverlay**:</span></span>

        class MainPage : EngagementPageOverlay
3. <span data-ttu-id="da8b7-145">I filen `MainPage.xaml`:</span><span class="sxs-lookup"><span data-stu-id="da8b7-145">In the `MainPage.xaml` file:</span></span>

    <span data-ttu-id="da8b7-146">a.</span><span class="sxs-lookup"><span data-stu-id="da8b7-146">a.</span></span> <span data-ttu-id="da8b7-147">Lägg till följande i namnområdesdeklarationerna:</span><span class="sxs-lookup"><span data-stu-id="da8b7-147">Add to your namespaces declarations:</span></span>

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    <span data-ttu-id="da8b7-148">b.</span><span class="sxs-lookup"><span data-stu-id="da8b7-148">b.</span></span> <span data-ttu-id="da8b7-149">Ersätt **Page** i XML-taggnamnet med **engagement:EngagementPageOverlay**</span><span class="sxs-lookup"><span data-stu-id="da8b7-149">Replace the **Page** in the XML tag name with **engagement:EngagementPageOverlay**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da8b7-150">Om sidan åsidosätter metoden `OnNavigatedTo` ska du anropa `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="da8b7-150">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="da8b7-151">Annars rapporteras inte aktiviteten (`EngagementPage` anropar `StartActivity` i tillhörande `OnNavigatedTo`-metod).</span><span class="sxs-lookup"><span data-stu-id="da8b7-151">Otherwise, the activity is not reported `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span> <span data-ttu-id="da8b7-152">Detta är i synnerhet viktigt i ett Windows Phone-projekt där standardmallen har en `OnNavigatedTo`-metod.</span><span class="sxs-lookup"><span data-stu-id="da8b7-152">This is especially important in a Windows Phone project where the default template has an `OnNavigatedTo` method.</span></span>
>
> <span data-ttu-id="da8b7-153">För **Windows 10 Universal-appar** använder du den metod som rekommenderas i avsnittet ”Recommended method: overload your Page classes” (Rekommenderad metod: överbelasta dina sidklasser) i [Advanced Reporting with the Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md) (Avancerad rapportering med Engagement-SDK för Windows Universal-appar) i stället för den som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="da8b7-153">For **Windows 10 Universal apps**, use the method recommended in the "Recommended method: overload your Page classes" section of [Advanced Reporting with the Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md), rather than the one mentioned above.</span></span>

## <span data-ttu-id="da8b7-154"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="da8b7-154"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="da8b7-155"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="da8b7-155"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="da8b7-156">Med Mobile Engagement kan du samverka med användarna, och köra kampanjer med push-meddelanden och meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-156">Mobile Engagement allows you to interact and reach your users with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="da8b7-157">Modulen som används för det heter REACH och finns i Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-157">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="da8b7-158">I följande avsnitt konfigurerar du appen för att ta emot dem.</span><span class="sxs-lookup"><span data-stu-id="da8b7-158">The following sections set up your app to receive them.</span></span>

### <a name="enable-your-app-to-receive-wns-push-notifications"></a><span data-ttu-id="da8b7-159">Konfigurera appen för att ta emot push-meddelanden med WNS</span><span class="sxs-lookup"><span data-stu-id="da8b7-159">Enable your app to receive WNS Push Notifications</span></span>
1. <span data-ttu-id="da8b7-160">Ställ in **Toast capable** (Popup-kompatibel) på **Yes** (Ja) i filen `Package.appxmanifest` på fliken **Application** (Program) under **Notifications** (Meddelanden).</span><span class="sxs-lookup"><span data-stu-id="da8b7-160">In the `Package.appxmanifest` file, in the **Application** tab, under **Notifications**, set **Toast capable:** to **Yes**</span></span>

    ![][5]

### <a name="initialize-the-reach-sdk"></a><span data-ttu-id="da8b7-161">Initiera REACH SDK</span><span class="sxs-lookup"><span data-stu-id="da8b7-161">Initialize the REACH SDK</span></span>
<span data-ttu-id="da8b7-162">I `App.xaml.cs` anropar du **EngagementReach.Instance.Init(e);** i funktionen **InitEngagement** direkt efter agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="da8b7-162">In `App.xaml.cs`, call **EngagementReach.Instance.Init(e);** in the **InitEngagement** function right after the agent initialization:</span></span>

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

<span data-ttu-id="da8b7-163">Du är redo att skicka ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="da8b7-163">You're ready to send a toast.</span></span> <span data-ttu-id="da8b7-164">Sedan kontrollerar vi att den grundläggande integrationen har genomförts.</span><span class="sxs-lookup"><span data-stu-id="da8b7-164">Next we verify that you have correctly carried out this basic integration.</span></span>

### <a name="grant-access-to-mobile-engagement-to-send-notifications"></a><span data-ttu-id="da8b7-165">Tillåt Mobile Engagement att skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="da8b7-165">Grant access to Mobile Engagement to send notifications</span></span>
1. <span data-ttu-id="da8b7-166">Öppna [Windows Store Dev Center] i webbläsaren, logga in och skapa ett konto om det behövs.</span><span class="sxs-lookup"><span data-stu-id="da8b7-166">Open [Windows Store Dev Center] in your web browser, login, and create an account if necessary.</span></span>
2. <span data-ttu-id="da8b7-167">Klicka på **instrumentpanelen** längst upp till höger och klicka sedan på **Create a new app** (Skapa en ny app) på menyn i den vänstra panelen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-167">Click **Dashboard** at the top right corner and then click **Create a new app** from the left panel menu.</span></span>

    ![][9]
3. <span data-ttu-id="da8b7-168">Skapa din app genom att reservera dess namn.</span><span class="sxs-lookup"><span data-stu-id="da8b7-168">Create your app by reserving its name.</span></span>

    ![][10]
4. <span data-ttu-id="da8b7-169">Navigera till **Services -> Push notifications** (Tjänster -> Push-meddelanden) på den vänstra menyn när appen har skapats.</span><span class="sxs-lookup"><span data-stu-id="da8b7-169">Once the app has been created, navigate to **Services -> Push notifications** from the left menu.</span></span>

    ![][11]
5. <span data-ttu-id="da8b7-170">Klicka på länken **Live Services site** (Plats med livetjänster) i push-meddelandeavsnittet.</span><span class="sxs-lookup"><span data-stu-id="da8b7-170">In the Push notifications section, click the **Live Services site** link.</span></span>

    ![][12]
6. <span data-ttu-id="da8b7-171">Då kommer du till avsnittet för push-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="da8b7-171">You navigate to the Push credentials section.</span></span> <span data-ttu-id="da8b7-172">Kontrollera att du befinner dig i avsnittet **App Settings** (Appinställningar) och kopiera sedan **Package SID** (Paket-SID) och **Client secret** (Klienthemlighet).</span><span class="sxs-lookup"><span data-stu-id="da8b7-172">Make sure you are in the **App Settings** section and then copy your **Package SID** and **Client secret**</span></span>

    ![][13]
7. <span data-ttu-id="da8b7-173">Navigera till **Settings** (Inställningar) i Mobile Engagement-portalen, och klicka på avsnittet **Native Push** (Systemspecifik push-avisering) till vänster.</span><span class="sxs-lookup"><span data-stu-id="da8b7-173">Navigate to the **Settings** of your Mobile Engagement portal, and click the **Native Push** section on the left.</span></span> <span data-ttu-id="da8b7-174">Klicka sedan på knappen **Edit** (Redigera) för att ange **Package security identifier (SID)** (Säkerhetsidentifierare för paket [SID]) och **Secret Key** (Hemlig nyckel) enligt bilden:</span><span class="sxs-lookup"><span data-stu-id="da8b7-174">Then, click the **Edit** button to enter your **Package security identifier (SID)** and your **Secret Key** as shown:</span></span>

    ![][6]
8. <span data-ttu-id="da8b7-175">Kontrollera slutligen att du har associerat Visual Studio-appen med den här skapade appen i appbutiken.</span><span class="sxs-lookup"><span data-stu-id="da8b7-175">Finally make sure that you have associated your Visual Studio app with this created app in the App store.</span></span> <span data-ttu-id="da8b7-176">Klicka på**Associate App with Store** (Associera appen med Store) i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="da8b7-176">Click **Associate App with Store** in Visual Studio.</span></span>

    ![][7]

## <span data-ttu-id="da8b7-177"><a id="send"></a>Skicka ett meddelande till din app</span><span class="sxs-lookup"><span data-stu-id="da8b7-177"><a id="send"></a>Send a notification to your app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="da8b7-178">Om appen körs visas en avisering via appen.</span><span class="sxs-lookup"><span data-stu-id="da8b7-178">If the app is running, you see an in-app notification.</span></span> <span data-ttu-id="da8b7-179">Om appen är stängd visas istället ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="da8b7-179">otherwise if the app is closed, you see a toast notification.</span></span>
<span data-ttu-id="da8b7-180">Om du ser en avisering via app men inte något popup-meddelande och du kör appen i felsökningsläge i Visual Studio ska du prova **Lifecycle events -> Suspend** (Livscykelhändelser -> Gör uppehåll) i verktygsfältet för att säkerställa att appen har gjort uppehåll.</span><span class="sxs-lookup"><span data-stu-id="da8b7-180">If you see an in-app notification but not a toast notification, and you are running the app in debug mode in Visual Studio, then try **Lifecycle events -> Suspend** in the toolbar to ensure that the app is suspended.</span></span> <span data-ttu-id="da8b7-181">Om du klickade på startsideknappen när du felsökte programmet i Visual Studio gör det inte alltid uppehåll och även om du ser aviseringen via appen visas den inte som ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="da8b7-181">If you clicked the Home button while debugging the application in Visual Studio, then it doesn't always get suspended and while you see the notification as in-app, it doesn't show up as a toast notification.</span></span>  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
<span data-ttu-id="da8b7-182">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592</span><span class="sxs-lookup"><span data-stu-id="da8b7-182">[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592</span></span>
<span data-ttu-id="da8b7-183">[Windows Store Dev Center]: https://dev.windows.com</span><span class="sxs-lookup"><span data-stu-id="da8b7-183">[Windows Store Dev Center]: https://dev.windows.com</span></span>
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
