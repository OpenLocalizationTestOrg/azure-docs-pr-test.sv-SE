---
title: aaaWindows Phone Silverlight Engagement SDK-Integration
description: Hur tooIntegrate Azure Mobile Engagement med Windows Phone Silverlight-appar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 447fea8d-f4e3-4ad4-8ec0-8e3cf1ad3ab0
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f65683a62e5256cea469a3a73d99ade4331cb6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a><span data-ttu-id="47864-103">Windows Phone Silverlight Engagement SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="47864-103">Windows Phone Silverlight Engagement SDK Integration</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="47864-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="47864-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md) 
> * [<span data-ttu-id="47864-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="47864-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [<span data-ttu-id="47864-106">iOS</span><span class="sxs-lookup"><span data-stu-id="47864-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md) 
> * [<span data-ttu-id="47864-107">Android</span><span class="sxs-lookup"><span data-stu-id="47864-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md) 
> 
> 

<span data-ttu-id="47864-108">Den här proceduren beskriver hello enklaste sättet tooactivate Azure Mobile Engagement analyser och övervakningsfunktionerna i Windows Phone Silverlight-program.</span><span class="sxs-lookup"><span data-stu-id="47864-108">This procedure describes hello simplest way tooactivate Azure Mobile Engagement's Analytics and Monitoring functions in your Windows Phone Silverlight application.</span></span>

<span data-ttu-id="47864-109">hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals.</span><span class="sxs-lookup"><span data-stu-id="47864-109">hello following steps are enough tooactivate hello report of logs needed toocompute all statistics regarding Users, Sessions, Activities, Crashes and Technicals.</span></span> <span data-ttu-id="47864-110">hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md) nedan) eftersom statistiken är programmet beroende.</span><span class="sxs-lookup"><span data-stu-id="47864-110">hello report of logs needed toocompute other statistics like Events, Errors and Jobs must be done manually using hello Engagement API (see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md) below) since these statistics are application-dependent.</span></span>

## <a name="supported-versions"></a><span data-ttu-id="47864-111">Versioner som stöds</span><span class="sxs-lookup"><span data-stu-id="47864-111">Supported versions</span></span>
<span data-ttu-id="47864-112">hello Mobile Engagement SDK för Windows Silverlight kan endast integreras i program som mål:</span><span class="sxs-lookup"><span data-stu-id="47864-112">hello Mobile Engagement SDK for Windows Silverlight can only be integrated into applications targeting :</span></span>

* <span data-ttu-id="47864-113">Windows Phone 8.0</span><span class="sxs-lookup"><span data-stu-id="47864-113">Windows Phone 8.0</span></span>
* <span data-ttu-id="47864-114">Windows Phone 8.1 Silverlight</span><span class="sxs-lookup"><span data-stu-id="47864-114">Windows Phone 8.1 Silverlight</span></span>

> [!NOTE]
> <span data-ttu-id="47864-115">Om du utvecklar för Windows Phone 8.1 (utan Silverlight) finns toohello [universella Windows-integration proceduren](mobile-engagement-windows-store-integrate-engagement.md).</span><span class="sxs-lookup"><span data-stu-id="47864-115">If you are targeting Windows Phone 8.1 (non-Silverlight) then refer toohello [Windows Universal integration procedure](mobile-engagement-windows-store-integrate-engagement.md).</span></span>
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a><span data-ttu-id="47864-116">Installera hello Mobile Engagement Silverlight SDK</span><span class="sxs-lookup"><span data-stu-id="47864-116">Install hello Mobile Engagement Silverlight SDK</span></span>
<span data-ttu-id="47864-117">hello Mobile Engagement SDK för Windows Silverlight är tillgänglig som ett Nuget-paket som kallas *MicrosoftAzure.MobileEngagement*.</span><span class="sxs-lookup"><span data-stu-id="47864-117">hello Mobile Engagement SDK for Windows Silverlight is available as a Nuget package called *MicrosoftAzure.MobileEngagement*.</span></span> <span data-ttu-id="47864-118">Du kan installera den från hello Pakethanteraren Nuget för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="47864-118">You can install it from hello Visual Studio Nuget Package Manager.</span></span> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="47864-119">Lägger till hello-funktioner</span><span class="sxs-lookup"><span data-stu-id="47864-119">Add hello capabilities</span></span>
<span data-ttu-id="47864-120">Hej Engagement SDK måste vissa funktioner i hello Windows Phone Silverlight SDK i ordning toowork korrekt.</span><span class="sxs-lookup"><span data-stu-id="47864-120">hello Engagement SDK needs some capabilities of hello Windows Phone Silverlight SDK in order toowork properly.</span></span>

