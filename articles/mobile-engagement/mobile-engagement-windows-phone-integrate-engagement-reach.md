---
title: Windows Phone Silverlight Reach SDK-Integration
description: Integrera Azure Mobile Engagement Reach med Windows Phone Silverlight-appar
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
ms.openlocfilehash: 0738f33df94d14fbb393bfaaf09e94c6560213cc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="windows-phone-silverlight-reach-sdk-integration"></a><span data-ttu-id="cfba5-103">Windows Phone Silverlight Reach SDK-Integration</span><span class="sxs-lookup"><span data-stu-id="cfba5-103">Windows Phone Silverlight Reach SDK Integration</span></span>
<span data-ttu-id="cfba5-104">Du måste följa proceduren integrering i den [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) innan du följer den här guiden.</span><span class="sxs-lookup"><span data-stu-id="cfba5-104">You must follow the integration procedure described in the [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) before following this guide.</span></span>

## <a name="embed-the-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a><span data-ttu-id="cfba5-105">Bädda in Engagement nå SDK i din Windows Phone Silverlight-projekt</span><span class="sxs-lookup"><span data-stu-id="cfba5-105">Embed the Engagement Reach SDK into your Windows Phone Silverlight project</span></span>
<span data-ttu-id="cfba5-106">Du har inte något att lägga till.</span><span class="sxs-lookup"><span data-stu-id="cfba5-106">You do not have anything to add.</span></span> <span data-ttu-id="cfba5-107">`EngagementReach`referenser och resurser finns redan i projektet.</span><span class="sxs-lookup"><span data-stu-id="cfba5-107">`EngagementReach` references and resources are already in your project.</span></span>

> [!TIP]
> <span data-ttu-id="cfba5-108">Du kan anpassa avbildningar som finns i den `Resources` mappen i ditt projekt, speciellt märke ikonen (som standard till ikonen Engagement).</span><span class="sxs-lookup"><span data-stu-id="cfba5-108">You can customize images located in the `Resources` folder of your project, especially the brand icon (that default to the Engagement icon).</span></span>
> 
> 

## <a name="add-the-capabilities"></a><span data-ttu-id="cfba5-109">Lägg till funktioner</span><span class="sxs-lookup"><span data-stu-id="cfba5-109">Add the capabilities</span></span>
<span data-ttu-id="cfba5-110">Engagement nå SDK måste vissa ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="cfba5-110">The Engagement Reach SDK needs some additional capabilities.</span></span>

<span data-ttu-id="cfba5-111">Öppna din `WMAppManifest.xml` filen och se till att följande funktioner har deklarerats:</span><span class="sxs-lookup"><span data-stu-id="cfba5-111">Open your `WMAppManifest.xml` file and be sure that the following capabilities are declared:</span></span>

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

<span data-ttu-id="cfba5-112">Den första som används av tjänsten MPNS att visningen av popup-meddelande.</span><span class="sxs-lookup"><span data-stu-id="cfba5-112">The first one is used by the MPNS service to allow the display of toast notification.</span></span> <span data-ttu-id="cfba5-113">Den andra som används för att bädda in en webbläsare aktivitet i SDK.</span><span class="sxs-lookup"><span data-stu-id="cfba5-113">The second one is used to embed a browser task into the SDK.</span></span>

<span data-ttu-id="cfba5-114">Redigera den `WMAppManifest.xml` och Lägg till i den `<Capabilities />` tagg:</span><span class="sxs-lookup"><span data-stu-id="cfba5-114">Edit the `WMAppManifest.xml` file and add inside the `<Capabilities />` tag :</span></span>

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-the-microsoft-push-notification-service"></a><span data-ttu-id="cfba5-115">Aktivera Microsoft Push Notification Service</span><span class="sxs-lookup"><span data-stu-id="cfba5-115">Enable the Microsoft Push Notification Service</span></span>
<span data-ttu-id="cfba5-116">För att kunna använda den **Microsoft Push Notification Service** (kallas MPNS) din `WMAppManifest.xml` filen måste ha en `<App />` taggen med en `Publisher` -attributet inställt på namnet på ditt projekt.</span><span class="sxs-lookup"><span data-stu-id="cfba5-116">In order to use the **Microsoft Push Notification Service** (referred as MPNS) your `WMAppManifest.xml` file must have an `<App />` tag with a `Publisher` attribute set to the name of your project.</span></span>

