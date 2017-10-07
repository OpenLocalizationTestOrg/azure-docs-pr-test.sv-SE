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
# <a name="windows-phone-silverlight-reach-sdk-integration"></a>Windows Phone Silverlight Reach SDK-Integration
Du måste följa hello integration beskrivs i hello [Windows Phone Silverlight Engagement SDK Integration](mobile-engagement-windows-phone-integrate-engagement.md) innan du följer den här guiden.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-phone-silverlight-project"></a>Bädda in hello Engagement nå SDK i Windows Phone Silverlight-projekt
Du har inte något tooadd. `EngagementReach`referenser och resurser finns redan i projektet.

> [!TIP]
> Du kan anpassa avbildningar som finns i hello `Resources` mapp i ditt projekt, särskilt hello varumärken-ikonen (som standard toohello Engagement ikon).
> 
> 

## <a name="add-hello-capabilities"></a>Lägger till hello-funktioner
Hej Engagement nå SDK måste vissa ytterligare funktioner.

Öppna din `WMAppManifest.xml` fil- och måste deklareras som hello följande funktioner:

* `ID_CAP_PUSH_NOTIFICATION`
* `ID_CAP_WEBBROWSERCOMPONENT`

hello först används en av hello MPNS service tooallow hello visningen av popup-meddelande. hello är andra tooembed används en webbläsare aktivitet i hello SDK.

Redigera hello `WMAppManifest.xml` och Lägg till i hello `<Capabilities />` tagg:

    <Capability Name="ID_CAP_PUSH_NOTIFICATION" />
    <Capability Name="ID_CAP_WEBBROWSERCOMPONENT" />

## <a name="enable-hello-microsoft-push-notification-service"></a>Aktivera hello Microsoft Push Notification Service
I ordning toouse hello **Microsoft Push Notification Service** (kallas MPNS) din `WMAppManifest.xml` filen måste ha en `<App />` taggen med en `Publisher` -attributet inställt toohello namnet på ditt projekt.

## <a name="initialize-hello-engagement-reach-sdk"></a>Initiera hello Engagement Reach SDK
### <a name="engagement-configuration"></a>Engagement konfiguration
Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.

Redigera filen toospecify reach konfigurationen:

* *Valfria*, ange om hello native push (MPNS) är aktiverad eller inte mellan `<enableNativePush>` och `</enableNativePush>` taggar (`true` som standard).
* *Valfria*, ange hello namnet på hello push kanal mellan `<channelName>` och `</channelName>` taggar, ange hello samma att ditt program kan använda eller lämna det tomt.

Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:

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
> Du kan ange hello namnet på hello MPNS push kanal för ditt program. Som standard skapar Engagement ett namn baserat på hello appId. Du har inget behov av toospecify hello namn själv, utom om du planerar toouse hello push kanal utanför Engagement.
> 
> 

### <a name="engagement-initialization"></a>Initieringen av engagement
Ändra hello `App.xaml.cs`:

* Lägg till tooyour `using` instruktioner:
  
      using Microsoft.Azure.Engagement;
* Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i `Application_Launching` :
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
         EngagementAgent.Instance.Init();
         EngagementReach.Instance.Init();
      }
* Infoga `EngagementReach.Instance.OnActivated` i hello `Application_Activated` metoden:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
         EngagementReach.Instance.OnActivated(e);
      }

> [!IMPORTANT]
> Hej `EngagementReach.Instance.Init` körs i en dedikerad tråd. Du har inte toodo den själv.
> 
> 

## <a name="app-store-submission-considerations"></a>App store skicka överväganden
Microsoft medför vissa regler när du använder hello push-meddelanden:

Från hello Microsoft [användningsprinciper] dokumentation, avsnittet 2.9:

1) Du måste be hello användaren tooaccept tooreceive push-meddelanden. Lägg sedan till en sätt toodisable hello push-meddelanden i dina inställningar.

Hej EngagementReach objektet innehåller två metoder toomanage hello i/opt-CEIP, `EnableNativePush()` och `DisableNativePush()`. Du kan till exempel skapa ett alternativ i hello inställningar med ett växla toodisable eller aktivera MPNS.

Du kan också bestämma toodeactivate MPNS via hello Engagement konfiguration\<windows phone-sdk-reach-konfiguration\>.

> 2.9.1) hello program måste först beskriva hello meddelanden toobe tillhandahålls och **hello användarens uttryckligt tillstånd (opt-in)**, och **måste ange en mekanism via vilken hello användaren kan välja att inaktivera mottagning push-meddelanden**. Alla meddelanden som tillhandahålls med hello Microsoft Push Notification Service måste stämma överens med hello beskrivning toohello användare och måste följa alla tillämpliga [användningsprinciper] [ Content Policies]och [ytterligare krav för specifika programtyper].
> 
> 

2) Du bör inte använda för många push-meddelanden. Engagement kommer att hantera meddelanden du.

> 2.9.2) hello programmet och dess användning av hello Microsoft Push Notification Service måste inte överdrivet använda nätverkskapaciteten eller bandbredden för hello Microsoft Push Notification Service eller annat otillbörligt belastar en Windows Phone-eller andra Microsoft-enhet eller tjänst med långa push-meddelanden, enligt Microsofts gottfinnande, och måste inte skada eller störa någon Microsoft-nätverk eller servrar eller servrar från tredje part eller nätverk anslutet toohello Microsoft Push Notification Service.
> 
> 

