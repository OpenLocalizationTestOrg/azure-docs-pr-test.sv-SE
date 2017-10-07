---
title: aaaWindows universella appar SDK uppgradera procedurer
description: "Universella Windows-appar SDK uppgradera procedurer för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 4c898175-2cd6-43db-b350-bb408332f24d
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 95aba5d55cd65d4190aad35737f872414b5ed443
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-sdk-upgrade-procedures"></a>Uppgradera procedurer för universella Windows-appar SDK
Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK. Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.

## <a name="from-330-too340"></a>Från 3.3.0 too3.4.0
### <a name="test-logs"></a>Testa loggar
Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras. toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="resources"></a>Resurser
hello Reach överlägget har förbättrats. Det är en del av hello SDK NuGet-paketet resurser.

Du kan välja om du vill tookeep befintliga filer från hello överlägg mappen för dina resurser eller inte under uppgraderingen toohello ny version av hello SDK:

* Om hello tidigare överlägget fungerar för dig eller du integrerar hello `WebView` element manuellt och sedan kan du bestämma tookeep dina befintliga filer, den fortfarande fungerar. 
* Om du vill tooupdate toohello nya överlägget, bara Ersätt hello hela `overlay` mapp från dina resurser med hello ny från hello SDK-paketet (UWP-appar: efter hello uppgraderingen kan du få hello ny överlägget mapp från % USERPROFILE %\\. nuget\ packages\MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Med hjälp av hello nya överlägget skrivs eventuella anpassningar på hello föregående version.
> 
> 

## <a name="from-320-too330"></a>Från 3.2.0 too3.3.0
### <a name="resources"></a>Resurser
Det här steget gäller anpassade resurser. Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.

## <a name="from-310-too320"></a>Från 3.1.0 too3.2.0
### <a name="resources"></a>Resurser
Det här steget gäller anpassade resurser. Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.

### <a name="webview-integration"></a>Webbvy integrering
Vissa förbättringar toomatch annan enhet formfaktorer introducerades i den här versionen. Se till att din integrering av hello webbvy matchar hello följande:

I din XAML sida ():

            <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Right" VerticalAlignment="Top"/>
            <WebView x:Name="engagement_announcement_content" Visibility="Collapsed" HorizontalAlignment="Right" VerticalAlignment="Top"/> 

Och tillhörande .cs-fil:

    using Microsoft.Azure.Engagement;
    using System;
    using Windows.ApplicationModel.Core;
    using Windows.UI.ViewManagement;
    using Windows.UI.Xaml;
    using Windows.UI.Xaml.Navigation;

    namespace My.Namespace.Example
    {
            /// <summary>
            /// An empty page that can be used on its own or navigated toowithin a Frame.
            /// </summary>
            public sealed partial class ExampleEngagementReachPage : EngagementPage
            {
              public ExampleEngagementReachPage()
              {
                this.InitializeComponent();

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

              #region tooimplement
              /* Attach events when page is navigated. */
              protected override void OnNavigatedTo(NavigationEventArgs e)
              {
                /* Update hello webview when hello app window is resized. */
                Window.Current.SizeChanged += DisplayProperties_OrientationChanged;

                /* Update hello webview when hello app/status bar is resized. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged += DisplayProperties_VisibleBoundsChanged; 
    #endif
                base.OnNavigatedTo(e);
              }

              /* When page is left ensure toodetach SizeChanged handler. */
              protected override void OnNavigatedFrom(NavigationEventArgs e)
              {
                Window.Current.SizeChanged -= DisplayProperties_OrientationChanged;
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
                ApplicationView.GetForCurrentView().VisibleBoundsChanged -= DisplayProperties_VisibleBoundsChanged;
    #endif
                base.OnNavigatedFrom(e);
              }

              /* "width" and "height" are hello current size of your application display. */
    #if WINDOWS_PHONE_APP || WINDOWS_UWP
              double width = ApplicationView.GetForCurrentView().VisibleBounds.Width;
              double height = ApplicationView.GetForCurrentView().VisibleBounds.Height;
    #else
              double width =  Window.Current.Bounds.Width;
              double height =  Window.Current.Bounds.Height;
    #endif

              /// <summary>
              /// Set your webview elements toohello correct size.
              /// </summary>
              /// <param name="width">hello width of your current display.</param>
              /// <param name="height">hello height of your current display.</param>
              private void SetWebView(double width, double height)
              {
                #pragma warning disable 4014
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal,
                        () =>
                        {
                          this.engagement_notification_content.Width = width;
                          this.engagement_announcement_content.Width = width;
                          this.engagement_announcement_content.Height = height;
                        });
              }

              /// <summary>
              /// Handler that takes hello Windows.Current.SizeChanged and indicates that webviews have toobe resized.
              /// </summary>
              /// <param name="sender">Original event trigger.</param>
              /// <param name="e">Window Size Changed Event arguments.</param>
              private void DisplayProperties_OrientationChanged(object sender, Windows.UI.Core.WindowSizeChangedEventArgs e)
              {
                double width = e.Size.Width;
                double height = e.Size.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }

    #if WINDOWS_PHONE_APP || WINDOWS_UWP              
              /// <summary>
              /// Handler that takes hello ApplicationView.VisibleBoundsChanged and indicates that webviews have toobe resized
              /// </summary>
              /// <param name="sender">hello related application view.</param>
              /// <param name="e">Related event arguments.</param>
              private void DisplayProperties_VisibleBoundsChanged(ApplicationView sender, Object e)
              {
                double width = sender.VisibleBounds.Width;
                double height = sender.VisibleBounds.Height;

                /* Set your webview elements toohello correct size. */
                SetWebView(width, height);
              }
    #endif
              #endregion
            }
    }

## <a name="from-200-too300"></a>Från 2.0.0 too3.0.0
### <a name="resources"></a>Resurser
Det här steget gäller anpassade resurser. Om du har anpassat hello-resurser som tillhandahålls av hello SDK (html, bilder, överlägget) och du har toobackup dem innan du uppgraderar och håll anpassningen på uppgraderas resurser.

## <a name="from-111-too200"></a>Från 1.1.1 too2.0.0
hello beskrivs nedan hur toomigrate SDK-integration från hello Capptain tjänsten erbjuds av Capptain SAS i en app med Azure Mobile Engagement. 

> [!IMPORTANT]
> Capptain och Mobile Engagement hello inte samma tjänster och hello proceduren endast anges nedan visar hur toomigrate hello-klientappen. Migrera hello SDK i hello app kommer inte att migrera data från hello Capptain servrar toohello Mobile Engagement-servrar
> 
> 

Om du migrerar från en tidigare version kan du kontakta hello Capptain webbplats toomigrate too1.1.1 först och sedan använda hello sätt

### <a name="nuget-package"></a>Nuget-paketet
Ersätt **Capptain.WindowsPhone** av **MicrosoftAzure.MobileEngagement** Nuget-paketet.

### <a name="applying-mobile-engagement"></a>Tillämpa Mobile Engagement
hello SDK använder hello termen `Engagement`. Du behöver tooupdate projekt-toomatch ändringen.

Du måste toouninstall aktuella Capptain nuget-paketet. Överväg att alla dina ändringar i mappen Capptain resurser tas bort. Om du vill tookeep göra en kopia av dem sedan i dessa filer.

Efter det att installera hello nya Microsoft Azure Engagement nuget-paketet på ditt projekt. Du hittar den direkt på [nuget webbplats]. eller här index. Den här åtgärden ersätter alla resurser filer som används av Engagement och lägger till hello nya Engagement DLL tooyour projektet refererar till.

Du har tooclean projektreferenserna genom att ta bort Capptain DLL-referenser. Om du inte gör detta hello version av Capptain kommer i konflikt och fel händer.

Om du har anpassat Capptain resurser, kopiera ditt gamla filer innehåll och klistra in dem i hello nya Engagement filer. Observera att både xaml och cs-filer har toobe uppdateras.

När de steg har du bara tooreplace gamla Capptain referenser från hello nya Engagement referenser.

1. Alla Capptain namnområden har toobe uppdateras.
   
    Före migrering:
   
        using Capptain.Agent;
        using Capptain.Reach;
   
    Efter migreringen:
   
        using Microsoft.Azure.Engagement;
2. Alla Capptain klasser som innehåller ”Capptain” ska innehålla ”Engagement”.
   
    Före migrering:
   
        public sealed partial class MainPage : CapptainPage
        {
          protected override string GetCapptainPageName()
          {
            return "Capptain Demo";
          }
          ...
        }
   
    Efter migreringen:
   
        public sealed partial class MainPage : EngagementPage
        {
          protected override string GetEngagementPageName()
          {
            return "Engagement Demo";
          }
          ...
        }
3. För xaml-filer Capptain namnområde och attribut även ändras.
   
    Före migrering:
   
        <capptain:CapptainPage
        ...
        xmlns:capptain="clr-namespace:Capptain.Agent;assembly=Capptain.Agent.WP"
        ...
        </capptain:CapptainPage>
   
    Efter migreringen:
   
        <engagement:EngagementPage
        ...
        xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
        ...
        </engagement:EngagementPage>
4. Överlägget sidan ändringar
   
   > [!IMPORTANT]
   > Överlägget ändras. Det nya namnområdet är `Microsoft.Azure.Engagement.Overlay`. Den har toobe som används i xaml- och cs-filer. Dessutom `CapptainGrid` heter toobe `EngagementGrid`, `capptain_notification_content` och `capptain_announcement_content` namnges `engagement_notification_content` och `engagement_announcement_content`.
   > 
   > 
   
    För överlägget:
   
        <capptain:CapptainPageOverlay
          xmlns:capptain="using:Capptain.Overlay"
          ...
        </capptain:CapptainPageOverlay>
   
    Blir den:
   
        <EngagementPageOverlay
          engagement="using:Microsoft.Azure.Engagement.Overlay"
          ...
        </engagement:EngagementPageOverlay>
5. För hello märke andra resurser som Capptain bilder och HTML-filer, Lägg till att de också har bytt namn till toouse ”Engagement”.

### <a name="project-declaration"></a>Deklarationen för projektet
På Package.appxmanifest `File Type Associations` har uppdaterats från:

* capptain\_nå\_innehåll tooengagement\_nå\_innehåll
* capptain\_loggen\_filen tooengagement\_loggen\_fil

### <a name="application-id--sdk-key"></a>Program-ID / SDK-nyckeln
Engagement använder en anslutningssträng. Du saknar toospecify ett program-ID och SDK-nyckeln med Mobile Engagement har endast toospecify en anslutningssträng. Du kan konfigurera det på EngagementConfiguration-fil.

Hej Engagement konfiguration kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.

Redigera den här filen toospecify:

* Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.

Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(args, engagementConfiguration);

hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.

### <a name="items-name-change"></a>Ändring av objekt
Alla objekt med namnet *capptain* namngetts *engagement*. På liknande sätt för *Capptain* för*Engagement*.

Exempel på vanliga Capptain objekt:

* CapptainConfiguration nu namnet EngagementConfiguration
* CapptainAgent nu namnet EngagementAgent
* CapptainReach nu namnet EngagementReach
* CapptainHttpConfig nu namnet EngagementHttpConfig
* GetCapptainPageName nu namnet GetEngagementPageName

Observera att byta namn på också påverkar åsidosätts metoder.