## <a name="initialize-the-engagement-reach-sdk"></a><span data-ttu-id="cfba5-117">Initiera Engagement Reach SDK</span><span class="sxs-lookup"><span data-stu-id="cfba5-117">Initialize the Engagement Reach SDK</span></span>
### <a name="engagement-configuration"></a><span data-ttu-id="cfba5-118">Engagement konfiguration</span><span class="sxs-lookup"><span data-stu-id="cfba5-118">Engagement configuration</span></span>
<span data-ttu-id="cfba5-119">Konfigurationen av Engagement centraliserad i den `Resources\EngagementConfiguration.xml` filen i projektet.</span><span class="sxs-lookup"><span data-stu-id="cfba5-119">The Engagement configuration is centralized in the `Resources\EngagementConfiguration.xml` file of your project.</span></span>

<span data-ttu-id="cfba5-120">Redigera den här filen om du vill ange reach-konfiguration:</span><span class="sxs-lookup"><span data-stu-id="cfba5-120">Edit this file to specify reach configuration :</span></span>

* <span data-ttu-id="cfba5-121">*Valfria*, ange om den interna push (MPNS) är aktiverad eller inte mellan `<enableNativePush>` och `</enableNativePush>` taggar (`true` som standard).</span><span class="sxs-lookup"><span data-stu-id="cfba5-121">*Optional*, indicate whether the native push (MPNS) is activated or not between `<enableNativePush>` and `</enableNativePush>` tags, (`true` by default).</span></span>
* <span data-ttu-id="cfba5-122">*Valfria*, ange namnet på push-kanal mellan `<channelName>` och `</channelName>` taggar, ger samma att ditt program kan använda eller lämna det tomt.</span><span class="sxs-lookup"><span data-stu-id="cfba5-122">*Optional*, indicate the name of the push channel between `<channelName>` and `</channelName>` tags, provide the same that your application may currently use or leave it empty.</span></span>

<span data-ttu-id="cfba5-123">Om du vill ange vid körning i stället kan du anropa metoden följande innan Engagement agentinitieringen:</span><span class="sxs-lookup"><span data-stu-id="cfba5-123">If you want to specify it at runtime instead, you can call the following method before the Engagement agent initialization :</span></span>

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    engagementConfiguration.Reach.EnableNativePush = true;                  
    /* [Optional] whether the native push (MPNS) is activated or not. */

    engagementConfiguration.Reach.ChannelName = "YOUR_PUSH_CHANNEL_NAME";   
    /* [Optional] Provide the same channel name that your application may currently use. */

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

> [!TIP]
> <span data-ttu-id="cfba5-124">Du kan ange namnet på den MPNS push-kanalen för ditt program.</span><span class="sxs-lookup"><span data-stu-id="cfba5-124">You can specify the name of the MPNS push channel of your application.</span></span> <span data-ttu-id="cfba5-125">Som standard skapar Engagement ett namn baserat på appId.</span><span class="sxs-lookup"><span data-stu-id="cfba5-125">By default, Engagement creates a name based on the appId.</span></span> <span data-ttu-id="cfba5-126">Du behöver inte ange namnet själv, utom om du planerar att använda push-kanal utanför Engagement.</span><span class="sxs-lookup"><span data-stu-id="cfba5-126">You have no need to specify the name yourself, except if you plan to use the push channel outside of Engagement.</span></span>
> 
> 

### <a name="engagement-initialization"></a><span data-ttu-id="cfba5-127">Initieringen av engagement</span><span class="sxs-lookup"><span data-stu-id="cfba5-127">Engagement initialization</span></span>
<span data-ttu-id="cfba5-128">Ändra den `App.xaml.cs`:</span><span class="sxs-lookup"><span data-stu-id="cfba5-128">Modify the `App.xaml.cs`:</span></span>

