---
title: aaaWindows universella appar Engagement SDK-Integration
description: Hur tooIntegrate Azure Mobile Engagement med universella Windows-appar
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 71236b68-5ebd-44aa-8c82-c7ca8098ea05
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 18543798099c233dbe55cc387ba0216e369c8596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-apps-engagement-sdk-integration"></a>Windows Universal-appar Engagement SDK-Integration
> [!div class="op_single_selector"]
> * [Universell Windows](mobile-engagement-windows-store-integrate-engagement.md) 
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md) 
> * [iOS](mobile-engagement-ios-integrate-engagement.md) 
> * [Android](mobile-engagement-android-integrate-engagement.md) 
> 
> 

Den här proceduren beskriver hello enklaste sättet tooactivate Engagement analyser och övervakningsfunktionerna i ditt universella Windows-program.

hello följande är tillräckligt med tooactivate hello loggar behövs en rapport för toocompute all statistik om användare, sessioner, aktiviteter, krascher och Technicals. hello loggar behövs en rapport toocompute annan statistik som händelser, fel och jobb måste göras manuellt med hjälp av hello Engagement API (se [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md) eftersom statistiken är programberoende.

## <a name="supported-versions"></a>Versioner som stöds
hello Mobile Engagement SDK för universella Windows-appar kan endast integreras i Windows Runtime och Uwp-tillämpningar för:

* Windows 8
* Windows 8.1
* Windows Phone 8.1
* Windows 10 (familjer stationär dator och mobil)

> [!NOTE]
> Om du utvecklar för Windows Phone Silverlight finns toohello [Windows Phone Silverlight-integrering proceduren](mobile-engagement-windows-phone-integrate-engagement.md).
> 
> 

## <a name="install-hello-mobile-engagement-universal-apps-sdk"></a>Installera hello Mobile Engagement-SDK Universal-appar
### <a name="all-platforms"></a>Alla plattformar
hello Mobile Engagement SDK för Windows Universal-appen är tillgänglig som ett Nuget-paket som kallas *MicrosoftAzure.MobileEngagement*. Du kan installera den från hello Pakethanteraren Nuget för Visual Studio.

### <a name="windows-8x-and-windows-phone-81"></a>Windows 8.x och Windows Phone 8.1
NuGet automatiskt distribuerar hello SDK resurser i hello `Resources` hello rotmapp från projektet program.

### <a name="windows-10-universal-windows-platform-applications"></a>Windows 10 Universal Windows Platform program
NuGet distribuerar inte automatiskt hello SDK-resurser i ditt program för UWP ännu. Du har det manuellt tills resursdistribution sätts in i NuGet toodo:

1. Öppna i Utforskaren.
2. Navigera toohello följande plats (**x.x.x** är hello version av Engagement som du installerar): *% USERPROFILE %\\.nuget\packages\MicrosoftAzure.MobileEngagement\\*  *x.x.x**\\content\win81*
3. Dra och släpp hello **resurser** mapp från hello file explorer toohello roten för ditt projekt i Visual Studio.
4. Välj ditt projekt i Visual Studio och aktivera hello **visa alla filer** ikonen ovanpå hello **Solution Explorer**.
5. Vissa filer ingår inte i hello-projekt. tooimport dem på samma gång högerklickar du på hello **resurser** mappen **undantas från projektet** och sedan en annan högerklickar du på hello **resurser** mappen **inkludera i projektet** toore-inkludera hello hela mappen. Alla filer från hello **resurser** mappen ingår nu i ditt projekt.

## <a name="add-hello-capabilities"></a>Lägger till hello-funktioner
Hej Engagement SDK måste vissa funktioner i hello Windows SDK i ordning toowork korrekt.

Öppna din `Package.appxmanifest` fil- och måste deklareras som hello följande funktioner:

* `Internet (Client)`

## <a name="initialize-hello-engagement-sdk"></a>Initiera hello Engagement SDK
### <a name="engagement-configuration"></a>Engagement konfiguration
Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet.

Redigera den här filen toospecify:

* Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.

Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);

hello anslutningssträngen för ditt program visas på hello klassiska Azure-portalen.

### <a name="engagement-initialization"></a>Initieringen av engagement
När du skapar ett nytt projekt, en `App.xaml.cs` -filen har genererats. Den här klassen som ärver från `Application` och innehåller många viktiga metoder. Det kommer även att använda tooinitialize hello Engagement SDK.

Ändra hello `App.xaml.cs`:

* Lägg till tooyour `using` instruktioner:
  
      using Microsoft.Azure.Engagement;
* Definiera metoden tooshare hello Engagement initiera en gång för alla samtal:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
  
        // or
  
        EngagementAgent.Instance.Init(e, engagementConfiguration);
      }
* Anropa `InitEngagement` i hello `OnLaunched` metoden:
  
      protected override void OnLaunched(LaunchActivatedEventArgs e)
      {
        InitEngagement(e);
      }
