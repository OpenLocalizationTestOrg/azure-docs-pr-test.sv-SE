---
title: "aaaWindows Phone Silverlight nå SDK-Integration"
description: "Hur tooIntegrate Azure Mobile Engagement nå med Windows Phone Silverlight-appar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: d3516a6b-db9f-4cdb-a475-4148edf81af1
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 09c8767216e11963c5c600755ab8d4d11cd92034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="2cbc6-103">Windows Phone Silverlight Reach SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="2cbc6-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="2cbc6-104">Du måste följa hello integration beskrivs i hello [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-104">You must follow hello integration procedure described in hello [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="2cbc6-105">Bädda in hello Engagement nå SDK i Windows Phone Silverlight-projekt</span><span class="sxs-lookup"><span data-stu-id="2cbc6-105">Embed hello Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="2cbc6-106">Du har inte något tooadd.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-106">You do not have anything tooadd.</span></span> <span data-ttu-id="2cbc6-107">`EngagementReach`referenser och resurser finns redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="2cbc6-108">Du kan anpassa avbildningar som finns i hello `Resources` mapp i ditt projekt, särskilt hello varumärken-ikonen (som standard toohello Engagement ikon).</span><span class="sxs-lookup"><span data-stu-id="2cbc6-108">You can customize images located in hello `Resources` folder of your project, especially hello brand icon (that default toohello Engagement icon).</span></span>
> 
> 

## <a name="add-hello-capabilities"></a><span data-ttu-id="2cbc6-109">Lägger till hello-funktioner</span><span class="sxs-lookup"><span data-stu-id="2cbc6-109">Add hello capabilities</span></span>
<span data-ttu-id="2cbc6-110">Hej Engagement nå SDK måste vissa ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-110">hello Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="2cbc6-111">Öppna din `WMAppManifest.xml` fil- och måste deklareras som hello följande funktioner:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-111">Open your `WMAppManifest.xml` file and be sure that hello following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="2cbc6-112">hello först används en av hello MPNS service tooallow hello visningen av popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-112">hello first one is used by hello MPNS service tooallow hello display of toast notification.</span></span> <span data-ttu-id="2cbc6-113">hello är andra tooembed används en webbläsare aktivitet i hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-113">hello second one is used tooembed a browser task into hello SDK.</span></span>

<span data-ttu-id="2cbc6-114">Redigera hello `WMAppManifest.xml` och Lägg till i hello `<Capabilities />` tagg:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-114">Edit hello `WMAppManifest.xml` file and add inside hello `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a><span data-ttu-id="2cbc6-115">Aktivera hello Microsoft Push Notification Service</span><span class="sxs-lookup"><span data-stu-id="2cbc6-115">Enable hello Microsoft Push Notification Service</span></span>
<span data-ttu-id="2cbc6-116">I ordning toouse hello **Microsoft Push Notification Service** (kallas MPNS) din `WMAppManifest.xml` filen måste ha en `<App />` taggen med en `Publisher` -attributet inställt toohello namnet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-116">In order toouse hello **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set toohello name of your project.</span></span>

## <a name="initialize-hello-engagement-reach-sdk"></a><span data-ttu-id="2cbc6-117">Initiera hello Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="2cbc6-117">Initialize hello Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="2cbc6-118">Engagement konfiguration</span><span class="sxs-lookup"><span data-stu-id="2cbc6-118">Engagement configuration</span></span>
<span data-ttu-id="2cbc6-119">Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-119">hello Engagement configuration is centralized in hello `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="2cbc6-120">Redigera filen toospecify reach konfigurationen:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-120">Edit this file toospecify reach configuration :</span></span>

* <span data-ttu-id="2cbc6-121">*Valfria*, ange om hello native push (MPNS) är aktiverad eller inte mellan `<enableNativePush>` och `</enableNativePush>` taggar (`true` som standard).</span><span class="sxs-lookup"><span data-stu-id="2cbc6-121">*Optional*, indicate whether hello native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="2cbc6-122">*Valfria*, ange hello namnet på hello push kanal mellan `<channelName>` och `</channelName>` taggar, ange hello samma att ditt program kan använda eller lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-122">*Optional*, indicate hello name of hello push channel between `<channelName>` and `</channelName>` tags, provide hello same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="2cbc6-123">Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-123">If you want toospecify it at runtime instead, you can call hello following method before hello Engagement agent initialization :</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether hello native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide hello same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> <span data-ttu-id="2cbc6-124">Du kan ange hello namnet på hello MPNS push kanal för ditt program.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-124">You can specify hello name of hello MPNS push channel of your application.</span></span> <span data-ttu-id="2cbc6-125">Som standard skapar Engagement ett namn baserat på hello appId.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-125">By default, Engagement creates a name based on hello appId.</span></span> <span data-ttu-id="2cbc6-126">Du har inget behov av toospecify hello namn själv, utom om du planerar toouse hello push kanal utanför Engagement.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-126">You have no need toospecify hello name yourself, except if you plan toouse hello push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="2cbc6-127">Initieringen av engagement</span><span class="sxs-lookup"><span data-stu-id="2cbc6-127">Engagement initialization</span></span>
<span data-ttu-id="2cbc6-128">Ändra hello `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-128">Modify hello `App.xaml.cs`:</span></span>

* <span data-ttu-id="2cbc6-129">Lägg till tooyour `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-129">Add tooyour `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="2cbc6-130">Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="2cbc6-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="2cbc6-131">Infoga `EngagementReach.Instance.OnActivated` i hello `Application_Activated` metoden:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-131">Insert `EngagementReach.Instance.OnActivated` in hello `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="2cbc6-132">Hej `EngagementReach.Instance.Init` körs i en dedikerad tråd.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-132">hello `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="2cbc6-133">Du har inte toodo den själv.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-133">You do not have toodo it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="2cbc6-134">App store skicka överväganden</span><span class="sxs-lookup"><span data-stu-id="2cbc6-134">App store submission considerations</span></span>
<span data-ttu-id="2cbc6-135">Microsoft medför vissa regler när du använder hello push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-135">Microsoft imposes some rules when using hello push notifications:</span></span>

<span data-ttu-id="2cbc6-136">Från hello Microsoft [användningsprinciper] dokumentation, avsnittet 2.9:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-136">From hello Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="2cbc6-137">Du måste be hello användaren tooaccept tooreceive push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-137">You must ask hello user tooaccept tooreceive push notifications.</span></span> <span data-ttu-id="2cbc6-138">Lägg sedan till en sätt toodisable hello push-meddelanden i dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-138">Then, in your settings, add a way toodisable hello push notifications.</span></span>

<span data-ttu-id="2cbc6-139">Hej EngagementReach objektet innehåller två metoder toomanage hello i/opt-CEIP, `EnableNativePush()` och `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-139">hello EngagementReach object provides two methods toomanage hello opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="2cbc6-140">Du kan till exempel skapa ett alternativ i hello inställningar med ett växla toodisable eller aktivera MPNS.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-140">You could, for example, create an option in hello settings with a toggle toodisable or enable MPNS.</span></span>

<span data-ttu-id="2cbc6-141">Du kan också bestämma toodeactivate MPNS via hello Engagement konfiguration\<windows phone-sdk-reach-konfiguration\>.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-141">You can also decide toodeactivate MPNS through hello Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="2cbc6-142">2.9.1) hello program måste först beskriva hello meddelanden toobe tillhandahålls och **hello användarens uttryckligt tillstånd (opt-in)**, och **måste ange en mekanism via vilken hello användaren kan välja att inaktivera mottagning push-meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-142">2.9.1) hello application must first describe hello notifications toobe provided and **obtain hello user’s express permission (opt-in)**, and **must provide a mechanism through which hello user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="2cbc6-143">Alla meddelanden som tillhandahålls med hello Microsoft Push Notification Service måste stämma överens med hello beskrivning toohello användare och måste följa alla tillämpliga [användningsprinciper] [ Content Policies]och [ytterligare krav för specifika programtyper].</span><span class="sxs-lookup"><span data-stu-id="2cbc6-143">All notifications provided using hello Microsoft Push Notification Service must be consistent with hello description provided toohello user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="2cbc6-144">Du bör inte använda för många push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-144">You should not use too many push notifications.</span></span> <span data-ttu-id="2cbc6-145">Engagement kommer att hantera meddelanden du.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="2cbc6-146">2.9.2) hello programmet och dess användning av hello Microsoft Push Notification Service måste inte överdrivet använda nätverkskapaciteten eller bandbredden för hello Microsoft Push Notification Service eller annat otillbörligt belastar en Windows Phone-eller andra Microsoft-enhet eller tjänst med långa push-meddelanden, enligt Microsofts gottfinnande, och måste inte skada eller störa någon Microsoft-nätverk eller servrar eller servrar från tredje part eller nätverk anslutet toohello Microsoft Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-146">2.9.2) hello application and its use of hello Microsoft Push Notification Service must not excessively use network capacity or bandwidth of hello Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected toohello Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="2cbc6-147">Förlita dig inte på MPNS toosend criticals information.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-147">Do not rely on MPNS toosend criticals information.</span></span> <span data-ttu-id="2cbc6-148">Engagement använder MPNS, så den här regeln gäller även för hello kampanjer som skapats i hello Engagement frontend.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-148">Engagement uses MPNS, so this rule also applies for hello campaigns created inside hello Engagement front-end.</span></span>