* <span data-ttu-id="cfba5-129">Lägg till din `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="cfba5-129">Add to your `using` statements :</span></span>
  
      using Microsoft.Azure.Engagement;
* <span data-ttu-id="cfba5-130">Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i `Application_Launching` :</span><span class="sxs-lookup"><span data-stu-id="cfba5-130">Insert `EngagementReach.Instance.Init` just after `EngagementAgent.Instance.Init` in `Application_Launching` :</span></span>
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* <span data-ttu-id="cfba5-131">Infoga `EngagementReach.Instance.OnActivated` i den `Application_Activated` metoden:</span><span class="sxs-lookup"><span data-stu-id="cfba5-131">Insert `EngagementReach.Instance.OnActivated` in the `Application_Activated` method :</span></span>
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> <span data-ttu-id="cfba5-132">Den `EngagementReach.Instance.Init` körs i en dedikerad tråd.</span><span class="sxs-lookup"><span data-stu-id="cfba5-132">The `EngagementReach.Instance.Init` runs in a dedicated thread.</span></span> <span data-ttu-id="cfba5-133">Du behöver inte göra det själv.</span><span class="sxs-lookup"><span data-stu-id="cfba5-133">You do not have to do it yourself.</span></span>
> 
> 

## <a name="app-store-submission-considerations"></a><span data-ttu-id="cfba5-134">App store skicka överväganden</span><span class="sxs-lookup"><span data-stu-id="cfba5-134">App store submission considerations</span></span>
<span data-ttu-id="cfba5-135">Microsoft medför vissa regler när du använder push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="cfba5-135">Microsoft imposes some rules when using the push notifications:</span></span>

<span data-ttu-id="cfba5-136">Från Microsoft [användningsprinciper] dokumentation, avsnittet 2.9:</span><span class="sxs-lookup"><span data-stu-id="cfba5-136">From the Microsoft [Application Policies] documentation, section 2.9:</span></span>

1) <span data-ttu-id="cfba5-137">Du måste be användaren att acceptera för att ta emot push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cfba5-137">You must ask the user to accept to receive push notifications.</span></span> <span data-ttu-id="cfba5-138">Lägg sedan till ett sätt att inaktivera push-meddelanden i dina inställningar.</span><span class="sxs-lookup"><span data-stu-id="cfba5-138">Then, in your settings, add a way to disable the push notifications.</span></span>

<span data-ttu-id="cfba5-139">Objektet EngagementReach erbjuder två metoder för att hantera den opt-i eller gå ur, `EnableNativePush()` och `DisableNativePush()`.</span><span class="sxs-lookup"><span data-stu-id="cfba5-139">The EngagementReach object provides two methods to manage the opt-in/opt-out, `EnableNativePush()` and `DisableNativePush()`.</span></span> <span data-ttu-id="cfba5-140">Du kan till exempel skapa ett alternativ i inställningarna med en växlingsknapp för att inaktivera eller aktivera MPNS.</span><span class="sxs-lookup"><span data-stu-id="cfba5-140">You could, for example, create an option in the settings with a toggle to disable or enable MPNS.</span></span>

<span data-ttu-id="cfba5-141">Du kan också välja att inaktivera MPNS genom konfigurationen av Engagement\<windows phone-sdk-reach-konfiguration\>.</span><span class="sxs-lookup"><span data-stu-id="cfba5-141">You can also decide to deactivate MPNS through the Engagement configuration\<windows-phone-sdk-reach-configuration\>.</span></span>

> <span data-ttu-id="cfba5-142">2.9.1) programmet måste först beskriva meddelanden ska tillhandahållas och **hämta användarens uttryckligt tillstånd (opt-in)**, och **måste ange en mekanism via vilken användaren kan välja bort tar emot push meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="cfba5-142">2.9.1) The application must first describe the notifications to be provided and **obtain the user’s express permission (opt-in)**, and **must provide a mechanism through which the user can opt out of receiving push notifications**.</span></span> <span data-ttu-id="cfba5-143">Alla meddelanden som tillhandahålls med Microsoft Push Notification Service måste stämma överens med den beskrivning som angetts för användaren och måste följa alla tillämpliga [användningsprinciper] [ Content Policies] och [Ytterligare krav för specifika programtyper].</span><span class="sxs-lookup"><span data-stu-id="cfba5-143">All notifications provided using the Microsoft Push Notification Service must be consistent with the description provided to the user and must comply with all applicable [Application Policies][Content Policies] and [Additional Requirements for Specific Application Types].</span></span>
> 
> 

2) <span data-ttu-id="cfba5-144">Du bör inte använda för många push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="cfba5-144">You should not use too many push notifications.</span></span> <span data-ttu-id="cfba5-145">Engagement kommer att hantera meddelanden du.</span><span class="sxs-lookup"><span data-stu-id="cfba5-145">Engagement will handle notifications for you.</span></span>

> <span data-ttu-id="cfba5-146">2.9.2) programmet och dess användning av Microsoft Push Notification Service måste inte överdrivet använder nätverkskapaciteten eller bandbredden för Microsoft Push Notification Service, eller på annat sätt otillbörligt belastningen en Windows Phone-eller andra Microsoft-enhet eller tjänst med långa push-meddelanden, enligt Microsofts gottfinnande, och måste inte skada eller störa någon Microsoft-nätverk eller servrar eller servrar från tredje part eller nätverk som är anslutna till tjänsten Microsoft Push Notification Service.</span><span class="sxs-lookup"><span data-stu-id="cfba5-146">2.9.2) The application and its use of the Microsoft Push Notification Service must not excessively use network capacity or bandwidth of the Microsoft Push Notification Service, or otherwise unduly burden a Windows Phone or other Microsoft device or service with excessive push notifications, as determined by Microsoft in its reasonable discretion, and must not harm or interfere with any Microsoft networks or servers or any third party servers or networks connected to the Microsoft Push Notification Service.</span></span>
> 
> 

3) <span data-ttu-id="cfba5-147">Förlita dig inte på MPNS skickar criticals information.</span><span class="sxs-lookup"><span data-stu-id="cfba5-147">Do not rely on MPNS to send criticals information.</span></span> <span data-ttu-id="cfba5-148">Engagement använder MPNS, så den här regeln gäller också för de kampanjer som skapats i hur engagerade frontend.</span><span class="sxs-lookup"><span data-stu-id="cfba5-148">Engagement uses MPNS, so this rule also applies for the campaigns created inside the Engagement front-end.</span></span>

> <span data-ttu-id="cfba5-149">2.9.3) i Microsoft Push Notification Service kanske inte används för att skicka meddelanden som är verksamhetskritiska kritiska eller på annat sätt kan påverka frågor för liv eller död, inklusive utan begränsning viktiga meddelanden relaterade till en medicinska enhet eller villkor.</span><span class="sxs-lookup"><span data-stu-id="cfba5-149">2.9.3) The Microsoft Push Notification Service may not be used to send notifications that are mission critical or otherwise could affect matters of life or death, including without limitation critical notifications related to a medical device or condition.</span></span> <span data-ttu-id="cfba5-150">MICROSOFT UTTRYCKLIGEN FRISKRIVER SIG HÄRMED FRÅN ALLA GARANTIER ATT ANVÄNDNINGEN AV MICROSOFT PUSH NOTIFICATION SERVICE ELLER LEVERANS AV MICROSOFT PUSH NOTIFICATION SERVICE MEDDELANDEN KOMMER ATT AVBROTT, FELFRI, ELLER PÅ ANNAT SÄTT GARANTERAS SÅ ATT DET SKER EN I REALTID.</span><span class="sxs-lookup"><span data-stu-id="cfba5-150">MICROSOFT EXPRESSLY DISCLAIMS ANY WARRANTIES THAT THE USE OF THE MICROSOFT PUSH NOTIFICATION SERVICE OR DELIVERY OF MICROSOFT PUSH NOTIFICATION SERVICE NOTIFICATIONS WILL BE UNINTERRUPTED, ERROR FREE, OR OTHERWISE GUARANTEED TO OCCUR ON A REAL-TIME BASIS.</span></span>
> 
> 

<span data-ttu-id="cfba5-151">**Vi kan inte garantera att programmet skickar valideringsprocessen om du inte tar hänsyn till dessa rekommendationer.**</span><span class="sxs-lookup"><span data-stu-id="cfba5-151">**We cannot guarantee that your application will pass the validation process if you do not respect these recommendations.**</span></span>

## <a name="handle-data-push-optional"></a><span data-ttu-id="cfba5-152">Hantera data-push (valfritt)</span><span class="sxs-lookup"><span data-stu-id="cfba5-152">Handle data push (optional)</span></span>
<span data-ttu-id="cfba5-153">Om du vill att programmet ska kunna ta emot Reach data push-meddelanden måste du implementera två händelser i klassen EngagementReach:</span><span class="sxs-lookup"><span data-stu-id="cfba5-153">If you want your application to be able to receive Reach data pushes, you have to implement two events of the EngagementReach class:</span></span>

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

<span data-ttu-id="cfba5-154">Du kan se att återanropet för varje metod returnerar ett booleskt värde.</span><span class="sxs-lookup"><span data-stu-id="cfba5-154">You can see that the callback of each method returns a boolean.</span></span> <span data-ttu-id="cfba5-155">Engagement skickar feedback till dess backend när data-push.</span><span class="sxs-lookup"><span data-stu-id="cfba5-155">Engagement sends a feedback to its back-end after dispatching the data push.</span></span> <span data-ttu-id="cfba5-156">Om återanropet returnerar värdet false, den `exit` feedback kommer att skicka.</span><span class="sxs-lookup"><span data-stu-id="cfba5-156">If the callback returns false, the `exit` feedback will be send.</span></span> <span data-ttu-id="cfba5-157">Annars blir `action`.</span><span class="sxs-lookup"><span data-stu-id="cfba5-157">Otherwise, it will be `action`.</span></span> <span data-ttu-id="cfba5-158">Om ingen motringning är inställd för händelser, den `drop` feedback återgår till Engagement.</span><span class="sxs-lookup"><span data-stu-id="cfba5-158">If no callback is set for the events, the `drop` feedback will be returned to Engagement.</span></span>

> [!WARNING]
> <span data-ttu-id="cfba5-159">Engagement går inte att ta emot multiplar feedback för en data-push.</span><span class="sxs-lookup"><span data-stu-id="cfba5-159">Engagement is not able to receive multiples feedbacks for a data push.</span></span> <span data-ttu-id="cfba5-160">Om du vill ange flera hanterare på en händelse, Tänk på att din feedback motsvarar den senast skickade en.</span><span class="sxs-lookup"><span data-stu-id="cfba5-160">If you plan to set several handlers on an event, be aware that the feedback will correspond to the last one sent.</span></span> <span data-ttu-id="cfba5-161">Vi rekommenderar i det här fallet returnerar alltid samma värde för att undvika att förvirrande feedback på klientdelen.</span><span class="sxs-lookup"><span data-stu-id="cfba5-161">In this case, we recommend to always returns the same value to avoid having confusing feedback on the front-end.</span></span>
> 
> 

## <a name="customize-ui-optional"></a><span data-ttu-id="cfba5-162">Anpassa Användargränssnittet (valfritt)</span><span class="sxs-lookup"><span data-stu-id="cfba5-162">Customize UI (optional)</span></span>
### <a name="first-step"></a><span data-ttu-id="cfba5-163">Första steget</span><span class="sxs-lookup"><span data-stu-id="cfba5-163">First step</span></span>
<span data-ttu-id="cfba5-164">Vi kan du anpassa räckvidden Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="cfba5-164">We allow you to customize the reach UI.</span></span>

<span data-ttu-id="cfba5-165">Om du vill göra det, måste du skapa en underklass till den `EngagementReachHandler` klass.</span><span class="sxs-lookup"><span data-stu-id="cfba5-165">To do so, you have to create a subclass of the `EngagementReachHandler` class.</span></span>

<span data-ttu-id="cfba5-166">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="cfba5-166">**Sample Code :**</span></span>

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

<span data-ttu-id="cfba5-167">Ställ sedan innehållet i den `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klassen inom den `Application_Launching` metoden.</span><span class="sxs-lookup"><span data-stu-id="cfba5-167">Then, set the content of the `EngagementReach.Instance.Handler` field with your custom object in your `App.xaml.cs` class within the `Application_Launching` method.</span></span>

