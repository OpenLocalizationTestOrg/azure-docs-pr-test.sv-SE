---
title: "Universella Windows-appar nå SDK-Integration"
description: Integrera Azure Mobile Engagement Reach med universella Windows-appar
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
ms.openlocfilehash: 9311e998e67d8d0d56da68fc9460df32ce7ce5a9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a><span data-ttu-id="12bd3-103">Universella Windows-appar nå SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="12bd3-103">Windows Universal Apps Reach SDK Integration</span></span>
<span data-ttu-id="12bd3-104">Du måste följa proceduren integrering i den [Windows Universal SDK-Integration av Engagement](mobile-engagement-windows-store-integrate-engagement.md) innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="12bd3-104">You must follow the integration procedure described in the [Windows Universal Engagement SDK Integration](mobile-engagement-windows-store-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a><span data-ttu-id="12bd3-105">Bädda in Engagement nå SDK i ditt universella Windows-projekt</span><span class="sxs-lookup"><span data-stu-id="12bd3-105">Embed the Engagement Reach SDK into your Windows Universal project</span></span>
<span data-ttu-id="12bd3-106">Du har inte något att lägga till.</span><span class="sxs-lookup"><span data-stu-id="12bd3-106">You do not have anything to add.</span></span> <span data-ttu-id="12bd3-107">`EngagementReach`referenser och resurser finns redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="12bd3-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="12bd3-108">Du kan anpassa avbildningar som finns i den `Resources` mappen i ditt projekt, speciellt märke ikonen (som standard till ikonen Engagement).</span><span class="sxs-lookup"><span data-stu-id="12bd3-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span> <span data-ttu-id="12bd3-109">På universella appar du kan också flytta den `Resources` mapp på din delade projektet att dela innehåll mellan appar, men du måste behålla den `Resources\EngagementConfiguration.xml` fil på dess ursprungliga plats eftersom den är beroende plattform.</span><span class="sxs-lookup"><span data-stu-id="12bd3-109">On Universal Apps you can also move the `Resources` folder on your shared project to share its content between apps, but you will have to keep the `Resources\EngagementConfiguration.xml` file on its default location as it is platform dependent.</span></span>
> 
> 

## <a name="enable-the-windows-notification-service"></a><span data-ttu-id="12bd3-110">Aktivera Windows Notification Service</span><span class="sxs-lookup"><span data-stu-id="12bd3-110">Enable the Windows Notification Service</span></span>
### <a name="windows-8x-and-windows-phone-81-only"></a><span data-ttu-id="12bd3-111">Windows 8.x och Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="12bd3-111">Windows 8.x and Windows Phone 8.1 only</span></span>
<span data-ttu-id="12bd3-112">För att kunna använda den **Windows Notification Service** (kallas WNS) i din `Package.appxmanifest` filen på `Application UI` klickar du på `All Image Assets` i rutan vänstra bot.</span><span class="sxs-lookup"><span data-stu-id="12bd3-112">In order to use the **Windows Notification Service** (referred as WNS) in your `Package.appxmanifest` file on `Application UI` click on `All Image Assets` in the left bot box.</span></span> <span data-ttu-id="12bd3-113">Längst till höger om rutan i `Notifications`, ändra `toast capable` från `(not set)` till `(Yes)`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-113">At the right of the box in `Notifications`, change `toast capable` from `(not set)` to `(Yes)`.</span></span>

### <a name="all-platforms"></a><span data-ttu-id="12bd3-114">Alla plattformar</span><span class="sxs-lookup"><span data-stu-id="12bd3-114">All platforms</span></span>
<span data-ttu-id="12bd3-115">Du behöver synkronisera din app till ditt Microsoft-konto och engagement-plattformen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-115">You need to synchronize your app to your Microsoft account and to the engagement platform.</span></span> <span data-ttu-id="12bd3-116">För detta måste du skapa ett konto eller logga in [windows dev center](https://dev.windows.com).</span><span class="sxs-lookup"><span data-stu-id="12bd3-116">For this you need to create an account or log on [windows dev center](https://dev.windows.com).</span></span> <span data-ttu-id="12bd3-117">Efter det att skapa ett nytt program och hitta SID och hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="12bd3-117">After that create a new application and find the SID and secret key.</span></span> <span data-ttu-id="12bd3-118">På engagement-klientdel går på din appinställningen i `native push` och klistra in dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="12bd3-118">On the engagement frontend, go on your app setting in `native push` and paste your credentials.</span></span> <span data-ttu-id="12bd3-119">Efter det, högerklickar du på projektet, Välj `store` och `Associate App with the Store...`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-119">After that, right click on your project, select `store` and `Associate App with the Store...`.</span></span> <span data-ttu-id="12bd3-120">Du behöver bara markera programmet som du har skapar innan för att synkronisera den.</span><span class="sxs-lookup"><span data-stu-id="12bd3-120">You just need to select the application you have create before to synchronize it.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="12bd3-121">Initiera Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="12bd3-121">Initialize the Engagement Reach SDK</span></span>
<span data-ttu-id="12bd3-122">Ändra den `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="12bd3-122">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="12bd3-123">Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i din `InitEngagement` metoden:</span><span class="sxs-lookup"><span data-stu-id="12bd3-123">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in your `InitEngagement` method:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  <span data-ttu-id="12bd3-124">Den `EngagementReach.Instance.Init` körs i en dedikerad tråd.</span><span class="sxs-lookup"><span data-stu-id="12bd3-124">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="12bd3-125">Du behöver inte göra det själv.</span><span class="sxs-lookup"><span data-stu-id="12bd3-125">You do not have to do it yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="12bd3-126">Om du använder push-meddelanden någon annanstans i ditt program så att du behöver [delar push-kanal](#push-channel-sharing) med Engagement nå.</span><span class="sxs-lookup"><span data-stu-id="12bd3-126">If you are using push notifications elsewhere in your application then you have to [share your push channel](#push-channel-sharing) with Engagement Reach.</span></span>
> 
> 

## <a name="integration"></a><span data-ttu-id="12bd3-127">Integrering</span><span class="sxs-lookup"><span data-stu-id="12bd3-127">Integration</span></span>
<span data-ttu-id="12bd3-128">Engagement tillhandahåller två sätt att lägga till Reach i appen sidhuvud och mellan enskilda muskler vyer för meddelanden och avsökningar i ditt program: överläggsintegration och den manuella integreringen av vyer.</span><span class="sxs-lookup"><span data-stu-id="12bd3-128">Engagement provides two ways to add the Reach in-app banners and interstitial views for announcements and polls in your application: the overlay integration and the web views manual integration.</span></span> <span data-ttu-id="12bd3-129">Du bör inte kombinera båda metod på samma sida.</span><span class="sxs-lookup"><span data-stu-id="12bd3-129">You should not combine both approach on the same page.</span></span>

<span data-ttu-id="12bd3-130">Välja mellan två integrationen kan sammanfattas det här sättet:</span><span class="sxs-lookup"><span data-stu-id="12bd3-130">The choice between the two integration could be summarized this way:</span></span>

* <span data-ttu-id="12bd3-131">Du kan välja överläggsintegration om sidorna ärver redan från agenten `EngagementPage`, är det bara en fråga för att ersätta `EngagementPage` av `EngagementPageOverlay` och `xmlns:engagement="using:Microsoft.Azure.Engagement"` av `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` på sidorna.</span><span class="sxs-lookup"><span data-stu-id="12bd3-131">You may choose the overlay integration if your pages already inherits from the Agent `EngagementPage`, it's just a matter of replacing `EngagementPage` by `EngagementPageOverlay` and `xmlns:engagement="using:Microsoft.Azure.Engagement"` by `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` in your pages.</span></span>
* <span data-ttu-id="12bd3-132">Du kan välja Webbvyer manuell integration om du vill att exakt nå Användargränssnittet i sidorna eller om du inte vill lägga till en annan nivå på sidorna.</span><span class="sxs-lookup"><span data-stu-id="12bd3-132">You may choose the web views manual integration if you want to precisely place the Reach UI within your pages or if you don't want to add another inheritance level to your pages.</span></span> 

### <a name="overlay-integration"></a><span data-ttu-id="12bd3-133">Överläggsintegration</span><span class="sxs-lookup"><span data-stu-id="12bd3-133">Overlay integration</span></span>
<span data-ttu-id="12bd3-134">Engagement överlägget dynamiskt lägger till de UI-element som används för att visa Reach-kampanjer på sidan.</span><span class="sxs-lookup"><span data-stu-id="12bd3-134">The Engagement overlay dynamically adds the UI elements used to display Reach campaigns in your page.</span></span> <span data-ttu-id="12bd3-135">Om överlägget inte passar din layout bör du Webbvyer manuell integrering i stället.</span><span class="sxs-lookup"><span data-stu-id="12bd3-135">If the overlay doesn't suit your layout you should consider the web views manual integration instead.</span></span>

<span data-ttu-id="12bd3-136">I XAML-filen ändringen `EngagementPage` hänvisar till`EngagementPageOverlay`</span><span class="sxs-lookup"><span data-stu-id="12bd3-136">In your .xaml file change `EngagementPage` reference to `EngagementPageOverlay`</span></span>

* <span data-ttu-id="12bd3-137">Lägg till följande i namnområdesdeklarationerna:</span><span class="sxs-lookup"><span data-stu-id="12bd3-137">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* <span data-ttu-id="12bd3-138">Ersätt `engagement:EngagementPage` med `engagement:EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="12bd3-138">Replace `engagement:EngagementPage` with `engagement:EngagementPageOverlay`:</span></span>

<span data-ttu-id="12bd3-139">**Med EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-139">**With EngagementPage:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

<span data-ttu-id="12bd3-140">**Med EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-140">**With EngagementPageOverlay:**</span></span>

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

<span data-ttu-id="12bd3-141">Tagga sedan sidan i i filen .cs `EngagementPageOverlay` i stället för `EngagementPage` och importera `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-141">Then in your .cs file tag your page in `EngagementPageOverlay` instead of `EngagementPage` and import `Microsoft.Azure.Engagement.Overlay`.</span></span>

            using Microsoft.Azure.Engagement.Overlay;

* <span data-ttu-id="12bd3-142">Ersätt `EngagementPage` med `EngagementPageOverlay`:</span><span class="sxs-lookup"><span data-stu-id="12bd3-142">Replace `EngagementPage` with `EngagementPageOverlay`:</span></span>

<span data-ttu-id="12bd3-143">**Med EngagementPage:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-143">**With EngagementPage:**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

<span data-ttu-id="12bd3-144">**Med EngagementPageOverlay:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-144">**With EngagementPageOverlay:**</span></span>

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


<span data-ttu-id="12bd3-145">Engagement överlägget lägger till en `Grid` element på sidan består av layouten och två `WebView` delar en för banderollen och det andra för mellan enskilda muskler vyn.</span><span class="sxs-lookup"><span data-stu-id="12bd3-145">The Engagement overlay adds a `Grid` element on top of your page composed of your layout and the two `WebView` elements one for the banner and the other one for the interstitial view.</span></span>

<span data-ttu-id="12bd3-146">Du kan anpassa överlägget element i direkt i den `EngagementPageOverlay.cs` filen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-146">You can customize the overlay elements directly in the `EngagementPageOverlay.cs` file.</span></span>

### <a name="web-views-manual-integration"></a><span data-ttu-id="12bd3-147">Web vyer manuell integrering</span><span class="sxs-lookup"><span data-stu-id="12bd3-147">Web views manual integration</span></span>
<span data-ttu-id="12bd3-148">Reach söker efter sidorna för två `WebView` element som ansvarar för att visa popup-meddelandet och mellan enskilda muskler vyn.</span><span class="sxs-lookup"><span data-stu-id="12bd3-148">Reach will be searching your pages for the two `WebView` elements responsible for displaying the banner and the interstitial view.</span></span> <span data-ttu-id="12bd3-149">Det enda du behöver göra är att lägga till de två `WebView` element någonstans i sidorna, här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="12bd3-149">The only thing you have to do is to add those two `WebView` elements somewhere in your pages, here is an example:</span></span>

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


<span data-ttu-id="12bd3-150">I det här exemplet i `WebView` element utsträckt så att de passar deras behållare som automatiskt storleken dem vid skärmen rotation fönster eller storlek ändras.</span><span class="sxs-lookup"><span data-stu-id="12bd3-150">In this example the `WebView` elements are stretched to fit their container which automatically re-sizes them in case of screen rotation or window size change.</span></span>

> [!WARNING]
> <span data-ttu-id="12bd3-151">Det är viktigt att behålla samma naming `engagement_notification_content` och `engagement_announcement_content` för den `WebView` element.</span><span class="sxs-lookup"><span data-stu-id="12bd3-151">It is important to keep the same naming `engagement_notification_content` and `engagement_announcement_content` for the `WebView` elements.</span></span> <span data-ttu-id="12bd3-152">Reach identifierar dem med deras namn.</span><span class="sxs-lookup"><span data-stu-id="12bd3-152">Reach is identifying them by their name.</span></span> 
> 
> 

## <a name="handle-datapush-optional"></a><span data-ttu-id="12bd3-153">Hantera datapush (valfritt)</span><span class="sxs-lookup"><span data-stu-id="12bd3-153">Handle datapush (optional)</span></span>
<span data-ttu-id="12bd3-154">Om du vill att programmet ska kunna ta emot Reach data push-meddelanden måste du implementera två händelser i klassen EngagementReach:</span><span class="sxs-lookup"><span data-stu-id="12bd3-154">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

<span data-ttu-id="12bd3-155">Lägg till i App.xaml.cs i konstruktorn App():</span><span class="sxs-lookup"><span data-stu-id="12bd3-155">In App.xaml.cs in the App() constructor add:</span></span>

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

<span data-ttu-id="12bd3-156">Du kan se att återanropet för varje metod returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="12bd3-156">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="12bd3-157">Engagement skickar feedback till dess backend när data-push.</span><span class="sxs-lookup"><span data-stu-id="12bd3-157">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="12bd3-158">Om återanropet returnerar värdet false, den `exit` feedback kommer att skicka.</span><span class="sxs-lookup"><span data-stu-id="12bd3-158">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="12bd3-159">Annars blir `action`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-159">Otherwise, it will be `action`.</span></span> <span data-ttu-id="12bd3-160">Om ingen motringning är inställd för händelser, den `drop` feedback återgår till Engagement.</span><span class="sxs-lookup"><span data-stu-id="12bd3-160">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="12bd3-161">Engagement går inte att ta emot multiplar feedback för en data-push.</span><span class="sxs-lookup"><span data-stu-id="12bd3-161">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="12bd3-162">Om du vill ange flera hanterare på en händelse, Tänk på att din feedback motsvarar den senast skickade en.</span><span class="sxs-lookup"><span data-stu-id="12bd3-162">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="12bd3-163">Vi rekommenderar i det här fallet returnerar alltid samma värde för att undvika att förvirrande feedback på klientdelen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-163">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="12bd3-164">Anpassa Användargränssnittet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="12bd3-164">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="12bd3-165">Första steget</span><span class="sxs-lookup"><span data-stu-id="12bd3-165">First step</span></span>
<span data-ttu-id="12bd3-166">Vi kan du anpassa räckvidden Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="12bd3-166">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="12bd3-167">Om du vill göra det, måste du skapa en underklass till den `EngagementReachHandler` klass.</span><span class="sxs-lookup"><span data-stu-id="12bd3-167">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="12bd3-168">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-168">**Sample Code :**</span></span>

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

<span data-ttu-id="12bd3-169">Ställ sedan innehållet i den `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klassen inom den `App()` metoden.</span><span class="sxs-lookup"><span data-stu-id="12bd3-169">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `App()` method.</span></span>

<span data-ttu-id="12bd3-170">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-170">**Sample Code :**</span></span>

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> <span data-ttu-id="12bd3-171">Som standard använder Engagement implementeringen av `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-171">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span>
> <span data-ttu-id="12bd3-172">Du behöver skapa en egen och om du gör det kan du inte ska åsidosätta alla metoder.</span><span class="sxs-lookup"><span data-stu-id="12bd3-172">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="12bd3-173">Standardinställningen är att välja Engagement basobjektet.</span><span class="sxs-lookup"><span data-stu-id="12bd3-173">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="web-view"></a><span data-ttu-id="12bd3-174">Webbvy</span><span class="sxs-lookup"><span data-stu-id="12bd3-174">Web View</span></span>
<span data-ttu-id="12bd3-175">Som standard använder Reach inbäddade resurser för DLL-filen för att visa meddelanden och sidor.</span><span class="sxs-lookup"><span data-stu-id="12bd3-175">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="12bd3-176">Om du vill ange en fullständig anpassning möjligheter använda vi bara webbvy.</span><span class="sxs-lookup"><span data-stu-id="12bd3-176">To provide a full customization possibilities we only use web view.</span></span> <span data-ttu-id="12bd3-177">Om du vill anpassa layouter åsidosätta direkt resurser filerna `EngagementAnnouncement.html` och `EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-177">If you want to customize layouts, override directly the resources files `EngagementAnnouncement.html` and `EngagementNotification.html`.</span></span> <span data-ttu-id="12bd3-178">Engagement måste all kod i `<body></body>` ska kunna köras korrekt.</span><span class="sxs-lookup"><span data-stu-id="12bd3-178">Engagement needs all code in `<body></body>` to run correctly.</span></span> <span data-ttu-id="12bd3-179">Men du kan lägga till tagg yttre `engagement_webview_area`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-179">But you can add tag outer `engagement_webview_area`.</span></span>

<span data-ttu-id="12bd3-180">Du kan dock välja att använda egna resurser.</span><span class="sxs-lookup"><span data-stu-id="12bd3-180">However, you can decide to use your own resources.</span></span>

<span data-ttu-id="12bd3-181">Du kan åsidosätta `EngagementReachHandler` metoder i din underklass ska berätta för Engagement att använda en layout, men var noga med att inbäddade mekanismen för engagement:</span><span class="sxs-lookup"><span data-stu-id="12bd3-181">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts, but take care to embedded the engagement mechanism:</span></span>

<span data-ttu-id="12bd3-182">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="12bd3-182">**Sample Code :**</span></span>

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


<span data-ttu-id="12bd3-183">Som standard är AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-183">By default, AnnouncementHTML is `ms-appx-web:///Resources/EngagementAnnouncement.html`.</span></span> <span data-ttu-id="12bd3-184">Den representerar html-fil som utforma innehållet i ett push-meddelande (Text meddelandet, Web anoucement och avsökning meddelande).</span><span class="sxs-lookup"><span data-stu-id="12bd3-184">It represents the html file that design the content of a push message (Text announcement, Web anoucement and Poll announcement).</span></span> <span data-ttu-id="12bd3-185">AnnouncementName är `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-185">AnnouncementName is `engagement_announcement_content`.</span></span> <span data-ttu-id="12bd3-186">Det är namnet på webbvy designen i xaml-sida.</span><span class="sxs-lookup"><span data-stu-id="12bd3-186">It is the name of the webview design in your xaml page.</span></span>

<span data-ttu-id="12bd3-187">NotfificationHTML är `ms-appx-web:///Resources/EngagementNotification.html`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-187">NotfificationHTML is `ms-appx-web:///Resources/EngagementNotification.html`.</span></span> <span data-ttu-id="12bd3-188">Den representerar html-fil som utforma meddelandet för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="12bd3-188">It represents the html file that design the notification of a push message.</span></span> <span data-ttu-id="12bd3-189">NotfificationName är `engagement_notification_content`.</span><span class="sxs-lookup"><span data-stu-id="12bd3-189">NotfificationName is `engagement_notification_content`.</span></span> <span data-ttu-id="12bd3-190">Det är namnet på webbvy designen i xaml-sida.</span><span class="sxs-lookup"><span data-stu-id="12bd3-190">It is the name of the webview design in your xaml page.</span></span>

### <a name="customization"></a><span data-ttu-id="12bd3-191">Anpassning</span><span class="sxs-lookup"><span data-stu-id="12bd3-191">Customization</span></span>
<span data-ttu-id="12bd3-192">Du kan anpassa meddelandet och meddelandet webbvy har du vill om du bevara Engagement objekt.</span><span class="sxs-lookup"><span data-stu-id="12bd3-192">You can customize notification and announcement web view has you want if you preserve Engagement object.</span></span> <span data-ttu-id="12bd3-193">Var försiktig så att webbvy objekt beskrivs tre gånger - första gången i din xaml, andra gången i filen .cs i metoden ”setwebview()” och tredje tid i html-fil.</span><span class="sxs-lookup"><span data-stu-id="12bd3-193">Take care that webview object is described three times - the first time in your xaml, second time in your .cs file in the "setwebview()" method, and third time in the html file.</span></span>

* <span data-ttu-id="12bd3-194">Beskriv den aktuella grafiska layout webbvy komponenten i din xaml.</span><span class="sxs-lookup"><span data-stu-id="12bd3-194">In your xaml you describe the current graphical layout webview component.</span></span>
* <span data-ttu-id="12bd3-195">I filen .cs kan du definiera ”setwebview()” där du anger dimension två webbvy (meddelande, meddelande).</span><span class="sxs-lookup"><span data-stu-id="12bd3-195">In your .cs file you can define "setwebview()" in which you set the dimension of the two webview (notification, announcement).</span></span> <span data-ttu-id="12bd3-196">Det är mycket effektivt när storleksändring av programmet.</span><span class="sxs-lookup"><span data-stu-id="12bd3-196">It is very effective when the application is resizing.</span></span>
* <span data-ttu-id="12bd3-197">Html-fil Engagement beskrivs webbvyinnehåll, design och element positioner mellan varandra.</span><span class="sxs-lookup"><span data-stu-id="12bd3-197">In the Engagement html file we describe the webview content, design and the elements positions between each other.</span></span>

### <a name="launch-message"></a><span data-ttu-id="12bd3-198">Starta meddelande</span><span class="sxs-lookup"><span data-stu-id="12bd3-198">Launch message</span></span>
<span data-ttu-id="12bd3-199">När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar programmet, läsa in innehållet i push-meddelanden och visa sidan för motsvarande kampanjen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-199">When a user clicks on a system notification (a toast), Engagement launches the application, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="12bd3-200">Det finns en fördröjning mellan starten av programmet och visas på sidan (beroende på nätverkets hastighet).</span><span class="sxs-lookup"><span data-stu-id="12bd3-200">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="12bd3-201">Om du vill visa användaren att något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="12bd3-201">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="12bd3-202">Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.</span><span class="sxs-lookup"><span data-stu-id="12bd3-202">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="12bd3-203">Om du vill implementera återanropet App.xaml.cs i ”gemensamma App() {}” Lägg till:</span><span class="sxs-lookup"><span data-stu-id="12bd3-203">To implement the callback, in App.xaml.cs in "Public App(){}" add:</span></span>

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="12bd3-204">Du kan ange återanropet i ”gemensamma App() {}”-metoden i din `App.xaml.cs` fil, helst innan det `EngagementReach.Instance.Init()` anropa.</span><span class="sxs-lookup"><span data-stu-id="12bd3-204">You can set the callback in your "Public App(){}" method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="12bd3-205">Varje hanterare anropas av UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="12bd3-205">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="12bd3-206">Du behöver inte oroa dig när du använder en MessageBox eller något UI-relaterade.</span><span class="sxs-lookup"><span data-stu-id="12bd3-206">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

## <span data-ttu-id="12bd3-207"><a id="push-channel-sharing"></a>Push-kanal delning</span><span class="sxs-lookup"><span data-stu-id="12bd3-207"><a id="push-channel-sharing"></a> Push channel sharing</span></span>
<span data-ttu-id="12bd3-208">Om du använder push-meddelanden för ett annat ändamål i ditt program har att använda push-kanal funktionen av Engagement SDK för delning.</span><span class="sxs-lookup"><span data-stu-id="12bd3-208">If you are using push notifications for another purpose in your application then you have to use the push channel sharing feature of the Engagement SDK.</span></span> <span data-ttu-id="12bd3-209">Detta är att undvika missade push.</span><span class="sxs-lookup"><span data-stu-id="12bd3-209">This is to avoid missed push.</span></span>

* <span data-ttu-id="12bd3-210">Du kan ange egna push-kanal till Engagement nå initieringen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-210">You can provide your own push channel to the Engagement Reach initialization.</span></span> <span data-ttu-id="12bd3-211">SDK: N används den i stället för att begära en ny.</span><span class="sxs-lookup"><span data-stu-id="12bd3-211">The SDK will use it instead of requesting a new one.</span></span>

<span data-ttu-id="12bd3-212">Uppdatera initiering av Engagement nå med din push-kanal i den `InitEngagement` metod från den `App.xaml.cs` filen:</span><span class="sxs-lookup"><span data-stu-id="12bd3-212">Update the Engagement Reach initialization with your push channel in the `InitEngagement` method from the `App.xaml.cs` file:</span></span>

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* <span data-ttu-id="12bd3-213">Om du vill använda push-kanalen när Reach-initieringen och du kan ange ett återanrop för Engagement kommer att hämta push-kanalen en gång skapats som också av SDK.</span><span class="sxs-lookup"><span data-stu-id="12bd3-213">Alternatively, if you just want to consume the push channel after the Reach initialization then you can set a callback on Engagement Reach to get the push channel once it is created by the SDK.</span></span>

<span data-ttu-id="12bd3-214">Ange din återanrop var som helst **när** Reach-initieringen:</span><span class="sxs-lookup"><span data-stu-id="12bd3-214">Set your callback at any place **after** the Reach initialization :</span></span>

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a><span data-ttu-id="12bd3-215">Anpassat schema-tips</span><span class="sxs-lookup"><span data-stu-id="12bd3-215">Custom scheme tip</span></span>
<span data-ttu-id="12bd3-216">Vi ger användning av anpassade schemat.</span><span class="sxs-lookup"><span data-stu-id="12bd3-216">We provide custom scheme use.</span></span> <span data-ttu-id="12bd3-217">Du kan skicka annan typ av URI från klientdel av engagement som ska användas i din engagement-programmet.</span><span class="sxs-lookup"><span data-stu-id="12bd3-217">You can send different type of URI from engagement frontend to be used in your engagement application.</span></span> <span data-ttu-id="12bd3-218">Standardschema som `http, ftp, ...` är hantera med Windows, kommer ett fönster fråga om de är inget installerat standardprogram på enheten.</span><span class="sxs-lookup"><span data-stu-id="12bd3-218">Default scheme like `http, ftp, ...` are manage by Windows, a window will prompt if they are no default application installed on device.</span></span> <span data-ttu-id="12bd3-219">Du kan också skapa egna anpassade schemat för ditt program.</span><span class="sxs-lookup"><span data-stu-id="12bd3-219">You can also create your own custom scheme for your application.</span></span>

<span data-ttu-id="12bd3-220">Enkelt sätt att ange ett eget schema i ditt program är att öppna din `Package.appxmanifest` gå i `Declarations` panelen.</span><span class="sxs-lookup"><span data-stu-id="12bd3-220">The simple way to set a custom scheme in your application is to open your `Package.appxmanifest` go in `Declarations` panel.</span></span> <span data-ttu-id="12bd3-221">Välj `Protocol` Bläddra rutan i tillgängliga deklarationer och lägga till den.</span><span class="sxs-lookup"><span data-stu-id="12bd3-221">Select `Protocol` in the Available Declarations scroll box and add it.</span></span> <span data-ttu-id="12bd3-222">Redigera den `Name` fält med din nya protokollet önskat namn.</span><span class="sxs-lookup"><span data-stu-id="12bd3-222">Edit the `Name` field with your new protocol desired name.</span></span>

<span data-ttu-id="12bd3-223">Nu om du vill använda det här protokollet redigera din `App.xaml.cs` med den `OnActivated` metoden och Glöm inte att initiera engagement här också:</span><span class="sxs-lookup"><span data-stu-id="12bd3-223">Now to use this protocol, edit your `App.xaml.cs` with the `OnActivated` method, and don't forget to initialize engagement here also:</span></span>

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

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