> <span data-ttu-id="2cbc6-149">2.9.3) hello Microsoft Push Notification Service inte kanske används toosend meddelanden som är uppdragskritiska kritiska eller på annat sätt kan påverka frågor för liv eller död, inklusive utan begränsning viktiga meddelanden relaterade tooa medicinska enhet eller villkor.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-149">2.9.3) hello Microsoft Push Notification Service may not be used toosend notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related tooa medical device or condition.</span></span> <span data-ttu-id="2cbc6-150">MICROSOFT uttryckligen friskriver sig härmed från alla garantier att hello användning av hello MICROSOFT PUSH NOTIFICATION SERVICE eller leverans av MICROSOFT PUSH NOTIFICATION SERVICE meddelanden kommer att utan avbrott, LEDIGT fel, eller på annat sätt GARANTERAS tooOCCUR ON A realtid BASIS.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT hello USE OF hello MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED tooOCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="2cbc6-151">**Vi kan inte garantera att programmet skickar hello verifieringsprocessen om du inte tar hänsyn till dessa rekommendationer.**</span><span class="sxs-lookup"><span data-stu-id="2cbc6-151">**We cannot guarantee that your application will pass hello validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="2cbc6-152">Hantera data-push (valfritt)</span><span class="sxs-lookup"><span data-stu-id="2cbc6-152">Handle data push (optional)</span></span>
<span data-ttu-id="2cbc6-153">Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden har tooimplement två händelser i hello EngagementReach klass:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-153">If you want your application toobe able tooreceive Reach data pushes, you have tooimplement two events of hello EngagementReach class:</span></span>

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

<span data-ttu-id="2cbc6-154">Du kan se att hello motringning för varje metod returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-154">You can see that hello callback of each method returns a boolean.</span></span> <span data-ttu-id="2cbc6-155">Engagement skickar en feedback tooits serverdel när hello data-push.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-155">Engagement sends a feedback tooits back-end after dispatching hello data push.</span></span> <span data-ttu-id="2cbc6-156">Om hello återanrop returnerar värdet false hello `exit` feedback kommer att skicka.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-156">If hello callback returns false, hello `exit` feedback will be send.</span></span> <span data-ttu-id="2cbc6-157">Annars blir `action`.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="2cbc6-158">Om ingen motringning är aktiverad för hello händelser hello `drop` feedback returneras tooEngagement.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-158">If no callback is set for hello events, hello `drop` feedback will be returned tooEngagement.</span></span>

> [!WARNING]
> <span data-ttu-id="2cbc6-159">Engagement är inte kan tooreceive multiplar feedback för en data-push.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-159">Engagement is not able tooreceive multiples feedbacks for a data push.</span></span> <span data-ttu-id="2cbc6-160">Om du planerar tooset flera hanterare på en händelse, Tänk på att hello feedback motsvarar toohello sista som skickas.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-160">If you plan tooset several handlers on an event, be aware that hello feedback will correspond toohello last one sent.</span></span> <span data-ttu-id="2cbc6-161">I detta fall rekommenderar vi tooalways returnerar hello samma värde tooavoid med förvirrande feedback om hello frontend.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-161">In this case, we recommend tooalways returns hello same value tooavoid having confusing feedback on hello front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="2cbc6-162">Anpassa Användargränssnittet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="2cbc6-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="2cbc6-163">Första steget</span><span class="sxs-lookup"><span data-stu-id="2cbc6-163">First step</span></span>
<span data-ttu-id="2cbc6-164">Vi kan toocustomize hello reach Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-164">We allow you toocustomize hello reach UI.</span></span>

<span data-ttu-id="2cbc6-165">toodo, alltså toocreate en underklass till hello `EngagementReachHandler` klass.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-165">toodo so, you have toocreate a subclass of hello `EngagementReachHandler` class.</span></span>

<span data-ttu-id="2cbc6-166">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="2cbc6-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="2cbc6-167">Ange hello innehållet i hello `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klass i hello `Application_Launching` metod.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-167">Then, set hello content of hello `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within hello `Application_Launching` method.</span></span>

<span data-ttu-id="2cbc6-168">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="2cbc6-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="2cbc6-169">Som standard använder Engagement implementeringen av `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="2cbc6-170">Du har inte toocreate egna, och om du gör det kan du inte toooverride varje metod.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-170">You don't have toocreate your own, and if you do so, you don't have toooverride every method.</span></span> <span data-ttu-id="2cbc6-171">hello standardbeteendet är tooselect hello Engagement basobjektet.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-171">hello default behavior is tooselect hello Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="2cbc6-172">Layouter</span><span class="sxs-lookup"><span data-stu-id="2cbc6-172">Layouts</span></span>
<span data-ttu-id="2cbc6-173">Som standard använder Reach hello inbäddade resurser av hello DLL toodisplay hello meddelanden och sidor.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-173">By default, Reach will use hello embedded resources of hello DLL toodisplay hello notifications and pages.</span></span>