<span data-ttu-id="cfba5-168">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="cfba5-168">**Sample Code :**</span></span>

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> <span data-ttu-id="cfba5-169">Som standard använder Engagement implementeringen av `EngagementReachHandler`.</span><span class="sxs-lookup"><span data-stu-id="cfba5-169">By default, Engagement uses its own implementation of `EngagementReachHandler`.</span></span> <span data-ttu-id="cfba5-170">Du behöver skapa en egen och om du gör det kan du inte ska åsidosätta alla metoder.</span><span class="sxs-lookup"><span data-stu-id="cfba5-170">You don't have to create your own, and if you do so, you don't have to override every method.</span></span> <span data-ttu-id="cfba5-171">Standardinställningen är att välja Engagement basobjektet.</span><span class="sxs-lookup"><span data-stu-id="cfba5-171">The default behavior is to select the Engagement base object.</span></span>
> 
> 

### <a name="layouts"></a><span data-ttu-id="cfba5-172">Layouter</span><span class="sxs-lookup"><span data-stu-id="cfba5-172">Layouts</span></span>
<span data-ttu-id="cfba5-173">Som standard använder Reach inbäddade resurser för DLL-filen för att visa meddelanden och sidor.</span><span class="sxs-lookup"><span data-stu-id="cfba5-173">By default, Reach will use the embedded resources of the DLL to display the notifications and pages.</span></span>

