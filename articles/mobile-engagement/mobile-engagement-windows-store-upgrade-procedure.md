---
title: "Uppgradera procedurer för universella Windows-appar SDK"
description: "Universella Windows-appar SDK uppgradera procedurer för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: fe85a99a92fb39082cafe7422b356de1f20f14bd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="e52a2-103">Uppgradera procedurer för universella Windows-appar SDK</span><span class="sxs-lookup"><span data-stu-id="e52a2-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="e52a2-104">Om du redan har integrerat en äldre version av Engagement till programmet, måste du Tänk på följande när du uppgraderar SDK.</span><span class="sxs-lookup"><span data-stu-id="e52a2-104">If you already have integrated an older version of Engagement into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="e52a2-105">Du kan behöva utföra flera procedurer om du har missat flera versioner av SDK.</span><span class="sxs-lookup"><span data-stu-id="e52a2-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="e52a2-106">Till exempel om du migrerar från 0.10.1 till 0.11.0 måste du först Följ proceduren ”från 0.9.0 till 0.10.1” sedan proceduren ”från 0.10.1 till 0.11.0”.</span><span class="sxs-lookup"><span data-stu-id="e52a2-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-330-to-340"></a><span data-ttu-id="e52a2-107">Från 3.3.0 till 3.4.0</span><span class="sxs-lookup"><span data-stu-id="e52a2-107">From 3.3.0 to 3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="e52a2-108">Testa loggar</span><span class="sxs-lookup"><span data-stu-id="e52a2-108">Test logs</span></span>
<span data-ttu-id="e52a2-109">Konsolen loggar som genereras av SDK kan nu vara aktiverat/inaktiverat/filtreras.</span><span class="sxs-lookup"><span data-stu-id="e52a2-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="e52a2-110">Anpassa detta genom att uppdatera egenskapen `EngagementAgent.Instance.TestLogEnabled` till ett värde som är tillgängliga från den `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="e52a2-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="e52a2-111">Resurser</span><span class="sxs-lookup"><span data-stu-id="e52a2-111">Resources</span></span>
<span data-ttu-id="e52a2-112">Reach-överlägget har förbättrats.</span><span class="sxs-lookup"><span data-stu-id="e52a2-112">The Reach overlay has been improved.</span></span> <span data-ttu-id="e52a2-113">Det är en del av resurserna som SDK NuGet-paketet.</span><span class="sxs-lookup"><span data-stu-id="e52a2-113">It is part of the SDK NuGet package resources.</span></span>

<span data-ttu-id="e52a2-114">När du uppgraderar till den nya versionen av SDK kan du välja om du vill behålla dina befintliga filer från mappen överlägget för dina resurser eller inte:</span><span class="sxs-lookup"><span data-stu-id="e52a2-114">While upgrading to the new version of the SDK you can choose whether you want to keep your existing files from the overlay folder of your resources or not:</span></span>