<span data-ttu-id="47864-121">Öppna din `WMAppManifest.xml` filen och se till att följande funktioner hello deklareras i hello `Capabilities` panelen:</span><span class="sxs-lookup"><span data-stu-id="47864-121">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared in hello `Capabilities` panel:</span></span>

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a><span data-ttu-id="47864-122">Initiera hello Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="47864-122">Initialize hello Engagement SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="47864-123">Engagement konfiguration</span><span class="sxs-lookup"><span data-stu-id="47864-123">Engagement configuration</span></span>
<span data-ttu-id="47864-124">Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="47864-124">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="47864-125">Redigera den här filen toospecify:</span><span class="sxs-lookup"><span data-stu-id="47864-125">Edit this file toospecify :</span></span>

* <span data-ttu-id="47864-126">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="47864-126">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="47864-127">Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="47864-127">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="47864-128">hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="47864-128">hello connection string for your application is displayed on hello Azure Classic Portal.</span></span>

### <a name="engagement-initialization"></a><span data-ttu-id="47864-129">Initieringen av engagement</span><span class="sxs-lookup"><span data-stu-id="47864-129">Engagement initialization</span></span>
<span data-ttu-id="47864-130">När du skapar ett nytt projekt, en `App.xaml.cs` -filen har genererats.</span><span class="sxs-lookup"><span data-stu-id="47864-130">When you create a new project, a `App.xaml.cs` file is generated.</span></span> <span data-ttu-id="47864-131">Den här klassen som ärver från `Application` och innehåller många viktiga metoder.</span><span class="sxs-lookup"><span data-stu-id="47864-131">This class inherits from `Application` and contains many important methods.</span></span> <span data-ttu-id="47864-132">Det kommer även att använda tooinitialize hello Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="47864-132">It will also be used tooinitialize hello Engagement SDK.</span></span>

<span data-ttu-id="47864-133">Ändra hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="47864-133">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="47864-134">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="47864-134">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="47864-135">Infoga `EngagementAgent.Instance.Init` i hello `Application_Launching` metoden:</span><span class="sxs-lookup"><span data-stu-id="47864-135">Insert `EngagementAgent.Instance.Init` in hello `Application_Launching` method :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* <span data-ttu-id="47864-136">Infoga `EngagementAgent.Instance.OnActivated` i hello `Application_Activated` metoden:</span><span class="sxs-lookup"><span data-stu-id="47864-136">Insert `EngagementAgent.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> <span data-ttu-id="47864-137">Vi avråder du tooadd hello Engagement initiering på en annan plats för ditt program.</span><span class="sxs-lookup"><span data-stu-id="47864-137">We strongly discourage you tooadd hello Engagement initialization in another place of your application.</span></span> <span data-ttu-id="47864-138">Men tänk på att hello `EngagementAgent.Instance.Init` -metoden körs på en dedikerad tråd och inte på hello UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="47864-138">However, be aware that hello `EngagementAgent.Instance.Init` method runs on a dedicated thread, and not on hello UI thread.</span></span>
> 
> 

## <a name="basic-reporting"></a><span data-ttu-id="47864-139">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="47864-139">Basic reporting</span></span>
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a><span data-ttu-id="47864-140">Rekommenderad metod: överlagra din `PhoneApplicationPage` klasser</span><span class="sxs-lookup"><span data-stu-id="47864-140">Recommended method : overload your `PhoneApplicationPage` classes</span></span>
<span data-ttu-id="47864-141">I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `PhoneApplicationPage` underordnade klasser ärver från hello `EngagementPage` klasser.</span><span class="sxs-lookup"><span data-stu-id="47864-141">In order tooactivate hello report of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, you can simply make all your `PhoneApplicationPage` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="47864-142">Här är ett exempel på hur toodo för en sida i ditt program.</span><span class="sxs-lookup"><span data-stu-id="47864-142">Here is an example of how toodo this for a page of your application.</span></span> <span data-ttu-id="47864-143">Du kan göra hello detsamma för alla sidor i ditt program.</span><span class="sxs-lookup"><span data-stu-id="47864-143">You can do hello same thing for all pages of your application.</span></span>

#### <a name="c-source-file"></a><span data-ttu-id="47864-144">C# källfilen</span><span class="sxs-lookup"><span data-stu-id="47864-144">C# Source file</span></span>
<span data-ttu-id="47864-145">Ändra sidan `.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="47864-145">Modify your page `.xaml.cs` file :</span></span>

* <span data-ttu-id="47864-146">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="47864-146">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="47864-147">Ersätt `PhoneApplicationPage` med `EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="47864-147">Replace `PhoneApplicationPage` with `EngagementPage` :</span></span>

<span data-ttu-id="47864-148">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="47864-148">**Without Engagement:**</span></span>

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

<span data-ttu-id="47864-149">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="47864-149">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> <span data-ttu-id="47864-150">Om sidan ärver från hello `OnNavigatedTo` metoden vara försiktig toolet hello `base.OnNavigatedTo(e)` anropa.</span><span class="sxs-lookup"><span data-stu-id="47864-150">If your page inherits from hello `OnNavigatedTo` method, be careful toolet hello `base.OnNavigatedTo(e)` call.</span></span> <span data-ttu-id="47864-151">Annars rapporteras hello aktivitet inte.</span><span class="sxs-lookup"><span data-stu-id="47864-151">Otherwise, hello activity will not be reported.</span></span> <span data-ttu-id="47864-152">Faktiskt hello `EngagementPage` anropar `StartActivity` inuti hello `OnNavigatedTo` metod.</span><span class="sxs-lookup"><span data-stu-id="47864-152">Indeed, hello `EngagementPage` is calling `StartActivity` inside hello `OnNavigatedTo` method.</span></span>
> 
> 

#### <a name="xaml-file"></a><span data-ttu-id="47864-153">XAML-fil</span><span class="sxs-lookup"><span data-stu-id="47864-153">XAML file</span></span>
<span data-ttu-id="47864-154">Ändra sidan `.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="47864-154">Modify your page `.xaml` file :</span></span>

* <span data-ttu-id="47864-155">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="47864-155">Add tooyour namespaces declarations :</span></span>
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* <span data-ttu-id="47864-156">Ersätt `phone:PhoneApplicationPage` med `engagement:EngagementPage` :</span><span class="sxs-lookup"><span data-stu-id="47864-156">Replace `phone:PhoneApplicationPage` with `engagement:EngagementPage` :</span></span>

<span data-ttu-id="47864-157">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="47864-157">**Without Engagement:**</span></span>

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

<span data-ttu-id="47864-158">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="47864-158">**With Engagement:**</span></span>

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a><span data-ttu-id="47864-159">Åsidosätta standardbeteendet för hello</span><span class="sxs-lookup"><span data-stu-id="47864-159">Override hello default behavior</span></span>
<span data-ttu-id="47864-160">Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra.</span><span class="sxs-lookup"><span data-stu-id="47864-160">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="47864-161">Om hello klassen använder hello ”Page” suffix, tas Engagement också bort.</span><span class="sxs-lookup"><span data-stu-id="47864-161">If hello class uses hello "Page" suffix, Engagement will also remove it.</span></span>

<span data-ttu-id="47864-162">Om du vill toooverride hello standardbeteendet för hello namn kan bara lägga till tooyour koden:</span><span class="sxs-lookup"><span data-stu-id="47864-162">If you want toooverride hello default behavior for hello name, simply add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

