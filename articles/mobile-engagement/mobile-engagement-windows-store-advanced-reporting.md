---
title: aaaWindows Universal avancerad rapportering med MobileApps Engagement
description: Hur tooIntegrate Azure Mobile Engagement med universella Windows-appar
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
ms.openlocfilehash: 20968f238ef7ae9dc0b8bb6dac3fb8bdb9bc3a10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-hello-windows-universal-apps-engagement-sdk"></a>Avancerade rapportering med hello Windows Universal-appar Engagement SDK
> [!div class="op_single_selector"]
> * [Universell Windows](mobile-engagement-windows-store-advanced-reporting.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Det här avsnittet beskrivs ytterligare rapporteringsscenarier i ditt universella Windows-program. Dessa scenarier innehåller alternativ som du kan välja tooapply toohello app som skapats i hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) kursen.

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

Innan du påbörjar de här självstudierna måste du först slutföra hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) självstudier, vilket är avsiktligt direkt och enkel. Den här kursen ingår ytterligare alternativ som du kan välja bland.

## <a name="specifying-engagement-configuration-at-runtime"></a>Ange engagement konfiguration vid körning
Hej Engagement configuration centraliserad i hello `Resources\EngagementConfiguration.xml` filen i projektet, vilket är där det har angetts i hello [komma igång](mobile-engagement-windows-store-dotnet-get-started.md) avsnittet.

Men du kan också ange det vid körning: du kan anropa hello följa metoden innan hello Engagement agentinitieringen:

          /* Engagement configuration. */
          EngagementConfiguration engagementConfiguration = new EngagementConfiguration();

          /* Set hello Engagement connection string. */
          engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

          /* Initialize Engagement angent with above configuration. */
          EngagementAgent.Instance.Init(e, engagementConfiguration);



## <a name="recommended-method-overload-your-page-classes"></a>Rekommenderad metod: överlagra din `Page` klasser
tooactivate hello rapportering av alla hello-loggar som krävs av Engagement toocompute användare, sessioner, aktiviteter, krascher och tekniska statistik, se alla dina `Page` underordnade klasser ärver från hello `EngagementPage` klasser.

Här är ett exempel på en sida av ditt program. Du kan göra hello detsamma för alla sidor i ditt program.

### <a name="c-source-file"></a>C# källfilen
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
> Om sidan åsidosätter hello `OnNavigatedTo` metoden vara säker på att toocall `base.OnNavigatedTo(e)`. Annars hello aktivitet inte är rapporteras (hello `EngagementPage` anrop `StartActivity` i dess `OnNavigatedTo` metod).
> 
> 

### <a name="xaml-file"></a>XAML-fil
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

### <a name="override-hello-default-behaviour"></a>Åsidosätt hello standard beteende
Som standard rapporteras hello klassnamnet för hello sida som hello aktivitetsnamn med utan extra. Om hello klassen använder hello ”Page” suffix, Engagement tar bort den.

toooverride hello standardbeteendet för hello namn, Lägg till den här koden:

        // in hello .xaml.cs file
        protected override string GetEngagementPageName()
        {
          /* your code */
          return "new name";
        }

tooreport extra information med din aktivitet, lägga till den här koden:

        // in hello .xaml.cs file
        protected override Dictionary<object,object> GetEngagementPageExtra()
        {
          /* your code */
          return extra;
        }

De här metoderna anropas från inom hello `OnNavigatedTo` metod på sidan.

### <a name="alternate-method-call-startactivity-manually"></a>Alternativ metod: anropa `StartActivity()` manuellt
Om du inte kan eller inte vill att toooverload din `Page` klasser, i stället kan du starta dina aktiviteter genom att anropa `EngagementAgent` metoder direkt.

Vi rekommenderar att anropa `StartActivity` i din `OnNavigatedTo` metod på sidan.

            protected override void OnNavigatedTo(NavigationEventArgs e)
            {
              base.OnNavigatedTo(e);
              EngagementAgent.Instance.StartActivity("MyPage");
            }

> [!IMPORTANT]
> Se till att du kan avsluta sessionen på rätt sätt.
> 
> hello Windows Universal SDK automatiskt anropar hello `EndActivity` metod när hello programmet stängs. Därför är det **hög** rekommenderas toocall hello `StartActivity` metod när hello aktiviteten hos hello användaren ändrar och för**aldrig** anrop hello `EndActivity` metod. Den här metoden meddelar hello Engagement server att hello den aktuella användaren har lämnat hello programmet, vilket påverkar alla programloggar.
> 
> 

## <a name="advanced-reporting"></a>Avancerad rapportering
Alternativt kan du tooreport programspecifika händelser, fel och jobb, toodo så, Använd hello andra metoder hittades i hello `EngagementAgent` klass. Hej Engagement API kan användas av alla Engagement avancerade funktioner.

Mer information finns i [hur toouse hello avancerade Mobile Engagement API-märkning i appen Windows Universal](mobile-engagement-windows-store-use-engagement-api.md).