<span data-ttu-id="cfba5-174">Du kan dock välja att använda egna resurser för att återspegla ditt varumärke för dessa komponenter.</span><span class="sxs-lookup"><span data-stu-id="cfba5-174">However, you can decide to use your own resources to reflect your brand in these components.</span></span>

<span data-ttu-id="cfba5-175">Du kan åsidosätta `EngagementReachHandler` metoder i din underklass ska berätta för Engagement att använda en layout:</span><span class="sxs-lookup"><span data-stu-id="cfba5-175">You can override `EngagementReachHandler` methods in your subclass to tell Engagement to use your layouts :</span></span>

<span data-ttu-id="cfba5-176">**Exempelkod:**</span><span class="sxs-lookup"><span data-stu-id="cfba5-176">**Sample Code :**</span></span>

    // In your subclass of EngagementReachHandler

    public override string GetTextViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetWebViewAnnouncementUri()
    {
       // return the path of your own xaml
    }

    public override string GetPollUri()
    {
       // return the path of your own xaml
    }

    public override EngagementNotificationView CreateNotification(EngagementNotificationViewModel viewModel)
    {
       // return a new instance of your own notification
    }

> [!TIP]
> <span data-ttu-id="cfba5-177">Den `CreateNotification` metoden kan returnera null.</span><span class="sxs-lookup"><span data-stu-id="cfba5-177">The `CreateNotification` method can return null.</span></span> <span data-ttu-id="cfba5-178">Meddelandet visas inte och kommer att släppas reach-kampanj.</span><span class="sxs-lookup"><span data-stu-id="cfba5-178">The notification won't be displayed and the reach campaign will be dropped.</span></span>
> 
> 