<span data-ttu-id="47864-163">Om du vill tooreport vissa extra information med dina aktiviteter, kan du lägga till tooyour koden:</span><span class="sxs-lookup"><span data-stu-id="47864-163">If you want tooreport some extra information with your activity, you can add this tooyour code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

<span data-ttu-id="47864-164">De här metoderna anropas från inom hello `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="47864-164">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="47864-165">Alternativ metod: anropa `StartActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="47864-165">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="47864-166">Om du inte kan eller inte vill att toooverload din `PhoneApplicationPage` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="47864-166">If you cannot or do not want toooverload your `PhoneApplicationPage` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="47864-167">Vi rekommenderar att anropa `StartActivity` i din `OnNavigatedTo` metod för din PhoneApplicationPage.</span><span class="sxs-lookup"><span data-stu-id="47864-167">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your PhoneApplicationPage.</span></span>

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> <span data-ttu-id="47864-168">Se till att du kan avsluta sessionen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="47864-168">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="47864-169">hello SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="47864-169">hello SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="47864-170">Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` metod.</span><span class="sxs-lookup"><span data-stu-id="47864-170">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="47864-171">Den här metoden skickar en message toohello Engagement server att hello aktuell användare har lämnat hello programmet detta påverkar alla programloggar.</span><span class="sxs-lookup"><span data-stu-id="47864-171">This method sends a message toohello Engagement server that hello current user has left hello application and this impacts all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="47864-172">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="47864-172">Advanced reporting</span></span>
<span data-ttu-id="47864-173">Alternativt kan du tooreport programmet specifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="47864-173">Optionally, you may want tooreport application specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="47864-174">Hej Engagement API kan toouse alla Engagement avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="47864-174">hello Engagement API allows toouse all of Engagement's advanced capabilities.</span></span>

<span data-ttu-id="47864-175">Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="47864-175">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Phone Silverlight app](mobile-engagement-windows-phone-use-engagement-api.md).</span></span>

## <a name="advanced-configuration"></a><span data-ttu-id="47864-176">Avancerad konfiguration</span><span class="sxs-lookup"><span data-stu-id="47864-176">Advanced configuration</span></span>
### <a name="disable-automatic-crash-reporting"></a><span data-ttu-id="47864-177">Inaktivera automatisk rapportering om krasch</span><span class="sxs-lookup"><span data-stu-id="47864-177">Disable automatic crash reporting</span></span>
<span data-ttu-id="47864-178">Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement.</span><span class="sxs-lookup"><span data-stu-id="47864-178">You can disable hello automatic crash reporting feature of Engagement.</span></span> <span data-ttu-id="47864-179">Sedan, när ett ohanterat undantag inträffar, Engagement inte göra något.</span><span class="sxs-lookup"><span data-stu-id="47864-179">Then, when an unhandled exception will occur, Engagement won't do anything.</span></span>

> [!WARNING]
> <span data-ttu-id="47864-180">Om du planerar toodisable funktionen, Tänk på att när en ohanterad krasch sker i din app Engagement inte kommer att skicka hello krascher **och** stängs inte hello-session och jobb.</span><span class="sxs-lookup"><span data-stu-id="47864-180">If you plan toodisable this feature, be aware that when a unhandled crash will occur in your app, Engagement will not send hello crash **AND** it will not close hello session and jobs.</span></span>
> 
> 

<span data-ttu-id="47864-181">toodisable automatisk krascher rapportering, helt enkelt anpassa konfigurationen beroende på hello sätt som du har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="47864-181">toodisable automatic crash reporting, just customize your configuration depending on hello way you declared it :</span></span>

#### <a name="from-engagementconfigurationxml-file"></a><span data-ttu-id="47864-182">Från `EngagementConfiguration.xml` fil</span><span class="sxs-lookup"><span data-stu-id="47864-182">From `EngagementConfiguration.xml` file</span></span>
<span data-ttu-id="47864-183">Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.</span><span class="sxs-lookup"><span data-stu-id="47864-183">Set report crash too`false` between `<reportCrash>` and `</reportCrash>` tags.</span></span>

#### <a name="from-engagementconfiguration-object-at-run-time"></a><span data-ttu-id="47864-184">Från `EngagementConfiguration` objekt vid körning.</span><span class="sxs-lookup"><span data-stu-id="47864-184">From `EngagementConfiguration` object at run time</span></span>
<span data-ttu-id="47864-185">Ange rapporten krascher toofalse med EngagementConfiguration-objektet.</span><span class="sxs-lookup"><span data-stu-id="47864-185">Set report crash toofalse using your EngagementConfiguration object.</span></span>

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a><span data-ttu-id="47864-186">Burst-läge</span><span class="sxs-lookup"><span data-stu-id="47864-186">Burst mode</span></span>
<span data-ttu-id="47864-187">Som standard loggas hello Engagement service-rapporter i realtid.</span><span class="sxs-lookup"><span data-stu-id="47864-187">By default, hello Engagement service reports logs in real time.</span></span> <span data-ttu-id="47864-188">Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”).</span><span class="sxs-lookup"><span data-stu-id="47864-188">If your application reports logs very frequently, it is better toobuffer hello logs and tooreport them all at once on a regular time base (this is called hello “burst mode”).</span></span>

<span data-ttu-id="47864-189">toodo anropa så hello-metoden:</span><span class="sxs-lookup"><span data-stu-id="47864-189">toodo so, call hello method:</span></span>

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

<span data-ttu-id="47864-190">hello-argumentet är ett värde i **millisekunder**.</span><span class="sxs-lookup"><span data-stu-id="47864-190">hello argument is a value in **milliseconds**.</span></span> <span data-ttu-id="47864-191">När som helst om du vill tooreactivate hello realtid loggning kan bara anropa hello metod utan någon parameter eller med hello 0-värde.</span><span class="sxs-lookup"><span data-stu-id="47864-191">At any time, if you want tooreactivate hello real-time logging, just call hello method without any parameter, or with hello 0 value.</span></span>

<span data-ttu-id="47864-192">hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas).</span><span class="sxs-lookup"><span data-stu-id="47864-192">hello burst mode slightly increase hello battery life but has an impact on hello Engagement Monitor: all sessions and jobs duration will be rounded toohello burst threshold (thus, sessions and jobs shorter than hello burst threshold may not be visible).</span></span> <span data-ttu-id="47864-193">Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s).</span><span class="sxs-lookup"><span data-stu-id="47864-193">It is recommended toouse a burst threshold no longer than 30000 (30s).</span></span> <span data-ttu-id="47864-194">Du har toobe medveten sparade loggarna är begränsad too300 objekt.</span><span class="sxs-lookup"><span data-stu-id="47864-194">You have toobe aware that saved logs are limited too300 items.</span></span> <span data-ttu-id="47864-195">Om det är för lång tid att skicka förlora du vissa loggar.</span><span class="sxs-lookup"><span data-stu-id="47864-195">If sending is too long you can lose some logs.</span></span>

> [!WARNING]
> <span data-ttu-id="47864-196">hello burst tröskelvärdet kan inte konfigureras tooa perioden som är mindre än en sekund.</span><span class="sxs-lookup"><span data-stu-id="47864-196">hello burst threshold cannot be configured tooa period lesser than one second.</span></span> <span data-ttu-id="47864-197">Om du försöker toodo så hello SDK visas en spårning med hello fel och automatiskt återställer toohello standardvärdet, det vill säga noll sekunder.</span><span class="sxs-lookup"><span data-stu-id="47864-197">If you try toodo so, hello SDK will show a trace with hello error and will automatically reset toohello default value, that is, zero seconds.</span></span> <span data-ttu-id="47864-198">Detta utlöser hello SDK tooreport hello-loggar i realtid.</span><span class="sxs-lookup"><span data-stu-id="47864-198">This will trigger hello SDK tooreport hello logs in real-time.</span></span>
> 
> 

