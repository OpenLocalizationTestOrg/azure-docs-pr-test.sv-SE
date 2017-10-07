---
title: insamling av aaaEnable i Azure Security Center | Microsoft Docs
description: " Lär dig hur tooenable datainsamling i Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.openlocfilehash: 78bbf9a3d852095e2a1387c1606ff4bbb778a0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-data-collection-in-azure-security-center"></a>Aktivera datainsamling i Azure Security Center

> [!NOTE]
> Från och med tidig juni 2017 Security Center använder hello Microsoft Monitoring Agent toocollect och lagra data. Det finns fler toolearn [Azure Security Center-plattformen migrering](security-center-platform-migration.md). hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>
>

Security Center samlar in data från virtuella datorer (VM)-tooassess sina säkerhetstillstånd anger säkerhetsrekommendationer och varna dig toothreats. Första gången du öppnar Security Center, har du alternativet hello tooenable datainsamling för alla virtuella datorer i din prenumeration. Insamling av data inte är aktiverad, rekommenderar Security Center att du aktiverar datainsamling i hello säkerhetsprincipen för den prenumerationen.

Security Center tillhandahåller hello Microsoft Monitoring Agent på alla befintliga stöd för Azure virtuella datorer och nya filer som skapas när datainsamling har aktiverats. hello Microsoft Monitoring Agent söker efter olika säkerhetsrelaterade konfigurationer. Dessutom genererar hello operativsystem händelser i händelseloggen. Exempel på sådana data är: operativsystemets typ och version, operativsystemloggar (Windows-händelseloggar), processer som körs, datornamn, IP-adresser, inloggad användare och klient-ID. hello Microsoft Monitoring Agent läser händelseloggposter och konfigurationer och kopierar hello tooyour arbetsyta för analys. hello Microsoft Monitoring Agent kopieras även krascher dump filer tooyour arbetsytan.

Om du använder hello kostnadsfria nivån av Security Center kan inaktivera du insamling av data från virtuella datorer genom att stänga av insamling av data i hello säkerhetsprincipen. Inaktivera datainsamling begränsar säkerhet bedömningar för dina virtuella datorer. Det finns fler toolearn [inaktivera datainsamling](#disabling-data-collection). Virtuella diskbilder aktiveras artefakt samlingen och även om datainsamling har inaktiverats. Insamling av data krävs för prenumerationer på hello standardnivån av Security Center.

> [!NOTE]
> Läs mer om Security Center lediga och Standard [prisnivåer](security-center-pricing.md).
>
>

## <a name="implement-hello-recommendation"></a>Implementera hello rekommendation

> [!NOTE]
> Det här dokumentet introducerar hello-tjänsten med hjälp av ett exempel på distribution. Det här dokumentet är inte en stegvis guide.
>
>

1. I hello **rekommendationer** bladet väljer **Aktivera datainsamling för prenumerationer**.  Då öppnas hello **Aktivera datainsamling** bladet.
   ![Bladet rekommendationer][2]
2. På hello **Aktivera datainsamling** bladet väljer din prenumeration. Hej **säkerhetsprincip** bladet för den prenumerationen öppnas.
3. På hello **säkerhetsprincip** bladet väljer **på** under **datainsamling** tooautomatically samla in loggar. Aktivera data samling bestämmelser hello övervakning tillägg på alla aktuella och nya stöd för virtuella datorer i hello prenumerationen.
4. Välj **Spara**.
5. Välj **OK**.

## <a name="disabling-data-collection"></a>Inaktivera datainsamling
Om du använder hello kostnadsfria nivån av Security Center kan inaktivera du insamling av data från virtuella datorer när som helst genom att stänga av insamling av data i hello säkerhetsprincipen. Insamling av data krävs för prenumerationer på hello standardnivån av Security Center.

1. Returnera toohello **Security Center** bladet och väljer hello **princip** panelen. Då öppnas hello **säkerhetsprincip – definiera princip per prenumeration** bladet.
   ![Välj hello princip sida vid sida][5]
2. På hello **säkerhetsprincip – definiera princip per prenumeration** bladet, väljer hello prenumeration som du vill toodisable datainsamling.
3. Hej **säkerhetsprincip** bladet för den prenumerationen öppnas.  Välj **av** under insamling av Data.
4. Välj **spara** i hello översta menyfliksområdet.

## <a name="next-steps"></a>Nästa steg
Den här artikeln visar hur hello tooimplement Security Center rekommendation ”Aktivera datainsamling”. toolearn mer om Security Center finns hello följande:

* [Ange säkerhetsprinciper i Azure Security Center](security-center-policies.md) – Lär dig hur tooconfigure säkerhetsprinciper för dina Azure-prenumerationer och resursgrupper.
* [Hantera säkerhetsrekommendationer i Azure Security Center](security-center-recommendations.md) – Lär dig rekommendationer för att skydda dina Azure-resurser.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md)– Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md)– Lär dig hur toomanage och svara toosecurity aviseringar.
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
- [Datasäkerhet i Azure Security Center](security-center-data-security.md) – Lär dig hur data hanteras och garanteras i Security Center.
* [Vanliga frågor om Azure Security Center](security-center-faq.md)--finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/)--hello Azure security nyheter och information.

<!--Image references-->
[2]: ./media/security-center-enable-data-collection/recommendations.png
[3]: ./media/security-center-enable-data-collection/data-collection.png
[4]: ./media/security-center-enable-data-collection/storage-account.png
[5]: ./media/security-center-enable-data-collection/policy.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
