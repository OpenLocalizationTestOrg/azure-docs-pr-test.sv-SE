---
title: "aaaGet igång med Windows Universal-appar Azure Mobile Engagement"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för universella Windows-appar."
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
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a><span data-ttu-id="d33de-103">Kom igång med Azure Mobile Engagement för universella Windows-appar</span><span class="sxs-lookup"><span data-stu-id="d33de-103">Get started with Azure Mobile Engagement for Windows Universal Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="d33de-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand din app användnings- och skicka push-meddelanden toosegmented användare i ett universella Windows-program.</span><span class="sxs-lookup"><span data-stu-id="d33de-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and send push notifications toosegmented users of a Windows Universal application.</span></span>
<span data-ttu-id="d33de-105">Den här kursen visar hello enkelt scenario för sändning med Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="d33de-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="d33de-106">Du skapar en tom universell Windows-app som samlar in grundläggande appanvändningsdata och tar emot push-meddelanden med Windows Notification Service (WNS).</span><span class="sxs-lookup"><span data-stu-id="d33de-106">You create a blank Windows Universal App that collects basic app usage data and receives push notifications using Windows Notification Service (WNS).</span></span>

> [!NOTE]
> <span data-ttu-id="d33de-107">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="d33de-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="d33de-108">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="d33de-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d33de-109">Krav</span><span class="sxs-lookup"><span data-stu-id="d33de-109">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a><span data-ttu-id="d33de-110">Konfigurera Mobile Engagement för din universella Windows-app</span><span class="sxs-lookup"><span data-stu-id="d33de-110">Set up Mobile Engagement for your Windows Universal app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="d33de-111"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="d33de-111"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="d33de-112">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minimal ange nödvändiga toocollect data och skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d33de-112">This tutorial presents a "basic integration," which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="d33de-113">hello fullständiga integrationsdokumentationen finns i hello [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d33de-113">hello complete integration documentation can be found in hello [Mobile Engagement Windows Universal SDK integration](mobile-engagement-windows-store-sdk-overview.md).</span></span>

<span data-ttu-id="d33de-114">Du kan skapa en grundläggande app med Visual Studio toodemonstrate hello-integration.</span><span class="sxs-lookup"><span data-stu-id="d33de-114">You create a basic app with Visual Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-windows-universal-app-project"></a><span data-ttu-id="d33de-115">Skapa ett universellt Windows-approjekt</span><span class="sxs-lookup"><span data-stu-id="d33de-115">Create a Windows Universal App project</span></span>
<span data-ttu-id="d33de-116">hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d33de-116">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span>

1. <span data-ttu-id="d33de-117">Starta Visual Studio och i hello **Start** väljer **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="d33de-117">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="d33de-118">Välj i popup-fönstret hello **Windows** -> **Universal** -> **tom App (Universal Windows)**.</span><span class="sxs-lookup"><span data-stu-id="d33de-118">In hello pop-up, select **Windows** -> **Universal** -> **Blank App (Universal Windows)**.</span></span> <span data-ttu-id="d33de-119">Fyll i hello app **namn** och **lösningsnamn**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d33de-119">Fill in hello app **Name** and **Solution name**, and then click **OK**.</span></span>

    ![][1]

<span data-ttu-id="d33de-120">Nu har du skapat en universell Windows-App-projekt där du sedan integreras hello Azure Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="d33de-120">You have now created a Windows Universal App project into which you next integrate hello Azure Mobile Engagement SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="d33de-121">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="d33de-121">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="d33de-122">Installera hello [MicrosoftAzure.MobileEngagement] Nuget-paket i projektet.</span><span class="sxs-lookup"><span data-stu-id="d33de-122">Install hello [MicrosoftAzure.MobileEngagement] Nuget package in your project.</span></span> <span data-ttu-id="d33de-123">Om du utvecklar för både Windows och Windows Phone-plattformar, behöver du toodo detta för båda projekten.</span><span class="sxs-lookup"><span data-stu-id="d33de-123">If you are targeting both Windows and Windows Phone platforms, you need toodo this for both projects.</span></span> <span data-ttu-id="d33de-124">För Windows 8.x och Windows Phone 8.1 hello samma Nuget-paketet platser hello rätt plattformsspecifika binärfiler i varje projekt.</span><span class="sxs-lookup"><span data-stu-id="d33de-124">For Windows 8.x and Windows Phone 8.1, hello same Nuget package places hello correct platform-specific binaries in each project.</span></span>
2. <span data-ttu-id="d33de-125">Öppna **Package.appxmanifest** och se till att hello följande funktion har lagts till:</span><span class="sxs-lookup"><span data-stu-id="d33de-125">Open **Package.appxmanifest** and make sure that hello following capability is added there:</span></span>

        Internet (Client)

    ![][2]
3. <span data-ttu-id="d33de-126">Nu kopiera hello anslutningssträng som du kopierade tidigare för Mobile Engagement-appen och klistra in den i hello `Resources\EngagementConfiguration.xml` fil mellan hello `<connectionString>` och `</connectionString>` taggar:</span><span class="sxs-lookup"><span data-stu-id="d33de-126">Now copy hello connection string that you copied earlier for your Mobile Engagement App and paste it in hello `Resources\EngagementConfiguration.xml` file, between hello `<connectionString>` and `</connectionString>` tags:</span></span>

    ![][3]

    > [!TIP]
    > <span data-ttu-id="d33de-127">Om appen används på både Windows- och Windows Phone-plattformen ska du fortfarande skapa två Mobile Engagement-program – ett för varje plattform som stöds.</span><span class="sxs-lookup"><span data-stu-id="d33de-127">If your App targets both Windows and Windows Phone platforms, you should still create two Mobile Engagement Applications - one for each supported platform.</span></span> <span data-ttu-id="d33de-128">Med två appar som gör att du kan skapa rätt segmentering av målgruppen hello och kan skicka lämpliga målanpassade meddelanden för varje plattform.</span><span class="sxs-lookup"><span data-stu-id="d33de-128">Having two apps ensures that you can create correct segmentation of hello audience and can send appropriately targeted notifications for each platform.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d33de-129">NuGet kopierar inte automatiskt hello SDK-resurser i din Windows 10 UWP-App.</span><span class="sxs-lookup"><span data-stu-id="d33de-129">NuGet does not automatically copy hello SDK resources in your Windows 10 UWP application.</span></span> <span data-ttu-id="d33de-130">Det har toodo manuellt följande hello som visas (viktigt.txt) när hello Nuget-paketet har installerats.</span><span class="sxs-lookup"><span data-stu-id="d33de-130">You have toodo it manually following hello steps which show up (readme.txt) when hello Nuget package is installed.</span></span>  

1. <span data-ttu-id="d33de-131">I hello `App.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="d33de-131">In hello `App.xaml.cs` file:</span></span>

    <span data-ttu-id="d33de-132">a.</span><span class="sxs-lookup"><span data-stu-id="d33de-132">a.</span></span> <span data-ttu-id="d33de-133">Lägg till hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="d33de-133">Add hello `using` statement:</span></span>

            using Microsoft.Azure.Engagement;

    <span data-ttu-id="d33de-134">b.</span><span class="sxs-lookup"><span data-stu-id="d33de-134">b.</span></span> <span data-ttu-id="d33de-135">Lägg till en metod som initierar hello Engagement:</span><span class="sxs-lookup"><span data-stu-id="d33de-135">Add a method that initializes hello Engagement:</span></span>

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    <span data-ttu-id="d33de-136">c.</span><span class="sxs-lookup"><span data-stu-id="d33de-136">c.</span></span> <span data-ttu-id="d33de-137">Initiera hello SDK i hello **OnLaunched** metod:</span><span class="sxs-lookup"><span data-stu-id="d33de-137">Initialize hello SDK in hello **OnLaunched** method:</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    <span data-ttu-id="d33de-138">c.</span><span class="sxs-lookup"><span data-stu-id="d33de-138">c.</span></span> <span data-ttu-id="d33de-139">Infoga följande hello i hello **OnActivated** metod och Lägg till hello-metoden om den inte redan finns:</span><span class="sxs-lookup"><span data-stu-id="d33de-139">Insert hello following in hello **OnActivated** method and add hello method if it is not already present:</span></span>

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <span data-ttu-id="d33de-140"><a id="monitor"></a>Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="d33de-140"><a id="monitor"></a>Enable real-time monitoring</span></span>
<span data-ttu-id="d33de-141">toostart skicka data och säkerställer att hello användarna är aktiva, måste du skicka minst en skärm (aktivitet) toohello Mobile Engagement-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="d33de-141">toostart sending data and ensuring that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

1. <span data-ttu-id="d33de-142">I hello **MainPage.xaml.cs**, Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="d33de-142">In hello **MainPage.xaml.cs**, add hello following `using` statement:</span></span>

    <span data-ttu-id="d33de-143">using Microsoft.Azure.Engagement.Overlay;</span><span class="sxs-lookup"><span data-stu-id="d33de-143">using Microsoft.Azure.Engagement.Overlay;</span></span>
2. <span data-ttu-id="d33de-144">Ändra hello basklassen **MainPage** från **sidan** för**EngagementPageOverlay**:</span><span class="sxs-lookup"><span data-stu-id="d33de-144">Change hello base class of **MainPage** from **Page** too**EngagementPageOverlay**:</span></span>

        class MainPage : EngagementPageOverlay
3. <span data-ttu-id="d33de-145">I hello `MainPage.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="d33de-145">In hello `MainPage.xaml` file:</span></span>

    <span data-ttu-id="d33de-146">a.</span><span class="sxs-lookup"><span data-stu-id="d33de-146">a.</span></span> <span data-ttu-id="d33de-147">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="d33de-147">Add tooyour namespaces declarations:</span></span>

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    <span data-ttu-id="d33de-148">b.</span><span class="sxs-lookup"><span data-stu-id="d33de-148">b.</span></span> <span data-ttu-id="d33de-149">Ersätt hello **sidan** i hello XML-taggnamnet med **engagement: EngagementPageOverlay**</span><span class="sxs-lookup"><span data-stu-id="d33de-149">Replace hello **Page** in hello XML tag name with **engagement:EngagementPageOverlay**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d33de-150">Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="d33de-150">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="d33de-151">Annars rapporteras inte aktiviteten hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).</span><span class="sxs-lookup"><span data-stu-id="d33de-151">Otherwise, hello activity is not reported `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span> <span data-ttu-id="d33de-152">Detta är särskilt viktigt i ett Windows Phone-projekt där standardmallen för hello har en `OnNavigatedTo` metod.</span><span class="sxs-lookup"><span data-stu-id="d33de-152">This is especially important in a Windows Phone project where hello default template has an `OnNavigatedTo` method.</span></span>
>
> <span data-ttu-id="d33de-153">För **Windows 10 Universal-appar**, använda hello-metod som rekommenderas i hello ”rekommenderad metod: överlagra sida-klasser” avsnittet av [avancerad rapportering med hello Windows Universal-appar Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md) , i stället för hello en nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="d33de-153">For **Windows 10 Universal apps**, use hello method recommended in hello "Recommended method: overload your Page classes" section of [Advanced Reporting with hello Windows Universal Apps Engagement SDK](mobile-engagement-windows-store-advanced-reporting.md), rather than hello one mentioned above.</span></span>

## <span data-ttu-id="d33de-154"><a id="monitor"></a>Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="d33de-154"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="d33de-155"><a id="integrate-push"></a>Aktivera push-meddelanden och meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="d33de-155"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="d33de-156">Mobile Engagement kan du toointeract och användarna med push-meddelanden och appmeddelanden i hello kontext kampanjer.</span><span class="sxs-lookup"><span data-stu-id="d33de-156">Mobile Engagement allows you toointeract and reach your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="d33de-157">Denna modul är heter REACH i hello Mobile Engagement-portalen.</span><span class="sxs-lookup"><span data-stu-id="d33de-157">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="d33de-158">hello följande avsnitt konfigurerar din app tooreceive dem.</span><span class="sxs-lookup"><span data-stu-id="d33de-158">hello following sections set up your app tooreceive them.</span></span>

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a><span data-ttu-id="d33de-159">Aktivera din app tooreceive Push-meddelanden med WNS</span><span class="sxs-lookup"><span data-stu-id="d33de-159">Enable your app tooreceive WNS Push Notifications</span></span>
1. <span data-ttu-id="d33de-160">I hello `Package.appxmanifest` -fil hello **programmet** fliken, under **meddelanden**, ange **popup-kompatibel:** för**Ja**</span><span class="sxs-lookup"><span data-stu-id="d33de-160">In hello `Package.appxmanifest` file, in hello **Application** tab, under **Notifications**, set **Toast capable:** too**Yes**</span></span>

    ![][5]

### <a name="initialize-hello-reach-sdk"></a><span data-ttu-id="d33de-161">Initiera hello REACH SDK</span><span class="sxs-lookup"><span data-stu-id="d33de-161">Initialize hello REACH SDK</span></span>
<span data-ttu-id="d33de-162">I `App.xaml.cs`, anropa **EngagementReach.Instance.Init(e);** i hello **InitEngagement** direkt efter agentinitieringen hello:</span><span class="sxs-lookup"><span data-stu-id="d33de-162">In `App.xaml.cs`, call **EngagementReach.Instance.Init(e);** in hello **InitEngagement** function right after hello agent initialization:</span></span>

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

<span data-ttu-id="d33de-163">Du är klar toosend ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d33de-163">You're ready toosend a toast.</span></span> <span data-ttu-id="d33de-164">Sedan kontrollerar vi att den grundläggande integrationen har genomförts.</span><span class="sxs-lookup"><span data-stu-id="d33de-164">Next we verify that you have correctly carried out this basic integration.</span></span>

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a><span data-ttu-id="d33de-165">Bevilja åtkomst tooMobile Engagement toosend meddelanden</span><span class="sxs-lookup"><span data-stu-id="d33de-165">Grant access tooMobile Engagement toosend notifications</span></span>
1. <span data-ttu-id="d33de-166">Öppna [Windows Store Dev Center] i webbläsaren, logga in och skapa ett konto om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d33de-166">Open [Windows Store Dev Center] in your web browser, login, and create an account if necessary.</span></span>
2. <span data-ttu-id="d33de-167">Klicka på **instrumentpanelen** hörn hello upp till höger och klicka sedan på **skapar en ny app** hello vänsterpanelen-menyn.</span><span class="sxs-lookup"><span data-stu-id="d33de-167">Click **Dashboard** at hello top right corner and then click **Create a new app** from hello left panel menu.</span></span>

    ![][9]
3. <span data-ttu-id="d33de-168">Skapa din app genom att reservera dess namn.</span><span class="sxs-lookup"><span data-stu-id="d33de-168">Create your app by reserving its name.</span></span>

    ![][10]
4. <span data-ttu-id="d33de-169">När hello app har skapats, navigera för**tjänster -> Push-meddelanden** hello vänstra menyn.</span><span class="sxs-lookup"><span data-stu-id="d33de-169">Once hello app has been created, navigate too**Services -> Push notifications** from hello left menu.</span></span>

    ![][11]
5. <span data-ttu-id="d33de-170">I hello Push-meddelanden genom att klicka på hello **webbplatsen Live-tjänster** länk.</span><span class="sxs-lookup"><span data-stu-id="d33de-170">In hello Push notifications section, click hello **Live Services site** link.</span></span>

    ![][12]
6. <span data-ttu-id="d33de-171">Du kan navigera toohello Push-autentiseringsuppgifter avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d33de-171">You navigate toohello Push credentials section.</span></span> <span data-ttu-id="d33de-172">Kontrollera att du befinner dig i hello **Appinställningar** avsnitt och kopiera ditt **paket-SID** och **klienthemlighet**</span><span class="sxs-lookup"><span data-stu-id="d33de-172">Make sure you are in hello **App Settings** section and then copy your **Package SID** and **Client secret**</span></span>

    ![][13]
7. <span data-ttu-id="d33de-173">Navigera toohello **inställningar** av Mobile Engagement-portalen och klicka på hello **Native Push** avsnittet hello vänster.</span><span class="sxs-lookup"><span data-stu-id="d33de-173">Navigate toohello **Settings** of your Mobile Engagement portal, and click hello **Native Push** section on hello left.</span></span> <span data-ttu-id="d33de-174">Klicka på hello **redigera** knappen tooenter din **säkerhetsidentifierare (SID) för paketet** och **hemlig nyckel** som visas:</span><span class="sxs-lookup"><span data-stu-id="d33de-174">Then, click hello **Edit** button tooenter your **Package security identifier (SID)** and your **Secret Key** as shown:</span></span>

    ![][6]
8. <span data-ttu-id="d33de-175">Kontrollera slutligen att du har associerat Visual Studio-appen med den här skapade appen i hello App store.</span><span class="sxs-lookup"><span data-stu-id="d33de-175">Finally make sure that you have associated your Visual Studio app with this created app in hello App store.</span></span> <span data-ttu-id="d33de-176">Klicka på**Associate App with Store** (Associera appen med Store) i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d33de-176">Click **Associate App with Store** in Visual Studio.</span></span>

    ![][7]

## <span data-ttu-id="d33de-177"><a id="send"></a>Skicka en avisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="d33de-177"><a id="send"></a>Send a notification tooyour app</span></span>
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

<span data-ttu-id="d33de-178">Om hello appen körs visas ett meddelande i appen.</span><span class="sxs-lookup"><span data-stu-id="d33de-178">If hello app is running, you see an in-app notification.</span></span> <span data-ttu-id="d33de-179">Annars om hello appen är stängd visas ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d33de-179">otherwise if hello app is closed, you see a toast notification.</span></span>
<span data-ttu-id="d33de-180">Om du ser en avisering via app men inte ett popup-meddelande, och du kör hello appen i felsökningsläge i Visual Studio, försök **Livscykelhändelser -> Suspend** i hello verktygsfältet tooensure hello appen har pausats.</span><span class="sxs-lookup"><span data-stu-id="d33de-180">If you see an in-app notification but not a toast notification, and you are running hello app in debug mode in Visual Studio, then try **Lifecycle events -> Suspend** in hello toolbar tooensure that hello app is suspended.</span></span> <span data-ttu-id="d33de-181">Om du klickade på hello startsideknappen när du felsöker programmet hello i Visual Studio sedan det alltid uppehåll inte och när du ser hello-meddelande via appen, den visas inte som ett popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d33de-181">If you clicked hello Home button while debugging hello application in Visual Studio, then it doesn't always get suspended and while you see hello notification as in-app, it doesn't show up as a toast notification.</span></span>  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows Store Dev Center]: https://dev.windows.com
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
