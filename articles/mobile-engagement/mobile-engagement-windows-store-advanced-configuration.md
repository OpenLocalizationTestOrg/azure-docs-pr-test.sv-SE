---
title: "aaaAdvanced konfiguration för Windows Universal-appar Engagement SDK"
description: "Avancerade konfigurationsalternativ för Azure Mobile Engagement med universella Windows-appar"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 23bd05012bc25d438d8d4985a112280bed0292b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Avancerad konfiguration för Windows Universal-appar Engagement SDK
> [!div class="op_single_selector"]
> * [Universell Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Den här proceduren beskriver hur tooconfigure olika konfigurationsalternativ för Azure Mobile Engagement Android-appar.

## <a name="prerequisites"></a>Krav
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Avancerad konfiguration
### <a name="disable-automatic-crash-reporting"></a>Inaktivera automatisk rapportering om krasch
Du kan inaktivera hello automatisk krascher reporting-funktionen i Engagement. Sedan, när ett ohanterat undantag inträffar Engagement ingenting.

> [!WARNING]
> Om du inaktiverar den här funktionen, så när ett ohanterat kraschar i din app Engagement inte skicka hello krascher **och** stängs inte hello-session och jobb.
> 
> 

toodisable automatisk krascher rapportering, anpassa konfigurationen beroende på hello sätt som du har deklarerats:

#### <a name="from-engagementconfigurationxml-file"></a>Från `EngagementConfiguration.xml` fil
Ställa in rapporten krasch också`false` mellan `<reportCrash>` och `</reportCrash>` taggar.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Från `EngagementConfiguration` objekt vid körning.
Ange rapporten krascher toofalse med EngagementConfiguration-objektet.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Inaktivera rapportering i realtid
Som standard loggas hello Engagement service-rapporter i realtid. Om ditt program rapporterar loggar ofta, är det bättre toobuffer hello loggar och tooreport dem samtidigt på en vanlig tidsschema. Detta kallas ”burst läge”.

toodo anropa så hello-metoden:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

hello-argumentet är ett värde i **millisekunder**. När du vill tooreactivate hello realtid loggning anropa hello metod utan någon parameter eller med hello 0-värde.

Burst läge något ökar hello batterilivslängd men påverkar hello Engagement-Övervakare: alla sessioner och jobb varaktighet är avrundade toohello burst tröskelvärdet (alltså sessioner och jobb som är kortare än hello burst tröskelvärdet inte kanske visas). Vi rekommenderar att du använder ett tröskelvärde i burst 30000 (30s). Sparade loggarna är begränsad too300 objekt. Om skickar är för lång kan förlora du vissa loggar.

> [!WARNING]
> hello burst tröskelvärde får inte vara konfigurerade tooa perioden som är mindre än en sekund. Om du gör det visas en spårning med hello fel hello SDK och återställer automatiskt toohello standardvärdet noll sekunder. Den här utlösare hello SDK tooreport hello loggar i realtid.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
