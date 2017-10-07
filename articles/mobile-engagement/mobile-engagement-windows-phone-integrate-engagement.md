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
# <a name="windows-phone-silverlight-engagement-sdk-integration"></a>Windows Phone Silverlight Engagement SDK-Integration
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Den här proceduren beskriver hello enklaste sättet tooactivate Azure Mobile Engagement analyser och övervakningsfunktionerna i Windows Phone Silverlight-program.

hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals. hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md) nedan) eftersom statistiken är programmet beroende.

## <a name="supported-versions"></a>Versioner som stöds
hello Mobile Engagement SDK för Windows Silverlight kan endast integreras i program som mål:

* Windows Phone 8.0
* Windows Phone 8.1 Silverlight

> [!NOTE]
> Om du utvecklar för Windows Phone 8.1 (utan Silverlight) finns toohello [universella Windows-integration proceduren](mobile-engagement-windows-store-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-silverlight-sdk"></a>Installera hello Mobile Engagement Silverlight SDK
hello Mobile Engagement SDK för Windows Silverlight är tillgänglig som ett Nuget-paket som kallas *MicrosoftAzure.MobileEngagement*. Du kan installera den från hello Pakethanteraren Nuget för Visual Studio. 

## <a name="add-hello-capabilities"></a>Lägger till hello-funktioner
Hej Engagement SDK måste vissa funktioner i hello Windows Phone Silverlight SDK i ordning toowork korrekt.

Öppna din `WMAppManifest.xml` filen och se till att följande funktioner hello deklareras i hello `Capabilities` panelen:

* `ID_CAP_NETWORKING`
* `ID_CAP_IDENTITY_DEVICE`

## <a name="initialize-hello-engagement-sdk"></a>Initiera hello Engagement SDK
### <a name="engagement-configuration"></a>Engagement konfiguration
Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.

Redigera den här filen toospecify:

* Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.

Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:

    /* Engagement configuration. */
    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

    /* Initialize Engagement agent with above configuration. */
    EngagementAgent.Instance.Init(engagementConfiguration);

hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.

### <a name="engagement-initialization"></a>Initieringen av engagement
När du skapar ett nytt projekt, en `App.xaml.cs` -filen har genererats. Den här klassen som ärver från `Application` och innehåller många viktiga metoder. Det kommer även att använda tooinitialize hello Engagement SDK.

Ändra hello `App.xaml.cs`:

* Lägg till tooyour `using` instruktioner:
  
      using Microsoft.Azure.Engagement;
* Infoga `EngagementAgent.Instance.Init` i hello `Application_Launching` metoden:
  
      private void Application_Launching(object sender, LaunchingEventArgs e)
      {
        EngagementAgent.Instance.Init();
      }
* Infoga `EngagementAgent.Instance.OnActivated` i hello `Application_Activated` metoden:
  
      private void Application_Activated(object sender, ActivatedEventArgs e)
      {
         EngagementAgent.Instance.OnActivated(e);
      }

> [!WARNING]
> Vi avråder du tooadd hello Engagement initiering på en annan plats för ditt program. Men tänk på att hello `EngagementAgent.Instance.Init` -metoden körs på en dedikerad tråd och inte på hello UI-tråden.
> 
> 

## <a name="basic-reporting"></a>Grundläggande reporting
### <a name="recommended-method--overload-your-phoneapplicationpage-classes"></a>Rekommenderad metod: överlagra din `PhoneApplicationPage` klasser
I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `PhoneApplicationPage` underordnade klasser ärver från hello `EngagementPage` klasser.

Här är ett exempel på hur toodo för en sida i ditt program. Du kan göra hello detsamma för alla sidor i ditt program.

#### <a name="c-source-file"></a>C# källfilen
Ändra sidan `.xaml.cs` fil:

* Lägg till tooyour `using` instruktioner:
  
      using Microsoft.Azure.Engagement;
* Ersätt `PhoneApplicationPage` med `EngagementPage` :

**Utan Engagement:**

        namespace Example
        {
          public partial class ExamplePage : PhoneApplicationPage
          {
            [...]
          }
        }

**Med Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!WARNING]
> Om sidan ärver från hello `OnNavigatedTo` metoden vara försiktig toolet hello `base.OnNavigatedTo(e)` anropa. Annars rapporteras hello aktivitet inte. Faktiskt hello `EngagementPage` anropar `StartActivity` inuti hello `OnNavigatedTo` metod.
> 
> 