* När programmet startas med ett anpassat schema, ett annat program eller hello kommandoraden sedan hello `OnActivated` metoden anropas. Du måste också tooinitiate hello Engagement SDK när appen är aktiverad. toodo så åsidosätta `OnActivated` metoden:
  
      protected override void OnActivated(IActivatedEventArgs args)
      {
        InitEngagement(args);
      }

> [!IMPORTANT]
> Vi avråder du tooadd hello Engagement initiering på en annan plats för ditt program.
> 
> 

## <a name="basic-reporting"></a>Grundläggande reporting
### <a name="recommended-method-overload-your-page-classes"></a>Rekommenderad metod: överlagra din `Page` klasser
I ordning tooactivate hello rapport med alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, kan du bara göra alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klasser.

Här är ett exempel på hur toodo för en sida i ditt program. Du kan göra hello detsamma för alla sidor i ditt program.

#### <a name="c-source-file"></a>C# källfilen
Ändra sidan `.xaml.cs` fil:

* Lägg till tooyour `using` instruktioner:
  
      using Microsoft.Azure.Engagement;
* Ersätt `Page` med `EngagementPage`:

**Utan Engagement:**

        namespace Example
        {
          public sealed partial class ExamplePage : Page
          {
            [...]
          }
        }

**Med Engagement:**

        using Microsoft.Azure.Engagement;

        namespace Example
        {
          public sealed partial class ExamplePage : EngagementPage 
          {
            [...]
          }
        }

> [!IMPORTANT]
> Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`. Annars hello aktivitet rapporteras inte (hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).
> 
> 

#### <a name="xaml-file"></a>XAML-fil
Ändra sidan `.xaml` fil:

* Lägg till tooyour namnområden deklarationer:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement"
* Ersätt `Page` med `engagement:EngagementPage`:

**Utan Engagement:**

        <Page>
            <!-- layout -->
            ...
        </Page>

**Med Engagement:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">
            <!-- layout -->
            ...
        </engagement:EngagementPage >

#### <a name="override-hello-default-behaviour"></a>Åsidosätt hello standard beteende
Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra. Om hello klassen använder hello ”Page” suffix, tas Engagement också bort.

Om du vill toooverride hello standard beteendet för hello namn kan bara lägga till tooyour koden:

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
Om du inte kan eller inte vill att toooverload din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.

Vi rekommenderar att toocall `StartActivity` i din `OnNavigatedTo` metod på sidan.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Se till att du kan avsluta sessionen på rätt sätt.
> 
> hello Windows Universal SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs. Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` -metoden den här metoden skickar tooEngagement servern att aktuell användare har lämna hello program, då påverkar alla programloggar.
> 
> 

## <a name="advanced-reporting"></a>Avancerad rapportering
Alternativt kan du tooreport programmet specifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass. Hej Engagement API kan toouse alla Engagement avancerade funktioner.

Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).

## <a name="advanced-configuration"></a>Avancerad konfiguration
### <a name="disable-automatic-crash-reporting"></a>Inaktivera automatisk rapportering om krasch
Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement. Sedan, när ett ohanterat undantag inträffar, Engagement inte göra något.

> [!WARNING]
> Om du planerar toodisable funktionen, Tänk på att när en ohanterad krasch sker i din app Engagement inte kommer att skicka hello krascher **och** inte och kommer att avslutas sessionen hello jobb.
> 
> 

toodisable automatisk krascher rapportering, helt enkelt anpassa konfigurationen beroende på hello sätt som du har deklarerats:

#### <a name="from-engagementconfigurationxml-file"></a>Från `EngagementConfiguration.xml` fil
Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Från `EngagementConfiguration` objekt vid körning.
Ange rapporten krascher toofalse med EngagementConfiguration-objektet.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="burst-mode"></a>Burst-läge
Som standard loggas hello Engagement service-rapporter i realtid. Om ditt program rapporterar loggar väldigt ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en återkommande tid för grundläggande (Detta kallas hello ”burst-läget”).

toodo anropa så hello-metoden:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello-argumentet är ett värde i **millisekunder**. När som helst om du vill tooreactivate hello realtid loggning kan bara anropa hello metod utan någon parameter eller med hello 0-värde.

hello burst läge öka något hello batteri livslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb kommer att vara avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas). Det är rekommenderat toouse ett tröskelvärde i burst 30000 (30s). Du har toobe medveten sparade loggarna är begränsad too300 objekt. Om det är för lång tid att skicka förlora du vissa loggar.

> [!WARNING]
> hello burst tröskelvärdet kan inte konfigureras tooa perioden som är mindre än 1s. Om du försöker toodo så återställa toohello standardvärdet, dvs 0s hello SDK visas en spårning med hello fel och automatiskt. Detta utlöser hello SDK tooreport hello-loggar i realtid.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview

