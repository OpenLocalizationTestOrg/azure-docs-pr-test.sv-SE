---
title: aaaWindows universella appar SDK uppgradera procedurer
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
ms.openlocfilehash: 95aba5d55cd65d4190aad35737f872414b5ed443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a><span data-ttu-id="492c5-103">Uppgradera procedurer för universella Windows-appar SDK</span><span class="sxs-lookup"><span data-stu-id="492c5-103">Windows Universal Apps SDK Upgrade Procedures</span></span>
<span data-ttu-id="492c5-104">Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="492c5-104">If you already have integrated an older version of Engagement into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="492c5-105">Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="492c5-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="492c5-106">Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.</span><span class="sxs-lookup"><span data-stu-id="492c5-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-330-too340"></a><span data-ttu-id="492c5-107">Från 3.3.0 too3.4.0</span><span class="sxs-lookup"><span data-stu-id="492c5-107">From 3.3.0 too3.4.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="492c5-108">Testa loggar</span><span class="sxs-lookup"><span data-stu-id="492c5-108">Test logs</span></span>
<span data-ttu-id="492c5-109">Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras.</span><span class="sxs-lookup"><span data-stu-id="492c5-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="492c5-110">toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="492c5-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a><span data-ttu-id="492c5-111">Resurser</span><span class="sxs-lookup"><span data-stu-id="492c5-111">Resources</span></span>
<span data-ttu-id="492c5-112">hello Reach överlägget har förbättrats.</span><span class="sxs-lookup"><span data-stu-id="492c5-112">hello Reach overlay has been improved.</span></span> <span data-ttu-id="492c5-113">Det är en del av hello SDK NuGet-paketet resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-113">It is part of hello SDK NuGet package resources.</span></span>

<span data-ttu-id="492c5-114">Du kan välja om du vill tookeep befintliga filer från hello överlägg mappen för dina resurser eller inte under uppgraderingen toohello ny version av hello SDK:</span><span class="sxs-lookup"><span data-stu-id="492c5-114">While upgrading toohello new version of hello SDK you can choose whether you want tookeep your existing files from hello overlay folder of your resources or not:</span></span>

* <span data-ttu-id="492c5-115">Om hello tidigare överlägget fungerar för dig eller du integrerar hello `WebView` element manuellt och sedan kan du bestämma tookeep dina befintliga filer, den fortfarande fungerar.</span><span class="sxs-lookup"><span data-stu-id="492c5-115">If hello previous overlay is working for you or you are integrating hello `WebView` elements manually then you can decide tookeep your exiting files, it will still work.</span></span> 
* <span data-ttu-id="492c5-116">Om du vill tooupdate toohello nya överlägget, bara Ersätt hello hela `overlay` mapp från dina resurser med hello ny från hello SDK-paketet (UWP-appar: efter hello uppgraderingen kan du få hello ny överlägget mapp från % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span><span class="sxs-lookup"><span data-stu-id="492c5-116">If you want tooupdate toohello new overlay, just replace hello whole `overlay` folder from your resources with hello new one from hello SDK package (UWP apps: after hello upgrade, you can get hello new overlay folder from %USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).</span></span>

> [!WARNING]
> <span data-ttu-id="492c5-117">Med hjälp av hello nya överlägget skrivs eventuella anpassningar på hello föregående version.</span><span class="sxs-lookup"><span data-stu-id="492c5-117">Using hello new overlay will overwrite any customizations made on hello previous version.</span></span>
> 
> 

## <a name="from-320-too330"></a><span data-ttu-id="492c5-118">Från 3.2.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="492c5-118">From 3.2.0 too3.3.0</span></span>
### <a name="resources"></a><span data-ttu-id="492c5-119">Resurser</span><span class="sxs-lookup"><span data-stu-id="492c5-119">Resources</span></span>
<span data-ttu-id="492c5-120">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-120">This step concerns customized resources only.</span></span> <span data-ttu-id="492c5-121">Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-121">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-310-too320"></a><span data-ttu-id="492c5-122">Från 3.1.0 too3.2.0</span><span class="sxs-lookup"><span data-stu-id="492c5-122">From 3.1.0 too3.2.0</span></span>
### <a name="resources"></a><span data-ttu-id="492c5-123">Resurser</span><span class="sxs-lookup"><span data-stu-id="492c5-123">Resources</span></span>
<span data-ttu-id="492c5-124">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-124">This step concerns customized resources only.</span></span> <span data-ttu-id="492c5-125">Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-125">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

### <a name="webview-integration"></a><span data-ttu-id="492c5-126">Webbvy integrering</span><span class="sxs-lookup"><span data-stu-id="492c5-126">Webview integration</span></span>
<span data-ttu-id="492c5-127">Vissa förbättringar toomatch annan enhet formfaktorer introducerades i den här versionen.</span><span class="sxs-lookup"><span data-stu-id="492c5-127">Some improvements toomatch different device form factors were introduced in this version.</span></span> <span data-ttu-id="492c5-128">Se till att din integrering av hello webbvy matchar hello följande:</span><span class="sxs-lookup"><span data-stu-id="492c5-128">Make sure that your integration of hello webview match hello following:</span></span>

<span data-ttu-id="492c5-129">I din XAML sida ():</span><span class="sxs-lookup"><span data-stu-id="492c5-129">In your XAML page ():</span></span>

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

<span data-ttu-id="492c5-130">Och tillhörande .cs-fil:</span><span class="sxs-lookup"><span data-stu-id="492c5-130">And in your associated .cs file:</span></span>

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated toowithin a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

              #region tooimplement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update hello webview when hello app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update hello webview when hello app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure toodetach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are hello current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements toohello correct size.
              /// </summary>
              /// <param name="width">hello width of your current display.</param>
              /// <param name="height">hello height of your current display.</param>
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
              /// Handler that takes hello Windows.Current.SizeChanged and indicates that webviews have toobe resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes hello ApplicationView.VisibleBoundsChanged and indicates that webviews have toobe resized
              /// </summary>
              /// <param name="sender">hello related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-too300"></a><span data-ttu-id="492c5-131">Från 2.0.0 too3.0.0</span><span class="sxs-lookup"><span data-stu-id="492c5-131">From 2.0.0 too3.0.0</span></span>
### <a name="resources"></a><span data-ttu-id="492c5-132">Resurser</span><span class="sxs-lookup"><span data-stu-id="492c5-132">Resources</span></span>
<span data-ttu-id="492c5-133">Det här steget gäller anpassade resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-133">This step concerns customized resources only.</span></span> <span data-ttu-id="492c5-134">Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.</span><span class="sxs-lookup"><span data-stu-id="492c5-134">If you have customized hello resources provided by hello SDK (html, images, overlay) then you have toobackup them before upgrading and reapply your customization on upgraded resources.</span></span>

## <a name="from-111-too200"></a><span data-ttu-id="492c5-135">Från 1.1.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="492c5-135">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="492c5-136">hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="492c5-136">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="492c5-137">Capptain och Mobile Engagement hello inte samma tjänster och hello proceduren endast anges nedan visar hur toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="492c5-137">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="492c5-138">Migrera hello SDK i hello app kommer inte att migrera data från hello Capptain servrar toohello Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="492c5-138">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="492c5-139">Om du migrerar från en tidigare version kan du kontakta hello Capptain webbplats toomigrate too1.1.1 först och sedan använda hello sätt</span><span class="sxs-lookup"><span data-stu-id="492c5-139">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="492c5-140">Nuget-paketet</span><span class="sxs-lookup"><span data-stu-id="492c5-140">Nuget package</span></span>
<span data-ttu-id="492c5-141">Ersätt **Capptain.WindowsPhone** av **MicrosoftAzure.MobileEngagement** Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="492c5-141">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="492c5-142">Tillämpa Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="492c5-142">Applying Mobile Engagement</span></span>
<span data-ttu-id="492c5-143">hello SDK använder hello termen `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="492c5-143">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="492c5-144">Du behöver tooupdate projekt-toomatch ändringen.</span><span class="sxs-lookup"><span data-stu-id="492c5-144">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="492c5-145">Du måste toouninstall aktuella Capptain nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="492c5-145">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="492c5-146">Överväg att alla dina ändringar i mappen Capptain resurser tas bort.</span><span class="sxs-lookup"><span data-stu-id="492c5-146">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="492c5-147">Om du vill tookeep göra en kopia av dem sedan i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="492c5-147">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="492c5-148">Efter det att installera hello nya Microsoft Azure Engagement nuget-paketet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="492c5-148">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="492c5-149">Du hittar den direkt på [nuget webbplats].</span><span class="sxs-lookup"><span data-stu-id="492c5-149">You can find it directly on [nuget website].</span></span> <span data-ttu-id="492c5-150">eller här index.</span><span class="sxs-lookup"><span data-stu-id="492c5-150">or here index.</span></span> <span data-ttu-id="492c5-151">Den här åtgärden ersätter alla resurser filer som används av Engagement och lägger till hello nya Engagement DLL tooyour projektet refererar till.</span><span class="sxs-lookup"><span data-stu-id="492c5-151">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="492c5-152">Du har tooclean projektreferenserna genom att ta bort Capptain DLL-referenser.</span><span class="sxs-lookup"><span data-stu-id="492c5-152">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="492c5-153">Om du inte gör detta hello version av Capptain kommer i konflikt och fel händer.</span><span class="sxs-lookup"><span data-stu-id="492c5-153">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="492c5-154">Om du har anpassat Capptain resurser, kopiera ditt gamla filer innehåll och klistra in dem i hello nya Engagement filer.</span><span class="sxs-lookup"><span data-stu-id="492c5-154">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="492c5-155">Observera att både xaml och cs-filer har toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="492c5-155">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="492c5-156">När de steg har du bara tooreplace gamla Capptain referenser från hello nya Engagement referenser.</span><span class="sxs-lookup"><span data-stu-id="492c5-156">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="492c5-157">Alla Capptain namnområden har toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="492c5-157">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="492c5-158">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="492c5-158">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="492c5-159">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="492c5-159">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="492c5-160">Alla Capptain klasser som innehåller ”Capptain” ska innehålla ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="492c5-160">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="492c5-161">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="492c5-161">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="492c5-162">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="492c5-162">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="492c5-163">För xaml-filer Capptain namnområde och attribut även ändras.</span><span class="sxs-lookup"><span data-stu-id="492c5-163">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="492c5-164">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="492c5-164">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="492c5-165">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="492c5-165">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="492c5-166">Överlägget sidan ändringar</span><span class="sxs-lookup"><span data-stu-id="492c5-166">Overlay page changes</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="492c5-167">Överlägget ändras.</span><span class="sxs-lookup"><span data-stu-id="492c5-167">Overlay also changes.</span></span> <span data-ttu-id="492c5-168">Det nya namnområdet är `Microsoft.Azure.Engagement.Overlay`.</span><span class="sxs-lookup"><span data-stu-id="492c5-168">Its new namespace is `Microsoft.Azure.Engagement.Overlay`.</span></span> <span data-ttu-id="492c5-169">Den har toobe som används i xaml- och cs-filer.</span><span class="sxs-lookup"><span data-stu-id="492c5-169">It has toobe used in both xaml and cs files.</span></span> <span data-ttu-id="492c5-170">Dessutom `CapptainGrid` heter toobe `EngagementGrid`, `capptain_notification_content` och `capptain_announcement_content` namnges `engagement_notification_content` och `engagement_announcement_content`.</span><span class="sxs-lookup"><span data-stu-id="492c5-170">Moreover `CapptainGrid` is toobe named `EngagementGrid`, `capptain_notification_content` and `capptain_announcement_content` are named `engagement_notification_content` and `engagement_announcement_content`.</span></span>
   > 
   > 
   
    <span data-ttu-id="492c5-171">För överlägget:</span><span class="sxs-lookup"><span data-stu-id="492c5-171">For overlay :</span></span>
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    <span data-ttu-id="492c5-172">Blir den:</span><span class="sxs-lookup"><span data-stu-id="492c5-172">It becomes :</span></span>
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. <span data-ttu-id="492c5-173">För hello märke andra resurser som Capptain bilder och HTML-filer, Lägg till att de också har bytt namn till toouse ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="492c5-173">For hello other resources like Capptain pictures and HTML files, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="project-declaration"></a><span data-ttu-id="492c5-174">Deklarationen för projektet</span><span class="sxs-lookup"><span data-stu-id="492c5-174">Project declaration</span></span>
<span data-ttu-id="492c5-175">På Package.appxmanifest `File Type Associations` har uppdaterats från:</span><span class="sxs-lookup"><span data-stu-id="492c5-175">On Package.appxmanifest `File Type Associations` has been updated from :</span></span>

* <span data-ttu-id="492c5-176">capptain\_nå\_innehåll tooengagement\_nå\_innehåll</span><span class="sxs-lookup"><span data-stu-id="492c5-176">capptain\_reach\_content tooengagement\_reach\_content</span></span>
* <span data-ttu-id="492c5-177">capptain\_loggen\_filen tooengagement\_loggen\_fil</span><span class="sxs-lookup"><span data-stu-id="492c5-177">capptain\_log\_file tooengagement\_log\_file</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="492c5-178">Program-ID / SDK-nyckeln</span><span class="sxs-lookup"><span data-stu-id="492c5-178">Application ID / SDK Key</span></span>
<span data-ttu-id="492c5-179">Engagement använder en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="492c5-179">Engagement uses a connection string.</span></span> <span data-ttu-id="492c5-180">Du saknar toospecify ett program-ID och SDK-nyckeln med Mobile Engagement har endast toospecify en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="492c5-180">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="492c5-181">Du kan konfigurera det på EngagementConfiguration-fil.</span><span class="sxs-lookup"><span data-stu-id="492c5-181">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="492c5-182">Hej Engagement konfiguration kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="492c5-182">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="492c5-183">Redigera den här filen toospecify:</span><span class="sxs-lookup"><span data-stu-id="492c5-183">Edit this file toospecify:</span></span>

* <span data-ttu-id="492c5-184">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="492c5-184">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="492c5-185">Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="492c5-185">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

<span data-ttu-id="492c5-186">hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="492c5-186">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="492c5-187">Ändring av objekt</span><span class="sxs-lookup"><span data-stu-id="492c5-187">Items name change</span></span>
<span data-ttu-id="492c5-188">Alla objekt med namnet *capptain* namngetts *engagement*.</span><span class="sxs-lookup"><span data-stu-id="492c5-188">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="492c5-189">På liknande sätt för *Capptain* för*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="492c5-189">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="492c5-190">Exempel på vanliga Capptain objekt:</span><span class="sxs-lookup"><span data-stu-id="492c5-190">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="492c5-191">CapptainConfiguration nu namnet EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="492c5-191">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="492c5-192">CapptainAgent nu namnet EngagementAgent</span><span class="sxs-lookup"><span data-stu-id="492c5-192">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="492c5-193">CapptainReach nu namnet EngagementReach</span><span class="sxs-lookup"><span data-stu-id="492c5-193">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="492c5-194">CapptainHttpConfig nu namnet EngagementHttpConfig</span><span class="sxs-lookup"><span data-stu-id="492c5-194">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="492c5-195">GetCapptainPageName nu namnet GetEngagementPageName</span><span class="sxs-lookup"><span data-stu-id="492c5-195">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="492c5-196">Observera att byta namn på också påverkar åsidosätts metoder.</span><span class="sxs-lookup"><span data-stu-id="492c5-196">Note that rename also affects overridden methods.</span></span>

