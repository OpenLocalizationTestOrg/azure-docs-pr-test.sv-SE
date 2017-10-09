---
title: aaaAlerts verifiering i Azure Security Center | Microsoft Docs
description: "Det här dokumentet hjälper dig att toovalidate hello säkerhetsaviseringar i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 030e9e74303758192eedaf517f1cb0d2e4a7852e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="alerts-validation-in-azure-security-center"></a>Aviseringsverifiering i Azure Security Center
Det här dokumentet hjälper dig att lära dig hur tooverify om systemet har konfigurerats korrekt för Azure Security Center-aviseringar.

## <a name="what-are-security-alerts"></a>Vad är säkerhetsaviseringar?
Security Center automatiskt samlar in, analyseras och integreras loggdata från din Azure-resurser och hello nätverk anslutna partnerlösningar som brandväggen och endpoint protection-lösningar, toodetect och aviseringen du toothreats. Läs [hantering och svarar toosecurity aviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts) för mer information om säkerhetsaviseringar och Läs [förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) toolearn mer Om hello olika typer av aviseringar.

## <a name="alert-validation"></a>Aviseringsverifiering
Security Center-agenten är installerad på datorn gör när hello nedan från hello dator där du vill att toobe hello angripna resursen hello avisering:

1. Kopiera ett körbart (till exempel calc.exe) toohello datorns skrivbord eller andra katalogen för din bekvämlighet.
2. Byt namn på den här filen för**ASC_AlertTest_662jfi039N.exe**.
3. Öppna hello-kommandotolk och kör den här filen med ett argument (bara en falsk argumentnamnet), exempelvis: *ASC_AlertTest_662jfi039N.exe - foo*
4. Vänta 5 minuter för too10 och öppna Security Center-aviseringar. Det bör du hitta en avisering liknande toofollowing en:

    ![Aviseringsverifiering](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

När du granskar den här aviseringen, kontrollera hello fältet argument granskning aktiverats visas som true. Om det är false, måste tooenable kommandoradsargument granskning. Du kan aktivera det här alternativet använder hello följande kommandorad:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


## <a name="see-also"></a>Se även
Den här artikeln introduceras toohello aviseringar verifieringsprocessen. Nu när du är bekant med den här verifieringen försök hello följande artiklar:

* [Hantera och svarar toosecurity aviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Lär dig hur toomanage aviseringar och svara toosecurity incidenter i Security Center.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md). Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Läs mer om hello olika typer av säkerhetsaviseringar.
* [Felsökningsguide för Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Lär dig hur tootroubleshoot vanliga problem i Security Center. 
* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md). Sök efter vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/). Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.