#### <a name="xaml-file"></a>XAML-fil
Ändra sidan `.xaml` fil:

* Lägg till tooyour namnområden deklarationer:
  
      xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
* Ersätt `phone:PhoneApplicationPage` med `engagement:EngagementPage` :

**Utan Engagement:**

        <phone:PhoneApplicationPage>
            <!-- layout -->
        </phone:PhoneApplicationPage>

**Med Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP">

            <!-- layout -->
        </engagement:EngagementPage >

#### <a name="override-hello-default-behavior"></a>Åsidosätta standardbeteendet för hello
Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra. Om hello klassen använder hello ”Page” suffix, tas Engagement också bort.

Om du vill toooverride hello standardbeteendet för hello namn kan bara lägga till tooyour koden:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
           /* your code */
           return "new name";
        }

Om du vill tooreport vissa extra information med dina aktiviteter, kan du lägga till tooyour koden:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
           /* your code */
           return extra;
        }

De här metoderna anropas från inom hello `OnNavigatedTo` metod på sidan.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativ metod: anropa `StartActivity()` manuellt
Om du inte kan eller inte vill att toooverload din `PhoneApplicationPage` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.

Vi rekommenderar att anropa `StartActivity` i din `OnNavigatedTo` metod för din PhoneApplicationPage.

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
           base.OnNavigatedTo(e);
           EngagementAgent.Instance.StartActivity("MyPage");
        }

> [!IMPORTANT]
> Se till att du kan avsluta sessionen på rätt sätt.
> 
> hello SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs. Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` metod. Den här metoden skickar en message toohello Engagement server att hello aktuell användare har lämnat hello programmet detta påverkar alla programloggar.
> 
> 

## <a name="advanced-reporting"></a>Avancerad rapportering
Alternativt kan du tooreport programmet specifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass. Hej Engagement API kan toouse alla Engagement avancerade funktioner.

Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md).

## <a name="advanced-configuration"></a>Avancerad konfiguration
### <a name="disable-automatic-crash-reporting"></a>Inaktivera automatisk rapportering om krasch
Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement. Sedan, när ett ohanterat undantag inträffar, Engagement inte göra något.

> [!WARNING]
> Om du planerar toodisable funktionen, Tänk på att när en ohanterad krasch sker i din app Engagement inte kommer att skicka hello krascher **och** stängs inte hello-session och jobb.
> 
> 

toodisable automatisk krascher rapportering, helt enkelt anpassa konfigurationen beroende på hello sätt som du har deklarerats:

#### <a name="from-engagementconfigurationxml-file"></a>Från `EngagementConfiguration.xml` fil
Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Från `EngagementConfiguration` objekt vid körning.
Ange rapporten krascher toofalse med EngagementConfiguration-objektet.

        /* Engagement configuration. */

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration(); engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";
        /\* Disable Engagement crash reporting. \*/ engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-läge
Som standard loggas hello Engagement service-rapporter i realtid. Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”).

toodo anropa så hello-metoden:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello-argumentet är ett värde i **millisekunder**. När som helst om du vill tooreactivate hello realtid loggning kan bara anropa hello metod utan någon parameter eller med hello 0-värde.

hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas). Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s). Du har toobe medveten sparade loggarna är begränsad too300 objekt. Om det är för lång tid att skicka förlora du vissa loggar.

> [!WARNING]
> hello burst tröskelvärdet kan inte konfigureras tooa perioden som är mindre än en sekund. Om du försöker toodo så hello SDK visas en spårning med hello fel och automatiskt återställer toohello standardvärdet, det vill säga noll sekunder. Detta utlöser hello SDK tooreport hello-loggar i realtid.
> 
> 