<span data-ttu-id="2cbc6-174">Dock kan du bestämma toouse egna resurser tooreflect ditt varumärke för dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-174">However, you can decide toouse your own resources tooreflect your brand in these components.</span></span>

<span data-ttu-id="2cbc6-175">Du kan åsidosätta `EngagementReachHandler` metoder i din underklass tootell Engagement toouse dina layouter:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-175">You can override `EngagementReachHandler` methods in your subclass tootell Engagement toouse your layouts :</span></span>

<span data-ttu-id="2cbc6-176">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="2cbc6-176">**Sample Code :**</span></span>

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return hello path of your own xaml
    }

    public override string GetPollUri()
    {
       // return hello path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> <span data-ttu-id="2cbc6-177">Hej `CreateNotification` metoden kan returnera null.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-177">hello `CreateNotification` method can return null.</span></span> <span data-ttu-id="2cbc6-178">hello-meddelande visas inte och kommer att släppas hello reach kampanj.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-178">hello notification won't be displayed and hello reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="2cbc6-179">toosimplify implementeringen layout vi erbjuder även våra egna xaml som kan fungera som bas för din kod.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-179">toosimplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="2cbc6-180">De finns i hello Engagement SDK-Arkiv (/ src/reach /).</span><span class="sxs-lookup"><span data-stu-id="2cbc6-180">They are located in hello Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="2cbc6-181">hello källor som vi tillhandahåller är hello exakt samma som vi använder.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-181">hello sources that we provide are hello exact same ones that we use.</span></span> <span data-ttu-id="2cbc6-182">Så om du vill toomodify dem direkt, inte glömma toochange hello namnområde och hello namn.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-182">So if you want toomodify them directly, don't forget toochange hello namespace and hello name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="2cbc6-183">Meddelande-position</span><span class="sxs-lookup"><span data-stu-id="2cbc6-183">Notification position</span></span>
<span data-ttu-id="2cbc6-184">Som standard visas ett meddelande i appen hello längst ned till vänster i hello program.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-184">By default, an in-app notification is displayed at hello bottom left side of hello application.</span></span> <span data-ttu-id="2cbc6-185">Du kan ändra detta genom att åsidosätta hello `GetNotificationPosition` metod för hello `EngagementReachHandler` objekt.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-185">You can change this behavior by overriding hello `GetNotificationPosition` method of hello `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="2cbc6-186">För närvarande kan du välja mellan hello `BOTTOM` (standard) och `TOP` positioner.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-186">Currently, you can choose between hello `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="2cbc6-187">Starta meddelande</span><span class="sxs-lookup"><span data-stu-id="2cbc6-187">Launch message</span></span>
<span data-ttu-id="2cbc6-188">När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar Hej app, load hello innehållet i hello push-meddelanden och visa hello-sida för hello motsvarande kampanj.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-188">When a user clicks on a system notification (a toast), Engagement launches hello app, load hello content of hello push messages, and display hello page for hello corresponding campaign.</span></span>

<span data-ttu-id="2cbc6-189">Det finns en fördröjning mellan hello lanseringen av hello program och hello visningen av hello sida (beroende på nätverkets hello hastighet).</span><span class="sxs-lookup"><span data-stu-id="2cbc6-189">There is a delay between hello launch of hello application and hello display of hello page (depending on hello speed of your network).</span></span>

<span data-ttu-id="2cbc6-190">tooindicate toohello användaren som något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-190">tooindicate toohello user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="2cbc6-191">Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="2cbc6-192">tooimplement Hej motringning, gör du:</span><span class="sxs-lookup"><span data-stu-id="2cbc6-192">tooimplement hello callback, do:</span></span>

    /* hello application has launched and hello content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

    /* hello application has finished loading hello content and hello page
     * is about toobe displayed.
     * You should hide hello indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

    /* hello content has been loaded, but an error has occurred.
     * You can provide an information toohello user.
     * You should hide hello indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="2cbc6-193">Du kan ange hello återanrop i din `Application_Launching` metod för din `App.xaml.cs` fil, helst före hello `EngagementReach.Instance.Init()` anropa.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-193">You can set hello callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before hello `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="2cbc6-194">Varje hanterare anropas av hello UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-194">Each handler is called by hello UI Thread.</span></span> <span data-ttu-id="2cbc6-195">Du har inte tooworry när du använder en MessageBox eller något UI-relaterade.</span><span class="sxs-lookup"><span data-stu-id="2cbc6-195">You do not have tooworry when using a MessageBox or something UI-related.</span></span>
> 
> 

[användningsprinciper]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[ytterligare krav för specifika programtyper]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

