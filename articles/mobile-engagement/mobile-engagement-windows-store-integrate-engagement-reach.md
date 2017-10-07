---
title: "aaaWindows universella appar nå SDK-Integration"
description: "Hur tooIntegrate Azure Mobile Engagement nå med universella Windows-appar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: af311c65940014083333853875a00173b8d6783e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Universella Windows-appar nå SDK-Integration
Du måste följa hello integration beskrivs i hello [Windows Universal SDK-Integration av Engagement](mobile-engagement-windows-store-integrate-engagement.md) innan du följer den här guiden.

## <a name="embed-hello-engagement-reach-sdk-into-your-windows-universal-project"></a>Bädda in hello Engagement nå SDK universella Windows-projektet
Du har inte något tooadd. `EngagementReach`referenser och resurser finns redan i projektet.

> [!TIP]
> Du kan anpassa avbildningar som finns i hello `Resources` mapp i ditt projekt, särskilt hello varumärken-ikonen (som standard toohello Engagement ikon). På universella appar du kan också flytta hello `Resources` mapp på din delade projektet tooshare dess innehåll mellan appar, men du kommer att ha tookeep hello `Resources\EngagementConfiguration.xml` fil på dess ursprungliga plats eftersom den är beroende plattform.
> 
> 

## <a name="enable-hello-windows-notification-service"></a>Aktivera hello Windows Notification Service
### <a name="windows-8x-and-windows-phone-81-only"></a>Windows 8.x och Windows Phone 8.1
I ordning toouse hello **Windows Notification Service** (kallas WNS) i din `Package.appxmanifest` filen på `Application UI` klickar du på `All Image Assets` i hello bot vänstra ruta. Vid hello höger om hello i `Notifications`, ändra `toast capable` från `(not set)` för`(Yes)`.