3) Förlita dig inte på MPNS toosend criticals information. Engagement använder MPNS, så den här regeln gäller även för hello kampanjer som skapats i hello Engagement frontend.

> 2.9.3) hello Microsoft Push Notification Service inte kanske används toosend meddelanden som är uppdragskritiska kritiska eller på annat sätt kan påverka frågor för liv eller död, inklusive utan begränsning viktiga meddelanden relaterade tooa medicinska enhet eller villkor. MICROSOFT uttryckligen friskriver sig härmed från alla garantier att hello användning av hello MICROSOFT PUSH NOTIFICATION SERVICE eller leverans av MICROSOFT PUSH NOTIFICATION SERVICE meddelanden kommer att utan avbrott, LEDIGT fel, eller på annat sätt GARANTERAS tooOCCUR ON A realtid BASIS.
> 
> 

**Vi kan inte garantera att programmet skickar hello verifieringsprocessen om du inte tar hänsyn till dessa rekommendationer.**

## <a name="handle-data-push-optional"></a>Hantera data-push (valfritt)
Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden har tooimplement två händelser i hello EngagementReach klass:

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

Du kan se att hello motringning för varje metod returnerar ett booleskt värde. Engagement skickar en feedback tooits serverdel när hello data-push. Om hello återanrop returnerar värdet false hello `exit` feedback kommer att skicka. Annars blir `action`. Om ingen motringning är aktiverad för hello händelser hello `drop` feedback returneras tooEngagement.

> [!WARNING]
> Engagement är inte kan tooreceive multiplar feedback för en data-push. Om du planerar tooset flera hanterare på en händelse, Tänk på att hello feedback motsvarar toohello sista som skickas. I detta fall rekommenderar vi tooalways returnerar hello samma värde tooavoid med förvirrande feedback om hello frontend.
> 
> 

## <a name="customize-ui-optional"></a>Anpassa Användargränssnittet (valfritt)
### <a name="first-step"></a>Första steget
Vi kan toocustomize hello reach Användargränssnittet.

toodo, alltså toocreate en underklass till hello `EngagementReachHandler` klass.

**Exempelkod:**

    using Microsoft.Azure.Engagement;

    namespace Example
    {
       internal class ExampleReachHandler : EngagementReachHandler
       {
          // Override EngagementReachHandler methods depending on your needs
       }
    }

Ange hello innehållet i hello `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klass i hello `Application_Launching` metod.

**Exempelkod:**

    private void Application_Launching(object sender, LaunchingEventArgs e)
    {
       // your app initialization 
       EngagementReach.Instance.Handler = new ExampleReachHandler();
       // Engagement Agent and Reach initialization
    }

> [!NOTE]
> Som standard använder Engagement implementeringen av `EngagementReachHandler`. Du har inte toocreate egna, och om du gör det kan du inte toooverride varje metod. hello standardbeteendet är tooselect hello Engagement basobjektet.
> 
> 

### <a name="layouts"></a>Layouter
Som standard använder Reach hello inbäddade resurser av hello DLL toodisplay hello meddelanden och sidor.

Dock kan du bestämma toouse egna resurser tooreflect ditt varumärke för dessa komponenter.

Du kan åsidosätta `EngagementReachHandler` metoder i din underklass tootell Engagement toouse dina layouter:

**Exempelkod:**

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
> Hej `CreateNotification` metoden kan returnera null. hello-meddelande visas inte och kommer att släppas hello reach kampanj.
> 
> 

toosimplify implementeringen layout vi erbjuder även våra egna xaml som kan fungera som bas för din kod. De finns i hello Engagement SDK-Arkiv (/ src/reach /).

> [!WARNING]
> hello källor som vi tillhandahåller är hello exakt samma som vi använder. Så om du vill toomodify dem direkt, inte glömma toochange hello namnområde och hello namn.
> 
> 

### <a name="notification-position"></a>Meddelande-position
Som standard visas ett meddelande i appen hello längst ned till vänster i hello program. Du kan ändra detta genom att åsidosätta hello `GetNotificationPosition` metod för hello `EngagementReachHandler` objekt.

    // In your subclass of EngagementReachHandler

    public override EngagementReachHandler.NotificationPosition GetNotificationPosition(EngagementNotificationViewModel viewModel)
    {
       // return a value of hello EngagementReachHandler.NotificationPosition enum (TOP or BOTTOM)
    }

För närvarande kan du välja mellan hello `BOTTOM` (standard) och `TOP` positioner.

### <a name="launch-message"></a>Starta meddelande
När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar Hej app, load hello innehållet i hello push-meddelanden och visa hello-sida för hello motsvarande kampanj.

Det finns en fördröjning mellan hello lanseringen av hello program och hello visningen av hello sida (beroende på nätverkets hello hastighet).

tooindicate toohello användaren som något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator. Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.

tooimplement Hej motringning, gör du:

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

Du kan ange hello återanrop i din `Application_Launching` metod för din `App.xaml.cs` fil, helst före hello `EngagementReach.Instance.Init()` anropa.

> [!TIP]
> Varje hanterare anropas av hello UI-tråden. Du har inte tooworry när du använder en MessageBox eller något UI-relaterade.
> 
> 

[användningsprinciper]:http://msdn.microsoft.com/library/windows/apps/hh184841(v=vs.105).aspx
[Content Policies]:http://msdn.microsoft.com/library/windows/apps/hh184842(v=vs.105).aspx
[ytterligare krav för specifika programtyper]:http://msdn.microsoft.com/library/windows/apps/hh184838(v=vs.105).aspx

