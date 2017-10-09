---
title: aaaWindows universella appar Engagement SDK-Integration
description: Hur tooIntegrate Azure Mobile Engagement med universella Windows-appar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="4b2c9-103">Windows Universal-appar Engagement SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="4b2c9-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4b2c9-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="4b2c9-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="4b2c9-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="4b2c9-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="4b2c9-106">iOS</span><span class="sxs-lookup"><span data-stu-id="4b2c9-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="4b2c9-107">Android</span><span class="sxs-lookup"><span data-stu-id="4b2c9-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="4b2c9-108">Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i ditt universella Windows-program.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-108">This procedure describes hello simplest way tooactivate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="4b2c9-109">hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="4b2c9-110">hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md) eftersom statistiken är programberoende.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="4b2c9-111">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="4b2c9-111">Supported versions</span></span>
<span data-ttu-id="4b2c9-112">hello Mobile Engagement SDK för universella Windows-appar kan endast integreras i Windows Runtime och Uwp-tillämpningar för:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-112">hello Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="4b2c9-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="4b2c9-113">Windows 8</span></span>
* <span data-ttu-id="4b2c9-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="4b2c9-114">Windows 8.1</span></span>
* <span data-ttu-id="4b2c9-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="4b2c9-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="4b2c9-116">Windows 10 (familjer stationär dator och mobil)</span><span class="sxs-lookup"><span data-stu-id="4b2c9-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="4b2c9-117">Om du utvecklar för Windows Phone Silverlight finns toohello [Windows Phone Silverlight-integrering proceduren](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-117">If you are targeting Windows Phone Silverlight then refer toohello [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="4b2c9-118">Installera hello Mobile Engagement-SDK Universal-appar</span><span class="sxs-lookup"><span data-stu-id="4b2c9-118">Install hello Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="4b2c9-119">Alla plattformar</span><span class="sxs-lookup"><span data-stu-id="4b2c9-119">All platforms</span></span>
<span data-ttu-id="4b2c9-120">hello Mobile Engagement SDK för Windows Universal-appen är tillgänglig som ett Nuget-paket som kallas *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-120">hello Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="4b2c9-121">Du kan installera den från hello Pakethanteraren Nuget för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-121">You can install it from hello Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="4b2c9-122">Windows 8.x och Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="4b2c9-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="4b2c9-123">NuGet automatiskt distribuerar hello SDK resurser i hello `Resources` hello rotmapp från projektet program.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-123">NuGet automatically deploys hello SDK resources in hello `Resources` folder at hello root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="4b2c9-124">Windows 10 Universal Windows Platform program</span><span class="sxs-lookup"><span data-stu-id="4b2c9-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="4b2c9-125">NuGet distribuerar inte automatiskt hello SDK-resurser i ditt program för UWP ännu.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-125">NuGet does not automatically deploy hello SDK resources in your UWP application yet.</span></span> <span data-ttu-id="4b2c9-126">Du har det manuellt tills resursdistribution sätts in i NuGet toodo:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-126">You have toodo it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="4b2c9-127">Öppna i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="4b2c9-128">Navigera toohello följande plats (**x.x.x** är hello version av Engagement som du installerar): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="4b2c9-128">Navigate toohello following location (**x.x.x** is hello version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="4b2c9-129">Dra och släpp hello **resurser** mapp från hello file explorer toohello roten för ditt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-129">Drag and drop hello **Resources** folder from hello file explorer toohello root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="4b2c9-130">Välj ditt projekt i Visual Studio och aktivera hello **visa alla filer** ikonen ovanpå hello **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-130">In Visual Studio select your project and activate hello **Show All files** icon on top of hello **Solution Explorer**.</span></span>
5. <span data-ttu-id="4b2c9-131">Vissa filer ingår inte i hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-131">Some files are not included in hello project.</span></span> <span data-ttu-id="4b2c9-132">tooimport dem på samma gång högerklickar du på hello **resurser** mappen **undantas från projektet** och sedan en annan högerklickar du på hello **resurser** mappen **inkludera i projektet** toore-inkludera hello hela mappen.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-132">tooimport them at once right click on hello **Resources** folder, **Exclude from project** then another right click on hello **Resources** folder, **Include in project** toore-include hello whole folder.</span></span> <span data-ttu-id="4b2c9-133">Alla filer från hello **resurser** mappen ingår nu i ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-133">All files from hello **Resources** folder are now included in your project.</span></span>

## <a name="add-hello-capabilities"></a><span data-ttu-id="4b2c9-134">Lägger till hello-funktioner</span><span class="sxs-lookup"><span data-stu-id="4b2c9-134">Add hello capabilities</span></span>
<span data-ttu-id="4b2c9-135">Hej Engagement SDK måste vissa funktioner i hello Windows SDK i ordning toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-135">hello Engagement SDK needs some capabilities of hello Windows SDK in order toowork properly.</span></span>

<span data-ttu-id="4b2c9-136">Öppna din `Package.appxmanifest` fil- och måste deklareras som hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-136">Open your `Package.appxmanifest` file and be sure that hello following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="4b2c9-137">Initiera hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="4b2c9-137">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="4b2c9-138">Engagement konfiguration</span><span class="sxs-lookup"><span data-stu-id="4b2c9-138">Engagement configuration</span></span>
<span data-ttu-id="4b2c9-139">Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-139">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="4b2c9-140">Redigera den här filen toospecify:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-140">Edit this file toospecify:</span></span>

* <span data-ttu-id="4b2c9-141">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="4b2c9-142">Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-142">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="4b2c9-143">hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-143">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="4b2c9-144">Initieringen av engagement</span><span class="sxs-lookup"><span data-stu-id="4b2c9-144">Engagement initialization</span></span>
<span data-ttu-id="4b2c9-145">När du skapar ett nytt projekt, en `App.xaml.cs` -filen har genererats.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="4b2c9-146">Den här klassen som ärver från `Application` och innehåller många viktiga metoder.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="4b2c9-147">Det kommer även att använda tooinitialize hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-147">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="4b2c9-148">Ändra hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-148">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="4b2c9-149">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-149">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="4b2c9-150">Definiera metoden tooshare hello Engagement initiera en gång för alla samtal:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-150">Define a method tooshare hello Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="4b2c9-151">Anropa `InitEngagement` i hello `OnLaunched` metoden:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-151">Call `InitEngagement` in hello `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="4b2c9-152">När programmet startas med ett anpassat schema, ett annat program eller hello kommandoraden sedan hello `OnActivated` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-152">When your application is launched using a custom scheme, another application or hello command line then hello `OnActivated` method is called.</span></span> <span data-ttu-id="4b2c9-153">Du måste också tooinitiate hello Engagement SDK när appen är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-153">You also need tooinitiate hello Engagement SDK when your app is activated.</span></span> <span data-ttu-id="4b2c9-154">toodo så åsidosätta `OnActivated` metoden:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-154">toodo so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="4b2c9-155">Vi avråder du tooadd hello Engagement initiering på en annan plats för ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-155">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="4b2c9-156">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="4b2c9-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="4b2c9-157">Rekommenderad metod: överlagra din `Page` klasser</span><span class="sxs-lookup"><span data-stu-id="4b2c9-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="4b2c9-158">I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klasser.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-158">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="4b2c9-159">Här är ett exempel på hur toodo för en sida i ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-159">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="4b2c9-160">Du kan göra hello detsamma för alla sidor i ditt program.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-160">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="4b2c9-161">C# källfilen</span><span class="sxs-lookup"><span data-stu-id="4b2c9-161">C# Source file</span></span>
<span data-ttu-id="4b2c9-162">Ändra sidan `.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="4b2c9-163">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-163">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="4b2c9-164">Ersätt `Page` med `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="4b2c9-165">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4b2c9-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="4b2c9-166">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4b2c9-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="4b2c9-167">Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-167">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="4b2c9-168">Annars hello aktivitet rapporteras inte (hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-168">Otherwise,  hello activity will not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="4b2c9-169">XAML-fil</span><span class="sxs-lookup"><span data-stu-id="4b2c9-169">XAML file</span></span>
<span data-ttu-id="4b2c9-170">Ändra sidan `.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="4b2c9-171">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-171">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="4b2c9-172">Ersätt `Page` med `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="4b2c9-173">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4b2c9-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="4b2c9-174">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="4b2c9-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a><span data-ttu-id="4b2c9-175">Åsidosätt hello standard beteende</span><span class="sxs-lookup"><span data-stu-id="4b2c9-175">Override hello default behaviour</span></span>
<span data-ttu-id="4b2c9-176">Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-176">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="4b2c9-177">Om hello klassen använder hello ”Page” suffix, tas Engagement också bort.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-177">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="4b2c9-178">Om du vill toooverride hello standard beteendet för hello namn kan bara lägga till tooyour koden:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-178">If you want toooverride hello default behaviour for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="4b2c9-179">Om du vill tooreport vissa extra information med dina aktiviteter, kan du lägga till tooyour koden:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-179">If you want tooreport some extra informations with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="4b2c9-180">De här metoderna anropas från inom hello `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-180">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="4b2c9-181">Alternativ metod: anropa `StartActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="4b2c9-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="4b2c9-182">Om du inte kan eller inte vill att toooverload din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-182">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="4b2c9-183">Vi rekommenderar att toocall `StartActivity` i din `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-183">We recommend toocall `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="4b2c9-184">Se till att du kan avsluta sessionen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="4b2c9-185">hello Windows Universal SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-185">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="4b2c9-186">Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` -metoden den här metoden skickar tooEngagement servern att aktuell användare har lämna hello program, då påverkar alla programloggar.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-186">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method, this method sends tooEngagement server that current user has leave hello application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="4b2c9-187">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="4b2c9-187">Advanced reporting</span></span>
<span data-ttu-id="4b2c9-188">Alternativt kan du tooreport programmet specifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-188">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="4b2c9-189">Hej Engagement API kan toouse alla Engagement avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-189">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="4b2c9-190">Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-190">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="4b2c9-191">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="4b2c9-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="4b2c9-192">Inaktivera automatisk rapportering om krasch</span><span class="sxs-lookup"><span data-stu-id="4b2c9-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="4b2c9-193">Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-193">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="4b2c9-194">Sedan, när ett ohanterat undantag inträffar, Engagement inte göra något.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="4b2c9-195">Om du planerar toodisable funktionen, Tänk på att när en ohanterad krasch sker i din app Engagement inte kommer att skicka hello krascher **och** inte och kommer att avslutas sessionen hello jobb.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-195">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="4b2c9-196">toodisable automatisk krascher rapportering, helt enkelt anpassa konfigurationen beroende på hello sätt som du har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-196">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="4b2c9-197">Från `EngagementConfiguration.xml` fil</span><span class="sxs-lookup"><span data-stu-id="4b2c9-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="4b2c9-198">Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-198">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="4b2c9-199">Från `EngagementConfiguration` objekt vid körning.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="4b2c9-200">Ange rapporten krascher toofalse med EngagementConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-200">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="4b2c9-201">Burst-läge</span><span class="sxs-lookup"><span data-stu-id="4b2c9-201">Burst mode</span></span>
<span data-ttu-id="4b2c9-202">Som standard loggas hello Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-202">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="4b2c9-203">Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-203">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="4b2c9-204">toodo anropa så hello-metoden:</span><span class="sxs-lookup"><span data-stu-id="4b2c9-204">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="4b2c9-205">hello-argumentet är ett värde i **millisekunder**.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-205">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="4b2c9-206">När som helst om du vill tooreactivate hello realtid loggning kan bara anropa hello metod utan någon parameter eller med hello 0-värde.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-206">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="4b2c9-207">hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-207">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="4b2c9-208">Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="4b2c9-208">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="4b2c9-209">Du har toobe medveten sparade loggarna är begränsad too300 objekt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-209">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="4b2c9-210">Om det är för lång tid att skicka förlora du vissa loggar.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="4b2c9-211">hello burst tröskelvärdet kan inte konfigureras tooa perioden som är mindre än 1s.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-211">hello burst threshold cannot be configured tooa period lesser than 1s.</span></span> <span data-ttu-id="4b2c9-212">Om du försöker toodo så återställa toohello standardvärdet, dvs 0s hello SDK visas en spårning med hello fel och automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-212">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, i.e., 0s.</span></span> <span data-ttu-id="4b2c9-213">Detta utlöser hello SDK tooreport hello-loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="4b2c9-213">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

