---
title: Windows Phone Silverlight SDK Uppgraderingsprocesser
description: "Windows Phone Silverlight SDK Uppgraderingsprocesser för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 87130026-9759-4659-9184-788a3627a165
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f87f65788075c7f4067e77946e1bcbc8f3709317
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="e03f4-103">Windows Phone Silverlight SDK Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="e03f4-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="e03f4-104">Om du redan har integrerat en äldre version av våra SDK i programmet, måste du Tänk på följande när du uppgraderar SDK.</span><span class="sxs-lookup"><span data-stu-id="e03f4-104">If you already have integrated an older version of our SDK into your application, you have to consider the following points when upgrading the SDK.</span></span>

<span data-ttu-id="e03f4-105">Du kan behöva utföra flera procedurer om du har missat flera versioner av SDK.</span><span class="sxs-lookup"><span data-stu-id="e03f4-105">You may have to follow several procedures if you missed several versions of the SDK.</span></span> <span data-ttu-id="e03f4-106">Till exempel om du migrerar från 0.10.1 till 0.11.0 måste du först Följ proceduren ”från 0.9.0 till 0.10.1” sedan proceduren ”från 0.10.1 till 0.11.0”.</span><span class="sxs-lookup"><span data-stu-id="e03f4-106">For example if you migrate from 0.10.1 to 0.11.0 you have to first follow the "from 0.9.0 to 0.10.1" procedure then the "from 0.10.1 to 0.11.0" procedure.</span></span>

## <a name="from-200-to-330"></a><span data-ttu-id="e03f4-107">Från 2.0.0 till 3.3.0</span><span class="sxs-lookup"><span data-stu-id="e03f4-107">From 2.0.0 to 3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="e03f4-108">Testa loggar</span><span class="sxs-lookup"><span data-stu-id="e03f4-108">Test logs</span></span>
<span data-ttu-id="e03f4-109">Konsolen loggar som genereras av SDK kan nu vara aktiverat/inaktiverat/filtreras.</span><span class="sxs-lookup"><span data-stu-id="e03f4-109">Console logs produced by the SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="e03f4-110">Anpassa detta genom att uppdatera egenskapen `EngagementAgent.Instance.TestLogEnabled` till ett värde som är tillgängliga från den `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="e03f4-110">To customize this, update the property `EngagementAgent.Instance.TestLogEnabled` to one of the value available from the `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-to-200"></a><span data-ttu-id="e03f4-111">Från 1.1.1 till 2.0.0</span><span class="sxs-lookup"><span data-stu-id="e03f4-111">From 1.1.1 to 2.0.0</span></span>
<span data-ttu-id="e03f4-112">Nedan beskrivs hur du migrerar en SDK-integration från tjänsten Capptain som erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="e03f4-112">The following describes how to migrate an SDK integration from the Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="e03f4-113">Capptain och Mobile Engagement är inte samma tjänster och proceduren nedan visar hur du migrerar klientappen endast.</span><span class="sxs-lookup"><span data-stu-id="e03f4-113">Capptain and Mobile Engagement are not the same services and the procedure given below only highlights how to migrate the client app.</span></span> <span data-ttu-id="e03f4-114">Migrera SDK i appen kommer inte migrera dina data från servrar som Capptain till Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="e03f4-114">Migrating the SDK in the app will NOT migrate your data from the Capptain servers to the Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="e03f4-115">Kontakta Capptain webbplats om du vill migrera till 1.1.1 först och sedan använda följande procedur om du migrerar från en tidigare version</span><span class="sxs-lookup"><span data-stu-id="e03f4-115">If you are migrating from an earlier version, please consult the Capptain web site to migrate to 1.1.1 first then apply the following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="e03f4-116">Nuget-paketet</span><span class="sxs-lookup"><span data-stu-id="e03f4-116">Nuget package</span></span>
<span data-ttu-id="e03f4-117">Ersätt **Capptain.WindowsPhone** av **MicrosoftAzure.MobileEngagement** Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="e03f4-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="e03f4-118">Tillämpa Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="e03f4-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="e03f4-119">SDK använder termen `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="e03f4-119">The SDK uses the term `Engagement`.</span></span> <span data-ttu-id="e03f4-120">Du behöver uppdatera ditt projekt för att matcha den här ändringen.</span><span class="sxs-lookup"><span data-stu-id="e03f4-120">You need to update your project to match this change.</span></span>

<span data-ttu-id="e03f4-121">Du måste avinstallera det aktuella Capptain nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="e03f4-121">You need to uninstall your current Capptain nuget package.</span></span> <span data-ttu-id="e03f4-122">Överväg att alla dina ändringar i mappen Capptain resurser tas bort.</span><span class="sxs-lookup"><span data-stu-id="e03f4-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="e03f4-123">Om du vill behålla dessa filer och sedan göra en kopia av dem.</span><span class="sxs-lookup"><span data-stu-id="e03f4-123">If you want to keep those files then make a copy of them.</span></span>

<span data-ttu-id="e03f4-124">Efter det att installera nya Microsoft Azure Engagement nuget-paketet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="e03f4-124">After that, install the new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="e03f4-125">Du kan hitta direkt på [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="e03f4-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="e03f4-126">Den här åtgärden ersätter alla filer för resurser som används av Engagement och lägger till nya Engagement DLL projektreferenserna.</span><span class="sxs-lookup"><span data-stu-id="e03f4-126">This action replaces all resources files used by Engagement and adds the new Engagement DLL to your project References.</span></span>

<span data-ttu-id="e03f4-127">Du måste rensa projektreferenserna genom att ta bort Capptain DLL-referenser.</span><span class="sxs-lookup"><span data-stu-id="e03f4-127">You have to clean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="e03f4-128">Om du inte göra detta, versionen av Capptain kommer i konflikt och fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="e03f4-128">If you do not make this, the version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="e03f4-129">Om du har anpassat Capptain resurser, kopiera ditt gamla filer innehåll och klistra in dem i de nya Engagement-filerna.</span><span class="sxs-lookup"><span data-stu-id="e03f4-129">If you have customized Capptain resources, copy your old files content and paste them in the new Engagement files.</span></span> <span data-ttu-id="e03f4-130">Observera att xaml- och cs-filer som behöver uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e03f4-130">Please note that both xaml and cs files have to be updated.</span></span>

<span data-ttu-id="e03f4-131">När de steg behöver du bara ersätta gamla Capptain referenser från de nya Engagement-referenserna.</span><span class="sxs-lookup"><span data-stu-id="e03f4-131">When those steps are completed you only have to replace old Capptain references by the new Engagement references.</span></span>

1. <span data-ttu-id="e03f4-132">Alla Capptain namnområden måste uppdateras.</span><span class="sxs-lookup"><span data-stu-id="e03f4-132">All Capptain namespaces have to be updated.</span></span>
   
    <span data-ttu-id="e03f4-133">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e03f4-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="e03f4-134">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e03f4-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="e03f4-135">Alla Capptain klasser som innehåller ”Capptain” ska innehålla ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="e03f4-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="e03f4-136">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e03f4-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="e03f4-137">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e03f4-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="e03f4-138">För xaml-filer Capptain namnområde och attribut även ändras.</span><span class="sxs-lookup"><span data-stu-id="e03f4-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="e03f4-139">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="e03f4-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="e03f4-140">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="e03f4-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="e03f4-141">Andra resurser som Capptain bilder, Observera att de även har döpts om du vill använda ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="e03f4-141">For the other resources like Capptain pictures, please note that they also have been renamed to use "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="e03f4-142">Program-ID / SDK-nyckeln</span><span class="sxs-lookup"><span data-stu-id="e03f4-142">Application ID / SDK Key</span></span>
<span data-ttu-id="e03f4-143">Engagement använder en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e03f4-143">Engagement uses a connection string.</span></span> <span data-ttu-id="e03f4-144">Du inte behöver ange ett program-ID och SDK-nyckeln med Mobile Engagement, behöver du bara ange en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="e03f4-144">You don't have to specify an application ID and an SDK key with Mobile Engagement, you only have to specify a connection string.</span></span> <span data-ttu-id="e03f4-145">Du kan konfigurera det på EngagementConfiguration-fil.</span><span class="sxs-lookup"><span data-stu-id="e03f4-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="e03f4-146">Konfigurationen av Engagement kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="e03f4-146">The Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="e03f4-147">Redigera den här filen för att ange:</span><span class="sxs-lookup"><span data-stu-id="e03f4-147">Edit this file to specify:</span></span>

* <span data-ttu-id="e03f4-148">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="e03f4-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="e03f4-149">Om du vill ange vid körning i stället kan du anropa metoden följande innan Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="e03f4-149">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="e03f4-150">Anslutningssträngen för ditt program visas i den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e03f4-150">The connection string for your application is displayed in the Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="e03f4-151">Ändring av objekt</span><span class="sxs-lookup"><span data-stu-id="e03f4-151">Items name change</span></span>
<span data-ttu-id="e03f4-152">Alla objekt med namnet *capptain* namngetts *engagement*.</span><span class="sxs-lookup"><span data-stu-id="e03f4-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="e03f4-153">På liknande sätt för *Capptain* till *Engagement*.</span><span class="sxs-lookup"><span data-stu-id="e03f4-153">Similarly for *Capptain* to *Engagement*.</span></span>

<span data-ttu-id="e03f4-154">Exempel på vanliga Capptain objekt:</span><span class="sxs-lookup"><span data-stu-id="e03f4-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="e03f4-155">CapptainConfiguration nu namnet EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="e03f4-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="e03f4-156">CapptainAgent nu namnet EngagementAgent</span><span class="sxs-lookup"><span data-stu-id="e03f4-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="e03f4-157">CapptainReach nu namnet EngagementReach</span><span class="sxs-lookup"><span data-stu-id="e03f4-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="e03f4-158">CapptainHttpConfig nu namnet EngagementHttpConfig</span><span class="sxs-lookup"><span data-stu-id="e03f4-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="e03f4-159">GetCapptainPageName nu namnet GetEngagementPageName</span><span class="sxs-lookup"><span data-stu-id="e03f4-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="e03f4-160">Observera att byta namn på också påverkar åsidosätts metoder.</span><span class="sxs-lookup"><span data-stu-id="e03f4-160">Note that rename also affects overridden methods.</span></span>

