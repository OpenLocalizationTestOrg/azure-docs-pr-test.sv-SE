---
title: "aaaWindows universella appar nå SDK-Integration"
description: "Hur tooIntegrate Azure Mobile Engagement nå med universella Windows-appar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af311c65940014083333853875a00173b8d6783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="dbe27-103">Universella Windows-appar nå SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="dbe27-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="dbe27-104">Du måste följa hello integration beskrivs i hello [Windows Universal SDK-Integration av Engagement](mobile-engagement-windows-store-integrate-engagement.md) innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="dbe27-104">You must follow hello integration procedure described in hello [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="dbe27-105">Bädda in hello Engagement nå SDK universella Windows-projektet</span><span class="sxs-lookup"><span data-stu-id="dbe27-105">Embed hello Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="dbe27-106">Du har inte något tooadd.</span><span class="sxs-lookup"><span data-stu-id="dbe27-106">You do not have anything tooadd.</span></span> <span data-ttu-id="dbe27-107">`EngagementReach`referenser och resurser finns redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="dbe27-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="dbe27-108">Du kan anpassa avbildningar som finns i hello `Resources` mapp i ditt projekt, särskilt hello varumärken-ikonen (som standard toohello Engagement ikon).</span><span class="sxs-lookup"><span data-stu-id="dbe27-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span> <span data-ttu-id="dbe27-109">På universella appar du kan också flytta hello `Resources` mapp på din delade projektet tooshare dess innehåll mellan appar, men du kommer att ha tookeep hello `Resources\EngagementConfiguration.xml` fil på dess ursprungliga plats eftersom den är beroende plattform.</span><span class="sxs-lookup"><span data-stu-id="dbe27-109">On Universal Apps you can also move hello `Resources` folder on your shared project tooshare its content between apps, but you will have tookeep hello `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-hello-windows-notification-service"></a><span data-ttu-id="dbe27-110">Aktivera hello Windows Notification Service</span><span class="sxs-lookup"><span data-stu-id="dbe27-110">Enable hello Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="dbe27-111">Windows 8.x och Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="dbe27-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="dbe27-112">I ordning toouse hello **Windows Notification Service** (kallas WNS) i din `Package.appxmanifest` filen på `Application UI` klickar du på `All Image Assets` i hello bot vänstra ruta.</span><span class="sxs-lookup"><span data-stu-id="dbe27-112">In order toouse hello **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in hello left bot box.</span></span> <span data-ttu-id="dbe27-113">Vid hello höger om hello i `Notifications`, ändra `toast capable` från `(not set)` för`(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-113">At hello right of hello box in `Notifications`, change `toast capable` from `(not set)` too`(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="dbe27-114">Alla plattformar</span><span class="sxs-lookup"><span data-stu-id="dbe27-114">All platforms</span></span>
<span data-ttu-id="dbe27-115">Du behöver toosynchronize din app tooyour Microsoft-konto och toohello engagement plattform.</span><span class="sxs-lookup"><span data-stu-id="dbe27-115">You need toosynchronize your app tooyour Microsoft account and toohello engagement platform.</span></span> <span data-ttu-id="dbe27-116">För detta behöver toocreate ett konto eller logga in [windows dev center](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="dbe27-116">For this you need toocreate an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="dbe27-117">Efter att skapa ett nytt program och hitta hello SID och en hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="dbe27-117">After that create a new application and find hello SID and secret key.</span></span> <span data-ttu-id="dbe27-118">På hello engagement klientdel går på din appinställningen i `native push` och klistra in dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dbe27-118">On hello engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="dbe27-119">Efter det, högerklickar du på projektet, Välj `store` och `Associate App with hello Store...`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-119">After that, right click on your project, select `store` and `Associate App with hello Store...`.</span></span> <span data-ttu-id="dbe27-120">Tooselect hello program måste du ha skapa innan toosynchronize den.</span><span class="sxs-lookup"><span data-stu-id="dbe27-120">You just need tooselect hello application you have create before toosynchronize it.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="dbe27-121">Initiera hello Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="dbe27-121">Initialize hello Engagement Reach SDK</span></span>
<span data-ttu-id="dbe27-122">Ändra hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="dbe27-122">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="dbe27-123">Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i din `InitEngagement` metoden:</span><span class="sxs-lookup"><span data-stu-id="dbe27-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="dbe27-124">Hej `EngagementReach.Instance.Init` körs i en dedikerad tråd.</span><span class="sxs-lookup"><span data-stu-id="dbe27-124">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="dbe27-125">Du har inte toodo den själv.</span><span class="sxs-lookup"><span data-stu-id="dbe27-125">You do not have toodo it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="dbe27-126">Om du använder push-meddelanden någon annanstans i ditt program så att du har för[delar push-kanal](#push-channel-sharing) med Engagement nå.</span><span class="sxs-lookup"><span data-stu-id="dbe27-126">If you are using push notifications elsewhere in your application then you have too[share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="dbe27-127">Integrering</span><span class="sxs-lookup"><span data-stu-id="dbe27-127">Integration</span></span>
<span data-ttu-id="dbe27-128">Engagement tillhandahåller två sätt tooadd hello Reach i appen sidhuvud och mellan enskilda muskler vyer för meddelanden och avsökningar i ditt program: hello överlägg integrering och hello vyer manuell integrering.</span><span class="sxs-lookup"><span data-stu-id="dbe27-128">Engagement provides two ways tooadd hello Reach in-app banners and interstitial views for announcements and polls in your application: hello overlay integration and hello web views manual integration.</span></span> <span data-ttu-id="dbe27-129">Du bör inte kombinera båda metod på hello samma sida.</span><span class="sxs-lookup"><span data-stu-id="dbe27-129">You should not combine both approach on hello same page.</span></span>

<span data-ttu-id="dbe27-130">hello val mellan två hello-integrering kan sammanfattas det här sättet:</span><span class="sxs-lookup"><span data-stu-id="dbe27-130">hello choice between hello two integration could be summarized this way:</span></span>

* <span data-ttu-id="dbe27-131">Du kan välja hello överläggsintegration om sidorna ärver redan från hello Agent `EngagementPage`, själv för att ersätta `EngagementPage` av `EngagementPageOverlay` och `xmlns:engagement="using:Microsoft.Azure.Engagement"` av `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` på sidorna.</span><span class="sxs-lookup"><span data-stu-id="dbe27-131">You may choose hello overlay integration if your pages already inherits from hello Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="dbe27-132">Du kan välja hello web vyer manuell integration om du vill tooprecisely plats hello nå Användargränssnittet i sidorna eller om du inte vill tooadd en annan arv nivå tooyour sidor.</span><span class="sxs-lookup"><span data-stu-id="dbe27-132">You may choose hello web views manual integration if you want tooprecisely place hello Reach UI within your pages or if you don't want tooadd another inheritance level tooyour pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="dbe27-133">Överläggsintegration</span><span class="sxs-lookup"><span data-stu-id="dbe27-133">Overlay integration</span></span>
<span data-ttu-id="dbe27-134">Hej Engagement överlägget dynamiskt lägger till hello gränssnittselement används toodisplay Reach-kampanjer på sidan.</span><span class="sxs-lookup"><span data-stu-id="dbe27-134">hello Engagement overlay dynamically adds hello UI elements used toodisplay Reach campaigns in your page.</span></span> <span data-ttu-id="dbe27-135">Om hello överlägget inte passar din layout bör du hello Webbvyer manuell integrering i stället.</span><span class="sxs-lookup"><span data-stu-id="dbe27-135">If hello overlay doesn't suit your layout you should consider hello web views manual integration instead.</span></span>

<span data-ttu-id="dbe27-136">I XAML-filen ändringen `EngagementPage` referera för`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="dbe27-136">In your .xaml file change `EngagementPage` reference too`EngagementPageOverlay`</span></span>

* <span data-ttu-id="dbe27-137">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="dbe27-137">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="dbe27-138">Ersätt `engagement:EngagementPage` med `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="dbe27-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="dbe27-139">**Med EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="dbe27-140">**Med EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="dbe27-141">Tagga sedan sidan i i filen .cs `EngagementPageOverlay` i stället för `EngagementPage` och importera `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="dbe27-142">Ersätt `EngagementPage` med `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="dbe27-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="dbe27-143">**Med EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="dbe27-144">**Med EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="dbe27-145">Hej Engagement överlägget lägger till en `Grid` element på sidan består av layout och hello två `WebView` delar en för hello banderoll och hello andra en för hello mellan enskilda muskler vyn.</span><span class="sxs-lookup"><span data-stu-id="dbe27-145">hello Engagement overlay adds a `Grid` element on top of your page composed of your layout and hello two `WebView` elements one for hello banner and hello other one for hello interstitial view.</span></span>

<span data-ttu-id="dbe27-146">Du kan anpassa hello överlägget element direkt i hello `EngagementPageOverlay.cs` fil.</span><span class="sxs-lookup"><span data-stu-id="dbe27-146">You can customize hello overlay elements directly in hello `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="dbe27-147">Web vyer manuell integrering</span><span class="sxs-lookup"><span data-stu-id="dbe27-147">Web views manual integration</span></span>
<span data-ttu-id="dbe27-148">Reach söker efter sidorna för hello två `WebView` element som visar hello banderoll och hello mellan enskilda muskler vyn.</span><span class="sxs-lookup"><span data-stu-id="dbe27-148">Reach will be searching your pages for hello two `WebView` elements responsible for displaying hello banner and hello interstitial view.</span></span> <span data-ttu-id="dbe27-149">Hej endast du har toodo är tooadd dessa två `WebView` element någonstans i sidorna, här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="dbe27-149">hello only thing you have toodo is tooadd those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="dbe27-150">I det här exemplet hello `WebView` element är sträckta toofit sina behållare som automatiskt storleken dem vid skärmen rotation fönster eller storlek ändras.</span><span class="sxs-lookup"><span data-stu-id="dbe27-150">In this example hello `WebView` elements are stretched toofit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="dbe27-151">Det är viktigt tookeep hello samma naming `engagement_notification_content` och `engagement_announcement_content` för hello `WebView` element.</span><span class="sxs-lookup"><span data-stu-id="dbe27-151">It is important tookeep hello same naming `engagement_notification_content` and `engagement_announcement_content` for hello `WebView` elements.</span></span> <span data-ttu-id="dbe27-152">Reach identifierar dem med deras namn.</span><span class="sxs-lookup"><span data-stu-id="dbe27-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="dbe27-153">Hantera datapush (valfritt)</span><span class="sxs-lookup"><span data-stu-id="dbe27-153">Handle datapush (optional)</span></span>
<span data-ttu-id="dbe27-154">Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden har tooimplement två händelser i hello EngagementReach klass:</span><span class="sxs-lookup"><span data-stu-id="dbe27-154">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

<span data-ttu-id="dbe27-155">Lägg till i App.xaml.cs i hello App() konstruktorn:</span><span class="sxs-lookup"><span data-stu-id="dbe27-155">In App.xaml.cs in hello App() constructor add:</span></span>

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

<span data-ttu-id="dbe27-156">Du kan se att hello motringning för varje metod returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="dbe27-156">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="dbe27-157">Engagement skickar en feedback tooits serverdel när hello data-push.</span><span class="sxs-lookup"><span data-stu-id="dbe27-157">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="dbe27-158">Om hello återanrop returnerar värdet false hello `exit` feedback kommer att skicka.</span><span class="sxs-lookup"><span data-stu-id="dbe27-158">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="dbe27-159">Annars blir `action`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="dbe27-160">Om ingen motringning är aktiverad för hello händelser hello `drop` feedback returneras tooEngagement.</span><span class="sxs-lookup"><span data-stu-id="dbe27-160">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="dbe27-161">Engagement är inte kan tooreceive multiplar feedback för en data-push.</span><span class="sxs-lookup"><span data-stu-id="dbe27-161">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="dbe27-162">Om du planerar tooset flera hanterare på en händelse, Tänk på att hello feedback motsvarar toohello sista som skickas.</span><span class="sxs-lookup"><span data-stu-id="dbe27-162">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="dbe27-163">I detta fall rekommenderar vi tooalways returnerar hello samma värde tooavoid med förvirrande feedback om hello frontend.</span><span class="sxs-lookup"><span data-stu-id="dbe27-163">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="dbe27-164">Anpassa Användargränssnittet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="dbe27-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="dbe27-165">Första steget</span><span class="sxs-lookup"><span data-stu-id="dbe27-165">First step</span></span>
<span data-ttu-id="dbe27-166">Vi kan toocustomize hello reach Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="dbe27-166">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="dbe27-167">toodo, alltså toocreate en underklass till hello `EngagementReachHandler` klass.</span><span class="sxs-lookup"><span data-stu-id="dbe27-167">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="dbe27-168">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="dbe27-169">Ange hello innehållet i hello `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klass i hello `App()` metod.</span><span class="sxs-lookup"><span data-stu-id="dbe27-169">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `App()` method.</span></span>

<span data-ttu-id="dbe27-170">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="dbe27-171">Som standard använder Engagement implementeringen av `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="dbe27-172">Du har inte toocreate egna, och om du gör det kan du inte toooverride varje metod.</span><span class="sxs-lookup"><span data-stu-id="dbe27-172">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="dbe27-173">hello standardbeteendet är tooselect hello Engagement basobjektet.</span><span class="sxs-lookup"><span data-stu-id="dbe27-173">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="dbe27-174">Webbvy</span><span class="sxs-lookup"><span data-stu-id="dbe27-174">Web View</span></span>
<span data-ttu-id="dbe27-175">Som standard använder Reach hello inbäddade resurser av hello DLL toodisplay hello meddelanden och sidor.</span><span class="sxs-lookup"><span data-stu-id="dbe27-175">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="dbe27-176">tooprovide en fullständig anpassning möjligheter som vi använder bara webbvy.</span><span class="sxs-lookup"><span data-stu-id="dbe27-176">tooprovide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="dbe27-177">Om du vill toocustomize layouter åsidosätta direkt hello resurser filer `EngagementAnnouncement.html` och `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-177">If you want toocustomize layouts, override directly hello resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="dbe27-178">Engagement måste all kod i `<body></body>` toorun korrekt.</span><span class="sxs-lookup"><span data-stu-id="dbe27-178">Engagement needs all code in `<body></body>` toorun correctly.</span></span> <span data-ttu-id="dbe27-179">Men du kan lägga till tagg yttre `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="dbe27-180">Du kan dock välja toouse egna resurser.</span><span class="sxs-lookup"><span data-stu-id="dbe27-180">However, you can decide toouse your own resources.</span></span>

<span data-ttu-id="dbe27-181">Du kan åsidosätta `EngagementReachHandler` metoder i din underklass tootell Engagement toouse din layout, men ta försiktighet tooembedded hello engagement mekanism:</span><span class="sxs-lookup"><span data-stu-id="dbe27-181">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts, but take care tooembedded hello engagement mechanism:</span></span>

<span data-ttu-id="dbe27-182">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="dbe27-182">**Sample Code :**</span></span>

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


<span data-ttu-id="dbe27-183">Som standard är AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="dbe27-184">Den representerar hello html-fil som utforma hello innehållet i ett push-meddelande (Text meddelandet, Web anoucement och avsökning meddelande).</span><span class="sxs-lookup"><span data-stu-id="dbe27-184">It represents hello html file that design hello content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="dbe27-185">AnnouncementName är `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="dbe27-186">Det är hello namnet på hello webbvy design i xaml-sida.</span><span class="sxs-lookup"><span data-stu-id="dbe27-186">It is hello name of hello webview design in your xaml page.</span></span>

<span data-ttu-id="dbe27-187">NotfificationHTML är `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="dbe27-188">Den representerar hello html-fil som utforma hello-meddelande om ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="dbe27-188">It represents hello html file that design hello notification of a push message.</span></span> <span data-ttu-id="dbe27-189">NotfificationName är `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="dbe27-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="dbe27-190">Det är hello namnet på hello webbvy design i xaml-sida.</span><span class="sxs-lookup"><span data-stu-id="dbe27-190">It is hello name of hello webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="dbe27-191">Anpassning</span><span class="sxs-lookup"><span data-stu-id="dbe27-191">Customization</span></span>
<span data-ttu-id="dbe27-192">Du kan anpassa meddelandet och meddelandet webbvy har du vill om du bevara Engagement objekt.</span><span class="sxs-lookup"><span data-stu-id="dbe27-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="dbe27-193">Se till att objektet webbvy är beskrivs tre gånger - hello första gången i din xaml andra tidpunkt i filen .cs i hello ”setwebview()” metod, och tredje tiden i hello html-fil.</span><span class="sxs-lookup"><span data-stu-id="dbe27-193">Take care that webview object is described three times - hello first time in your xaml, second time in your .cs file in hello "setwebview()" method, and third time in hello html file.</span></span>

* <span data-ttu-id="dbe27-194">Beskriv hello aktuella grafisk layout webbvy komponenten i din xaml.</span><span class="sxs-lookup"><span data-stu-id="dbe27-194">In your xaml you describe hello current graphical layout webview component.</span></span>
* <span data-ttu-id="dbe27-195">I filen .cs kan du definiera ”setwebview()” där du anger hello dimension hello två webbvy (meddelande, meddelande).</span><span class="sxs-lookup"><span data-stu-id="dbe27-195">In your .cs file you can define "setwebview()" in which you set hello dimension of hello two webview (notification, announcement).</span></span> <span data-ttu-id="dbe27-196">Det är mycket effektivt när programmet hello storleksändring.</span><span class="sxs-lookup"><span data-stu-id="dbe27-196">It is very effective when hello application is resizing.</span></span>
* <span data-ttu-id="dbe27-197">Vi beskrivs hello webbvy innehåll, design och hello element positioner mellan varandra i hello Engagement HTML-fil.</span><span class="sxs-lookup"><span data-stu-id="dbe27-197">In hello Engagement html file we describe hello webview content, design and hello elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="dbe27-198">Starta meddelande</span><span class="sxs-lookup"><span data-stu-id="dbe27-198">Launch message</span></span>
<span data-ttu-id="dbe27-199">När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar hello program, load hello innehållet i hello push-meddelanden och visar hello-sida för hello motsvarande kampanj.</span><span class="sxs-lookup"><span data-stu-id="dbe27-199">When a user clicks on a system notification (a toast), Engagement launches hello application, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="dbe27-200">Det finns en fördröjning mellan hello lanseringen av hello program och hello visningen av hello sida (beroende på nätverkets hello hastighet).</span><span class="sxs-lookup"><span data-stu-id="dbe27-200">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="dbe27-201">tooindicate toohello användaren som något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="dbe27-201">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="dbe27-202">Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.</span><span class="sxs-lookup"><span data-stu-id="dbe27-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="dbe27-203">tooimplement hello återanrop i App.xaml.cs i ”gemensamma App() {}” Lägg till:</span><span class="sxs-lookup"><span data-stu-id="dbe27-203">tooimplement hello callback, in App.xaml.cs in "Public App(){}" add:</span></span>

            /* hello application has launched and hello content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* hello application has finished loading hello content and hello page
             * is about toobe displayed.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* hello content has been loaded, but an error has occurred.
             * You can provide an information toohello user.
             * You should hide hello indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="dbe27-204">Du kan ange hello återanrop i ”gemensamma App() {}”-metoden i din `App.xaml.cs` fil, helst före hello `EngagementReach.Instance.Init()` anropa.</span><span class="sxs-lookup"><span data-stu-id="dbe27-204">You can set hello callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="dbe27-205">Varje hanterare anropas av hello UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="dbe27-205">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="dbe27-206">Du har inte tooworry när du använder en MessageBox eller något UI-relaterade.</span><span class="sxs-lookup"><span data-stu-id="dbe27-206">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="dbe27-207"><a id="push-channel-sharing"></a>Push-kanal delning</span><span class="sxs-lookup"><span data-stu-id="dbe27-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="dbe27-208">Om du använder push-meddelanden för ett annat ändamål i ditt program har toouse hello push kanal delning funktion i hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="dbe27-208">If you are using push notifications for another purpose in your application then you have toouse hello push channel sharing feature of hello Engagement SDK.</span></span> <span data-ttu-id="dbe27-209">Detta är tooavoid missade push.</span><span class="sxs-lookup"><span data-stu-id="dbe27-209">This is tooavoid missed push.</span></span>

* <span data-ttu-id="dbe27-210">Du kan ange egna push kanal toohello Engagement nå initiering.</span><span class="sxs-lookup"><span data-stu-id="dbe27-210">You can provide your own push channel toohello Engagement Reach initialization.</span></span> <span data-ttu-id="dbe27-211">hello SDK används den i stället för att begära en ny.</span><span class="sxs-lookup"><span data-stu-id="dbe27-211">hello SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="dbe27-212">Uppdatera hello Engagement nå initiering med din push-kanal i hello `InitEngagement` metod från hello `App.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="dbe27-212">Update hello Engagement Reach initialization with your push channel in hello `InitEngagement` method from hello `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="dbe27-213">Du kan också om du bara vill tooconsume hello push-kanal efter hello Reach initiering sedan kan du definiera ett återanrop på Engagement nå tooget hello push kanal när den har skapats av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="dbe27-213">Alternatively, if you just want tooconsume hello push channel after hello Reach initialization then you can set a callback on Engagement Reach tooget hello push channel once it is created by hello SDK.</span></span>

<span data-ttu-id="dbe27-214">Ange din återanrop var som helst **när** hello Reach-initieringen:</span><span class="sxs-lookup"><span data-stu-id="dbe27-214">Set your callback at any place **after** hello Reach initialization :</span></span>

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="dbe27-215">Anpassat schema-tips</span><span class="sxs-lookup"><span data-stu-id="dbe27-215">Custom scheme tip</span></span>
<span data-ttu-id="dbe27-216">Vi ger användning av anpassade schemat.</span><span class="sxs-lookup"><span data-stu-id="dbe27-216">We provide custom scheme use.</span></span> <span data-ttu-id="dbe27-217">Du kan skicka annan typ av URI från engagement klientdel toobe används i din engagement-programmet.</span><span class="sxs-lookup"><span data-stu-id="dbe27-217">You can send different type of URI from engagement frontend toobe used in your engagement application.</span></span> <span data-ttu-id="dbe27-218">Standardschema som `http, ftp, ...` är hantera med Windows, kommer ett fönster fråga om de är inget installerat standardprogram på enheten.</span><span class="sxs-lookup"><span data-stu-id="dbe27-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="dbe27-219">Du kan också skapa egna anpassade schemat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="dbe27-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="dbe27-220">hello enkelt tooset ett anpassat schema i ditt program är tooopen din `Package.appxmanifest` gå i `Declarations` panelen.</span><span class="sxs-lookup"><span data-stu-id="dbe27-220">hello simple way tooset a custom scheme in your application is tooopen your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="dbe27-221">Välj `Protocol` i hello tillgängliga deklarationer rulla rutan och lägga till den.</span><span class="sxs-lookup"><span data-stu-id="dbe27-221">Select `Protocol` in hello Available Declarations scroll box and add it.</span></span> <span data-ttu-id="dbe27-222">Redigera hello `Name` fält med din nya protokollet önskat namn.</span><span class="sxs-lookup"><span data-stu-id="dbe27-222">Edit hello `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="dbe27-223">Nu toouse detta protokoll, redigera din `App.xaml.cs` med hello `OnActivated` -metoden och Glöm inte tooinitialize engagement här också:</span><span class="sxs-lookup"><span data-stu-id="dbe27-223">Now toouse this protocol, edit your `App.xaml.cs` with hello `OnActivated` method, and don't forget tooinitialize engagement here also:</span></span>

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action toodo when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

