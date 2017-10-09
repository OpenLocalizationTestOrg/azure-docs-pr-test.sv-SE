---
title: aaaWindows Phone Silverlight SDK uppgradera procedurer
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
ms.openlocfilehash: d72e7b8a59ef2c0a95b22efbf1e5257271399ddc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a><span data-ttu-id="3aa12-103">Windows Phone Silverlight SDK Uppgraderingsprocesser</span><span class="sxs-lookup"><span data-stu-id="3aa12-103">Windows Phone Silverlight SDK Upgrade Procedures</span></span>
<span data-ttu-id="3aa12-104">Om du redan har integrerat en äldre version av våra SDK i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.</span><span class="sxs-lookup"><span data-stu-id="3aa12-104">If you already have integrated an older version of our SDK into your application, you have tooconsider hello following points when upgrading hello SDK.</span></span>

<span data-ttu-id="3aa12-105">Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK.</span><span class="sxs-lookup"><span data-stu-id="3aa12-105">You may have toofollow several procedures if you missed several versions of hello SDK.</span></span> <span data-ttu-id="3aa12-106">Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.</span><span class="sxs-lookup"><span data-stu-id="3aa12-106">For example if you migrate from 0.10.1 too0.11.0 you have toofirst follow hello "from 0.9.0 too0.10.1" procedure then hello "from 0.10.1 too0.11.0" procedure.</span></span>

## <a name="from-200-too330"></a><span data-ttu-id="3aa12-107">Från 2.0.0 too3.3.0</span><span class="sxs-lookup"><span data-stu-id="3aa12-107">From 2.0.0 too3.3.0</span></span>
### <a name="test-logs"></a><span data-ttu-id="3aa12-108">Testa loggar</span><span class="sxs-lookup"><span data-stu-id="3aa12-108">Test logs</span></span>
<span data-ttu-id="3aa12-109">Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras.</span><span class="sxs-lookup"><span data-stu-id="3aa12-109">Console logs produced by hello SDK can now be enabled/disabled/filtered.</span></span> <span data-ttu-id="3aa12-110">toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:</span><span class="sxs-lookup"><span data-stu-id="3aa12-110">toocustomize this, update hello property `EngagementAgent.Instance.TestLogEnabled` tooone of hello value available from hello `EngagementTestLogLevel` enumeration, for instance:</span></span>

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

## <a name="from-111-too200"></a><span data-ttu-id="3aa12-111">Från 1.1.1 too2.0.0</span><span class="sxs-lookup"><span data-stu-id="3aa12-111">From 1.1.1 too2.0.0</span></span>
<span data-ttu-id="3aa12-112">hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="3aa12-112">hello following describes how toomigrate an SDK integration from hello Capptain service offered by Capptain SAS into an app powered by Azure Mobile Engagement.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3aa12-113">Capptain och Mobile Engagement hello inte samma tjänster och hello proceduren endast anges nedan visar hur toomigrate hello-klientappen.</span><span class="sxs-lookup"><span data-stu-id="3aa12-113">Capptain and Mobile Engagement are not hello same services and hello procedure given below only highlights how toomigrate hello client app.</span></span> <span data-ttu-id="3aa12-114">Migrera hello SDK i hello app kommer inte att migrera data från hello Capptain servrar toohello Mobile Engagement-servrar</span><span class="sxs-lookup"><span data-stu-id="3aa12-114">Migrating hello SDK in hello app will NOT migrate your data from hello Capptain servers toohello Mobile Engagement servers</span></span>
> 
> 

<span data-ttu-id="3aa12-115">Om du migrerar från en tidigare version kan du kontakta hello Capptain webbplats toomigrate too1.1.1 först och sedan använda hello sätt</span><span class="sxs-lookup"><span data-stu-id="3aa12-115">If you are migrating from an earlier version, please consult hello Capptain web site toomigrate too1.1.1 first then apply hello following procedure</span></span>

### <a name="nuget-package"></a><span data-ttu-id="3aa12-116">Nuget-paketet</span><span class="sxs-lookup"><span data-stu-id="3aa12-116">Nuget package</span></span>
<span data-ttu-id="3aa12-117">Ersätt **Capptain.WindowsPhone** av **MicrosoftAzure.MobileEngagement** Nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="3aa12-117">Replace **Capptain.WindowsPhone** by **MicrosoftAzure.MobileEngagement** Nuget package.</span></span>

