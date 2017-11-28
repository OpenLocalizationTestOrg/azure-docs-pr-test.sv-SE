---
title: Windows Universal avancerade rapportering med MobileApps Engagement
description: Integrera Azure Mobile Engagement med universella Windows-appar
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
ms.openlocfilehash: feac309db1ffce0945012e293bfc1df417aed876
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="advanced-reporting-with-the-windows-universal-apps-engagement-sdk"></a><span data-ttu-id="f5f7b-103">Avancerade rapportering med Windows Universal-appar Engagement SDK</span><span class="sxs-lookup"><span data-stu-id="f5f7b-103">Advanced Reporting with the Windows Universal Apps Engagement SDK</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f5f7b-104">Universell Windows</span><span class="sxs-lookup"><span data-stu-id="f5f7b-104">Universal Windows</span></span>](mobile-engagement-windows-store-advanced-reporting.md)
> * [<span data-ttu-id="f5f7b-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="f5f7b-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="f5f7b-106">iOS</span><span class="sxs-lookup"><span data-stu-id="f5f7b-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="f5f7b-107">Android</span><span class="sxs-lookup"><span data-stu-id="f5f7b-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="f5f7b-108">Det här avsnittet beskrivs ytterligare rapporteringsscenarier i ditt universella Windows-program.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-108">This topic describes additional reporting scenarios in your Windows Universal application.</span></span> <span data-ttu-id="f5f7b-109">Dessa scenarier innehåller alternativ som du kan välja att tillämpa på appen som skapats i den [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-109">These scenarios include options that you can choose to apply to the app created in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5f7b-110">Krav</span><span class="sxs-lookup"><span data-stu-id="f5f7b-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

<span data-ttu-id="f5f7b-111">Innan du påbörjar de här självstudierna måste du först slutföra den [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) självstudier, vilket är avsiktligt direkt och enkel.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-111">Before starting this tutorial, you must first complete the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) tutorial, which is deliberately direct and simple.</span></span> <span data-ttu-id="f5f7b-112">Den här kursen ingår ytterligare alternativ som du kan välja bland.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-112">This tutorial covers additional options you can choose from.</span></span>

## <a name="specifying-engagement-configuration-at-runtime"></a><span data-ttu-id="f5f7b-113">Ange engagement konfiguration vid körning</span><span class="sxs-lookup"><span data-stu-id="f5f7b-113">Specifying engagement configuration at runtime</span></span>
<span data-ttu-id="f5f7b-114">Konfigurationen av Engagement centraliserad i den `Resources\EngagementConfiguration.xml` filen i projektet, vilket är där det har angetts i den [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-114">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project, which is where it was specified in the [Getting Started](mobile-engagement-windows-store-dotnet-get-started.md) topic.</span></span>

<span data-ttu-id="f5f7b-115">Men du kan också ange det vid körning: du kan anropa metoden följande innan Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-115">But you can also specify it at runtime: you can call the following method before the Engagement agent initialization:</span></span>

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set the Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a><span data-ttu-id="f5f7b-116">Rekommenderad metod: överlagra din `Page` klasser</span><span class="sxs-lookup"><span data-stu-id="f5f7b-116">Recommended method: overload your `Page` classes</span></span>
<span data-ttu-id="f5f7b-117">Om du vill aktivera rapportering av alla loggar som krävs av Engagement att beräkna användare, sessioner, aktiviteter, krascher och tekniska statistik, se alla dina `Page` underordnade klasser ärver från den `EngagementPage` klasser.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-117">To activate the reporting of all the logs required by Engagement to compute Users, Sessions, Activities, Crashes and Technical statistics, make all your `Page` sub-classes inherit from the `EngagementPage` classes.</span></span>

<span data-ttu-id="f5f7b-118">Här är ett exempel på en sida av ditt program.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-118">Here is an example for a page of your application.</span></span> <span data-ttu-id="f5f7b-119">Du kan göra samma sak för alla sidor i ditt program.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-119">You can do the same thing for all pages of your application.</span></span>

### <a name="c-source-file"></a><span data-ttu-id="f5f7b-120">C# källfilen</span><span class="sxs-lookup"><span data-stu-id="f5f7b-120">C# Source file</span></span>
<span data-ttu-id="f5f7b-121">Ändra sidan `.xaml.cs` fil:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-121">Modify your page `.xaml.cs` file:</span></span>

* <span data-ttu-id="f5f7b-122">Lägg till din `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-122">Add to your `using` statements:</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="f5f7b-123">Ersätt `Page` med `EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-123">Replace `Page` with `EngagementPage`:</span></span>

<span data-ttu-id="f5f7b-124">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f5f7b-124">**Without Engagement:**</span></span>

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

<span data-ttu-id="f5f7b-125">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f5f7b-125">**With Engagement:**</span></span>

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage
          {
            [...]
          }
        }

> [!IMPORTANT]
> <span data-ttu-id="f5f7b-126">Om sidan åsidosätter metoden `OnNavigatedTo` ska du anropa `base.OnNavigatedTo(e)`.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-126">If your page overrides the `OnNavigatedTo` method, be sure to call `base.OnNavigatedTo(e)`.</span></span> <span data-ttu-id="f5f7b-127">Aktiviteten är annars inte rapporteras (den `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).</span><span class="sxs-lookup"><span data-stu-id="f5f7b-127">Otherwise, the activity is not be reported (the `EngagementPage` calls `StartActivity` inside its `OnNavigatedTo` method).</span></span>
> 
> 

### <a name="xaml-file"></a><span data-ttu-id="f5f7b-128">XAML-fil</span><span class="sxs-lookup"><span data-stu-id="f5f7b-128">XAML file</span></span>
<span data-ttu-id="f5f7b-129">Ändra sidan `.xaml` fil:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-129">Modify your page `.xaml` file:</span></span>

* <span data-ttu-id="f5f7b-130">Lägg till följande i namnområdesdeklarationerna:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-130">Add to your namespaces declarations:</span></span>
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* <span data-ttu-id="f5f7b-131">Ersätt `Page` med `engagement:EngagementPage`:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-131">Replace `Page` with `engagement:EngagementPage`:</span></span>

<span data-ttu-id="f5f7b-132">**Utan Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f5f7b-132">**Without Engagement:**</span></span>

        <Page>
            <!-- layout -->
            ...
        </Page>

<span data-ttu-id="f5f7b-133">**Med Engagement:**</span><span class="sxs-lookup"><span data-stu-id="f5f7b-133">**With Engagement:**</span></span>

        <engagement:EngagementPage
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

### <a name="override-the-default-behaviour"></a><span data-ttu-id="f5f7b-134">Åsidosätta beteenden som standard</span><span class="sxs-lookup"><span data-stu-id="f5f7b-134">Override the default behaviour</span></span>
<span data-ttu-id="f5f7b-135">Klassnamnet för sidan har rapporterats som aktivitetsnamn med utan extra som standard.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-135">By default, the class name of the page is reported as the activity name, with no extra.</span></span> <span data-ttu-id="f5f7b-136">Om klassen använder suffixet ”Page”, Engagement tar bort den.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-136">If the class uses the "Page" suffix, Engagement removes it.</span></span>

<span data-ttu-id="f5f7b-137">Om du vill åsidosätta standardbeteendet för namnet, lägger du till den här koden:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-137">To override the default behavior for the name, add this code:</span></span>

        // in the .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

<span data-ttu-id="f5f7b-138">Rapportera extra information med dina aktiviteter, lägga till den här koden:</span><span class="sxs-lookup"><span data-stu-id="f5f7b-138">To report extra information with your activity, add this code:</span></span>

        // in the .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

<span data-ttu-id="f5f7b-139">De här metoderna anropas inifrån den `OnNavigatedTo` metoden på sidan.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-139">These methods are called from within the `OnNavigatedTo` method of your page.</span></span>

### <a name="alternate-method-call-startactivity-manually"></a><span data-ttu-id="f5f7b-140">Alternativ metod: anropa `StartActivity()` manuellt</span><span class="sxs-lookup"><span data-stu-id="f5f7b-140">Alternate method: call `StartActivity()` manually</span></span>
<span data-ttu-id="f5f7b-141">Om du inte kan eller inte vill överlagra din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-141">If you cannot or do not want to overload your `Page` classes, you can instead start your activities by calling `EngagementAgent` methods directly.</span></span>

<span data-ttu-id="f5f7b-142">Vi rekommenderar att anropa `StartActivity` i din `OnNavigatedTo` metod på sidan.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-142">We recommend calling `StartActivity` inside your `OnNavigatedTo` method of your Page.</span></span>

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> <span data-ttu-id="f5f7b-143">Se till att du kan avsluta sessionen på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-143">Ensure you end your session correctly.</span></span>
> 
> <span data-ttu-id="f5f7b-144">Windows Universal SDK automatiskt anropar den `EndActivity` metoden när programmet stängs.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-144">The Windows Universal SDK automatically calls the `EndActivity` method when the application is closed.</span></span> <span data-ttu-id="f5f7b-145">Därför är det **hög** rekommenderas att anropa den `StartActivity` metod när aktiviteten användaren ändrar och **aldrig** anropa den `EndActivity` metoden.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-145">Thus, it is **HIGHLY** recommended to call the `StartActivity` method whenever the activity of the user change, and to **NEVER** call the `EndActivity` method.</span></span> <span data-ttu-id="f5f7b-146">Den här metoden meddelar Engagement-servern att den aktuella användaren har lämnat programmet, vilket påverkar alla programloggar.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-146">This method notifies the Engagement server that the current user has left the application, which will impact all application logs.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="f5f7b-147">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="f5f7b-147">Advanced reporting</span></span>
<span data-ttu-id="f5f7b-148">Alternativt kan du kanske vill rapportera programspecifika händelser, fel och jobb för att göra det, kan du använda andra metoder hittades i den `EngagementAgent` klass.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-148">Optionally, you may want to report application-specific events, errors and jobs, to do so, use the others methods found in the `EngagementAgent` class.</span></span> <span data-ttu-id="f5f7b-149">Engagement API kan användningen av alla Engagement avancerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="f5f7b-149">The Engagement API allows use of all Engagement's advanced capabilities.</span></span>

<span data-ttu-id="f5f7b-150">Mer information finns i [hur du använder avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="f5f7b-150">For further information, see [How to use the advanced Mobile Engagement tagging API in your Windows Universal app](mobile-engagement-windows-store-use-engagement-api.md).</span></span>