<span data-ttu-id="cfba5-179">För att förenkla implementeringen layout, erbjuder vi även våra egna xaml som kan fungera som bas för din kod.</span><span class="sxs-lookup"><span data-stu-id="cfba5-179">To simplify your layout implementation, we also provide our own xaml which can serve as a basis for your code.</span></span> <span data-ttu-id="cfba5-180">De finns i arkivet Engagement SDK (/ src/reach /).</span><span class="sxs-lookup"><span data-stu-id="cfba5-180">They are located in the Engagement SDK archive (/src/reach/).</span></span>

> [!WARNING]
> <span data-ttu-id="cfba5-181">Källor som vi tillhandahåller är exakt samma de som vi använder.</span><span class="sxs-lookup"><span data-stu-id="cfba5-181">The sources that we provide are the exact same ones that we use.</span></span> <span data-ttu-id="cfba5-182">Så om du vill ändra dem direkt Glöm inte att ändra namnområdet och namn.</span><span class="sxs-lookup"><span data-stu-id="cfba5-182">So if you want to modify them directly, don't forget to change the namespace and the name.</span></span>
> 
> 

### <a name="notification-position"></a><span data-ttu-id="cfba5-183">Meddelande-position</span><span class="sxs-lookup"><span data-stu-id="cfba5-183">Notification position</span></span>
<span data-ttu-id="cfba5-184">Som standard visas ett meddelande i appen längst ned till vänster sida av programmet.</span><span class="sxs-lookup"><span data-stu-id="cfba5-184">By default, an in-app notification is displayed at the bottom left side of the application.</span></span> <span data-ttu-id="cfba5-185">Du kan ändra detta genom att åsidosätta den `GetNotificationPosition` metod för den `EngagementReachHandler` objekt.</span><span class="sxs-lookup"><span data-stu-id="cfba5-185">You can change this behavior by overriding the `GetNotificationPosition` method of the `EngagementReachHandler` object.</span></span>

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of the EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

