---
title: aaaAzure Mobile Engagement Windows Universal SDK Integration | Microsoft Docs
description: "Windows Universal-integrering för SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 9ded187d-5c07-4377-a41c-ce205dd38b50
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: 2f88e58adb349a2a4eb43b0f182f99b3e8b8cfd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-universal-sdk-integration-for-azure-mobile-engagement"></a>Windows Universal SDK-Integration för Azure Mobile Engagement
Det här dokumentet beskriver alla hello integrering och vilka konfigurationsalternativ tillgängliga för hello Azure Mobile Engagement Windows Universal SDK.

## <a name="prerequisites"></a>Krav
Innan du påbörjar de här självstudierna måste du först slutföra vår [15 minuter kursen](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="advanced-features"></a>Avancerade funktioner
### <a name="reporting-features"></a>Funktioner för rapportering
Du kan lägga till dessa funktioner:

1. [Avancerade alternativ för rapportering](mobile-engagement-windows-store-advanced-reporting.md)
2. [Avancerade konfigurationsalternativ](mobile-engagement-windows-store-advanced-configuration.md)

### <a name="notifications"></a>Meddelanden
[Hur toointegrate Reach (meddelanden) i din universella Windows-app](mobile-engagement-windows-store-integrate-engagement-reach.md)

### <a name="tag-plan-implementation"></a>Taggen planera implementering:
[Hur toouse hello avancerade Mobile Engagement API-märkning i din universella Windows-app](mobile-engagement-windows-store-use-engagement-api.md)

## <a name="release-notes"></a>Viktig information
### <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Stabilitetsförbättringar.

Tidigare versioner finns hello [slutföra viktig information](mobile-engagement-windows-store-release-notes.md)

## <a name="upgrade-procedures"></a>Uppgraderingsprocesser
Om du redan har integrerat en äldre version av Engagement i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Om du har missat flera versioner av hello SDK kan du ha toofollow flera procedurer. Se hello fullständig [uppgradera procedurer](mobile-engagement-windows-store-upgrade-procedure.md). Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.

### <a name="from-330-too340"></a>Från 3.3.0 too3.4.0
#### <a name="test-logs"></a>Testa loggar
Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras. toocustomize uppdatering hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värden som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, exempelvis:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

#### <a name="resources"></a>Resurser
hello Reach överlägget har förbättrats. Det är en del av hello SDK NuGet-paketet resurser.

Du kan välja om du vill tookeep befintliga filer från hello överlägg mappen för dina resurser eller inte under uppgraderingen toohello ny version av hello SDK:

* Om hello tidigare överlägget fungerar för dig, eller du integrerar hello `WebView` element manuellt, sedan kan du bestämma tookeep din avslutas filer, fungerar fortfarande.
* tooupdate toohello nya överlägget, Ersätt hello hela `overlay` mapp från dina resurser med hello ny från hello SDK-paketet (UWP-appar: efter hello uppgraderingen kan du få hello ny överlägget mapp från % USERPROFILE %\\.nuget\packages\ MicrosoftAzure.MobileEngagement\3.4.0\content\win81\Resources).

> [!WARNING]
> Med hjälp av hello nya överlägget skriver över eventuella anpassningar på hello föregående version.
> 
> 

### <a name="upgrade-from-older-versions"></a>Uppgradera från tidigare versioner
Se [uppgradera procedurer](mobile-engagement-windows-store-upgrade-procedure.md)