* <span data-ttu-id="e52a2-115">Om det tidigare överlägget fungerar för dig eller om du integrerar de `WebView` element manuellt sedan kan du välja att behålla din befintliga filer, den fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="e52a2-115">If the previous overlay is working for you or you are integrating the `WebView` elements manually then you can decide to keep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="e52a2-116">Om du vill uppdatera till det nya överlägget bara ersätta hela `overlay` mapp från dina resurser med den nya från SDK-paketet (UWP-appar: efter uppgraderingen måste du hämta mappen överlägget från % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="e52a2-116">If you want to update to the new overlay, just replace the whole `overlay` folder from your resources with the new one from the SDK package (UWP apps: after the upgrade, you can get the new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="e52a2-117">Med det nya överlägget skrivs alla anpassningar som gjorts på den tidigare versionen.</span><span class="sxs-lookup"><span data-stu-id="e52a2-117">Using the new overlay will overwrite any customizations made on the previous version.</span></span>
> 
> 

## <a name="from-320-to-330"></a><span data-ttu-id="e52a2-118">Från 3.2.0 till 3.3.0</span><span class="sxs-lookup"><span data-stu-id="e52a2-118">From 3.2.0 to 3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="e52a2-119">Resurser</span><span class="sxs-lookup"><span data-stu-id="e52a2-119">Resources</span></span>
<span data-ttu-id="e52a2-120">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-120">This step concerns customized resources only.</span></span> <span data-ttu-id="e52a2-121">Om du har anpassat de resurser som tillhandahålls av SDK (html, bilder, överlägget) ha säkerhetskopiera dem innan du uppgraderar och tillämpa anpassningen på uppgraderade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-121">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-to-320"></a><span data-ttu-id="e52a2-122">Från 3.1.0 till 3.2.0</span><span class="sxs-lookup"><span data-stu-id="e52a2-122">From 3.1.0 to 3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="e52a2-123">Resurser</span><span class="sxs-lookup"><span data-stu-id="e52a2-123">Resources</span></span>
<span data-ttu-id="e52a2-124">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-124">This step concerns customized resources only.</span></span> <span data-ttu-id="e52a2-125">Om du har anpassat de resurser som tillhandahålls av SDK (html, bilder, överlägget) ha säkerhetskopiera dem innan du uppgraderar och tillämpa anpassningen på uppgraderade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-125">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="e52a2-126">Webbvy integrering</span><span class="sxs-lookup"><span data-stu-id="e52a2-126">Webview integration</span></span>
<span data-ttu-id="e52a2-127">Vissa förbättringar så att den matchar en annan enhet formfaktorer introducerades i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="e52a2-127">Some improvements to match different device form factors were introduced in this version.</span></span> <span data-ttu-id="e52a2-128">Se till att din integrering av webbvyn matchar följande:</span><span class="sxs-lookup"><span data-stu-id="e52a2-128">Make sure that your integration of the webview match the following:</span></span>

<span data-ttu-id="e52a2-129">I din XAML sida ():</span><span class="sxs-lookup"><span data-stu-id="e52a2-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="e52a2-130">Och tillhörande .cs-fil:</span><span class="sxs-lookup"><span data-stu-id="e52a2-130">And in your associated .cs file:</span></span>

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated to within a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

              #region to implement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update the webview when the app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update the webview when the app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure to detach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are the current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements to the correct size.
              /// </summary>
              /// <param name="width">The width of your current display.</param>
              /// <param name="height">The height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes the Windows.Current.SizeChanged and indicates that webviews have to be resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes the ApplicationView.VisibleBoundsChanged and indicates that webviews have to be resized
              /// </summary>
              /// <param name="sender">The related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements to the correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-to-300"></a><span data-ttu-id="e52a2-131">Från 2.0.0 till 3.0.0</span><span class="sxs-lookup"><span data-stu-id="e52a2-131">From 2.0.0 to 3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="e52a2-132">Resurser</span><span class="sxs-lookup"><span data-stu-id="e52a2-132">Resources</span></span>
<span data-ttu-id="e52a2-133">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-133">This step concerns customized resources only.</span></span> <span data-ttu-id="e52a2-134">Om du har anpassat de resurser som tillhandahålls av SDK (html, bilder, överlägget) ha säkerhetskopiera dem innan du uppgraderar och tillämpa anpassningen på uppgraderade resurser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-134">If you have customized the resources provided by the SDK (html, images, overlay) then you have to backup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-to-200"></a><span data-ttu-id="e52a2-135">Från 1.1.1 till 2.0.0</span><span class="sxs-lookup"><span data-stu-id="e52a2-135">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="e52a2-136">Nedan beskrivs hur du migrerar en SDK-integration från tjänsten Capptain som erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e52a2-136">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e52a2-137">Capptain och Mobile Engagement är inte samma tjänster och proceduren nedan visar hur du migrerar klientappen endast.</span><span class="sxs-lookup"><span data-stu-id="e52a2-137">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="e52a2-138">Migrera SDK i appen kommer inte migrera dina data från servrar som Capptain till Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="e52a2-138">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="e52a2-139">Kontakta Capptain webbplats om du vill migrera till 1.1.1 först och sedan använda följande procedur om du migrerar från en tidigare version</span><span class="sxs-lookup"><span data-stu-id="e52a2-139">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="e52a2-140">Nuget-paketet</span><span class="sxs-lookup"><span data-stu-id="e52a2-140">Nuget package</span></span>
<span data-ttu-id="e52a2-141">Ersätt **Capptain.WindowsPhone** av **MicrosoftAzure.MobileEngagement** Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="e52a2-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="e52a2-142">Tillämpa Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e52a2-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="e52a2-143">SDK använder termen `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="e52a2-143">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="e52a2-144">Du behöver uppdatera ditt projekt för att matcha den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="e52a2-144">You need to update your project to match this change.</span></span>

<span data-ttu-id="e52a2-145">Du måste avinstallera det aktuella Capptain nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="e52a2-145">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="e52a2-146">Överväg att alla dina ändringar i mappen Capptain resurser tas bort.</span><span class="sxs-lookup"><span data-stu-id="e52a2-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="e52a2-147">Om du vill behålla dessa filer och sedan göra en kopia av dem.</span><span class="sxs-lookup"><span data-stu-id="e52a2-147">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="e52a2-148">Efter det att installera nya Microsoft Azure Engagement nuget-paketet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="e52a2-148">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="e52a2-149">Du hittar den direkt på [nuget webbplats].</span><span class="sxs-lookup"><span data-stu-id="e52a2-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="e52a2-150">eller här index.</span><span class="sxs-lookup"><span data-stu-id="e52a2-150">or here index.</span></span> <span data-ttu-id="e52a2-151">Den här åtgärden ersätter alla filer för resurser som används av Engagement och lägger till nya Engagement DLL projektreferenserna.</span><span class="sxs-lookup"><span data-stu-id="e52a2-151">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="e52a2-152">Du måste rensa projektreferenserna genom att ta bort Capptain DLL-referenser.</span><span class="sxs-lookup"><span data-stu-id="e52a2-152">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="e52a2-153">Om du inte göra detta, versionen av Capptain kommer i konflikt och fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="e52a2-153">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="e52a2-154">Om du har anpassat Capptain resurser, kopiera ditt gamla filer innehåll och klistra in dem i de nya Engagement-filerna.</span><span class="sxs-lookup"><span data-stu-id="e52a2-154">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="e52a2-155">Observera att xaml- och cs-filer som behöver uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e52a2-155">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="e52a2-156">När de steg behöver du bara ersätta gamla Capptain referenser från de nya Engagement-referenserna.</span><span class="sxs-lookup"><span data-stu-id="e52a2-156">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="e52a2-157">Alla Capptain namnområden måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e52a2-157">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="e52a2-158">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e52a2-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="e52a2-159">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e52a2-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="e52a2-160">Alla Capptain klasser som innehåller ”Capptain” ska innehålla ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="e52a2-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="e52a2-161">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e52a2-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="e52a2-162">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e52a2-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="e52a2-163">För xaml-filer Capptain namnområde och attribut även ändras.</span><span class="sxs-lookup"><span data-stu-id="e52a2-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="e52a2-164">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e52a2-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="e52a2-165">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e52a2-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="e52a2-166">Överlägget sidan ändringar</span><span class="sxs-lookup"><span data-stu-id="e52a2-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="e52a2-167">Överlägget ändras.</span><span class="sxs-lookup"><span data-stu-id="e52a2-167">Overlay also changes.</span></span> <span data-ttu-id="e52a2-168">Det nya namnområdet är `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="e52a2-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="e52a2-169">Den måste användas i xaml- och cs-filer.</span><span class="sxs-lookup"><span data-stu-id="e52a2-169">It has to be used in both xaml and cs files.</span></span> <span data-ttu-id="e52a2-170">Dessutom `CapptainGrid` är att namnges `EngagementGrid`, `capptain_notification_content` och `capptain_announcement_content` namnges `engagement_notification_content` och `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="e52a2-170">Moreover `CapptainGrid` is to be named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="e52a2-171">För överlägget:</span><span class="sxs-lookup"><span data-stu-id="e52a2-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="e52a2-172">Blir den:</span><span class="sxs-lookup"><span data-stu-id="e52a2-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="e52a2-173">Andra resurser som Capptain bilder och HTML-filer, Observera att de även har döpts om du vill använda ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="e52a2-173">For the other resources like Capptain pictures and HTML files, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="e52a2-174">Deklarationen för projektet</span><span class="sxs-lookup"><span data-stu-id="e52a2-174">Project declaration</span></span>
<span data-ttu-id="e52a2-175">På Package.appxmanifest `File Type Associations` har uppdaterats från:</span><span class="sxs-lookup"><span data-stu-id="e52a2-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="e52a2-176">capptain\_nå\_innehåll engagement\_nå\_innehåll</span><span class="sxs-lookup"><span data-stu-id="e52a2-176">capptain\_reach\_content to engagement\_reach\_content</span></span>
* <span data-ttu-id="e52a2-177">capptain\_loggen\_filen engagement\_loggen\_fil</span><span class="sxs-lookup"><span data-stu-id="e52a2-177">capptain\_log\_file to engagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="e52a2-178">Program-ID / SDK-nyckeln</span><span class="sxs-lookup"><span data-stu-id="e52a2-178">Application ID / SDK Key</span></span>
<span data-ttu-id="e52a2-179">Engagement använder en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e52a2-179">Engagement uses a connection string.</span></span> <span data-ttu-id="e52a2-180">Du inte behöver ange ett program-ID och SDK-nyckeln med Mobile Engagement, behöver du bara ange en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e52a2-180">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="e52a2-181">Du kan konfigurera det på EngagementConfiguration-fil.</span><span class="sxs-lookup"><span data-stu-id="e52a2-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="e52a2-182">Konfigurationen av Engagement kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="e52a2-182">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="e52a2-183">Redigera den här filen för att ange:</span><span class="sxs-lookup"><span data-stu-id="e52a2-183">Edit this file to specify:</span></span>

* <span data-ttu-id="e52a2-184">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="e52a2-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="e52a2-185">Om du vill ange vid körning i stället kan du anropa metoden följande innan Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="e52a2-185">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="e52a2-186">Anslutningssträngen för ditt program visas på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e52a2-186">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="e52a2-187">Ändring av objekt</span><span class="sxs-lookup"><span data-stu-id="e52a2-187">Items name change</span></span>
<span data-ttu-id="e52a2-188">Alla objekt med namnet *capptain* namngetts *engagement*.</span><span class="sxs-lookup"><span data-stu-id="e52a2-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="e52a2-189">På liknande sätt för *Capptain* till *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="e52a2-189">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="e52a2-190">Exempel på vanliga Capptain objekt:</span><span class="sxs-lookup"><span data-stu-id="e52a2-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="e52a2-191">CapptainConfiguration nu namnet EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="e52a2-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="e52a2-192">CapptainAgent nu namnet EngagementAgent</span><span class="sxs-lookup"><span data-stu-id="e52a2-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="e52a2-193">CapptainReach nu namnet EngagementReach</span><span class="sxs-lookup"><span data-stu-id="e52a2-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="e52a2-194">CapptainHttpConfig nu namnet EngagementHttpConfig</span><span class="sxs-lookup"><span data-stu-id="e52a2-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="e52a2-195">GetCapptainPageName nu namnet GetEngagementPageName</span><span class="sxs-lookup"><span data-stu-id="e52a2-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="e52a2-196">Observera att byta namn på också påverkar åsidosätts metoder.</span><span class="sxs-lookup"><span data-stu-id="e52a2-196">Note that rename also affects overridden methods.</span></span>

