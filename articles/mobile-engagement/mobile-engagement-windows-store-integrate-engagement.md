---
title: Windows Universal-appar Engagement SDK-Integration
description: Integrera Azure Mobile Engagement med universella Windows-appar
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
ms.openlocfilehash: 898160814304fa8ec65622056a77ca9d4caf2c99
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a><span data-ttu-id="1ea08-103">Windows Universal-appar Engagement SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="1ea08-103">Windows Universal Apps Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1ea08-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="1ea08-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="1ea08-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="1ea08-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="1ea08-106">iOS</span><span class="sxs-lookup"><span data-stu-id="1ea08-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="1ea08-107">Android</span><span class="sxs-lookup"><span data-stu-id="1ea08-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="1ea08-108">Den här proceduren beskriver det enklaste sättet att aktivera Engagement analyser och övervakningsfunktionerna i ditt universella Windows-program.</span><span class="sxs-lookup"><span data-stu-id="1ea08-108">This procedure describes the simplest way to activate Engagement's Analytics and Monitoring functions in your Windows Universal application.</span></span>

<span data-ttu-id="1ea08-109">Följande steg är tillräckligt för att aktivera rapporten i loggar som behövs för att beräkna all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="1ea08-109">The following steps are enough to activate the report of logs needed to compute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="1ea08-110">Rapporten loggar som behövs för att beräkna andra statistik som händelser, fel och jobb måste göras manuellt med hjälp av Engagement-API (se [hur du använder avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md) eftersom dessa statistik är programberoende.</span><span class="sxs-lookup"><span data-stu-id="1ea08-110">The report of logs needed to compute other statistics like Events, Errors and Jobs must be done manually using the Engagement API (see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md) since these statistics are application dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="1ea08-111">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="1ea08-111">Supported versions</span></span>
<span data-ttu-id="1ea08-112">Mobile Engagement SDK för Windows universella appar kan endast integreras i Windows Runtime och Uwp-tillämpningar för:</span><span class="sxs-lookup"><span data-stu-id="1ea08-112">The Mobile Engagement SDK for Windows Universal Apps can only be integrated into Windows Runtime and Universal Windows Platform applications targeting :</span></span>

* <span data-ttu-id="1ea08-113">Windows 8</span><span class="sxs-lookup"><span data-stu-id="1ea08-113">Windows 8</span></span>
* <span data-ttu-id="1ea08-114">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="1ea08-114">Windows 8.1</span></span>
* <span data-ttu-id="1ea08-115">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="1ea08-115">Windows Phone 8.1</span></span>
* <span data-ttu-id="1ea08-116">Windows 10 (familjer stationär dator och mobil)</span><span class="sxs-lookup"><span data-stu-id="1ea08-116">Windows 10 (desktop and mobile families)</span></span>

> [!NOTE]
> <span data-ttu-id="1ea08-117">Om du utvecklar för Windows Phone Silverlight sedan referera till den [Windows Phone Silverlight-integrering proceduren](mobile-engagement-windows-phone-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="1ea08-117">If you are targeting Windows Phone Silverlight then refer to the [Windows Phone Silverlight integration procedure](mobile-engagement-windows-phone-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-the-mobile-engagement-universal-apps-sdk"></a><span data-ttu-id="1ea08-118">Installera Mobile Engagement-SDK för universella appar</span><span class="sxs-lookup"><span data-stu-id="1ea08-118">Install the Mobile Engagement Universal Apps SDK</span></span>
### <a name="all-platforms"></a><span data-ttu-id="1ea08-119">Alla plattformar</span><span class="sxs-lookup"><span data-stu-id="1ea08-119">All platforms</span></span>
<span data-ttu-id="1ea08-120">Mobile Engagement SDK för Windows Universal appen är tillgänglig som ett Nuget-paket som kallas *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="1ea08-120">The Mobile Engagement SDK for Windows Universal App is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="1ea08-121">Du kan installera den från Visual Studio Nuget Package Manager.</span><span class="sxs-lookup"><span data-stu-id="1ea08-121">You can install it from the Visual Studio Nuget Package Manager.</span></span>

### <a name="windows-8x-and-windows-phone-81"></a><span data-ttu-id="1ea08-122">Windows 8.x och Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="1ea08-122">Windows 8.x and Windows Phone 8.1</span></span>
<span data-ttu-id="1ea08-123">NuGet automatiskt distribuerar SDK-resurser i den `Resources` mapp i roten av projektet program.</span><span class="sxs-lookup"><span data-stu-id="1ea08-123">NuGet automatically deploys the SDK resources in the `Resources` folder at the root of your application project.</span></span>

### <a name="windows-10-universal-windows-platform-applications"></a><span data-ttu-id="1ea08-124">Windows 10 Universal Windows Platform program</span><span class="sxs-lookup"><span data-stu-id="1ea08-124">Windows 10 Universal Windows Platform applications</span></span>
<span data-ttu-id="1ea08-125">NuGet distribuerar inte automatiskt SDK-resurser i UWP-appen ännu.</span><span class="sxs-lookup"><span data-stu-id="1ea08-125">NuGet does not automatically deploy the SDK resources in your UWP application yet.</span></span> <span data-ttu-id="1ea08-126">Du behöver göra det manuellt tills resursdistribution sätts in i NuGet:</span><span class="sxs-lookup"><span data-stu-id="1ea08-126">You have to do it manually until resources deployment is reintroduced in NuGet:</span></span>

1. <span data-ttu-id="1ea08-127">Öppna i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="1ea08-127">Open your File Explorer.</span></span>
2. <span data-ttu-id="1ea08-128">Navigera till följande plats (**x.x.x** är version av Engagement som du installerar): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*</span><span class="sxs-lookup"><span data-stu-id="1ea08-128">Navigate to the following location (**x.x.x** is the version of Engagement you are installing): *%USERPROFILE%\\.nuget\packages\MicrosoftAzure.MobileEngagement\\**x.x.x**\\content\win81*</span></span>
3. <span data-ttu-id="1ea08-129">Dra och släpp den **resurser** mappen från Utforskaren till roten för ditt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1ea08-129">Drag and drop the **Resources** folder from the file explorer to the root of your project in Visual Studio.</span></span>
4. <span data-ttu-id="1ea08-130">Välj ditt projekt i Visual Studio och aktivera den **visa alla filer** ikonen ovanpå det **Solution Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1ea08-130">In Visual Studio select your project and activate the **Show All files** icon on top of the **Solution Explorer**.</span></span>
5. <span data-ttu-id="1ea08-131">Vissa filer inte längre ingår i projektet.</span><span class="sxs-lookup"><span data-stu-id="1ea08-131">Some files are not included in the project.</span></span> <span data-ttu-id="1ea08-132">Importera dem på samma gång högerklickar du på den **resurser** mappen **undantas från projektet** och sedan en annan högerklickar du på den **resurser** mappen **inkludera i projektet** till nytt omfattar hela mappen.</span><span class="sxs-lookup"><span data-stu-id="1ea08-132">To import them at once right click on the **Resources** folder, **Exclude from project** then another right click on the **Resources** folder, **Include in project** to re-include the whole folder.</span></span> <span data-ttu-id="1ea08-133">Alla filer från den **resurser** mappen ingår nu i ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="1ea08-133">All files from the **Resources** folder are now included in your project.</span></span>

## <a name="add-the-capabilities"></a><span data-ttu-id="1ea08-134">Lägg till funktioner</span><span class="sxs-lookup"><span data-stu-id="1ea08-134">Add the capabilities</span></span>
<span data-ttu-id="1ea08-135">Engagement-SDK måste vissa funktioner i Windows SDK för att fungera korrekt.</span><span class="sxs-lookup"><span data-stu-id="1ea08-135">The Engagement SDK needs some capabilities of the Windows SDK in order to work properly.</span></span>

<span data-ttu-id="1ea08-136">Öppna din `Package.appxmanifest` filen och se till att följande funktioner har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="1ea08-136">Open your `Package.appxmanifest` file and be sure that the following capabilities are declared:</span></span>

* `Internet (Client)`

## <a name="initialize-the-engagement-sdk"></a><span data-ttu-id="1ea08-137">Initiera Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="1ea08-137">Initialize the Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="1ea08-138">Engagement konfiguration</span><span class="sxs-lookup"><span data-stu-id="1ea08-138">Engagement configuration</span></span>
<span data-ttu-id="1ea08-139">Konfigurationen av Engagement centraliserad i den `Resources\EngagementConfiguration.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="1ea08-139">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="1ea08-140">Redigera den här filen för att ange:</span><span class="sxs-lookup"><span data-stu-id="1ea08-140">Edit this file to specify:</span></span>

* <span data-ttu-id="1ea08-141">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="1ea08-141">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="1ea08-142">Om du vill ange vid körning i stället kan du anropa metoden följande innan Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="1ea08-142">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

<span data-ttu-id="1ea08-143">Anslutningssträngen för ditt program visas på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ea08-143">The connection string for your application is displayed on the Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="1ea08-144">Initieringen av engagement</span><span class="sxs-lookup"><span data-stu-id="1ea08-144">Engagement initialization</span></span>
<span data-ttu-id="1ea08-145">När du skapar ett nytt projekt, en `App.xaml.cs` -filen har genererats.</span><span class="sxs-lookup"><span data-stu-id="1ea08-145">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="1ea08-146">Den här klassen som ärver från `Application` och innehåller många viktiga metoder.</span><span class="sxs-lookup"><span data-stu-id="1ea08-146">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="1ea08-147">Den också används för att initiera Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="1ea08-147">It will also be used to initialize the Engagement SDK.</span></span>

<span data-ttu-id="1ea08-148">Ändra den `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="1ea08-148">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="1ea08-149">Lägg till din `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="1ea08-149">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="1ea08-150">Definiera en metod för att dela Engagement initieringen en gång för alla samtal:</span><span class="sxs-lookup"><span data-stu-id="1ea08-150">Define a method to share the Engagement initialization once for all calls:</span></span>
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* <span data-ttu-id="1ea08-151">Anropa `InitEngagement` i den `OnLaunched` metoden:</span><span class="sxs-lookup"><span data-stu-id="1ea08-151">Call `InitEngagement` in the `OnLaunched` method:</span></span>
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* <span data-ttu-id="1ea08-152">När programmet startas med ett anpassat schema, ett annat program eller kommandoraden sedan `OnActivated` metoden anropas.</span><span class="sxs-lookup"><span data-stu-id="1ea08-152">When your application is launched using a custom scheme, another application or the command line then the `OnActivated` method is called.</span></span> <span data-ttu-id="1ea08-153">Du måste också starta Engagement SDK när appen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="1ea08-153">You also need to initiate the Engagement SDK when your app is activated.</span></span> <span data-ttu-id="1ea08-154">Det gör du genom att åsidosätta `OnActivated` metoden:</span><span class="sxs-lookup"><span data-stu-id="1ea08-154">To do so, override `OnActivated` method:</span></span>
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> <span data-ttu-id="1ea08-155">Vi avråder dig för att lägga till initiering av Engagement på en annan plats för ditt program.</span><span class="sxs-lookup"><span data-stu-id="1ea08-155">We strongly discourage you to add the Engagement initialization in another place of your application.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="1ea08-156">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="1ea08-156">Basic reporting</span></span>
### <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="1ea08-157">Rekommenderad metod: överlagra din `Page` klasser</span><span class="sxs-lookup"><span data-stu-id="1ea08-157">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="1ea08-158">För att aktivera rapporten i alla loggar som krävs av Engagement att beräkna användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `Page` underordnade klasser ärver från den `EngagementPage` klasser.</span><span class="sxs-lookup"><span data-stu-id="1ea08-158">In order to activate the report of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="1ea08-159">Här är ett exempel på hur du gör detta för en sida i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1ea08-159">Here is an example of how to do this for a page of your application.</span></span> <span data-ttu-id="1ea08-160">Du kan göra samma sak för alla sidor i ditt program.</span><span class="sxs-lookup"><span data-stu-id="1ea08-160">You can do the same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="1ea08-161">C# källfilen</span><span class="sxs-lookup"><span data-stu-id="1ea08-161">C# Source file</span></span>
<span data-ttu-id="1ea08-162">Ändra sidan `.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="1ea08-162">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="1ea08-163">Lägg till din `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="1ea08-163">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="1ea08-164">Ersätt `Page` med `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="1ea08-164">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="1ea08-165">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1ea08-165">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="1ea08-166">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1ea08-166">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="1ea08-167">Om sidan åsidosätter metoden `OnNavigatedTo` ska du anropa `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="1ea08-167">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="1ea08-168">Annars rapporteras inte aktiviteten (`EngagementPage` anropar `StartActivity` i tillhörande `OnNavigatedTo`-metod).</span><span class="sxs-lookup"><span data-stu-id="1ea08-168">Otherwise,  the activity will not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="1ea08-169">XAML-fil</span><span class="sxs-lookup"><span data-stu-id="1ea08-169">XAML file</span></span>
<span data-ttu-id="1ea08-170">Ändra sidan `.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="1ea08-170">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="1ea08-171">Lägg till följande i namnområdesdeklarationerna:</span><span class="sxs-lookup"><span data-stu-id="1ea08-171">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="1ea08-172">Ersätt `Page` med `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="1ea08-172">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="1ea08-173">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1ea08-173">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="1ea08-174">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="1ea08-174">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-the-default-behaviour"></a><span data-ttu-id="1ea08-175">Åsidosätta beteenden som standard</span><span class="sxs-lookup"><span data-stu-id="1ea08-175">Override the default behaviour</span></span>
<span data-ttu-id="1ea08-176">Klassnamnet för sidan har rapporterats som aktivitetsnamn med utan extra som standard.</span><span class="sxs-lookup"><span data-stu-id="1ea08-176">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="1ea08-177">Om klassen använder suffixet ”Page”, tas Engagement också bort.</span><span class="sxs-lookup"><span data-stu-id="1ea08-177">If the class uses the "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="1ea08-178">Om du vill åsidosätta standard beteendet för namn, lägger du till detta i koden:</span><span class="sxs-lookup"><span data-stu-id="1ea08-178">If you want to override the default behaviour for the name, simply add this to your code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="1ea08-179">Om du vill rapportera vissa extra information med dina aktiviteter kan du lägga till detta koden:</span><span class="sxs-lookup"><span data-stu-id="1ea08-179">If you want to report some extra informations with your activity, you can add this to your code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="1ea08-180">De här metoderna anropas inifrån den `OnNavigatedTo` metoden på sidan.</span><span class="sxs-lookup"><span data-stu-id="1ea08-180">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="1ea08-181">Alternativ metod: anropa `StartActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="1ea08-181">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="1ea08-182">Om du inte kan eller inte vill överlagra din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="1ea08-182">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="1ea08-183">Vi rekommenderar för att anropa `StartActivity` i din `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="1ea08-183">We recommend to call `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="1ea08-184">Se till att du kan avsluta sessionen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="1ea08-184">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="1ea08-185">Windows Universal SDK automatiskt anropar den `EndActivity` metoden när programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="1ea08-185">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="1ea08-186">Därför är det **hög** rekommenderas att anropa den `StartActivity` metod när aktiviteten användaren ändrar och **aldrig** anropa den `EndActivity` metoden den här metoden skickar till servern för Engagement som aktuell användare har lämnar programmet, då påverkar alla programloggar.</span><span class="sxs-lookup"><span data-stu-id="1ea08-186">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method, this method sends to Engagement server that current user has leave the application, this will impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="1ea08-187">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="1ea08-187">Advanced reporting</span></span>
<span data-ttu-id="1ea08-188">Du kanske vill rapporten programmet specifika händelser, fel och jobb, gör du genom använda andra metoder hittades i den `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="1ea08-188">Optionally, you may want to report application specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="1ea08-189">Engagement API kan använda alla Engagement avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="1ea08-189">The Engagement API allows to use all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="1ea08-190">Mer information finns i [hur du använder avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="1ea08-190">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="1ea08-191">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="1ea08-191">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="1ea08-192">Inaktivera automatisk rapportering om krasch</span><span class="sxs-lookup"><span data-stu-id="1ea08-192">Disable automatic crash reporting</span></span>
<span data-ttu-id="1ea08-193">Du kan inaktivera automatisk kraschen reporting-funktionen i Engagement.</span><span class="sxs-lookup"><span data-stu-id="1ea08-193">You can disable the automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="1ea08-194">Sedan, när ett ohanterat undantag inträffar, Engagement inte göra något.</span><span class="sxs-lookup"><span data-stu-id="1ea08-194">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="1ea08-195">Om du planerar att inaktivera den här funktionen måste vara medveten om att när en ohanterad krasch sker i din app Engagement inte kommer att skicka kraschen **och** kommer inte att stänga sessionen och jobb.</span><span class="sxs-lookup"><span data-stu-id="1ea08-195">If you plan to disable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send the crash **AND** will not close the session and jobs.</span></span>
> 
> 

<span data-ttu-id="1ea08-196">Om du vill inaktivera automatisk rapportering om krasch du bara anpassa konfigurationen beroende på hur du har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="1ea08-196">To disable automatic crash reporting, just customize your configuration depending on the way you declared it:</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="1ea08-197">Från `EngagementConfiguration.xml` fil</span><span class="sxs-lookup"><span data-stu-id="1ea08-197">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="1ea08-198">Ange rapport-krascher till `false` mellan `<reportCrash>` och `</reportCrash>` taggar.</span><span class="sxs-lookup"><span data-stu-id="1ea08-198">Set report crash to `false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="1ea08-199">Från `EngagementConfiguration` objekt vid körning.</span><span class="sxs-lookup"><span data-stu-id="1ea08-199">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="1ea08-200">Ange rapport-krascher till false med EngagementConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="1ea08-200">Set report crash to false using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="1ea08-201">Burst-läge</span><span class="sxs-lookup"><span data-stu-id="1ea08-201">Burst mode</span></span>
<span data-ttu-id="1ea08-202">Som standard loggas Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="1ea08-202">By default, the Engagement service reports logs in real time.</span></span> <span data-ttu-id="1ea08-203">Om ditt program rapporterar loggar väldigt ofta, är det bättre att buffra loggarna och rapportera dem samtidigt på en vanlig tiden som bas (Detta kallas ”burst-läge”).</span><span class="sxs-lookup"><span data-stu-id="1ea08-203">If your application reports logs very frequently, it is better to buffer the logs and to report them all at once on a regular time base (this is called the “burst mode”).</span></span>

<span data-ttu-id="1ea08-204">Det gör du genom att anropa metoden:</span><span class="sxs-lookup"><span data-stu-id="1ea08-204">To do so, call the method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="1ea08-205">Argumentet är ett värde i **millisekunder**.</span><span class="sxs-lookup"><span data-stu-id="1ea08-205">The argument is a value in **milliseconds**.</span></span> <span data-ttu-id="1ea08-206">När som helst om du vill återaktivera realtid loggning bara att anropa metoden utan någon parameter eller med värdet 0.</span><span class="sxs-lookup"><span data-stu-id="1ea08-206">At any time, if you want to reactivate the real-time logging, just call the method without any parameter, or with the 0 value.</span></span>

<span data-ttu-id="1ea08-207">Burst-läge något öka för batterilivslängd men påverkar Engagement-Övervakare: varaktighet för alla sessioner och jobb ska avrundas till burst tröskelvärdet (alltså sessioner och jobb som är kortare än tröskelvärdet burst inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="1ea08-207">The burst mode slightly increase the battery life but has an impact on the Engagement Monitor: all sessions and jobs duration will be rounded to the burst threshold (thus, sessions and jobs shorter than the burst threshold may not be visible).</span></span> <span data-ttu-id="1ea08-208">Det rekommenderas att använda ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="1ea08-208">It is recommended to use a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="1ea08-209">Du måste vara medveten om sparade loggarna är begränsade till 300 objekt.</span><span class="sxs-lookup"><span data-stu-id="1ea08-209">You have to be aware that saved logs are limited to 300 items.</span></span> <span data-ttu-id="1ea08-210">Om det är för lång tid att skicka förlora du vissa loggar.</span><span class="sxs-lookup"><span data-stu-id="1ea08-210">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="1ea08-211">Tröskelvärdet burst kan inte konfigureras för en period som är mindre än 1s.</span><span class="sxs-lookup"><span data-stu-id="1ea08-211">The burst threshold cannot be configured to a period lesser than 1s.</span></span> <span data-ttu-id="1ea08-212">Om du försöker göra det, visas en spårning med fel SDK och återställer automatiskt till standardvärdet, dvs 0s.</span><span class="sxs-lookup"><span data-stu-id="1ea08-212">If you try to do so, the SDK will show a trace with the error and will automatically reset to the default value, i.e., 0s.</span></span> <span data-ttu-id="1ea08-213">Detta utlöser SDK om du vill rapportera loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="1ea08-213">This will trigger the SDK to report the logs in real-time.</span></span>
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