### <a name="all-platforms"></a>Alla plattformar
Du behöver toosynchronize din app tooyour Microsoft-konto och toohello engagement plattform. För detta behöver toocreate ett konto eller logga in [windows dev center](https://dev.windows.com). Efter att skapa ett nytt program och hitta hello SID och en hemlig nyckel. På hello engagement klientdel går på din appinställningen i `native push` och klistra in dina autentiseringsuppgifter. Efter det, högerklickar du på projektet, Välj `store` och `Associate App with hello Store...`. Tooselect hello program måste du ha skapa innan toosynchronize den.

## <a name="initialize-hello-engagement-reach-sdk"></a>Initiera hello Engagement Reach SDK
Ändra hello `App.xaml.cs`:

* Infoga `EngagementReach.Instance.Init` precis efter `EngagementAgent.Instance.Init` i din `InitEngagement` metoden:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  Hej `EngagementReach.Instance.Init` körs i en dedikerad tråd. Du har inte toodo den själv.

> [!NOTE]
> Om du använder push-meddelanden någon annanstans i ditt program så att du har för[delar push-kanal](#push-channel-sharing) med Engagement nå.
> 
> 

## <a name="integration"></a>Integrering
Engagement tillhandahåller två sätt tooadd hello Reach i appen sidhuvud och mellan enskilda muskler vyer för meddelanden och avsökningar i ditt program: hello överlägg integrering och hello vyer manuell integrering. Du bör inte kombinera båda metod på hello samma sida.

hello val mellan två hello-integrering kan sammanfattas det här sättet:

* Du kan välja hello överläggsintegration om sidorna ärver redan från hello Agent `EngagementPage`, själv för att ersätta `EngagementPage` av `EngagementPageOverlay` och `xmlns:engagement="using:Microsoft.Azure.Engagement"` av `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` på sidorna.
* Du kan välja hello web vyer manuell integration om du vill tooprecisely plats hello nå Användargränssnittet i sidorna eller om du inte vill tooadd en annan arv nivå tooyour sidor. 

### <a name="overlay-integration"></a>Överläggsintegration
Hej Engagement överlägget dynamiskt lägger till hello gränssnittselement används toodisplay Reach-kampanjer på sidan. Om hello överlägget inte passar din layout bör du hello Webbvyer manuell integrering i stället.

I XAML-filen ändringen `EngagementPage` referera för`EngagementPageOverlay`

* Lägg till tooyour namnområden deklarationer:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Ersätt `engagement:EngagementPage` med `engagement:EngagementPageOverlay`:

**Med EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**Med EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Tagga sedan sidan i i filen .cs `EngagementPageOverlay` i stället för `EngagementPage` och importera `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Ersätt `EngagementPage` med `EngagementPageOverlay`:

**Med EngagementPage:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Med EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Hej Engagement överlägget lägger till en `Grid` element på sidan består av layout och hello två `WebView` delar en för hello banderoll och hello andra en för hello mellan enskilda muskler vyn.

Du kan anpassa hello överlägget element direkt i hello `EngagementPageOverlay.cs` fil.

### <a name="web-views-manual-integration"></a>Web vyer manuell integrering
Reach söker efter sidorna för hello två `WebView` element som visar hello banderoll och hello mellan enskilda muskler vyn. Hej endast du har toodo är tooadd dessa två `WebView` element någonstans i sidorna, här är ett exempel:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


I det här exemplet hello `WebView` element är sträckta toofit sina behållare som automatiskt storleken dem vid skärmen rotation fönster eller storlek ändras.

> [!WARNING]
> Det är viktigt tookeep hello samma naming `engagement_notification_content` och `engagement_announcement_content` för hello `WebView` element. Reach identifierar dem med deras namn. 
> 
> 

## <a name="handle-datapush-optional"></a>Hantera datapush (valfritt)
Om du vill att din ansökan toobe kan tooreceive Reach data push-meddelanden har tooimplement två händelser i hello EngagementReach klass:

Lägg till i App.xaml.cs i hello App() konstruktorn:

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

Ange hello innehållet i hello `EngagementReach.Instance.Handler` med din anpassade objekt i din `App.xaml.cs` klass i hello `App()` metod.

**Exempelkod:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Som standard använder Engagement implementeringen av `EngagementReachHandler`.
> Du har inte toocreate egna, och om du gör det kan du inte toooverride varje metod. hello standardbeteendet är tooselect hello Engagement basobjektet.
> 
> 

### <a name="web-view"></a>Webbvy
Som standard använder Reach hello inbäddade resurser av hello DLL toodisplay hello meddelanden och sidor.

tooprovide en fullständig anpassning möjligheter som vi använder bara webbvy. Om du vill toocustomize layouter åsidosätta direkt hello resurser filer `EngagementAnnouncement.html` och `EngagementNotification.html`. Engagement måste all kod i `<body></body>` toorun korrekt. Men du kan lägga till tagg yttre `engagement_webview_area`.

Du kan dock välja toouse egna resurser.

Du kan åsidosätta `EngagementReachHandler` metoder i din underklass tootell Engagement toouse din layout, men ta försiktighet tooembedded hello engagement mekanism:

**Exempelkod:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Som standard är AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Den representerar hello html-fil som utforma hello innehållet i ett push-meddelande (Text meddelandet, Web anoucement och avsökning meddelande). AnnouncementName är `engagement_announcement_content`. Det är hello namnet på hello webbvy design i xaml-sida.

NotfificationHTML är `ms-appx-web:///Resources/EngagementNotification.html`. Den representerar hello html-fil som utforma hello-meddelande om ett push-meddelande. NotfificationName är `engagement_notification_content`. Det är hello namnet på hello webbvy design i xaml-sida.

### <a name="customization"></a>Anpassning
Du kan anpassa meddelandet och meddelandet webbvy har du vill om du bevara Engagement objekt. Se till att objektet webbvy är beskrivs tre gånger - hello första gången i din xaml andra tidpunkt i filen .cs i hello ”setwebview()” metod, och tredje tiden i hello html-fil.

* Beskriv hello aktuella grafisk layout webbvy komponenten i din xaml.
* I filen .cs kan du definiera ”setwebview()” där du anger hello dimension hello två webbvy (meddelande, meddelande). Det är mycket effektivt när programmet hello storleksändring.
* Vi beskrivs hello webbvy innehåll, design och hello element positioner mellan varandra i hello Engagement HTML-fil.

### <a name="launch-message"></a>Starta meddelande
När en användare klickar på en Systemmeddelande (ett popup-meddelande), Engagement startar hello program, load hello innehållet i hello push-meddelanden och visar hello-sida för hello motsvarande kampanj.

Det finns en fördröjning mellan hello lanseringen av hello program och hello visningen av hello sida (beroende på nätverkets hello hastighet).

tooindicate toohello användaren som något läses in, bör du ange en visuell information, t.ex. en förloppsindikator eller en förloppsindikator. Engagement kan inte hantera sig själv, men innehåller några hanterare för dig.

tooimplement hello återanrop i App.xaml.cs i ”gemensamma App() {}” Lägg till:

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

Du kan ange hello återanrop i ”gemensamma App() {}”-metoden i din `App.xaml.cs` fil, helst före hello `EngagementReach.Instance.Init()` anropa.

> [!TIP]
> Varje hanterare anropas av hello UI-tråden. Du har inte tooworry när du använder en MessageBox eller något UI-relaterade.
> 
> 

## <a id="push-channel-sharing"></a>Push-kanal delning
Om du använder push-meddelanden för ett annat ändamål i ditt program har toouse hello push kanal delning funktion i hello Engagement SDK. Detta är tooavoid missade push.

* Du kan ange egna push kanal toohello Engagement nå initiering. hello SDK används den i stället för att begära en ny.

Uppdatera hello Engagement nå initiering med din push-kanal i hello `InitEngagement` metod från hello `App.xaml.cs` fil:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* Du kan också om du bara vill tooconsume hello push-kanal efter hello Reach initiering sedan kan du definiera ett återanrop på Engagement nå tooget hello push kanal när den har skapats av hello SDK.

Ange din återanrop var som helst **när** hello Reach-initieringen:

    /* Set action on hello SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* hello forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Anpassat schema-tips
Vi ger användning av anpassade schemat. Du kan skicka annan typ av URI från engagement klientdel toobe används i din engagement-programmet. Standardschema som `http, ftp, ...` är hantera med Windows, kommer ett fönster fråga om de är inget installerat standardprogram på enheten. Du kan också skapa egna anpassade schemat för ditt program.

hello enkelt tooset ett anpassat schema i ditt program är tooopen din `Package.appxmanifest` gå i `Declarations` panelen. Välj `Protocol` i hello tillgängliga deklarationer rulla rutan och lägga till den. Redigera hello `Name` fält med din nya protokollet önskat namn.

Nu toouse detta protokoll, redigera din `App.xaml.cs` med hello `OnActivated` -metoden och Glöm inte tooinitialize engagement här också:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action toodo when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion

