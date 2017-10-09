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
# <a name="windows-phone-silverlight-sdk-upgrade-procedures"></a>Windows Phone Silverlight SDK Uppgraderingsprocesser
Om du redan har integrerat en äldre version av våra SDK i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK. Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.

## <a name="from-200-too330"></a>Från 2.0.0 too3.3.0
### <a name="test-logs"></a>Testa loggar
Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras. toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

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

Efter det att installera hello nya Microsoft Azure Engagement nuget-paketet på ditt projekt. Du kan hitta direkt på [Nuget](http://www.nuget.org/packages/MicrosoftAzure.MobileEngagement). Den här åtgärden ersätter alla resurser filer som används av Engagement och lägger till hello nya Engagement DLL tooyour projektet refererar till.

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
4. För hello märke andra resurser som Capptain bilder, Lägg till att de också har bytt namn till toouse ”Engagement”.

### <a name="application-id--sdk-key"></a>Program-ID / SDK-nyckeln
Engagement använder en anslutningssträng. Du saknar toospecify ett program-ID och SDK-nyckeln med Mobile Engagement har endast toospecify en anslutningssträng. Du kan konfigurera det på EngagementConfiguration-fil.

Hej Engagement konfiguration kan anges i din `Resources\EngagementConfiguration.xml` -filen för ditt projekt.

Redigera den här filen toospecify:

* Anslutningssträngen program mellan taggarna `<connectionString>` och `<\connectionString>`.

Om du vill toospecify den vid körning i stället kan du anropa hello följande metod innan hello Engagement agentinitieringen:

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Initialize Engagement angent with above configuration. */
        EngagementAgent.Instance.Init(engagementConfiguration);

hello anslutningssträngen för ditt program visas i hello klassiska Azure-portalen.

### <a name="items-name-change"></a>Ändring av objekt
Alla objekt med namnet *capptain* namngetts *engagement*. På liknande sätt för *Capptain* för*Engagement*.

Exempel på vanliga Capptain objekt:

* CapptainConfiguration nu namnet EngagementConfiguration
* CapptainAgent nu namnet EngagementAgent
* CapptainReach nu namnet EngagementReach
* CapptainHttpConfig nu namnet EngagementHttpConfig
* GetCapptainPageName nu namnet GetEngagementPageName

Observera att byta namn på också påverkar åsidosätts metoder.