### <a name="applying-mobile-engagement"></a><span data-ttu-id="3aa12-118">Tillämpa Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="3aa12-118">Applying Mobile Engagement</span></span>
<span data-ttu-id="3aa12-119">hello SDK använder hello termen `Engagement`.</span><span class="sxs-lookup"><span data-stu-id="3aa12-119">hello SDK uses hello term `Engagement`.</span></span> <span data-ttu-id="3aa12-120">Du behöver tooupdate projekt-toomatch ändringen.</span><span class="sxs-lookup"><span data-stu-id="3aa12-120">You need tooupdate your project toomatch this change.</span></span>

<span data-ttu-id="3aa12-121">Du måste toouninstall aktuella Capptain nuget-paketet.</span><span class="sxs-lookup"><span data-stu-id="3aa12-121">You need toouninstall your current Capptain nuget package.</span></span> <span data-ttu-id="3aa12-122">Överväg att alla dina ändringar i mappen Capptain resurser tas bort.</span><span class="sxs-lookup"><span data-stu-id="3aa12-122">Consider that all your changes in Capptain Resources folder will be removed.</span></span> <span data-ttu-id="3aa12-123">Om du vill tookeep göra en kopia av dem sedan i dessa filer.</span><span class="sxs-lookup"><span data-stu-id="3aa12-123">If you want tookeep those files then make a copy of them.</span></span>

<span data-ttu-id="3aa12-124">Efter det att installera hello nya Microsoft Azure Engagement nuget-paketet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="3aa12-124">After that, install hello new Microsoft Azure Engagement nuget package on your project.</span></span> <span data-ttu-id="3aa12-125">Du kan hitta direkt på [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span><span class="sxs-lookup"><span data-stu-id="3aa12-125">You can find it directly on [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement).</span></span> <span data-ttu-id="3aa12-126">Den här åtgärden ersätter alla resurser filer som används av Engagement och lägger till hello nya Engagement DLL tooyour projektet refererar till.</span><span class="sxs-lookup"><span data-stu-id="3aa12-126">This action replaces all resources files used by Engagement and adds hello new Engagement DLL tooyour project References.</span></span>

<span data-ttu-id="3aa12-127">Du har tooclean projektreferenserna genom att ta bort Capptain DLL-referenser.</span><span class="sxs-lookup"><span data-stu-id="3aa12-127">You have tooclean your project references by deleting Capptain DLL references.</span></span> <span data-ttu-id="3aa12-128">Om du inte gör detta hello version av Capptain kommer i konflikt och fel händer.</span><span class="sxs-lookup"><span data-stu-id="3aa12-128">If you do not make this, hello version of Capptain will conflict and errors will happen.</span></span>

<span data-ttu-id="3aa12-129">Om du har anpassat Capptain resurser, kopiera ditt gamla filer innehåll och klistra in dem i hello nya Engagement filer.</span><span class="sxs-lookup"><span data-stu-id="3aa12-129">If you have customized Capptain resources, copy your old files content and paste them in hello new Engagement files.</span></span> <span data-ttu-id="3aa12-130">Observera att både xaml och cs-filer har toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="3aa12-130">Please note that both xaml and cs files have toobe updated.</span></span>

<span data-ttu-id="3aa12-131">När de steg har du bara tooreplace gamla Capptain referenser från hello nya Engagement referenser.</span><span class="sxs-lookup"><span data-stu-id="3aa12-131">When those steps are completed you only have tooreplace old Capptain references by hello new Engagement references.</span></span>

1. <span data-ttu-id="3aa12-132">Alla Capptain namnområden har toobe uppdateras.</span><span class="sxs-lookup"><span data-stu-id="3aa12-132">All Capptain namespaces have toobe updated.</span></span>
   
    <span data-ttu-id="3aa12-133">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="3aa12-133">Before migration:</span></span>
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    <span data-ttu-id="3aa12-134">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="3aa12-134">After migration:</span></span>
   
        using Microsoft.Azure.Engagement;
2. <span data-ttu-id="3aa12-135">Alla Capptain klasser som innehåller ”Capptain” ska innehålla ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="3aa12-135">All Capptain classes that contain "Capptain" should contain "Engagement".</span></span>
   
    <span data-ttu-id="3aa12-136">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="3aa12-136">Before migration:</span></span>
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    <span data-ttu-id="3aa12-137">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="3aa12-137">After migration:</span></span>
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. <span data-ttu-id="3aa12-138">För xaml-filer Capptain namnområde och attribut även ändras.</span><span class="sxs-lookup"><span data-stu-id="3aa12-138">For xaml files Capptain namespace and attributes also change.</span></span>
   
    <span data-ttu-id="3aa12-139">Före migrering:</span><span class="sxs-lookup"><span data-stu-id="3aa12-139">Before migration:</span></span>
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    <span data-ttu-id="3aa12-140">Efter migreringen:</span><span class="sxs-lookup"><span data-stu-id="3aa12-140">After migration:</span></span>
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. <span data-ttu-id="3aa12-141">För hello märke andra resurser som Capptain bilder, Lägg till att de också har bytt namn till toouse ”Engagement”.</span><span class="sxs-lookup"><span data-stu-id="3aa12-141">For hello other resources like Capptain pictures, please note that they also have been renamed toouse "Engagement".</span></span>

### <a name="application-id--sdk-key"></a><span data-ttu-id="3aa12-142">Program-ID / SDK-nyckeln</span><span class="sxs-lookup"><span data-stu-id="3aa12-142">Application ID / SDK Key</span></span>
<span data-ttu-id="3aa12-143">Engagement använder en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="3aa12-143">Engagement uses a connection string.</span></span> <span data-ttu-id="3aa12-144">Du saknar toospecify ett program-ID och SDK-nyckeln med Mobile Engagement har endast toospecify en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="3aa12-144">You don't have toospecify an application ID and an SDK key with Mobile Engagement, you only have toospecify a connection string.</span></span> <span data-ttu-id="3aa12-145">Du kan konfigurera det på EngagementConfiguration-fil.</span><span class="sxs-lookup"><span data-stu-id="3aa12-145">You can set it up on your EngagementConfiguration file.</span></span>

<span data-ttu-id="3aa12-146">Hej Engagement konfiguration kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="3aa12-146">hello Engagement configuration can be set in your `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="3aa12-147">Redigera den här filen toospecify:</span><span class="sxs-lookup"><span data-stu-id="3aa12-147">Edit this file toospecify:</span></span>

* <span data-ttu-id="3aa12-148">Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.</span><span class="sxs-lookup"><span data-stu-id="3aa12-148">Your application connection string between tags `<connectionString>` and `<\connectionString>`.</span></span>

<span data-ttu-id="3aa12-149">Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="3aa12-149">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization:</span></span>

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

<span data-ttu-id="3aa12-150">hello anslutningssträngen för ditt program visas i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3aa12-150">hello connection string for your application is displayed in hello Azure Classic Portal.</span></span>

### <a name="items-name-change"></a><span data-ttu-id="3aa12-151">Ändring av objekt</span><span class="sxs-lookup"><span data-stu-id="3aa12-151">Items name change</span></span>
<span data-ttu-id="3aa12-152">Alla objekt med namnet *capptain* namngetts *engagement*.</span><span class="sxs-lookup"><span data-stu-id="3aa12-152">All items named *capptain* have been named *engagement*.</span></span> <span data-ttu-id="3aa12-153">På liknande sätt för *Capptain* för*Engagement*.</span><span class="sxs-lookup"><span data-stu-id="3aa12-153">Similarly for *Capptain* too*Engagement*.</span></span>

<span data-ttu-id="3aa12-154">Exempel på vanliga Capptain objekt:</span><span class="sxs-lookup"><span data-stu-id="3aa12-154">Examples of commonly used Capptain items :</span></span>

* <span data-ttu-id="3aa12-155">CapptainConfiguration nu namnet EngagementConfiguration</span><span class="sxs-lookup"><span data-stu-id="3aa12-155">CapptainConfiguration now named EngagementConfiguration</span></span>
* <span data-ttu-id="3aa12-156">CapptainAgent nu namnet EngagementAgent</span><span class="sxs-lookup"><span data-stu-id="3aa12-156">CapptainAgent now named EngagementAgent</span></span>
* <span data-ttu-id="3aa12-157">CapptainReach nu namnet EngagementReach</span><span class="sxs-lookup"><span data-stu-id="3aa12-157">CapptainReach now named EngagementReach</span></span>
* <span data-ttu-id="3aa12-158">CapptainHttpConfig nu namnet EngagementHttpConfig</span><span class="sxs-lookup"><span data-stu-id="3aa12-158">CapptainHttpConfig now named EngagementHttpConfig</span></span>
* <span data-ttu-id="3aa12-159">GetCapptainPageName nu namnet GetEngagementPageName</span><span class="sxs-lookup"><span data-stu-id="3aa12-159">GetCapptainPageName now named GetEngagementPageName</span></span>

<span data-ttu-id="3aa12-160">Observera att byta namn på också påverkar åsidosätts metoder.</span><span class="sxs-lookup"><span data-stu-id="3aa12-160">Note that rename also affects overridden methods.</span></span>

