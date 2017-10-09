---
title: aaaWindows Universal avancerad rapportering med MobileApps Engagement
description: Hur tooIntegrate Azure Mobile Engagement med universella Windows-appar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: ea5030bf-73ac-49b7-bc3e-c25fc10e945a
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="e006c-103">Avancerade rapportering med hello Windows Universal-appar Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="e006c-103">Advanced Reporting with hello Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e006c-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="e006c-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="e006c-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="e006c-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="e006c-106">iOS</span><span class="sxs-lookup"><span data-stu-id="e006c-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="e006c-107">Android</span><span class="sxs-lookup"><span data-stu-id="e006c-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="e006c-108">Det här avsnittet beskrivs ytterligare rapporteringsscenarier i ditt universella Windows-program.</span><span class="sxs-lookup"><span data-stu-id="e006c-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="e006c-109">Dessa scenarier innehåller alternativ som du kan välja tooapply toohello app som skapats i hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="e006c-109">These scenarios include options that you can choose tooapply toohello app created in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e006c-110">Krav</span><span class="sxs-lookup"><span data-stu-id="e006c-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="e006c-111">Innan du påbörjar de här självstudierna måste du först slutföra hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) självstudier, vilket är avsiktligt direkt och enkel.</span><span class="sxs-lookup"><span data-stu-id="e006c-111">Before starting this tutorial, you must first complete hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="e006c-112">Den här kursen ingår ytterligare alternativ som du kan välja bland.</span><span class="sxs-lookup"><span data-stu-id="e006c-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="e006c-113">Ange engagement konfiguration vid körning</span><span class="sxs-lookup"><span data-stu-id="e006c-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="e006c-114">Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet, vilket är där det har angetts i hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e006c-114">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in hello [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="e006c-115">Men du kan också ange det vid körning: du kan anropa hello följa metoden innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="e006c-115">But you can also specify it at runtime: you can call hello following method before hello Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="e006c-116">Rekommenderad metod: överlagra din `Page` klasser</span><span class="sxs-lookup"><span data-stu-id="e006c-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="e006c-117">tooactivate hello rapportering av alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, se alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klasser.</span><span class="sxs-lookup"><span data-stu-id="e006c-117">tooactivate hello reporting of all hello logs required by Engagement toocompute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from hello `EngagementPage` classes.</span></span>

<span data-ttu-id="e006c-118">Här är ett exempel på en sida av ditt program.</span><span class="sxs-lookup"><span data-stu-id="e006c-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="e006c-119">Du kan göra hello detsamma för alla sidor i ditt program.</span><span class="sxs-lookup"><span data-stu-id="e006c-119">You can do hello same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="e006c-120">C# källfilen</span><span class="sxs-lookup"><span data-stu-id="e006c-120">C# Source file</span></span>
<span data-ttu-id="e006c-121">Ändra sidan `.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="e006c-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="e006c-122">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="e006c-122">Add tooyour `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="e006c-123">Ersätt `Page` med `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="e006c-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="e006c-124">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e006c-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="e006c-125">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e006c-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="e006c-126">Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="e006c-126">If your page overrides hello `OnNavigatedTo` method, be sure toocall `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="e006c-127">Annars hello aktivitet inte är rapporteras (hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).</span><span class="sxs-lookup"><span data-stu-id="e006c-127">Otherwise, hello activity is not be reported (hello `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="e006c-128">XAML-fil</span><span class="sxs-lookup"><span data-stu-id="e006c-128">XAML file</span></span>
<span data-ttu-id="e006c-129">Ändra sidan `.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="e006c-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="e006c-130">Lägg till tooyour namnområden deklarationer:</span><span class="sxs-lookup"><span data-stu-id="e006c-130">Add tooyour namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="e006c-131">Ersätt `Page` med `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="e006c-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="e006c-132">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e006c-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="e006c-133">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="e006c-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-hello-default-behaviour"></a><span data-ttu-id="e006c-134">Åsidosätt hello standard beteende</span><span class="sxs-lookup"><span data-stu-id="e006c-134">Override hello default behaviour</span></span>
<span data-ttu-id="e006c-135">Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra.</span><span class="sxs-lookup"><span data-stu-id="e006c-135">By default, hello class name of hello page is reported as hello activity name, with no extra.</span></span> <span data-ttu-id="e006c-136">Om hello klassen använder hello ”Page” suffix, Engagement tar bort den.</span><span class="sxs-lookup"><span data-stu-id="e006c-136">If hello class uses hello "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="e006c-137">toooverride hello standardbeteendet för hello namn, Lägg till den här koden:</span><span class="sxs-lookup"><span data-stu-id="e006c-137">toooverride hello default behavior for hello name, add this code:</span></span>

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="e006c-138">tooreport extra information med din aktivitet, lägga till den här koden:</span><span class="sxs-lookup"><span data-stu-id="e006c-138">tooreport extra information with your activity, add this code:</span></span>

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="e006c-139">De här metoderna anropas från inom hello `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="e006c-139">These methods are called from within hello `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="e006c-140">Alternativ metod: anropa `StartActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="e006c-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="e006c-141">Om du inte kan eller inte vill att toooverload din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="e006c-141">If you cannot or do not want toooverload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="e006c-142">Vi rekommenderar att anropa `StartActivity` i din `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="e006c-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="e006c-143">Se till att du kan avsluta sessionen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="e006c-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="e006c-144">hello Windows Universal SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="e006c-144">hello Windows Universal SDK automatically calls hello `EndActivity` method when hello application is closed.</span></span> <span data-ttu-id="e006c-145">Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` metod.</span><span class="sxs-lookup"><span data-stu-id="e006c-145">Thus, it is **HIGHLY** recommended toocall hello `StartActivity` method whenever hello activity of hello user change, and too**NEVER** call hello `EndActivity` method.</span></span> <span data-ttu-id="e006c-146">Den här metoden meddelar hello Engagement server att hello den aktuella användaren har lämnat hello programmet, vilket påverkar alla programloggar.</span><span class="sxs-lookup"><span data-stu-id="e006c-146">This method notifies hello Engagement server that hello current user has left hello application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="e006c-147">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="e006c-147">Advanced reporting</span></span>
<span data-ttu-id="e006c-148">Alternativt kan du tooreport programspecifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="e006c-148">Optionally, you may want tooreport application-specific events, errors and jobs, toodo so, use hello others methods found in hello `EngagementAgent` class.</span></span> <span data-ttu-id="e006c-149">Hej Engagement API kan användas av alla Engagement avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="e006c-149">hello Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="e006c-150">Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="e006c-150">For further information, see [How toouse hello advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

