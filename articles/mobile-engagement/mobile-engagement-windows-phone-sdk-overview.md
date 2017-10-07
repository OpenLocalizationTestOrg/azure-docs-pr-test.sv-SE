---
title: "aaaAzure Mobile Engagement Windows Phone Silverlight SDK översikt | Microsoft Docs"
description: "Översikt över hello Windows Phone Silverlight SDK för Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 0e3d2420-0509-4952-8891-392e3dad9aaf
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
ms.openlocfilehash: ff2febed2202127e0538373ebbabe674993ce39d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="windows-phone-silverlight-sdk-overview-for-azure-mobile-engagement"></a>Windows Phone Silverlight SDK översikt för Azure Mobile Engagement
Börja här tooget hello information om hur toointegrate Azure Mobile Engagement i en Windows Phone Silverlight-App. Om du vill att toogive den ett försök att först kontrollera att du har slutfört vår [15 minuter att slutföra kursen](mobile-engagement-windows-phone-get-started.md).

Klicka på toosee hello [SDK innehåll](mobile-engagement-windows-phone-sdk-content.md)

## <a name="integration-procedures"></a>Integration procedurer
1. Börja här: [hur toointegrate Mobile Engagement i din Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement.md)
2. För aviseringar: [hur toointegrate Reach (meddelanden) i Windows Phone Silverlight-app](mobile-engagement-windows-phone-integrate-engagement-reach.md)
3. Tagga planera implementeringen: [hur toouse hello avancerade Mobile Engagement API-märkning i Windows Phone Silverlight-app](mobile-engagement-windows-phone-use-engagement-api.md)

## <a name="release-notes"></a>Viktig information
###<a name="331-11032016"></a>3.3.1 (11/03/2016)
En del av hello *MicrosoftAzure.MobileEngagement* Nuget-paketet **v3.4.1**

* Stabilitetsförbättringar.

Tidigare version finns hello [slutföra viktig information](mobile-engagement-windows-phone-release-notes.md)

## <a name="upgrade-procedures"></a>Uppgraderingsprocesser
Om du redan har integrerat en äldre version av våra SDK i ditt program, har du tooconsider hello följande punkter när du uppgraderar hello SDK.

Du kan ha toofollow flera procedurer om du har missat flera versioner av hello SDK. Se hello fullständig [uppgradera procedurer](mobile-engagement-windows-phone-upgrade-procedure.md). Till exempel om du migrerar från 0.10.1 too0.11.0 som du har toofirst följer hello ”från 0.9.0 too0.10.1” proceduren sedan hello ”från 0.10.1 too0.11.0” proceduren.

### <a name="from-200-too330"></a>Från 2.0.0 too3.3.0
#### <a name="test-logs"></a>Testa loggar
Konsolen loggar som genereras av hello SDK kan nu vara aktiverat/inaktiverat/filtreras. toocustomize, uppdatera hello egenskapen `EngagementAgent.Instance.TestLogEnabled` tooone hello-värde som är tillgängliga från hello `EngagementTestLogLevel` uppräkning, till exempel:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

### <a name="upgrade-from-older-versions"></a>Uppgradera från tidigare versioner
Se [uppgradera procedurer](mobile-engagement-windows-phone-upgrade-procedure.md)