<span data-ttu-id="cfba5-186">För närvarande kan du välja mellan de `BOTTOM` (standard) och `TOP` positioner.</span><span class="sxs-lookup"><span data-stu-id="cfba5-186">Currently, you can choose between the `BOTTOM` (default) and `TOP` positions.</span></span>

### <a name="launch-message"></a><span data-ttu-id="cfba5-187">Starta meddelande</span><span class="sxs-lookup"><span data-stu-id="cfba5-187">Launch message</span></span>
<span data-ttu-id="cfba5-188">När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar appen, läsa in innehållet i push-meddelanden och visa sidan för motsvarande kampanjen.</span><span class="sxs-lookup"><span data-stu-id="cfba5-188">When a user clicks on a system notification (a toast), Engagement launches the app, load the content of the push messages, and display the page for the corresponding campaign.</span></span>

<span data-ttu-id="cfba5-189">Det finns en fördröjning mellan starten av programmet och visas på sidan (beroende på nätverkets hastighet).</span><span class="sxs-lookup"><span data-stu-id="cfba5-189">There is a delay between the launch of the application and the display of the page (depending on the speed of your network).</span></span>

<span data-ttu-id="cfba5-190">Om du vill visa användaren att något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator.</span><span class="sxs-lookup"><span data-stu-id="cfba5-190">To indicate to the user that something is loading, you should provide a visual information, like a progress bar or a progress indicator.</span></span> <span data-ttu-id="cfba5-191">Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.</span><span class="sxs-lookup"><span data-stu-id="cfba5-191">Engagement cannot handle that itself, but provides a few handlers for you.</span></span>

<span data-ttu-id="cfba5-192">Om du vill implementera återanropet gör du:</span><span class="sxs-lookup"><span data-stu-id="cfba5-192">To implement the callback, do:</span></span>

    /* The application has launched and the content is loading.
     * You should display an indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

    /* The application has finished loading the content and the page
     * is about to be displayed.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

    /* The content has been loaded, but an error has occurred.
     * You can provide an information to the user.
     * You should hide the indicator here.
     */
    EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

<span data-ttu-id="cfba5-193">Du kan ange återanropet i din `Application_Launching` metod för din `App.xaml.cs` fil, helst innan det `EngagementReach.Instance.Init()` anropa.</span><span class="sxs-lookup"><span data-stu-id="cfba5-193">You can set the callback in your `Application_Launching` method of your `App.xaml.cs` file, preferably before the `EngagementReach.Instance.Init()` call.</span></span>

> [!TIP]
> <span data-ttu-id="cfba5-194">Varje hanterare anropas av UI-tråden.</span><span class="sxs-lookup"><span data-stu-id="cfba5-194">Each handler is called by the UI Thread.</span></span> <span data-ttu-id="cfba5-195">Du behöver inte oroa dig när du använder en MessageBox eller något UI-relaterade.</span><span class="sxs-lookup"><span data-stu-id="cfba5-195">You do not have to worry when using a MessageBox or something UI-related.</span></span>
> 
> 

<span data-ttu-id="cfba5-196">[användningsprinciper]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="cfba5-196">[Application Policies]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx</span></span>
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
<span data-ttu-id="cfba5-197">[Ytterligare krav för specifika programtyper]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="cfba5-197">[Additional Requirements for Specific Application Types]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx</span></span>

